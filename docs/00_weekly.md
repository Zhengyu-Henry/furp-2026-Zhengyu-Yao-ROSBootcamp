
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
  1. topic：持续数据流，多对多，异步。典型例子有/scan、/image、/odom。
  2. service：一次请求，一次回答。典型例子有保存地图、查询状态、重置计数器。
  3. action：长任务，有反馈，可取消。典型例子有导航到目标点、执行轨迹、抓取任务。
  4. parameter：运行时配置。典型例子有速度上限、frame 名、阈值。
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

**Attended this week's meeting:** Yes 

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
  5. LaunchCofiguration：表示一个可以在运行时获取值的变量（即命令行参数的值），为整个导航栈定义可动态配置的输入参数，以便在不同场景（真实机器人/仿真/多机器人）下复用，而不需要修改 Launch 文件本身。
  6. get_package_share_directory：查找某个包的安装路径。
  7. LaunchDescription：每个Launch文件的必须返回值；定义generate_launch_description()函数：这是ROS2 launch文件的入口点，必须叫这个名字。
  8. DeclareLaunchArgument：声明一个可以在命令行覆盖的参数。
  9. IncludeLaunchDescription：包含另一个Launch文件；launch_arguments：把一堆参数传给被包含的launch文件，这些参数会覆盖被包含文件内部的默认值（如果它们有同名参数的话），如果没有设置被包含文件中的一些参数，则这些参数使用被包含文件中的默认值。
  10. IfCondition：条件判断。
  11. PythonLaunchDescriptionSource：指定被包含的launch文件的来源（Python格式）。
  12. PythonExpression：允许在launch文件中嵌入Python表达式。
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

**Attended this week's meeting:** Yes 

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
- diff_drive_controller（底盘驱动）：是ros2_control中专门针对差速驱动机器人的官方控制器。它订阅/cmd_vel，接收了linear.x和angular.z；计算了左右轮速度；发布了/odom（根据轮子实际转动，积分计算里程计数据，发布nav_msgs/Odometry；广播odom到base_link的TF，让整个系统知道机器人的位置；把“目标轮速”转换成电机硬件能执行的指令。
- odom是怎么发布的？
  1. odom到base_link这个TF变换，只能由一个权威节点发布，但这个节点可以是底盘驱动节点，也可以是EKF融合节点。
  2. 底盘直接发布/odom：底盘驱动（如diff_drive_controller）靠轮子编码器算位置，它的数据是纯局部的，简单、平滑，但打滑或路径不平就会漂移，且永远不知道自己偏了。
  3. 融合定位（带IMU的高级配置，EKF发布）：EKF robot_localization是官方EKF节点，可以看作一个“智能数据融合大脑”，它能将轮式里程计、IMU等多个传感器数据融合在一起，计算出机器人最有可能的精确位置。这样系统中所有需要位置信息的节点（如Nav2、RViz）都从同一个来源（EKF）获取位置，避免了数据不一致。
  4. 如果使用EKF发布，则底盘驱动只提供原始测量数据。底盘驱动不再直接发布/odom话题，而是发布另一个话题（比如/raw_odom或/wheel_odom），并不再发布odom到base_link的TF。EKF节点订阅这个/raw_odom，把它当作输入源之一。
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
   3. Nav2的输入：/map（地图）, /tf（坐标系变换）, /scan（激光雷达）, /odom（里程计）；输出：/cmd_vel（速度指令）。使用ros2 topic list和ros2 run tf2_tools view_frames来确认它们都存在。
   4. AMCL：是Nav2中的定位节点，职责是根据当前激光雷达扫描数据，在地图上推测机器人最可能的位置。
   5. Map Server（地图服务器）：加载并发布静态地图——启动时读取参数指定的YAML文件，将地图加载为nav_msgs/OccupancyGrid格式，并持续在/map话题上发布；提供动态地图服务——
   6. Localization（定位）：定位模块负责回答“机器人在哪”的问题。
   7. Planner Server（规划器服务器）：即“全局规划器“。它的任务是根据当前地图和机器人位置，计算出一条从起点到目标点的全局最优路径。
   8. Controller Server（控制器服务器）：即”局部控制器“。它负责执行规划器生成的全局路径，将路径转换成具体的速度指令发送给电机。它主要关注机器人周围局部的动态环境，进行实时避障。
   9. Recovery Server（恢复服务器）/ Behavior_server（行为服务器，恢复服务器的升级版本）：处理卡住/异常情况（后退、旋转、重新规划）。
   10. Behavior Tree Navigator（行为树导航器）：是Nav2的决策和调度核心。它使用行为树(BT)来定义和组织复杂的导航行为。
   11. Costmap（代价地图）：是机器人用来表示环境”通行代价“的2D网格图。
      - Global Costmap（全局代价地图）：基于整个静态地图构建，范围大、更新慢。Planner Server使用它来规划全局路径。
      - Local Costmap（局部代价地图）：只关注机器人周围的动态环境，是一个跟随机器人移动的小窗口，更新频率高。Controller Server使用它来进行实时避障和生成局部轨迹。
   11. Layer（层）：就像是在一张代价地图上叠加的不同透明胶片，每张胶片负责提供一种环境信息，叠加在一起，就形成了机器人用于导航的完整“世界视图”。
      - Inflation Layer（膨胀层）：在障碍物周围根据距离计算出一个”危险梯度“。越靠近障碍物，代价值越高，以此确保规划出的路径会与障碍物保持安全距离。global_costmap和local_costmap都必须包含。
      - Static Layer（静态层）：来自预先构建好的地图（如SLAM建图结果），它描绘了墙壁等固定不变的障碍物。主要用于global_costmap。
      - Obstacle Layer（障碍层）：实时处理传感器数据（如激光雷达），将检测到的障碍物标记在代价地图上。global_costmap和local_costmap都会用到。
      - Voxel Layer（体素层）：Obstacle Layer的3D升级版，处理点云等3D数据，能更精确地处理高于或低于机器人的障碍物。常用于local_costmap实现更精确的实时避障。
   12. 如何检查导航链路是否接通：ros2 topic echo /plan（查看是否生成路径）, ros2 topic echo /cmd_vel（查看是否发出速度指令）, ros2 node list（查看各节点）, ros2 node info（查看各节点状态）。
   13. 参数调整的基本原则：
       - 路径规划：tolerance（容差）调大，路径更直；调小，路径更贴墙。
       - 避障：inflation_radius（膨胀半径）调大，机器人更怕障碍物；调小，更“勇敢”。
       - 速度：max_vel_x, max_vel_theta限制机器人的最大运动能力。
       - 目标检查：xy_goal_tolerance, yaw_goal_tolerance控制机器人认为“到达目标”的误差范围。
   14. 一些参数：
      - trace_unknown_space：是Nav2全局代价地图中的一个关键的布尔参数，决定了未被传感器探测到的区域（未知空间）是否会被纳入路径规划的考量。推荐填true，即将未知区域视为可通行但有风险的空间，路径规划会尽量避免穿越，但如果别无选择，也可以从中穿行，这能帮你在探索建图时找到通往未知区域的路。
      - controller_plugin: "dwb_core::DWBLocalPlanner"：DWBLocalPlanner的核心任务就是解决实时避障与跟踪全局路径的问题。它根据机器人当前的速度和加减速能力，生成大量可能的运动方案，然后对每个方案进行短时间内的运动模拟，再用一套打分系统(Critic Plugins)对模拟结果打分，最后选出得分最高的方案，转换成速度指令发布出去。当你不确定用哪个局部规划器时，用它通常是最稳妥的选择。
  
    
       
- Lifecycle Node（生命周期节点）：生命周期节点把启动分解成了“先配置，再激活”的严谨步骤。它能确保所有硬件和依赖准备就绪后，再让机器人动起来；当某个节点出问题时，也能有序地让整个系统安全关闭，避免失控。
  1. Primary States（主状态）
     - unconfigured（未配置）：节点刚被实例化（创建）出来时的初始状态。
     - inactive（未激活）：节点已配置好，但不处理任何数据。
     - active（激活）：节点的正常工作状态。
     - finalized（已终结）：节点的最终状态，生命周期的终点。
  2. Transition States（过渡状态）：在主状态之间切换，必须经过短暂的过渡状态。
     - configure：从unconfigured到inactive。
     - activate：从inactive到active。
     - deactivate：从active到inactive。
     - cleanup：从inactive到unconfigured。
     - shutdown：从unconfigured/inactive/active到finalized。
  3. 通过ros2 lifecycle get /节点名 来查看其当前状态
**Challenges & blockers**
- 如何正确Debug？（调了两天Nav2配置失败教训）
  1. 从“改到对”变成“找到错”：先假设，再验证。面对任何报错，先不要动手改代码，而是先想或写出一个明确的猜测，然后去执行命令验证这个猜测，猜错了就换下一个猜测，直到猜中为止。
  2. 二分法，把整个可能都有错误的系统拆成两个部分，例如Nav2启动不了可以先不管导航，只测底层硬件和TF，这样能先把一半的错误可能排除掉。
  3. 利用rosbag，先把雷达数据、里程计数据、TF数据等录制下来(ros2 bag record -a)，后续可以用ros2 bag play重播数据，只启动算法节点。
  4. 每次改完参数都记下来改了什么，现象是什么。

**Next steps**
- _What will you do next week?_

**Hours spent (optional):** _e.g. 6h_

**Links (optional):** _commits, notebooks, docs, datasets..._

### Week 4 — 2026-06-29

**Attended this week's meeting:** Yes 

**Progress this week**
- sensor_msgs/Laserscan（激光雷达数据）：激光雷达是机器人的“眼睛”，它发射激光并测量反射回来的时间，从而知道“每个方向上最近的障碍物有多远”。LaserScan就是这个测量结果在ROS2中的标准消息格式，它描述了：在某个时刻，激光雷达扫描了一圈，得到了哪些距离值。
  1. 消息结构
```
sensor_msgs/LaserScan:
  header:                # 时间戳 + frame_id
    stamp: 时刻
    frame_id:    # 告诉系统：这些距离是在 ... 坐标系里测的
  angle_min:       # 起始角度（弧度）
  angle_max:        # 结束角度
  angle_increment:  # 每两个测量点之间的角度增量
  time_increment:    # 每个点之间的时间增量
  scan_time:         # 完整扫描一圈所需的时间（秒）
  range_min:         # 最小可测距离（米）
  range_max:        # 最大可测距离（米）
  ranges: [...]   # 距离值数组（单位：米）
  intensities: [...]     # 反射强度（可选）
```
  2. 作用：
     - 建图：SLAM节点订阅/scan，把障碍物位置累计成地图；
     - 定位(AMCL)：把当前扫描和地图匹配，推断机器人位置；
     - 避障(Local Costmap)：实时检测障碍物，动态更新代价地图。
  3. 错误排查：检查ros2 topic echo /scan --once看是否有数据。
- sensor_msgs/Imu：即Inertial Measurement Unit（惯性测量单元），是机器人的“内耳”和“陀螺仪”，像一个“迷你GPS”，但它不依赖外部信号，完全靠自己感知运动。
  1. 消息结构
```
sensor_msgs/Imu:
  header:                # 时间戳 + frame_id（通常是 "imu_link"）
  orientation:           # 四元数（x, y, z, w），描述机器人相对世界坐标系的朝向
  orientation_covariance: [0.01, 0, 0, 0, 0.01, 0, 0, 0, 0.01]  # 3x3 协方差矩阵
  angular_velocity:      # 角速度（rad/s），绕 X、Y、Z 轴
  angular_velocity_covariance: ...
  linear_acceleration:   # 线性加速度（m/s²），沿 X、Y、Z 轴
  linear_acceleration_covariance: ...  
```
     协方差：数值越大表示噪声越大，融合算法会“降低信任度”。
  2. 噪声(Noise)：是传感器测量结果中，你不需要的、随机的、无规律的微小波动或误差。
- timestamp与frame_id
  1. frame_id相当于“身份证”，即数据是在哪个坐标系中的。
  2. timestamp相当于“出生证”，即数据是什么时候的。
  3. 验证frame_id：ros2 topic echo /scan --once | grep frame_id
  4. 验证timestamp与仿真时间的同步：确认在启动所有节点时，都加上了use_sim_time:=true。
- 使用rosbag复现实验（数据记录与回放）
  1. 录制数据：ros2 bag record -o carter_run /scan /odom /imu /tf /tf_static /clock，这会记录激光雷达、里程计、IMU、TF变换和仿真时间，数据包会保存在当前目录下。
  2. 回放数据：ros2 bag play carter_run --clock：这会把录制的数据原样重新发布出来，--clock会让回放器发布仿真时间，确保时间戳对齐。
- 扫描匹配(Scan Matching)：机器人不知道自己的精确位置，当它测到了一帧激光数据后，扫描匹配要做的，就是疯狂地平移、旋转这帧激光数据，让它和上一帧（或已有地图）完美贴合。
- 图优化(Graph Optimization)：本质上是一个“后端优化(Back-end)”算法。
  1. 图由什么组成：
     - 顶点(Nodes/Vertices)：代表待估计的变量，通常是机器人在不同时刻的位姿（位置+朝向），或者是地图中某个路标点(Landmark)的坐标。
     - 边(Edges/Constraints)：代表顶点之间的约束关系。它是一种“测量值“，比如里程计告诉你”从A点走了1米到B点“。
  2. 工作原理（两步走）：
     - 构建误差函数（找矛盾）：每条”边“都有一个对应的残差(Residual)，即“实际测量值”和“根据当前顶点估计值反算出的预测值”之间的差异。
     - 最小化全局误差（调矛盾）：找到一个最优的顶点配置（即所有历史位姿和路标点的最佳估计值），使得所有边的残差平方和最小。
  3. 使用场景：
     - 当机器人第一次探索环境并绘制地图时，所有传感器数据都是带噪声的，图优化负责“事后算总账”，可以消除累计误差，闭合环路。
     - 静态地图生成，离线优化，输出高精度/map。
     - 在线导航阶段是“拿着地图找路”，基本不用图优化。
- 局部里程计(Local Odometry)
  1. 它在干什么：只关心从上一秒的位置，相对移动了多少。
  2. 输入：它依赖的是机器人自身的“本体感觉”，轮子转了几圈（编码器）、身体倾斜的角度（IMU）、或者眼睛看地面纹理的移动（视觉里程计）。
  3. 输出：它发布/odom话题和odom到base_link的TF变换。
  4. 特性：
     - 连续、平滑：它每秒钟更新几十次，指令非常顺滑，适合用来做实时控制（比如转弯、避障）。
     - 短期可信：在1秒钟内，它告诉你“往前走了0.5米”，这个数据是极准的。
     - 长期会漂（累计误差）。
- 全局定位(Global Localization)
  1. 它在干什么：只关心现在在哪个地图上的哪个位置。
  2. 输入：它依赖的是绝对参照物，提前建好的地图、当前看到的墙壁形状（激光雷达scan）、以及一个大概的初始位置。
  3. 输出：它发布map到odom的TF变换，或者直接给出机器人在map坐标系下的绝对坐标（x, y, 0）。
  4. 特性：
     - 能修正漂移：会强行把机器人“拉”回正确位置。
     - 可能跳变或丢失
- Path（路径）：是一个按时间或空间顺序排列的坐标点(Pose)列表。
  1. 定义：消息类型为nav_msgs/msg/Path。结构包括Header（头：记录这条路径属于哪个坐标系和时间戳）和Poses（位姿数组：一个PoseStamped的列表，每个点都包含三维位置和朝向）。
  2. 如何实现（两阶段生成）：
     - 第一步：全局路径(Global Path)，由Planner Server生成。原理：将Global Costmap网格化，算法把机器人当成一个点，在地图上从起点“探路”到终点，寻找代价值总和最低的格子序列。产出：一条从起点到终点的粗略折线，通常由几百个离散点组成。
     - 第二部：局部路径(Local Path/Trajectory)，由Controller Server生成。原理：截取全局路径的一段，结合Local Costmap里的动态障碍物，对路径进行“光滑”和“扭曲”处理，同时生成一条未来几秒内带有速度和时间信息的轨迹(Trajectory)。产出：一条平滑的、符合机器人运动学约束（如最小转弯半径）的行驶曲线，最终被拆解成/cmd_vel速度指令发给底盘。
     - Trajectory（轨迹）：可以理解为“Path + 时间表”。它不仅仅是告诉机器人“走哪条路”，更明确规定了在什么时间点、以什么速度、加速度到达哪个位置。Controller Server负责生成Trajectory，它使用局部轨迹规划期的算法，将Path转换为Trajectory。
  
- nota_carter_mini机器人计算图解析
  1. Nav2的核心行为动作服务器(Action Server)，均包含子话题/status（服务器是否繁忙/成功）和/feedback（当前进度，比如走了多远）
      - /navigate_to_pose（单点导航）：用于接收“把机器人从A点开到B点”的单一目标指令。
      - /navigate_through_poses（途径点导航）：用于接收一串路径点，机器人必须按顺序经过这些点，最后到达终点，常用于狭窄通道或需要特定姿态通过的场景。
      - /follow_waypoints（路径跟踪）：用于执行更底层的路径追踪任务，常用于沿着一整条预先计算好的密集路径点行走。
  2. /rviz_navigation_dialog_action_client：作为“传话筒”，把Action Server接收到的指令转交给
  3. /clock：专门发布时间戳的全局标准话题（消息类型是rosgraph_msgs/msg/Clock，内容只有一个时间字段），强制让所有ROS节点使用仿真时间。
  4. /bond：用于实现节点间“心跳保活”机制的通信话题。例如节点A与/bond连接后，通过/bond定时发送“心跳包”，只要心跳正常，就证明对方运行良好。
  5. /local_costmap：
     - /local_costmap/costmap_raw（原始局部代价地图）：数据更“原汁原味”，适合需要精确代价值的算法模块。
     - /local_costmap/published_footprint（已发布足迹）：它定义了机器人在水平面上的真实“占地面积”（轮廓多边形）。这个信息会与代价地图结合，用来精确检查机器人是否与障碍物发生碰撞。
  6. /initialpose：是RViz工具栏中的一个工具（就是那个“2D Pose Estimate”按钮），当你在地图上点击一个位置并拖出方向时，RViz会发布一条消息到/initialpose话题，内容是geometry_msgs/PoseWithCovarianceStamped（包含坐标和朝向）。
  7. /tf：话题，是所有动态变换（如odom到base_link）的传输通道；/tf_static：话题，专门承载永远不变的静态变换（如base_link到laser_link），静态变换只会被发布一次，后续节点从缓存中读取，不需要重复发送，可以节省带宽。
  8. /robot_state_publisher（机器人状态发布者）：节点，加载URDF模型，将/joint_states话题的关节数据转换为所有活动关节的坐标变换，发布到/tf话题。
- Pipeline（管道）：数据流处理，每个链都负责将上游的原始数据，经过特定算法加工后，输出给下游模块使用。
  1. 建图链 (Mapping Chain) —— “记忆系统”
   - 输入：/scan（激光雷达）、/camera/depth/points（深度点云）、/odom（里程计）。
   - 核心算法：SLAM（如 Cartographer 或 GMapping）。前端负责帧间匹配，后端（图优化）负责消除累积误差和闭环检测。
   - 输出：静态的 /map（占据栅格地图）。
   - 作用：这是所有导航的前置基础，为机器人提供环境的“先验知识”。
  2. 定位链 (Localization Chain) —— “坐标意识”
   - 输入：静态 /map + 实时 /scan + /odom。
   - 核心算法：AMCL（自适应蒙特卡洛定位） 或 卡尔曼滤波。它通过粒子滤波，将实时传感器数据与静态地图进行匹配。
   - 输出：/amcl_pose 和 TF 坐标变换（map → odom → base_link）。
   - 作用：实时回答“我在全局地图中的精确位置是哪里”。
  3. Planner 链 (全局规划链) —— “战略家”
   - 输入：定位结果（当前位置）+ 用户给的 Goal（目标点）+ Global Costmap（全局代价地图）。
   - 核心算法：A* 或 Dijkstra（图搜索算法）。
   - 输出：/plan（一条从起点到终点的粗略坐标点数组，即 Path）。
   - 作用：在静态地图上规划出一条“最优路线”（避开固定墙壁）。
  4. Controller 链 (局部控制链) —— “战术家/驾驶员”
   - 输入：Planner 输出的 /plan（全局路线）+ Local Costmap（局部代价地图，含动态障碍物）+ /odom。
   - 核心算法：TEB（时间弹性带） 或 MPPI（模型预测路径积分）。它会考虑机器人的物理限制（最大转弯半径、加减速能力）。
   - 输出：/cmd_vel（线速度和角速度指令，即 Twist 消息）。
   - 作用：将全局粗路径“平滑化”，并生成带时间/速度信息的轨迹（Trajectory），实时躲避突然出现的人。
  5. BT 编排链 (行为树编排链) —— “总指挥官”
   - 这是 Nav2 区别于传统架构的最大亮点，它负责逻辑的切换与容错。
   - 核心组件：/bt_navigator（行为树导航器）。它通过读取 .xml 文件来组织任务逻辑。
   - 典型节点：Sequence（顺序执行）、Fallback（备选/重试）、RecoveryNode（恢复节点）。
   - 作用：当 Controller 发现“卡住了”，BT 链会触发“恢复链”（如下发后退指令、原地旋转重新规划）。它是一个非线性的决策大脑，决定是先规划再走，还是走不动了就“倒车”。
   - /navigate_to_pose（处理单个目标点）和/navigate_through_poses（处理一连串必须经过的关键点）是bt_navigator这个节点对外提供的标准动作接口，相当于BT编排链的入口。
  6. /waypoint_follower和/follow_waypoints
   - 它们不属于Nav2的底层核心BT链，而是一个独立的“应用层”节点。它坐在BT链的上方，负责执行“巡逻”或“多点送货”这类高级任务。
   - 工作机制：/waypoint_follower节点收到/follow_waypoints传过来的一大串点后，它不会自己去规划路径。它会内部拆解任务，循环调用底层BT链的/navigate_to_pose接口，一个一个地把这些点“喂”给BT导航器去执行。
  7. Lifecycle & Bond Chain（状态监控链）：负责节点间的“心跳(Bond)”监测。如果Planner或Controller节点崩溃，它会自动将整个系统降级为未激活(Inactive)状态，防止机器人失控。
     
### Week 5 — 2026-07-06

**Attended this week's meeting:** Yes

**Progress this week**
- Gazebo仿真器学习
  1. ros_gz_bridge：是一个网络桥接包，它的核心作用是在ROS2和Gazebo（具体来说是Gazebo Transport）之间搭建一个双向翻译通道。
     - parameter_bridge工具：使用ros2 run ros_gz_bridge parameter_bridge命令来运行，并指定要连接的话题和消息类型。方向符号中“@”表示双向，“[”表示Gazebo到ROS，“]”表示ROS到Gazebo。
  2. 一旦桥接好了后，有两种选择来通过命令运行仿真机器人：
     - ros2 topic pub /model/vehicle_blue/cmd_vel geometry_msgs/Twist "linear: { x: 0.1 }"。
     - 使用teleop_twist_keyboard包，通过键盘按键来运行：ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r /cmd_vel:=/model/vehicle_blue/cmd_vel。
  3. Meshes（网格）：指的就是机器人的3D外观模型文件，它描述了机器人外壳的每一个曲面和细节。为了装上Meshes，我们需要在package.xml中加入<gazebo_ros gazebo_model_path="${prefix}/.."/>。
     - Gazebo本身不理解ROS的package://路径，所以找不到文件，需要使用${prefix}/..
     - ${prefix}：不是文件夹的名字，而是一个CMake变量。它会在编译/安装时，被自动替换成这个包的安装目录。例如你的工作空间叫ros_ws，包叫my_robot_description，当你执行colcon build后，这个包会被安装到/home/zhengyu/ros2_ws/install/my_robot_description，这个路径就是${prefix}的值。
     - /..：代表上一级目录，所以${prefix}/..的意思就是从我这个包的安装目录，往上一级退。
  4. Gazebo GUI（图形用户界面）：在运行Gazebo仿真时看到的那个3D窗口，是仿真世界的可视化客户端，显示仿真引擎内部的物理世界（物体、重力、碰撞）。
  5. URDF只是“骨架”和“外观”。要让机器人在Gazebo里能动、能感知，你必须往URDF里注入Gazebo插件(Plugins)和ROS2控制器(Controllers)。
```
<ros2_control name="GazeboSystem" type="system">
  <hardware>
    <plugin>gazebo_ros2_control/GazeboSystem</plugin>
  </hardware>
  <joint name="head_swivel" />
</ros2_control>

<gazebo>
  <plugin filename="libgazebo_ros2_control.so" name="gazebo_ros2_control">
    <parameters>$(find urdf_sim_tutorial)/config/09a-minimal.yaml</parameters>
  </plugin>
</gazebo>
```
<ros2_control>是ROS2控制框架的标准标签，用来声明“这个机器人有一个控制系统”。<hardware>：指底层硬件接口。<plugin>gazebo_ros2_control/GazeboSystem</plugin>：加载Gazebo专用的硬件插件，它会把Gazebo物理引擎“伪装”成ROS2控制框架里的硬件。<joint name="head_swivel" />：至少指定一个关节（这里是头部旋转关节），否则控制器无法初始化，后面可以添加更多关节。<gazebo>：这是一个URDF扩展标签，专门用于Gazebo仿真器的配置。
  6. 之前完成了ROS2和Gazebo之间的连接通道，现在开始往这个通道里“安装具体的控制器”，让Gazebo里的机器人真正开始向外汇报信息
    - controller_manager：是ROS2 ros2_controll框架中的一个核心节点，起到“总控制台”的作用，负责管理和协调所有控制器，你可以通过它加载(load)、启动(start)、停止(stop)或卸载(unload)不同类型的控制器，它本身不控制机器人，而是管理控制器的“管家”。
```
controller_manager:
  ros__parameters:
    update_rate: 100
    use_sim_time: true
```
让controller_manager以100Hz的频率检查所有控制器的状态，并且所有控制指令都基于仿真时钟运行。
    - 第一个控制器：
```
joint_state_broadcaster:
      type: joint_state_broadcaster/JointStateBroadcaster
```
这个控制器用于读取Gazebo物理引擎中所有关节的当前状态（位置、速度、力矩），然后封装成sensors_msgs/JointState消息，发布到/joint_states话题上。它不会控制任何关节，只是听取Gazebo物理引擎的数据，并转述给ROS2。这个控制器由joint_state_broadcaster包提供，是ROS2官方维护的标准控制器之一。
    - joint_state_broadcaster虽然启动了，但它不知道要读取哪些关节，所以需要你在URDF或配置文件中显示列出哪些关节需要被监控。
  7. 在URDF中声明“接口”来让数据真正流动起来
```
<joint name="head_swivel">
  <command_interface name="position" />
  <command_interface name="velocity" />
  <state_interface name="position"/>
  <state_interface name="velocity"/>
</joint>
```
<command_interface name="position" />：声明这个关节可以被命令移动到某个精确角度（例如“把头转到 90 度”）。<command_interface name="velocity" />：声明这个关节可以被命令以某个速度旋转（例如“以 0.5 rad/s 的速度转头”）。<state_interface name="position" />：声明这个关节会报告它的当前位置（例如“我现在在 0.2 rad 位置”）。<state_interface name="velocity" />：声明这个关节会报告它的当前速度（例如“我现在以 0.01 rad/s 的速度转动”）。

**Challenges & blockers**
- _What got in the way? What are you stuck on?_

**Next steps**
- _What will you do next week?_

**Hours spent (optional):** _e.g. 6h_

**Links (optional):** _commits, notebooks, docs, datasets..._  

**Challenges & blockers**
- _What got in the way? What are you stuck on?_

**Next steps**
- _What will you do next week?_

**Hours spent (optional):** _e.g. 6h_

**Links (optional):** _commits, notebooks, docs, datasets..._

