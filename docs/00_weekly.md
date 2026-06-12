
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
- _..._

**Next steps**
- _..._

**Hours spent (optional):**

**Links (optional):**
