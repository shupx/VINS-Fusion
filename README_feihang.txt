# 仿真环境
roslaunch px4 indoor1.launch

# VINS-Fusion
cd ~/FHB_szx_ws/
bash scripts/xtdrone_run_vio.sh

# 坐标转换
cd ~/FHB_szx_ws/src/
python vins_transfer.py iris 0
# 会出现
# INFO  [ecl/EKF] 1213644000: reset position to ev position
# INFO  [ecl/EKF] 1213644000: commencing external vision position fusion
# INFO  [ecl/EKF] 1213644000: commencing external vision yaw fusion

# 建立通信
cd ~/FHB_szx_ws/src/communication
python multirotor_communication.py iris 0

# 键盘控制
cd ~/FHB_szx_ws/src/control/keyboard
python multirotor_keyboard_control.py iris 1 vel
# 推荐起飞流程，按i把向上速度加到0.3以上，再按b切offboard模式，最后按t解锁。

# 官方启动
roslaunch ue4_ros_drivers multi_test.launch robot:=drone1
roslaunch ue4_ros_drivers multi_test.launch robot:=drone2

# 比赛启动
rosrun vins vins_node ~/vins_ws/src/VINS-Fusion/config/FHB/FHB_drone1.yaml
rosrun vins vins_node ~/vins_ws/src/VINS-Fusion/config/FHB/FHB_drone1.yaml __ns:=/drone1
roslaunch vins vins_rviz.launch
roslaunch vins FHB_run_vio.launch

# 闭环优化
rosrun loop_fusion loop_fusion_node ~/vins_ws/src/VINS-Fusion/config/FHB/FHB_drone1.yaml

# 修改配置文件
gedit ~/FHB_szx_ws/src/VINS-Fusion/config/xtdrone_sitl/FHB_drone1.yaml

# 坐标系转换


# A_LOAM
roslaunch aloam_velodyne aloam_velodyne_VLP_16.launch

# 数据回放
rosbag play ~/FHB_ws/data/2022-06-28-23-11-49.bag
rosbag play ~/FHB_ws/data/2022-06-27-20-31-18.bag
rosbag play ~/FHB_ws/data/2022-07-22-23-16-00.bag
