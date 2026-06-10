
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



**Challenges & blockers**
- _..._

**Next steps**
- _..._

**Hours spent (optional):**

**Links (optional):**
