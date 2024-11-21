# RobotV_Power-inspection
Electric power inspection robot

roscore

cd /home/asir4060/ORBSLAM2/Examples/ROS/ORB_SLAM2
rosrun ORB_SLAM2 RGBD ../../../Vocabulary/ORBvoc.txt TUM1.yaml 

cd /home/asir4060/datasets
rosbag play rgbd_dataset_freiburg1_xyz.bag


roscore 
rosrun rqt_tf_tree rqt_tf_tree

检查 scan话题 base_link to odom   laser to base_link
rosrun gmapping slam_gmapping
rviz
添加激光和map
rosrun teleop_twist_keyboard teleop_twist_keyboard.py


保存地图
rosrun map_server map_saver -f map    （-f后自定义名字）
指定订阅话题：
rosrun gmapping slam_gmapping scan:=base_scan
更改参数
rosparam set /slam_gmapping/map_update_interval 5.0 （float，默认值：5.0）
在更新地图之间的时间长度（以秒为单位）。降低该数量更频繁地更新占用网格，代价是更大的计算负荷。
rosparam set /slam_gmapping/minimumScore 0（float，default：0.0）
最小匹配得分，这个参数很重要，它决定了对激光的一个置信度，越高说明对激光匹配算法的要求越高，激光的匹配也越容易失败而转去使用里程计数据，而设的太低又会使地图中出现大量噪声，所以需要权衡调整。
rosparam set /slam_gmapping/linearUpdate 0.000000000001（float，default：1.0）每次机器人移动这么远时，处理一次扫描。
rosparam set /slam_gmapping/angularUpdate 0.000000000000001（float，default：1.0）每次机器人旋转这么远时处理一次扫描。
rosparam set /slam_gmapping/particles 30 (int, default: 30)
算法中的粒子数，因为gmapping使用的是粒子滤波算法，粒子在不断地迭代更新，所以选取一个合适的粒子数可以让算法在保证比较准确的同时有较高的速度。


navigation 

rosrun map_server map_server map.yaml

 roslaunch urdf_tutorial display.launch model:='/home/dsc/ur5_description/ur5_rviz.urdf‘
 
在进行导航的时候需要显示机器人模型。本文使用的是isaac sim   extention  usd to urdf exporter 
导出机器人描述语言以及相关mesh  
需注意的是 本文使用的urdf 需要注意的点
1. mesh文件应当和urdf一个等级目录下而不是urdf的子目录。
2. mesh文件 正常导入需要三个重要的点 1.设置set up 。bash环境变量 2.提高文件查询的管理员等级 保证mesh文件可被正常查询到 具体代码可由AI生成 3.规范urdf的读取目录格式 。在isaac sim 的extension中直接生成的读取目录格式是不能使用的。找到对应的读取obj代码行，更改为：<mesh filename="package://webots_ros2_ur_demo/meshes/visual/base.dae"/>
具体解释为urdf 文件中的 mesh 导入，应该使用如下格式：    
<mesh filename="package://webots_ros2_ur_demo/meshes/visual/base.dae"/>
而不是诸如<mesh filename="file://$(find ur5_pick_and_place)/meshes/visual/base.dae"/>之类的格式。
3.TF树需要正常发布，关于小车的各个关节应有对应的代码发布块。
4. 直接导入的小车模型依旧存在两个问题  1.轮子并不是竖直，其旋转方向错误，2.轮子运动过程中车身并未一起运动。

 

