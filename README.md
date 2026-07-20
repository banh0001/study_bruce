<img src="https://github.com/AtsushiSakai/PythonRobotics/raw/master/icon.png?raw=true" align="right" width="200" alt="header pic"/>

# study_bruce — คลังความรู้อัลกอริทึมหุ่นยนต์

รวม reference สำหรับทำความเข้าใจอัลกอริทึมที่ใช้จริงใน **ROS2 Nav2** และ **slam_toolbox** บนหุ่นยนต์ luna_mecanum

> แต่ละ script รันได้ด้วย Python เดี่ยว ไม่ต้องใช้ ROS2 — ดู animation เพื่อเข้าใจ concept ก่อนไปแตะ config จริง
>
> ขอบคุณ [AtsushiSakai/PythonRobotics](https://github.com/AtsushiSakai/PythonRobotics) ที่เป็นต้นฉบับ

---

## เริ่มต้นใช้งาน

```bash
git clone --recurse-submodules https://github.com/banh0001/study_bruce.git
cd study_bruce
python3 -m venv .venv
source .venv/bin/activate
pip install -r PythonRobotics/requirements/requirements.txt
```

แล้วรัน script ที่ต้องการ:
```bash
cd PythonRobotics
python PathPlanning/AStar/a_star.py
```

---

## สารบัญ

- [การระบุตำแหน่ง (Localization)](#การระบุตำแหน่ง-localization)
- [การสร้างแผนที่ (Mapping)](#การสร้างแผนที่-mapping)
- [SLAM](#slam)
- [การวางแผนเส้นทาง (Path Planning)](#การวางแผนเส้นทาง-path-planning)
- [การติดตามเส้นทาง (Path Tracking)](#การติดตามเส้นทาง-path-tracking)
- [แขนกล (Arm Navigation)](#แขนกล-arm-navigation)
- [โดรน (Aerial Navigation)](#โดรน-aerial-navigation)
- [หุ่นยนต์สองขา (Bipedal)](#หุ่นยนต์สองขา-bipedal)

> 🤖 = ใช้จริงใน Nav2 / slam_toolbox บน luna_mecanum

---

## การระบุตำแหน่ง (Localization)

### Extended Kalman Filter (EKF) 🤖
> ใช้ใน `robot_localization` package — รวม odom + IMU เข้าด้วยกัน

![EKF](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Localization/extended_kalman_filter/animation.gif)

```bash
python Localization/extended_kalman_filter/extended_kalman_filter.py
```

### Particle Filter 🤖
> หลักการของ **AMCL** ใน `nav2_localization.launch.py` — particle กระจายแล้ว converge เมื่อ robot เคลื่อนที่

![Particle Filter](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Localization/particle_filter/animation.gif)

```bash
python Localization/particle_filter/particle_filter.py
```

### Histogram Filter
> concept พื้นฐานที่สุดของ localization บน grid map

![Histogram Filter](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Localization/histogram_filter/animation.gif)

```bash
python Localization/histogram_filter/histogram_filter.py
```

---

## การสร้างแผนที่ (Mapping)

### Gaussian Grid Map
> แผนที่แบบ probabilistic — แต่ละ cell มีค่าความน่าจะเป็นว่ามีสิ่งกีดขวาง

![Gaussian Grid Map](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/gaussian_grid_map/animation.gif)

```bash
python Mapping/gaussian_grid_map/gaussian_grid_map.py
```

### Ray Casting Grid Map 🤖
> วิธีที่ slam_toolbox ใช้ update occupancy grid — ยิง ray จาก LiDAR แล้วอัปเดต cell

![Ray Casting](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/raycasting_grid_map/animation.gif)

```bash
python Mapping/ray_casting_grid_map/ray_casting_grid_map.py
```

### Lidar to Grid Map 🤖
> แปลง LiDAR scan → occupancy grid map เหมือนที่ slam_toolbox สร้าง

![Lidar to Grid](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/lidar_to_grid_map/animation.gif)

```bash
python Mapping/lidar_to_grid_map/lidar_to_grid_map.py
```

### K-Means Clustering
> จัดกลุ่ม point cloud จาก LiDAR

![K-Means](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/kmeans_clustering/animation.gif)

```bash
python Mapping/kmeans_clustering/kmeans_clustering.py
```

### Rectangle Fitting
> วิเคราะห์รูปร่างสิ่งกีดขวางจาก LiDAR

![Rectangle Fitting](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/rectangle_fitting/animation.gif)

```bash
python Mapping/rectangle_fitting/rectangle_fitting.py
```

---

## SLAM

### Iterative Closest Point (ICP) 🤖
> scan matching — วิธีเปรียบเทียบ scan ปัจจุบันกับ map เพื่อหาตำแหน่ง

![ICP](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/SLAM/iterative_closest_point/animation.gif)

```bash
python SLAM/ICPMatching/icp_matching.py
```

### FastSLAM 1.0 🤖
> ทำ SLAM และ localization พร้อมกัน — หลักการพื้นฐานของ SLAM

![FastSLAM](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/SLAM/FastSLAM1/animation.gif)

```bash
python SLAM/FastSLAM1/fast_slam1.py
```

---

## การวางแผนเส้นทาง (Path Planning)

### Dynamic Window Approach (DWA) 🤖
> ใช้ใน `controller_server` — `DWBLocalPlanner` ใน `navigation_mecanum.yaml`
> ประเมิน trajectory หลายเส้นแบบ real-time แล้วเลือกเส้นที่ดีที่สุด เหมาะกับ holonomic robot

![DWA](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/DynamicWindowApproach/animation.gif)

```bash
python PathPlanning/DynamicWindowApproach/dynamic_window_approach.py
```

### Dijkstra 🤖
> ใช้ใน `planner_server` — `NavfnPlanner` ใน `navigation.yaml`
> ค้นหาเส้นทางสั้นที่สุดแบบ exhaustive บน grid

![Dijkstra](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/Dijkstra/animation.gif)

```bash
python PathPlanning/Dijkstra/dijkstra.py
```

### A* 🤖
> ใช้ใน `planner_server` — `NavfnPlanner` (use_astar: true) ใน `navigation_mecanum.yaml`
> เร็วกว่า Dijkstra เพราะมี heuristic ช่วยนำทาง

![A*](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/AStar/animation.gif)

```bash
python PathPlanning/AStar/a_star.py
```

### D*
> วางแผนเส้นทางแบบ dynamic — อัปเดตได้เมื่อ map เปลี่ยน

![D*](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/DStar/animation.gif)

```bash
python PathPlanning/DStar/dstar.py
```

### D* Lite
> version ที่เบากว่า D* เหมาะกับ replanning บน robot จริง

![D* Lite](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/DStarLite/animation.gif)

```bash
python PathPlanning/DStarLite/d_star_lite.py
```

### Potential Field
> แรงดึงดูดจาก goal + แรงผลักจาก obstacle — concept พื้นฐาน costmap

![Potential Field](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/PotentialFieldPlanning/animation.gif)

```bash
python PathPlanning/PotentialFieldPlanning/potential_field_planning.py
```

### Grid Based Sweep Coverage Path Planning
> วางแผนเส้นทางกวาดพื้นที่ทั้งหมด — ใช้กับหุ่นยนต์ทำความสะอาด

![Grid Sweep CPP](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/GridBasedSweepCPP/animation.gif)

```bash
python PathPlanning/GridBasedSweepCPP/grid_based_sweep_coverage_path_planner.py
```

### Particle Swarm Optimization (PSO)
> หาเส้นทางด้วยวิธี swarm intelligence

![PSO](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/ParticleSwarmOptimization/animation.gif)

```bash
python PathPlanning/ParticleSwarmOptimization/particle_swarm_optimization.py
```

### State Lattice Planner 🤖
> ใช้ใน `SmacPlannerLattice` — วางแผนบน motion primitive lattice

**Biased Polar Sampling**

![State Lattice Biased](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/StateLatticePlanner/BiasedPolarSampling.gif)

**Lane Sampling**

![State Lattice Lane](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/StateLatticePlanner/LaneSampling.gif)

```bash
python PathPlanning/StateLatticePlanner/state_lattice_planner.py
```

### Probabilistic Road-Map (PRM)
> สร้าง roadmap สุ่มแล้วหาเส้นทางบนนั้น

![PRM](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/ProbabilisticRoadMap/animation.gif)

```bash
python PathPlanning/ProbabilisticRoadMap/probabilistic_road_map.py
```

### RRT*
> Rapidly-Exploring Random Trees แบบ optimal — หาเส้นทางในพื้นที่ต่อเนื่อง

![RRT*](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/RRTstar/animation.gif)

```bash
python PathPlanning/RRTStar/rrt_star.py
```

### RRT* with Reeds-Shepp
> RRT* สำหรับรถที่ถอยหลังได้

![RRT* RS](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/RRTStarReedsShepp/animation.gif)

```bash
python PathPlanning/RRTStarReedsShepp/rrt_star_reeds_shepp.py
```

### LQR-RRT*
> รวม LQR controller เข้ากับ RRT*

![LQR-RRT*](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/LQRRRTStar/animation.gif)

```bash
python PathPlanning/LQRRRTStar/lqr_rrt_star.py
```

### Quintic Polynomials Planner
> สร้าง trajectory เรียบด้วย polynomial ดีกรี 5

![Quintic](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/QuinticPolynomialsPlanner/animation.gif)

```bash
python PathPlanning/QuinticPolynomialsPlanner/quintic_polynomials_planner.py
```

### Reeds-Shepp Path
> เส้นทางสั้นที่สุดสำหรับรถที่เลี้ยวได้และถอยหลังได้

![Reeds-Shepp](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/ReedsSheppPath/animation.gif?raw=true)

```bash
python PathPlanning/ReedsSheppPath/reeds_shepp_path_planning.py
```

### LQR Planner
> วางแผนเส้นทางด้วย Linear Quadratic Regulator

![LQR Planner](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/LQRPlanner/animation.gif?raw=true)

```bash
python PathPlanning/LQRPlanner/lqr_planner.py
```

### Frenet Optimal Trajectory
> วางแผนบน Frenet frame — ใช้มากใน self-driving car

![Frenet](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/FrenetOptimalTrajectory/animation.gif)

```bash
python PathPlanning/FrenetOptimalTrajectory/frenet_optimal_trajectory.py
```

---

## การติดตามเส้นทาง (Path Tracking)

### Move to Pose
> ควบคุม robot ให้เดินไปยังตำแหน่งที่กำหนด

![Move to Pose](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Control/move_to_pose/animation.gif)

```bash
python PathTracking/move_to_pose/move_to_pose.py
```

### Stanley Control
> ใช้มากใน self-driving car — ควบคุม steering ตาม cross-track error

![Stanley](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/stanley_controller/animation.gif)

```bash
python PathTracking/stanley_control/stanley_control.py
```

### Rear Wheel Feedback Control
> ควบคุม robot ผ่าน rear wheel

![Rear Wheel](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/rear_wheel_feedback/animation.gif)

```bash
python PathTracking/rear_wheel_feedback_control/rear_wheel_feedback_control.py
```

### LQR Speed and Steering Control
> ควบคุม speed และ steering พร้อมกันด้วย LQR — optimal control

![LQR Speed Steer](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/lqr_speed_steer_control/animation.gif)

```bash
python PathTracking/lqr_speed_steer_control/lqr_speed_steer_control.py
```

### Model Predictive Control (MPC)
> คำนวณ optimal control sequence ล่วงหน้า — แม่นที่สุดแต่กิน CPU มากที่สุด

![MPC](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/model_predictive_speed_and_steer_control/animation.gif)

```bash
python PathTracking/model_predictive_speed_and_steer_control/model_predictive_speed_and_steer_control.py
```

### Nonlinear MPC (C-GMRES)
> MPC สำหรับระบบ nonlinear — ขั้นสูงที่สุดใน Path Tracking

![NMPC](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/cgmres_nmpc/animation.gif)

```bash
python PathTracking/cgmres_nmpc/cgmres_nmpc.py
```

---

## แขนกล (Arm Navigation)

### N-Joint Arm to Point Control
> ควบคุม robot arm หลายข้อต่อให้ปลายแขนไปยังตำแหน่งที่กำหนด

![N Joint Arm](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/ArmNavigation/n_joint_arm_to_point_control/animation.gif)

```bash
python ArmNavigation/n_joint_arm_to_point_control/n_joint_arm_to_point_control.py
```

### Arm Obstacle Avoidance
> robot arm หลีกเลี่ยงสิ่งกีดขวาง

![Arm Obstacle](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/ArmNavigation/arm_obstacle_navigation/animation.gif)

```bash
python ArmNavigation/arm_obstacle_navigation/arm_obstacle_navigation.py
```

---

## โดรน (Aerial Navigation)

### Drone 3D Trajectory Following
> ควบคุม quadrotor ให้บินตาม 3D trajectory

![Drone 3D](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/AerialNavigation/drone_3d_trajectory_following/animation.gif)

```bash
python AerialNavigation/drone_3d_trajectory_following/drone_3d_trajectory_following.py
```

### Rocket Powered Landing
> ควบคุมจรวดให้ลงจอดตรงจุด

![Rocket](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/AerialNavigation/rocket_powered_landing/animation.gif)

```bash
python AerialNavigation/rocket_powered_landing/rocket_powered_landing.py
```

---

## หุ่นยนต์สองขา (Bipedal)

### Bipedal Planner with Inverted Pendulum
> วางแผนการเดินของหุ่นยนต์สองขาด้วย inverted pendulum model

![Bipedal](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Bipedal/bipedal_planner/animation.gif)

```bash
python Bipedal/bipedal_planner/bipedal_planner.py
```

---

## Launch files ที่เกี่ยวข้องใน luna_mecanum

```
luna_mecanum_navigation/launch/
├── slam.launch.py                  → FastSLAM / Graph-Based SLAM (async)
├── slam_online_sync.launch.py      → Graph-Based SLAM (sync)
├── slam_offline.launch.py          → SLAM จาก rosbag
├── slam_localization.launch.py     → Localization ด้วย slam_toolbox
├── nav2_localization.launch.py     → Localization ด้วย AMCL (Particle Filter)
├── nav2_navigation_only.launch.py  → Nav2 stack เดี่ยว
└── nav2_slam_with_saver.launch.py  → SLAM + map_saver
```

---

## อ้างอิง

- [AtsushiSakai/PythonRobotics](https://github.com/AtsushiSakai/PythonRobotics)
- [Nav2 Documentation](https://docs.nav2.org)
- [slam_toolbox](https://github.com/SteveMacenski/slam_toolbox)
