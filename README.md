# study_bruce — คลังความรู้อัลกอริทึมหุ่นยนต์

รวม reference สำหรับทำความเข้าใจอัลกอริทึมที่ใช้จริงในระบบนำทางหุ่นยนต์ luna_mecanum (ROS2 Nav2 + slam_toolbox)

> แต่ละ script รันได้ด้วย Python เดี่ยว ไม่ต้องใช้ ROS2 — ดู animation เพื่อเข้าใจ concept ก่อนไปแตะ config จริง

---

## เริ่มต้นใช้งาน

```bash
# clone พร้อม submodule
git clone --recurse-submodules https://github.com/banh0001/study_bruce.git
cd study_bruce

# สร้าง virtual environment
python3 -m venv .venv
source .venv/bin/activate
pip install -r PythonRobotics/requirements/requirements.txt
```

---

## แผนที่อัลกอริทึม → Nav2

### การวางแผนเส้นทางหลัก (Global Path Planning)
> ใช้ใน `planner_server` ของ Nav2

| อัลกอริทึม | script | Nav2 Plugin | หมายเหตุ |
|---|---|---|---|
| A\* | `PathPlanning/AStar/a_star.py` | `NavfnPlanner` | navigation_mecanum.yaml ใช้อันนี้ |
| Dijkstra | `PathPlanning/Dijkstra/dijkstra.py` | `NavfnPlanner` | navigation.yaml ใช้อันนี้ |
| Hybrid A\* | `PathPlanning/HybridAStar/hybrid_a_star.py` | `SmacPlannerHybrid` | เหมาะรถมี kinematic constraint |
| Theta\* | `PathPlanning/ThetaStar/theta_star.py` | `SmacPlannerLattice` | เส้นทางเฉียงได้ |
| RRT | `PathPlanning/RRT/rrt.py` | — | พื้นฐาน sampling-based |
| RRT\* | `PathPlanning/RRTStar/rrt_star.py` | — | version ที่ optimal กว่า |
| Potential Field | `PathPlanning/PotentialFieldPlanning/potential_field_planning.py` | — | concept obstacle avoidance |

```bash
source .venv/bin/activate
cd PythonRobotics

# เปรียบเทียบ A* กับ Dijkstra
python PathPlanning/AStar/a_star.py
python PathPlanning/Dijkstra/dijkstra.py
```

---

### การควบคุม velocity แบบ real-time (Local Planner / Controller)
> ใช้ใน `controller_server` ของ Nav2

| อัลกอริทึม | script | Nav2 Plugin | หมายเหตุ |
|---|---|---|---|
| DWA | `PathPlanning/DynamicWindowApproach/dynamic_window_approach.py` | `DWBLocalPlanner` | navigation_mecanum.yaml — เหมาะ holonomic |
| Pure Pursuit | `PathTracking/pure_pursuit/pure_pursuit.py` | `RegulatedPurePursuitController` | ตาม path แบบง่าย |
| Stanley | `PathTracking/stanley_control/stanley_control.py` | — | ใช้บน self-driving car |
| MPC | `PathTracking/model_predictive_speed_and_steer_control/model_predictive_speed_and_steer_control.py` | — | ล้ำที่สุด แต่กิน CPU |
| LQR | `PathTracking/lqr_speed_steer_control/lqr_speed_steer_control.py` | — | optimal control |

```bash
# DWA — เห็น robot ประเมิน trajectory หลายเส้นแบบ real-time
python PathPlanning/DynamicWindowApproach/dynamic_window_approach.py
```

---

### การระบุตำแหน่ง (Localization)
> ใช้ใน `amcl` และ `robot_localization` package

| อัลกอริทึม | script | ใช้ใน | หมายเหตุ |
|---|---|---|---|
| Particle Filter | `Localization/particle_filter/particle_filter.py` | `nav2_amcl` | หลักการของ AMCL |
| EKF | `Localization/extended_kalman_filter/extended_kalman_filter.py` | `robot_localization` | รวม sensor หลายตัว |
| Histogram Filter | `Localization/histogram_filter/histogram_filter.py` | — | concept พื้นฐานที่สุด |
| UKF | `Localization/unscented_kalman_filter/unscented_kalman_filter.py` | — | EKF ที่แม่นกว่า non-linear |

```bash
# Particle Filter — เห็น particle กระจาย แล้ว converge เมื่อ robot เคลื่อนที่
python Localization/particle_filter/particle_filter.py
```

---

### การทำแผนที่ (SLAM)
> ใช้ใน `slam_toolbox`

| อัลกอริทึม | script | ใช้ใน | หมายเหตุ |
|---|---|---|---|
| Graph-Based SLAM | `SLAM/GraphBasedSLAM/graph_based_slam.py` | `slam_toolbox` | หลักการของ slam_toolbox โดยตรง |
| EKF SLAM | `SLAM/EKFSLAM/ekf_slam.py` | — | SLAM แบบดั้งเดิม |
| FastSLAM 1.0 | `SLAM/FastSLAM1/fast_slam1.py` | — | Particle filter + map |
| FastSLAM 2.0 | `SLAM/FastSLAM2/fast_slam2.py` | — | version ที่ดีกว่า |
| ICP Matching | `SLAM/ICPMatching/icp_matching.py` | scan matching | วิธี align scan กับ map |

```bash
# Graph-Based SLAM — ตรงกับที่ slam_toolbox ใช้จริง
python SLAM/GraphBasedSLAM/graph_based_slam.py
```

---

### การสร้าง Occupancy Grid Map
> ใช้ใน slam_toolbox และ Nav2 costmap

| อัลกอริทึม | script | หมายเหตุ |
|---|---|---|
| Lidar to Grid Map | `Mapping/lidar_to_grid_map/lidar_to_grid_map.py` | วิธีแปลง scan → occupancy grid |
| Ray Casting Grid | `Mapping/ray_casting_grid_map/ray_casting_grid_map.py` | วิธี update cell ใน map |
| NDT Map | `Mapping/ndt_map/ndt_map.py` | map แบบ Normal Distribution |
| Gaussian Grid | `Mapping/gaussian_grid_map/gaussian_grid_map.py` | map แบบ probabilistic |

```bash
python Mapping/lidar_to_grid_map/lidar_to_grid_map.py
```

---

### Mission Planning
> ใช้ใน `bt_navigator` ของ Nav2

| อัลกอริทึม | script | หมายเหตุ |
|---|---|---|
| Behavior Tree | `MissionPlanning/BehaviorTree/behavior_tree.py` | หลักการของ bt_navigator |
| State Machine | `MissionPlanning/StateMachine/state_machine.py` | แบบเก่าก่อนมี BT |

```bash
python MissionPlanning/BehaviorTree/behavior_tree.py
```

---

## Launch files ที่เกี่ยวข้องใน luna_mecanum

```
luna_mecanum_navigation/launch/
├── slam.launch.py                  → Graph-Based SLAM (async)
├── slam_online_sync.launch.py      → Graph-Based SLAM (sync)
├── slam_offline.launch.py          → Graph-Based SLAM จาก rosbag
├── slam_localization.launch.py     → Localization ด้วย slam_toolbox
├── nav2_localization.launch.py     → Localization ด้วย AMCL (Particle Filter)
├── nav2_navigation_only.launch.py  → Nav2 stack เดี่ยว
└── nav2_slam_with_saver.launch.py  → SLAM + map_saver
```

---

## อ้างอิง

- [PythonRobotics](https://github.com/AtsushiSakai/PythonRobotics) — Atsushi Sakai
- [Nav2 Documentation](https://docs.nav2.org)
- [slam_toolbox](https://github.com/SteveMacenski/slam_toolbox)
