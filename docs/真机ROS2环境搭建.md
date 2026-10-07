# 真机运行流程
## 准备工作：
1. 进入VMware，选择你要使用的虚拟机（关机状态），把Network调成bridged，同时勾选下方的Replicate physical network connection state，点OK完成设置。
2. 打开小车开关，找到Wheeltec名字的网络，电脑连接（可能要等一小会）。连接上后是无法上网的，想要上网需要用手机（仅限Android）连接小车，分享手机热点。
3. 进入虚拟机，打开一个终端，输入ssh -Y wheeltec@192.168.0.100，密码是dongguan，这样就进入小车的终端了。该步骤每新开一个终端都需要进行一次。

## 停掉ROS1，确认串口空出来
```
pkill -f roslaunch; pkill -f wheeltec_robot_node; pkill -f rosmaster
pkill -f robot_state_publisher; pkill -f robot_pose_ekf; pkill -f lslidar
sleep 3
sudo fuser -v /dev/wheeltec_controller 2>&1     # ★ 必须无输出
```

## 定死时钟（这步只在当前开机状态下有效，一旦小车断电重启，则需要重新设置）
```
sudo timedatectl set-ntp false    #关掉 NTP，防止跑到一半又跳。
watch -n 1 date +"%Y-%m-%d %H:%M:%S"    #查看实时时间。
sudo date -s "2026-09-20 10:00:00"    #写当前时间，对的越准越好。
```

## 确保小车和虚拟机具有相同的环境变量（注意，每个终端都要设置）
```
export ROS_DOMAIN_ID=42    
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp    
unset ROS_LOCALHOST_ONLY    
```
- 设置ROS2的域ID为42，只有真机和虚拟机的域ID都等于42，它们才能互相看到对方的话题。
- 强制使用CycloneDDS作为通信中间件。
- ROS_LOCALHOST_ONLY环境变量会让ROS2只允许本机和自己通信，取消这个环境变量的设置，保证跨机通信。

## 起容器 + source环境 + 容器新窗口
```
sudo docker run -it --rm --name s100 \
  --network host \
  --device=/dev/ttyCH343USB0 --device=/dev/ttyCH343USB1 \
  -e ROS_DOMAIN_ID=42 \
  -e RMW_IMPLEMENTATION=rmw_cyclonedds_cpp \
  -e CYCLONEDDS_URI=file:///root/ros2_ws/cyclonedds.xml \
  -v /home/wheeltec/s100_ros2_ws:/root/ros2_ws \
  -w /root/ros2_ws \
  s100-humble:latest \
  bash -c 'ln -sf /dev/ttyCH343USB0 /dev/wheeltec_controller; ln -sf /dev/ttyCH343USB1 /dev/wheeltec_lidar; exec bash'
```
- 容器名叫s100，用完即毁（按exit退出容器后，这个容器自动删除，不留垃圾）。
- 网络不隔离，让容器直接使用真机的网卡和端口。
- 域ID为42。
- 强制使用CycloneDDS作为通信中间件。
- 代码共享。把真机里的代码目录，挂载到容器的 /root/ros2_ws。你在容器里改代码，真机里立刻同步；反之亦然。
- 容器启动后默认把工作目录切换到/root/ros2_ws这里，省得进去还要敲cd。
- 指定使用s100-humble:latest镜像。
- 硬件设备直通，把真机上的两个 USB 串口（底盘控制器和雷达）直接穿透进容器里。
```
source /opt/ros/humble/setup.bash
source install/setup.bash
```
想要在一个新的终端里再次打开容器，就用这条命令，而且命令里已经包含source了。
```
sudo docker exec -it s100 bash -lc \
  'source /opt/ros/humble/setup.bash && source /root/ros2_ws/install/setup.bash && exec bash'
```

## 启动雷达、底盘、SLAM、Nav2
底盘启动，但SLAM是全栈式启动，即启动SLAM的同时也启动了底盘和雷达，因此无单独需要启动SLAM即可。
```
ros2 launch turn_on_wheeltec_robot turn_on_wheeltec_robot.launch.py
```
雷达启动，但SLAM是全栈式启动，即启动SLAM的同时也启动了底盘和雷达，因此无单独需要启动SLAM即可。
```
ros2 launch turn_on_wheeltec_robot wheeltec_lidar.launch.py（无需启动，包含在SLAM里了）
```
底盘 + 雷达 + SLAM全栈式启动。
```
ros2 launch wheeltec_slam_toolbox online_async_launch.py
```
Nav2启动，边建图边导航模式。
```
ros2 launch nav2_bringup navigation_launch.py \
  params_file:=/root/ros2_ws/src/wheeltec_robot_nav2/param/wheeltec_params/param_V650_diff.yaml \
  use_sim_time:=false \
  autostart:=true
```
- SLAM的参数文件为mapping_online_async.yaml。
- Nav2的参数文件为param_V650_diff.yaml。

## 建图与图像查看
保存地图
```
ros2 run nav2_map_server map_saver_cli -f /root/ros2_ws/maps/"你要保存地图的名字"
```
查看地图
```
xdg-open ~/s100_ros2_ws/maps/"你要保存地图的名字".pgm
```

## 在虚拟机上打开RViz2
在虚拟机的终端里设置好环境。
```
export ROS_DOMAIN_ID=42
export RMW_IMPLEMENTATION=rmw_cyclonedds_cpp
unset ROS_LOCALHOST_ONLY
export CYCLONEDDS_URI=file:///home/zhengyu/cyclonedds.xml
```
如果能看到/tf、/scan、/odom等一大堆话题，就说明和真机是通的。
```
ros2 topic list
```
把厂商已经设置好的RViz配置放到虚拟机里，省去了手动配置。
```
scp wheeltec@192.168.0.100:~/s100_ros2_ws/nav2_view.rviz ~/nav2_view.rviz
```
打开RViz
```
rviz2 -d ~/nav2_view.rviz
```

## 一些常用的命令
### 真机里找文件并修改
当前路径下所有的文件内容。
```
ls
```
在当前搜索目录的路径下通过名字来寻找文件。
```
find <搜索目录> -name "<文件名>"
```
如果你知道这个文件属于哪个包，但不知道装哪了，就用这条命令。
```
ros2 pkg prefix <包名>
```
进入编辑界面
```
vim /"路径"/“文件名”
vim 文件名
```
进入界面后，点"i"进入编辑模式，编辑完esc退出，按:wq回车保存。如果显示readonly，就在vim命令前面加上sudo。

### 杀进程命令
```
pkill -f 'ros2 launch' 
pkill -f wheeltec_robot
pkill -f lslidar
pkill -f slam_toolbox
pkill -f ekf
sleep 5
```

## 平凡替身实验
- 用最笨的假节点，即平凡替身，替换掉可疑部件，只要平凡替身复现了症状，病灶就在你替身掉的那一层之下。此时任何留在原层的调整都是浪费时间。
- 本次是针对/scan延迟增加和掉频的问题，引入了假发布者与反订阅者。
1. 假发布者是一个纯 Python 脚本，它没有任何复杂的算法，没有串口读取，没有 C++ 的互斥锁，只是单纯地以 12Hz 的固定频率往 DDS 里扔 1667 个点。
2. 哑订阅者更极端，它订阅了数据但立刻扔进垃圾桶（> /dev/null），不做任何计算。
3. 说明的问题：如果连这种“极简的无脑程序”加上一个“只看不做的哑巴订阅者”都会导致掉频，那就证明了问题既不在 SLAM 算法，也不在 CPU 算力，更不在雷达驱动的 C++ 代码里，传输层（DDS + 网络接口）才是罪魁祸首。
4. 
