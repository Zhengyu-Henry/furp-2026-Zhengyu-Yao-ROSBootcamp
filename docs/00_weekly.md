
> Update this file **every week**. Add a new entry at the top for each week.
> This is the first thing we check during review. Keep it honest and specific — it also feeds your attendance record (Rule 1).

**How to use:** copy the *Week template* block below for each new week. Newest week goes at the top.

---

## Week template — copy me

### Week N — YYYY-MM-DD

**Attended this week's meeting:** Yes / No (if No, did you email leave? Yes / No)

**Progress this week**
- _What did you actually do / finish?_

**Challenges & blockers**
- _What got in the way? What are you stuck on?_

**Next steps**
- _What will you do next week?_

**Hours spent (optional):** _e.g. 6h_

**Links (optional):** _commits, notebooks, docs, datasets..._

---

<!-- =================  YOUR ENTRIES BELOW  ================= -->

### Week 1 — YYYY-MM-DD

**Attended this week's meeting: Yes 

**Progress this week**
- Set up repository from the FURP template.
- 预备周(week0)已完成对nodes, topics, services, parameters, actions以及它们的相关CTI Tools的学习，本周将继续完成剩余CTI Tools的学习，同时进入对Client Libraries的学习。
- 使用rqt_console来查看和过滤日志消息，通过turtlesim演示了出现意外时（turtle撞墙了）显示的日志消息，并认识了日志消息不同的级别顺序以及设置日志级别的命令。
   <img width="2187" height="1392" alt="截图 2026-06-09 11-41-53" src="https://github.com/user-attachments/assets/aaae2f64-9aae-4d5e-8a5f-d58fb88a9a1a" />
- 认识Launch文件，用于解决手动启动节点较繁杂的问题。我使用了一个python格式的Launch文件，直接同时打开了两个turtlesim，即同时启动了两个节点。
   <img width="2187" height="1392" alt="截图 2026-06-09 15-16-40" src="https://github.com/user-attachments/assets/19d41244-0d60-4c66-a8c6-807a307b8320" />
- 认识ros2 bag，用于记录和回放话题上的数据。依然使用turtlesim来演示，图片一展示了记录单一话题的相关命令，
  图片二展示了同时记录两个话题并进行回放的过程，输入回放命令后观察到turtle按照先前记录的轨迹移动，两次的移动轨迹是大致相同的（呈两个不规则的近圆形）
  <img width="2187" height="1392" alt="截图 2026-06-09 15-34-58" src="https://github.com/user-attachments/assets/f43c4f91-ee36-4055-a1db-05896c12e046" />
  <img width="2187" height="1392" alt="截图 2026-06-09 16-20-22" src="https://github.com/user-attachments/assets/4d4d4582-b43f-44b7-bf05-a5e671f39ddf" />
  自此正式进入对Client Libraries的学习。
- Using colcon to build packages
  1. 认识ros workspace的基本概念：一个里面按照特定结构组织代码的文件夹，包含src/(源代码目录), build/(中间文件目录), install/(安装目录), log/(日志目录)这些目录。可以类比为整个“公司”的办公室大楼。
  2. 认识underlay和overlay的基本概念：underlay指已经存在的ROS2环境，提供基础的ROS2库、工具和依赖；overlay指自己创建的workspace，在这里编写和编译自己的包，可以覆盖或扩展underlay中的功能。
     overlay优先级高于underlay，在overlay中的编译修改不会影响到underlay。
     需要特别注意的是：在编译自己的ROS2 workspace之前，必须先source系统安装的ROS2环境(underlay)，然后workspace(overlay)才能正确构建和运行。
  3. 认识功能包(package)是一个具体的功能模块，包含代码、配置文件、描述文件，可以类比为公司的一个“部门”。
  4. 认识colcon_cd命令：不用每次都输入长路径，就能直接跳到某个功能包的目录下。
  5. 认识colcon mixins：colcon的一个快捷方式/预设功能，用于简化代码，提供准确度。
  6. 认识colcon build：是ROS2中用于编译workspace的命令，即把源代码变成可运行的节点。
- Creating a workspace
  1. 使用mkdir命令创建一个workspace，命名为ros_ws，刚开始该文件夹中的src/目录是空的。
  2. clone a sample repo(ros_tutorials)
  3. 通过rosdep解决依赖（最好每次clone后都确认一遍依赖是否完全）
     Rosdep是ROS官方为了方便开发者管理依赖而设计的工具。
       - sudo resdep init：只需在整个ROS环境中执行一次，负责为ROS系统初始化“软件源”。
         软件源就是存放软件包的“仓库地址”，ROS有自己的软件包和依赖，这些包不在Ubuntu默认源里，所以ROS需要告诉rosdep工具软件源在哪里，这个“告诉”的动作就是初始化软件源。
       - rosdep update：负责从刚才配置好的源网址，下载并更新本地的rosdep数据库。
  4. 通过colcon完成对workspace的编译（colcon build）
  5. 分别source ROS2核心环境(underlay)和刚刚编译好的workspace中的install目录（因为ROS2编译后生成的环境脚本和可执行文件都放在install目录下），效果和只source ROS2核心环境的效果是一样的。
     source /opt/ros/humble/setup.bash
     source install/local_setup.bash
  6. 尝试demo（分别运行了一个publisher node和subscriber node）
  <img width="2187" height="1392" alt="截图 2026-06-10 10-28-27" src="https://github.com/user-attachments/assets/d8391dc5-972f-47ac-8793-b491dd1bb7cd" />
- creating a package
  必须在workspace中的src/目录下创建（colcon默认只查找src/子目录下的包）
  创建包的命令：ros2 pkg create --build-type ament_python --license Apache-2.0 py_pubsub。
              ros2 pkg create：创建新功能包。
              --build-type ament_python：指定包的构建类型为Python(使用ament_python模板)。另一种常用的是ament_cmake(C++包)。
              --license Apache-2.0：指定包的许可证为Apache 2.0。
              py_pubsub：要创建的功能包的名称。
- Writing a simple publisher and subscriber(Python)
  1. 基本理解了一段简单节点代码的结构。
```
import rclpy
from rclpy.node import Node

from std_msgs.msg import String


class MinimalPublisher(Node):

    def __init__(self):
        super().__init__('minimal_publisher')
        self.publisher_ = self.create_publisher(String, 'topic', 10)
        timer_period = 0.5  # seconds
        self.timer = self.create_timer(timer_period, self.timer_callback)
        self.i = 0

    def timer_callback(self):
        msg = String()
        msg.data = 'Hello World: %d' % self.i
        self.publisher_.publish(msg)
        self.get_logger().info('Publishing: "%s"' % msg.data)
        self.i += 1


def main(args=None):
    rclpy.init(args=args)

    minimal_publisher = MinimalPublisher()

    rclpy.spin(minimal_publisher)

    # Destroy the node explicitly
    # (optional - otherwise it will be done automatically
    # when the garbage collector destroys the node object)
    minimal_publisher.destroy_node()
    rclpy.shutdown()


if __name__ == '__main__':
    main()
```
  其中
  - class MinimalPublisher(Node)：定义一个名为MinimalPublisher的类，并且它继承自Node（括号内的Node表示父类）。class + 类名 + （父类）：表示创建子类，子类拥有父类的所有属性和方法。
  - __init__：是Python中类的构造函数（初始化方法）。当创建一个类的实例（对象）时，Python会自动调用这个方法，用来设置对象的初始状态或执行必要的准备工作。它的第一个参数必须是self，代表即将被创建的实例本身。
  - super().__init__('minimal_publisher')：是Python中调用父类的构造函数的标准写法，并将节点名称传递给它。
  - self.publisher_ = self.create_publisher(String, 'topic', 10)：创建一个发布者，它将在名为“topic”的话题上发布std_msgs/String类型的消息，并允许最多缓存10条未处理的消息，该发布者对象保存在self.pulisher_中，供后续的发布操作使用。
  - self.timer = self.create_timer(timer_period, self.timer_callback)：创建一个定时器，参数包括周期（0.5秒）和回调函数，该定时器对象保存在self.timer中。
  -  msg.data = 'Hello World: %d' % self.i：msg是一个对象，类型是std_msgs.msg.String，data是该消息对象的一个字段（属性），用于存放实际要发送的字符串。
      
  
  
  2. 节点代码本身只描述了“做什么”，但ROS2的构建系统和运行时工具需要额外的声明文件来知道“这个包需要什么依赖”以及“如何找到这个包”
     因此编写好节点代码后必须在package.xml中声明所有直接依赖（上述节点代码中的依赖是rclpy和std_msgs）
     package.xml是ROS2生态的包的“身份证”文件，专用于ROS2的构建系统的colcon。
```
<exec_depend>rclpy</exec_depend>
<exec_depend>std_msgs</exec_depend>
```
  3. 节点代码是一个普通的Python脚本，如果想要运行需要ros2 run找到它。
     setup.py是Python生态的标准打包配置文件，为Python的打包工具setuptools提供信息。
     而entry_points是setuptools的一个机制，用于创建控制台脚本(console scripts)。
     控制台脚本则是能把包里的Python函数，直接变成一个你可以在终端里运行的命令，相当于一个“快捷方式”。
```
entry_points={
        'console_scripts': [
                'talker = py_pubsub.publisher_member_function:main',
        ],
},
```
     这里talker就是最终在终端运行使用的快捷方式，而后续的那一长串相当于定位，告诉setuptools当用户输入talker命令时，应该去找py_pubhub包的publisher_member_function.py文件中找到main函数并执行。
- Writing a simple service and client(Python)
  1. 创建package时使用--dependencies，可以不用手动再将dependencies加入到package.xml中了。
  2. 按照tutorial创建了一个service和一个client，并成功运行，实现了消息的发出、执行和接受，基本理解了简单service代码和client代码的结构。
  3. 认识AddTwoInts：ROS2内置的服务接口（位于example_interfaces包），包含请求字段int64 a和int64 b，响应字段int64 sum。
  4. 认识sys：用于读取命令行参数。
- Creating custom msg and srv files
  1. 认识rosidl_default_generators：是一个构建工具，负责将.msg和.srv文件转换成不同语言（C++、Python等）的代码。需要在构建时使用它，所以要用<buildtool_depend>声明。
  2. 认识rosidl_default_runtime：是运行时依赖，提供解析和使用自定义消息类型所需的基础库，因此需要<exec_depend>。
  3. 如果自定义消息引用了其他包的消息，则必须通过<depend>或<build_depend>+<exec_depend>声明对那个包的依赖，确保编译和运行时都能找到它。
  4. <member_of_group>rosidl_interface_packages</member_of_group>：是ROS2的约定，用于将此包标记为“接口包”，使得其他包能够通过find_package正确找到你的接口。

**Challenges & blockers**
- 目前虽然进度一直在推进，但对大量新概念和新逻辑仍有些混乱，有待仔细梳理一下。
- 编写node, service, client等代码仍有较大困难，主要是Python基础尚不熟练。

**Next steps**
- 继续推进ROS2的学习，同时开始第二周主线任务的学习（Carter建模、URDF、Xacro和TF），尽早学完能开始动手实践。
- 逐步梳理大量的新学知识，不但能看懂还要会写会用。
- 通过精读tutorial给出的样本代码（node, service, client等），逐步熟悉Python相关知识，提高自己编写代码的能力。

### Week 2 — 2026-6-15

**Attended this week's meeting:** Yes / No (if No, did you email leave? Yes / No)

**Progress this week**
- 学习/cmd_vel
  1. 含义：是ROS2中一个标准的、约定俗成的话题名称，用于向机器人发送速度控制指令。
  2. /cmd_vel话题上传送的是geometry_msgs/Twist类型的消息，包含linear（线速度）和angular（角速度）两个部分，线速度控制机器人前进或后退的快慢，角速度控制机器人旋转的快慢。
- 学习Launch文件
  1. 认识Launch文件的基本含义：是ROS2的“一键启动脚本”，能够批量启动节点、自动配置参数、管理节点属性、控制启动顺序。
  2. 能够创建一个Launch文件并基本认识Launch文件的Python写法
     - from launch import LaunchDescription：LaunchDescription是launch文件的“剧本”，里面列出要启动的节点。
     - def generate_launch_description()：定义的这个函数必须叫这个名字，ROS2的ros2 launch命令会调用它返回LaunchDescription对象。
  3. 如果要将Launch文件融入到功能包里，需要创建存放Launch文件的结构，因为ROS2的ros2 launch命令会按照约定路线查找launch文件，并且构建系统(colcon)需要知道把这些文件安装到哪里去。
     - Launch文件必须放在包的launch/目录下，并且构建后必须被复制到install/share/my_package/launch/。如果没有这个结构，ros2 launch就会报找不到文件。
     - 因此需要通过setup.py的data_files参数显示告诉setuptools：请把launch/目录下的文件复制到share/包名/launch下。
     - 基本理解了输入到setup.py中的内容。其中os模块用于路径拼接，glob模块用于匹配文件模式。
  4. 认识Substitution：是一种在执行时才被计算和替换的变量，它让你可以在Launch文件中使用动态的值，而不是写死固定的字符串。
- 认识时间戳(timestamp)：在ROS2中，绝大多数消息（特别是传感器数据和TF变换）都包含一个时间戳字段，用来告诉系统“我是在这个时间点被测量/生成的”。
- 认识传感器(sensor)：相当于“机器人感知真实世界的器官”。它把物理世界中的信号（光、声音、距离、力、温度等）转换成机器人能理解的电信号或数据。
- 学习XML，为后续学习URDF做准备
  1. <?xml version="1.0" encoding="UTF-8"?>：XML声明，指定版本和编码。必写，但一般不用修改。
  2. <launch>：所有XML launch文件的根元素
  3. <arg name="参数名" default="默认值" />：定义参数，可以在调用时传入值覆盖默认值。
  4. <let name="变量名" value="变量的值" />：定义一个局部变量。
  5. <node pkg="包名" namespace="命名空间" exec="可执行文件" name="节点名" />：启动一个节点。
  6. <param name="参数名" value="参数值" />：设置节点参数。
  7. <executable cmd="命令内容" />：在系统shell中执行一条命令。
  8. <timer period="周期" />：定义一个计时器，每多久（周期）执行一次内部的内容。
  9. if属性：条件判断，只有条件为真时才执行。
  10. <include file="指定被包含文件的路径">：引用另一个launch文件。
  11. $()：命令替换语法。先执行括号里面的命令，然后把命令的输出结果替换到这里。
  12. find-pkg-share 包名：查找某个包的share目录。
- Writing a static broadcaster(Python)
  1. 认识Broadcaster（广播器）：专门负责把坐标变换(Transform)发布出去的工具。你把一个写好的TransformStamped对象塞给它，它立刻把这个数据打包，发送到/tf话题上，任何订阅了/tf的节点都能收到这个广播。
     - 普通广播器（动态）：你每隔0.1秒调用一次sendTransform，广播一次当前时间点的最新坐标（比如odom到base_link在不断变化）。
     - 静态广播器(Static Broadcaster)：专门用来发永远不变的变化（比如base_link到laser_link的安装位置）。它只发一次，且发到/tf_static话题，而不是/tf，节约带宽。
  2. static broadcaster node
     - math, numpy：用于数学计算，特别是四元数转换。
     - from geometry_msgs.msg import TransformStamped：TF消息类型，包含时间戳、父坐标系（变换的参考基准）、子坐标系（被描述的对象）、平移和旋转。是ROS2中用来表达Transform的具体消息结构，可以理解为带上了时间戳、收件人和发件人信息的Transform。
     - StaticTransformBroadcaster(self)：创建一个专门用来发布静态坐标变换的广播器对象，并且把这个对象交给当前这个节点(self)来管理。
  3. ROS2已经准备好了现成的命令行工具和launch节点，不用写代码。ros2 run tf2_ros static_transform_publisher --x 0 --y 0 --z 1 --yaw 0 --pitch 0 --roll 0 --frame-id world --child-frame-id mystaticturtle：系统会发布一个静态变换，表明mystaticturtle坐标系始终固定在world坐标系上方1米处，且朝向完全一致。
- Writing a broadcaster (Python)
  1. broadcaster node
     - from tf2_ros import TransformBroadcaster：是TF系统的专属发布者，专门用来把TransformStamped这种消息发布到/tf话题上。（Broadcaster是一个大类，TransformBroadcaster是其中的一员）。
     - from turtlesim.msg import Pose：乌龟位姿的消息类型（包含x，y，theta，线速度，角速度）。
- Writing a listener (Python)
  1. listener node
     - from geometry_msgs.msg import Twist：速度消息，用于控制乌龟移动。
     - from tf2_ros.buffer import Buffer：一个存储所有TF变换信息的“内存数据库”。把你查询过的、或者接收到的所有坐标变换数据，按照时间戳和坐标系名称分门别类地存起来。
     - from tf2_ros.transform_listener import TransformListener：自动接收系统里广播器发来的所有TransformStamped消息，然后一条一条存进你Buffer仓库里。
     - from turtlesim.srv import Spawn：服务，用于生成一只新乌龟。
- Adding a frame (Python)
  1. TF Tree
     - 一个坐标系只能有一个父坐标系，但可以有多个子坐标系。根坐标系是整个系统的绝对基准（通常叫做map或world），它没有父坐标系。
     - 想象你手里拿着一个激光笔，站在房间中央：父坐标系=你自己的身体，即参考基准；子坐标系=墙上的红点，即被描述的东西。
  2. ros2 run tf2_tools view_frames：这个命令会启动一个监听器，在5秒内收集系统中所有的TF广播数据，然后在当前终端所在的目录下生成一个名为frames.pdf的文件（即TF树）。
- 初步认识frame和TF：frame即坐标系，在ROS2中，系统通过frame的名字（world, turtle1, base_link, laser_link等）来识别不同的坐标系。frame之间的连接叫做TF，frame相当于地图上的一个地标点，而TF描述了从一个地标到另一个地标的距离和方向。例如雷达说前方1米有障碍，这个1米是相对于laser_link的，只有通过TF把它转换到base_link或map，机器人才知道这个障碍相对于我的底盘在哪里。
- 认识map：是ROS2中一个非常特殊的坐标系，代表了机器人在真实世界中的绝对参考点，提供绝对位置
- 认识odom：即odometry（里程计），是ROS2中一个约定的坐标系名称，是一个“从起始点开始，通过机器人自身运动估算出来的实时位置”的参考系，提供高频、平滑的短期运动估计。
- 认识transform（变换）：描述的是一个坐标系相对于另一个坐标系在三维空间中的位置（平移）和朝向关系（旋转），即在X、Y、Z三个方向上的距离偏移和三维空间中的朝向（ROS2用四元数表示）
- URDF学习
  1. 什么是URDF？
     - Unified Robot Description Format，统一机器人描述格式。
     - 可以解析URDF文件中使用XML格式描述的机器人模型。
     - 包含link（连杆）和joint（关节）自身及相关属性的描述信息。
  2. <link>：描述机器人某个刚体部分的外观和物理属性；描述连杆尺寸（size）、颜色（color）、形状（shape）、惯性矩阵（inertial matrix）、碰撞参数（collision properties）等；每个link会成为一个坐标系
  3. <joint>：描述两个link之间的关系，分为六种类型(continuous, revolute, prismatic, fixed, floating, planar);包括关节运动的位置和速度限制；描述机器人关节的运动学和动力学属性。
  4. <robot>：完整机器人模型的最顶层标签；<link>和<joint>标签都必须包含在<robot>标签内；一个完整的机器人模型，由一系列<joint>和<link>组成。
  5. RGBA：一种颜色描述方式，即R（红色）+G（绿色）+B（蓝色）+A（透明度），数值范围在0.0~1.0（浮点数），0.0表示完全没有该颜色或完全透明，1.0表示完全饱和或完全不透明。
  6. <origin xyz="" rpy="" />：URDF中用来定义“位置和姿态”的标签。xyz定义平移，单位为米；rpy定义旋转，即roll（翻滚，绕X轴旋转）, pitch（俯仰，绕Y轴旋转）, yaw（偏航，绕Z轴旋转），单位为弧度。
  7. <axis xyz="" />：是URDF中专门用来定义“关节怎么动”的标签，不关心长度，只关心方向。
  8. <collision>：是URDF中为了“物理碰撞计算”而存在的简化几何体。可以理解为游戏里的碰撞体积。
  9. <inertial>：是URDF中专门给物理仿真引擎（如Gazebo）看的物理属性信息。如果只是用RViz显示机器人模型，可以不写（因为RViz只负责画图，不关心物理），但如果要在Gazebo里做物理仿真，就必须写。包含<mass>和<inertia>这两个子标签。<mass>就是这个零件的质量，单位为千克；<inertia>是转动惯量（转起来的阻力），包含ixx, ixy, ixz, iyy, iyz, izz六个值，ixx, iyy, izz为主对角线，表示绕X、Y、Z轴旋转的“阻力”有多大，数值越大，越难让它转起来，也越难让它停下来，ixy, ixz, iyz为副对角线，描述一个轴旋转时会不会带动另一个轴跟着转，对于对称形状，这些值通常为0。
- Xacro学习
  1. Xacro即XML Macros，引入“宏”(Macro)的概念，可以像定义函数一样，把重复的部件逻辑封装成一个“宏”，并给它设定参数。
  2. xacro model.xacro > model.urdf：把.xacro文件转换成.urdf文件。
  3. 一个小例子：
```
<xacro:property name="robotname" value="marvin" />
<link name="${robotname}s_leg" />
```
${}中的内容将替代掉${}，即<link name="marvins_leg" />
```
<cylinder radius="${wheeldiam/2}" length="0.1"/>
<origin xyz="${reflect*(width+.02)} 0 0.25" />
```
${}中还可以包含数学计算。
  4. “宏”的使用（重点！）
```
<xacro:macro name="default_inertial" params="mass">
    <inertial>
            <mass value="${mass}" />
            <inertia ixx="1e-3" ixy="0.0" ixz="0.0"
                 iyy="1e-3" iyz="0.0"
                 izz="1e-3" />
    </inertial>
</xacro:macro>
```
这段代码中把inertial这个标签的内容打包成一个“宏”，命名为default_inertial，并准备接受一个参数mass。调用时：<xacro:default_inertial mass="10"/>。输出如下：
```
<inertial>
    <mass value="10" />
    <inertia ixx="1e-3" ... izz="1e-3 />
<inertial>
```
- RViz学习
  1. 在RViz里，Fixed Frame（固定坐标系）是所有可视化数据的共同参考基准，相当于整个3D世界的“大地”；Target Frame（目标坐标系）是3D视图摄像头要追踪观察的目标，决定了视图的“焦点”和观察方式。
  2. 

**Challenges & blockers**
- _What got in the way? What are you stuck on?_

**Next steps**
- _What will you do next week?_

**Hours spent (optional):** _e.g. 6h_

**Links (optional):** _commits, notebooks, docs, datasets..._


### Week 3 — 2026-06-22

**Attended this week's meeting:** Yes / No (if No, did you email leave? Yes / No)

**Progress this week**
- /cmd_vel到左右轮速度（差速运动学）
  1. 核心问题：ROS2和电机说的不是同一种语言，ROS2（上位机）说的是线速度和角速度（即/cmd_vel里的linear.x和angular.z）；电机（下位机）说的是左轮转多快（rad/s）和右轮转多快（rad/s）（即电机的速度指令）。因此需要一个翻译官来把ROS2的“前后+转弯”翻译成“左轮速度+右轮速度”，这个翻译官就叫做“差速运动学”。
  2. 机器人两个驱动轮中心之间的距离为w（即轮距，wheelbase），左轮速度(vl)=linear.x - (angular.z × w)/2，右轮速度(vr)=linear.x + (angular.z × w)/2。
  3. ros2_control中的diff_drive_controller已经内置了这些公式，只需要在YAML配置文件中告诉它轮距(w)和轮子半径(r)即可，一旦配置好，diff_drive_controller会自动订阅/cmd_vel，计算左右轮速度，并发送给电机硬件。
- 编码器tick到轮速
  1. 编码器的作用：它是电机的“眼睛”和“记速器”，能精准测量轮子实际转了多少圈、多块。这样你才能知道机器人真实走了多远，而不是你以为它走了多远（因为真实世界有摩擦力、地面打滑、电池电压波动等）。
  2. 编码器tick：编码器内部有一个圆盘，上面有一圈均匀的栅格或透光缝隙。电机每转过一个栅格，编码器就输出一个脉冲信号，这就是一个tick。
     - tick是整数，是编码器输出的原始计数（比如0，1，2，3...）
     - 如果往前转则tick单调递增，往后转则tick单调递减。
     - 电机转得越快，单位时间内收到的tick数量越多。
     - PPR(Pulses Per Revolution，每转脉冲数)：电机轴每转一整圈，编码器产生tick的数量。
   3. 你需要把tick的变化率换算成轮子的线速度和角速度。同样diff_drive_controller内部已经封装了“编码器反馈处理”模块，只需要在YAML配置文件中告诉它轮子半径(r)和多久发布一次里程计（通常是50Hz）即可，控制器会订阅底层硬件节点发来的原始tick消息，自动完成公式计算，并最终生成/odom话题里的速度值，以及TF树中的odom到base_link变换。
- ros2_control：是ROS2官方提供的硬件抽象框架，它为不同机器人提供一套统一的控制接口。
- diff_drive_controller：是ros2_control中专门针对差速驱动机器人的官方控制器。它订阅/cmd_vel，接收了linear.x和angular.z；计算了左右轮速度；发布了/odom（根据轮子实际转动，积分计算里程计数据，发布nav_msgs/Odometry；广播odom到base_link的TF，让整个系统知道机器人的位置；把“目标轮速”转换成电机硬件能执行的指令。
- 速度限制与加速度限制：为线速度和角速度设置最大速度和最大加速度，防止电机跳闸、打滑或烧毁。
- SLAM Toolbox
  1. SLAM（Simultaneous Localization and Mapping，同步定位于地图构建）不需要任何先验信息，只用传感器数据和数学算法，同时估算位置和地图。
  2. 输入：
     - /scan（激光雷达数据）：像无数根红外线尺子，告诉你前方几米有墙等信息。
     - /odom（里程计数据）：告诉你大概走了多远，大概转了多少度。是大概，会有漂移。
     - /tf（坐标变换）：告诉你雷达装在底盘的哪里，底盘相对于世界的朝向。
  3. 处理：
     - 扫描匹配：把当前这帧激光雷达数据，和之前的几帧进行“拼图”，来分辨机器人动没动。
     - 图优化
  4. 输出（机器人“画出”了什么）
     - /map
     - 修正后的位姿：SLAM输出的机器人位置比纯里程计准确得多，它会发布map到odom的TF变换来纠正漂移。
  5. 参数
     - 坐标系名称(Frames)
     - 话题名称(Topics)
     - 运行模式(Modes)：建图模式填mapping，定位模式填localization。
     - 地图分辨率(Resolution)
     - 传感器范围(Range)
     - 仿真时间(use_sim_time)：填true，即使用仿真时间，否则时间戳对不齐。
 - Nav2架构
   1. Nav2是一个用于机器人自主导航的模块化框架，它通过多个独立服务器协同工作，让机器人能理解环境、规划路径并躲避障碍。
   2. Nav2采用插件化架构，核心是行为树导航器(Behavior Tree Nevigator)，它像一个总指挥，通过调用各个独立的功能服务器（如规划、控制等）来完成任务。
   3. Map Server（地图服务器）：用于加载、提供和保存环境地图。
   4. Localization（定位）：定位模块负责回答“机器人在哪”的问题。
   5. Planner Server（规划器服务器）：即“全局规划器“。它的任务是根据当前地图和机器人位置，计算出一条从起点到目标点的全局最优路径。
   6. Controller Server（控制器服务器）：即”局部控制器“。它负责执行规划器生成的全局路径，将路径转换成具体的速度指令发送给电机。它主要关注机器人周围局部的动态环境，进行实时避障。
   7. Behavior Tree Navigator（行为树导航器）：是Nav2的决策和调度核心。它使用行为树(BT)来定义和组织复杂的导航行为。
   8. Costmap（代价地图）：是机器人用来表示环境”通行代价“的2D网格图。
      - Global Costmap（全局代价地图）：基于整个静态地图构建，范围大、更新慢。Planner Server使用它来规划全局路径。
      - Local Costmap（局部代价地图）：只关注机器人周围的动态环境，是一个跟随机器人移动的小窗口，更新频率高。Controller Server使用它来进行实时避障和生成局部轨迹。
   9. Obstacle Layer（障碍层）：构成代价地图的核心图层。实时处理传感器数据（如激光雷达），将检测到的障碍物标记在代价地图上。
   10. Inflation Layer（膨胀层）：构成代价地图的核心图层。在障碍物周围根据距离计算出一个”危险梯度“。越靠近障碍物，代价值越高，以此确保规划出的路径会与障碍物保持安全距离。
**Challenges & blockers**
- _What got in the way? What are you stuck on?_

**Next steps**
- _What will you do next week?_

**Hours spent (optional):** _e.g. 6h_

**Links (optional):** _commits, notebooks, docs, datasets..._
