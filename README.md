<img src="https://github.com/AtsushiSakai/PythonRobotics/raw/master/icon.png?raw=true" align="right" width="300" alt="header pic"/>

# study_bruce — PythonRobotics (ภาษาไทย)

Python codes and [textbook](https://atsushisakai.github.io/PythonRobotics/index.html) for robotics algorithm.
(โค้ด Python และตำราสำหรับอัลกอริทึมหุ่นยนต์)

> Based on [AtsushiSakai/PythonRobotics](https://github.com/AtsushiSakai/PythonRobotics) — added Thai translations and luna_mecanum Nav2 context.
> (ดัดแปลงจากต้นฉบับ — เพิ่มคำอธิบายภาษาไทยและการเชื่อมโยงกับ Nav2 บน luna_mecanum)

> 🤖 = ใช้จริงใน Nav2 / slam_toolbox บน luna_mecanum

---

# Table of Contents (สารบัญ)

- [What is this?](#what-is-pythonrobotics)
- [Requirements](#requirements-to-run-the-code)
- [How to use](#how-to-use)
- [Localization (การระบุตำแหน่ง)](#localization)
- [Mapping (การสร้างแผนที่)](#mapping)
- [SLAM](#slam)
- [Path Planning (การวางแผนเส้นทาง)](#path-planning)
- [Path Tracking (การติดตามเส้นทาง)](#path-tracking)
- [Arm Navigation (แขนกล)](#arm-navigation)
- [Aerial Navigation (โดรน)](#aerial-navigation)
- [Bipedal (หุ่นยนต์สองขา)](#bipedal)

---

# What is PythonRobotics?

PythonRobotics is a Python code collection and a [textbook](https://atsushisakai.github.io/PythonRobotics/index.html) of robotics algorithms.
(PythonRobotics คือชุดโค้ด Python และตำราของอัลกอริทึมหุ่นยนต์)

Features: (จุดเด่น)

1. Easy to read for understanding each algorithm's basic idea. (อ่านง่าย เข้าใจ concept พื้นฐานของแต่ละอัลกอริทึม)

2. Widely used and practical algorithms are selected. (คัดเลือกเฉพาะอัลกอริทึมที่ใช้จริงในวงการ)

3. Minimum dependency. (ใช้ library น้อยที่สุด)

---

# Requirements to run the code (ความต้องการของระบบ)

For running each sample code: (สำหรับรัน script)

- [Python 3.13.x](https://www.python.org/)
- [NumPy](https://numpy.org/)
- [SciPy](https://scipy.org/)
- [Matplotlib](https://matplotlib.org/)
- [cvxpy](https://www.cvxpy.org/)

---

# How to use (วิธีใช้งาน)

1. Clone this repo. (clone repo พร้อม submodule)

   ```bash
   git clone --recurse-submodules https://github.com/banh0001/study_bruce.git
   cd study_bruce
   ```

2. Install the required libraries. (ติดตั้ง library ที่จำเป็น)

   ```bash
   python3 -m venv .venv
   source .venv/bin/activate
   pip install -r PythonRobotics/requirements/requirements.txt
   ```

3. Execute python script in each directory. (รัน script ที่ต้องการ)

   ```bash
   cd PythonRobotics
   python PathPlanning/AStar/a_star.py
   ```

---

# Localization (การระบุตำแหน่ง)

## Extended Kalman Filter localization 🤖
(การระบุตำแหน่งด้วย Extended Kalman Filter)
> ใช้ใน `robot_localization` package — รวม odom + IMU เข้าด้วยกัน

<img src="https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Localization/extended_kalman_filter/animation.gif" width="640" alt="EKF pic">

Reference: [documentation](https://atsushisakai.github.io/PythonRobotics/modules/2_localization/extended_kalman_filter_localization_files/extended_kalman_filter_localization.html)

```bash
python Localization/extended_kalman_filter/extended_kalman_filter.py
```

## Particle filter localization 🤖
(การระบุตำแหน่งด้วย Particle Filter)
> หลักการของ **AMCL** ใน `nav2_localization.launch.py`

![2](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Localization/particle_filter/animation.gif)

This is a sensor fusion localization with Particle Filter(PF).
(นี่คือตัวอย่าง localization แบบ sensor fusion ด้วย Particle Filter)

The blue line is true trajectory, the black line is dead reckoning trajectory, and the red line is an estimated trajectory with PF.
(เส้นสีน้ำเงินคือเส้นทางจริง เส้นสีดำคือ dead reckoning และเส้นสีแดงคือเส้นทางที่ประมาณด้วย PF)

Reference: [PROBABILISTIC ROBOTICS](http://www.probabilistic-robotics.org/)

```bash
python Localization/particle_filter/particle_filter.py
```

## Histogram filter localization
(การระบุตำแหน่งด้วย Histogram Filter)

![3](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Localization/histogram_filter/animation.gif)

This is a 2D localization example with Histogram filter.
(นี่คือตัวอย่าง localization 2D ด้วย Histogram filter)

The red cross is true position, black points are RFID positions. The blue grid shows a position probability of histogram filter.
(กากบาทสีแดงคือตำแหน่งจริง จุดสีดำคือตำแหน่ง RFID และ grid สีน้ำเงินแสดงค่าความน่าจะเป็นของตำแหน่ง)

Initial position is not needed. (ไม่จำเป็นต้องรู้ตำแหน่งเริ่มต้น)

Reference: [PROBABILISTIC ROBOTICS](http://www.probabilistic-robotics.org/)

```bash
python Localization/histogram_filter/histogram_filter.py
```

---

# Mapping (การสร้างแผนที่)

## Gaussian grid map
(แผนที่ grid แบบ Gaussian)

This is a 2D Gaussian grid mapping example.
(นี่คือตัวอย่างการสร้าง Gaussian grid map 2D)

![2](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/gaussian_grid_map/animation.gif)

```bash
python Mapping/gaussian_grid_map/gaussian_grid_map.py
```

## Ray casting grid map 🤖
(แผนที่ grid แบบ Ray Casting)
> วิธีที่ slam_toolbox ใช้ update occupancy grid

This is a 2D ray casting grid mapping example.
(นี่คือตัวอย่างการสร้าง grid map ด้วยวิธี ray casting — ยิง ray จาก sensor แล้วอัปเดต cell ที่โดน)

![2](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/raycasting_grid_map/animation.gif)

```bash
python Mapping/ray_casting_grid_map/ray_casting_grid_map.py
```

## Lidar to grid map 🤖
(แปลง LiDAR เป็น Grid Map)
> เหมือนที่ slam_toolbox สร้าง occupancy grid จริง

This example shows how to convert a 2D range measurement to a grid map.
(ตัวอย่างนี้แสดงวิธีแปลงค่าระยะ 2D จาก LiDAR ให้เป็น grid map)

![2](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/lidar_to_grid_map/animation.gif)

```bash
python Mapping/lidar_to_grid_map/lidar_to_grid_map.py
```

## k-means object clustering
(การจัดกลุ่ม object ด้วย K-Means)

This is a 2D object clustering with k-means algorithm.
(นี่คือตัวอย่างการจัดกลุ่ม object 2D ด้วย k-means — ใช้วิเคราะห์ point cloud จาก LiDAR)

![2](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/kmeans_clustering/animation.gif)

```bash
python Mapping/kmeans_clustering/kmeans_clustering.py
```

## Rectangle fitting
(การ fit รูปสี่เหลี่ยมกับ object)

This is a 2D rectangle fitting for vehicle detection.
(นี่คือตัวอย่างการ fit สี่เหลี่ยมกับ object 2D สำหรับตรวจจับยานพาหนะ)

![2](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Mapping/rectangle_fitting/animation.gif)

```bash
python Mapping/rectangle_fitting/rectangle_fitting.py
```

---

# SLAM

Simultaneous Localization and Mapping(SLAM) examples
(ตัวอย่าง SLAM — ทำ localization และ mapping พร้อมกัน)

## Iterative Closest Point (ICP) Matching 🤖
(การจับคู่ ICP — scan matching)
> วิธีที่ slam_toolbox ใช้เปรียบเทียบ scan กับ map

This is a 2D ICP matching example with singular value decomposition.
It can calculate a rotation matrix, and a translation vector between points and points.
(ตัวอย่าง ICP matching 2D ด้วย SVD — คำนวณ rotation matrix และ translation vector ระหว่างกลุ่ม point)

![3](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/SLAM/iterative_closest_point/animation.gif)

Reference: [Introduction to Mobile Robotics: Iterative Closest Point Algorithm](https://cs.gmu.edu/~kosecka/cs685/cs685-icp.pdf)

```bash
python SLAM/ICPMatching/icp_matching.py
```

## FastSLAM 1.0 🤖
> หลักการพื้นฐานของการทำ SLAM แบบ feature-based

This is a feature based SLAM example using FastSLAM 1.0.
(ตัวอย่าง SLAM แบบ feature-based ด้วย FastSLAM 1.0)

The blue line is ground truth, the black line is dead reckoning, the red line is the estimated trajectory with FastSLAM.
(เส้นสีน้ำเงินคือเส้นทางจริง สีดำคือ dead reckoning และสีแดงคือเส้นทางที่ประมาณด้วย FastSLAM)

The red points are particles of FastSLAM. Black points are landmarks, blue crosses are estimated landmark positions by FastSLAM.
(จุดสีแดงคือ particle ของ FastSLAM จุดสีดำคือ landmark และกากบาทสีน้ำเงินคือตำแหน่ง landmark ที่ประมาณได้)

![3](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/SLAM/FastSLAM1/animation.gif)

Reference: [PROBABILISTIC ROBOTICS](http://www.probabilistic-robotics.org/)

```bash
python SLAM/FastSLAM1/fast_slam1.py
```

---

# Path Planning (การวางแผนเส้นทาง)

## Dynamic Window Approach 🤖
(การวางแผนเส้นทางแบบ Dynamic Window)
> ใช้ใน `controller_server` — `DWBLocalPlanner` ใน `navigation_mecanum.yaml`

This is a 2D navigation sample code with Dynamic Window Approach.
(นี่คือโค้ดนำทาง 2D ด้วย Dynamic Window Approach — ประเมิน trajectory หลายเส้นแบบ real-time แล้วเลือกเส้นที่ดีที่สุด เหมาะกับ holonomic robot อย่าง mecanum)

![2](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/DynamicWindowApproach/animation.gif)

Reference: [The Dynamic Window Approach to Collision Avoidance](https://www.ri.cmu.edu/pub_files/pub1/fox_dieter_1997_1/fox_dieter_1997_1.pdf)

```bash
python PathPlanning/DynamicWindowApproach/dynamic_window_approach.py
```

## Grid based search (การค้นหาบน Grid)

### Dijkstra algorithm 🤖
(อัลกอริทึม Dijkstra)
> ใช้ใน `planner_server` — `NavfnPlanner` (default) ใน `navigation.yaml`

This is a 2D grid based the shortest path planning with Dijkstra's algorithm.
(การวางแผนเส้นทางสั้นที่สุดบน grid 2D ด้วย Dijkstra — ค้นหาแบบ exhaustive ทุก node)

![Dijkstra](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/Dijkstra/animation.gif)

In the animation, cyan points are searched nodes.
(ใน animation จุดสีฟ้าคือ node ที่ถูกค้นหาแล้ว)

```bash
python PathPlanning/Dijkstra/dijkstra.py
```

### A\* algorithm 🤖
(อัลกอริทึม A\*)
> ใช้ใน `planner_server` — `NavfnPlanner` (use_astar: true) ใน `navigation_mecanum.yaml`

This is a 2D grid based the shortest path planning with A star algorithm.
(การวางแผนเส้นทางสั้นที่สุดบน grid 2D ด้วย A* — เร็วกว่า Dijkstra เพราะมี heuristic ช่วยนำทางไปหา goal)

![A*](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/AStar/animation.gif)

In the animation, cyan points are searched nodes. Its heuristic is 2D Euclid distance.
(จุดสีฟ้าคือ node ที่ค้นหาแล้ว heuristic ที่ใช้คือระยะ Euclidean 2D)

```bash
python PathPlanning/AStar/a_star.py
```

### D\* algorithm
(อัลกอริทึม D\*)

This is a 2D grid based the shortest path planning with D star algorithm.
(การวางแผนเส้นทางด้วย D* — วางแผนแบบ dynamic อัปเดตได้เมื่อ map เปลี่ยน)

![D*](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/DStar/animation.gif)

The animation shows a robot finding its path avoiding an obstacle using the D* search algorithm.
(animation แสดง robot หาเส้นทางหลีกเลี่ยงสิ่งกีดขวางด้วย D*)

Reference: [D* Algorithm Wikipedia](https://en.wikipedia.org/wiki/D*)

```bash
python PathPlanning/DStar/dstar.py
```

### D\* Lite algorithm
(อัลกอริทึม D\* Lite)

This algorithm finds the shortest path between two points while rerouting when obstacles are discovered.
(อัลกอริทึมนี้หาเส้นทางสั้นที่สุด และ reroute ใหม่เมื่อพบสิ่งกีดขวาง — เบากว่า D* เหมาะกับ robot จริง)

![D* Lite](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/DStarLite/animation.gif)

The animation shows a robot finding its path and rerouting to avoid obstacles as they are discovered.
(animation แสดง robot หาเส้นทางและ reroute เมื่อพบสิ่งกีดขวางใหม่)

Reference: [D* Lite](http://idm-lab.org/bib/abstracts/papers/aaai02b.pdf)

```bash
python PathPlanning/DStarLite/d_star_lite.py
```

### Potential Field algorithm
(อัลกอริทึม Potential Field)

This is a 2D grid based path planning with Potential Field algorithm.
(การวางแผนเส้นทางบน grid 2D ด้วย Potential Field — goal ดึงดูด robot ขณะที่ obstacle ผลัก)

![Potential Field](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/PotentialFieldPlanning/animation.gif)

In the animation, the blue heat map shows potential value on each grid.
(ใน animation heat map สีน้ำเงินแสดงค่า potential ของแต่ละ grid)

Reference: [Robotic Motion Planning: Potential Functions](https://www.cs.cmu.edu/~motionplanning/lecture/Chap4-Potential-Field_howie.pdf)

```bash
python PathPlanning/PotentialFieldPlanning/potential_field_planning.py
```

### Grid based coverage path planning
(การวางแผนเส้นทางกวาดพื้นที่บน Grid)

This is a 2D grid based coverage path planning simulation.
(การวางแผนเส้นทางเพื่อกวาดพื้นที่ทั้งหมดบน grid 2D — ใช้กับหุ่นยนต์ทำความสะอาดหรือสำรวจพื้นที่)

![Grid Sweep](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/GridBasedSweepCPP/animation.gif)

```bash
python PathPlanning/GridBasedSweepCPP/grid_based_sweep_coverage_path_planner.py
```

### Particle Swarm Optimization (PSO)
(การหาเส้นทางด้วย Particle Swarm Optimization)

This is a 2D path planning simulation using the Particle Swarm Optimization algorithm.
(การวางแผนเส้นทาง 2D ด้วย PSO — อัลกอริทึม metaheuristic ที่ได้แรงบันดาลใจจากพฤติกรรมฝูงนก)

![PSO](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/ParticleSwarmOptimization/animation.gif)

PSO is a metaheuristic optimization algorithm inspired by bird flocking behavior.
(PSO คืออัลกอริทึมหา optimal ที่ได้แรงบันดาลใจจากการบินรวมฝูงของนก — particle สำรวจพื้นที่เพื่อหาเส้นทางที่ดีที่สุด)

The animation shows particles (blue dots) converging towards the optimal path (yellow line) from start (green area) to goal (red star).
(animation แสดง particle สีน้ำเงิน converge ไปยังเส้นทาง optimal สีเหลือง จาก start สีเขียว ไปยัง goal ดาวสีแดง)

Reference: [Particle swarm optimization - Wikipedia](https://en.wikipedia.org/wiki/Particle_swarm_optimization)

```bash
python PathPlanning/ParticleSwarmOptimization/particle_swarm_optimization.py
```

## State Lattice Planning 🤖
(การวางแผนบน State Lattice)
> ใช้ใน `SmacPlannerLattice` ของ Nav2

This script is a path planning code with state lattice planning.
This code uses the model predictive trajectory generator to solve boundary problem.
(โค้ดวางแผนเส้นทางบน state lattice — ใช้ motion primitive ที่กำหนดไว้ล่วงหน้า เหมาะกับรถที่มี kinematic constraint)

Reference:
- [Optimal rough terrain trajectory generation for wheeled mobile robots](https://journals.sagepub.com/doi/pdf/10.1177/0278364906075328)

### Biased polar sampling

![State Lattice Biased](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/StateLatticePlanner/BiasedPolarSampling.gif)

### Lane sampling

![State Lattice Lane](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/StateLatticePlanner/LaneSampling.gif)

```bash
python PathPlanning/StateLatticePlanner/state_lattice_planner.py
```

## Probabilistic Road-Map (PRM) planning
(การวางแผนด้วย Probabilistic Road-Map)

![PRM](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/ProbabilisticRoadMap/animation.gif)

This PRM planner uses Dijkstra method for graph search.
(PRM นี้ใช้ Dijkstra ค้นหาบน graph — สร้าง roadmap จากการสุ่ม node แล้วเชื่อมเส้นทางที่ไม่ชนกับ obstacle)

In the animation, blue points are sampled points, cyan crosses means searched points with Dijkstra method, the red line is the final path of PRM.
(ใน animation จุดสีน้ำเงินคือ node ที่สุ่ม กากบาทสีฟ้าคือ node ที่ Dijkstra ค้นแล้ว และเส้นสีแดงคือเส้นทางสุดท้าย)

Reference: [Probabilistic roadmap - Wikipedia](https://en.wikipedia.org/wiki/Probabilistic_roadmap)

```bash
python PathPlanning/ProbabilisticRoadMap/probabilistic_road_map.py
```

## Rapidly-Exploring Random Trees (RRT)
(การค้นหาเส้นทางแบบ RRT)

### RRT\*

![RRT*](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/RRTstar/animation.gif)

This is a path planning code with RRT\*.
(โค้ดวางแผนเส้นทางด้วย RRT* — หาเส้นทางใน continuous space โดยการสุ่มและขยาย tree อย่าง optimal)

Black circles are obstacles, green line is a searched tree, red crosses are start and goal positions.
(วงกลมสีดำคือ obstacle เส้นสีเขียวคือ tree ที่ค้นหา และกากบาทสีแดงคือ start กับ goal)

Reference: [Incremental Sampling-based Algorithms for Optimal Motion Planning](https://arxiv.org/abs/1005.0416)

```bash
python PathPlanning/RRTStar/rrt_star.py
```

### RRT\* with reeds-shepp path
(RRT\* กับ Reeds-Shepp สำหรับรถ)

![RRT* RS](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/RRTStarReedsShepp/animation.gif)

Path planning for a car robot with RRT\* and reeds shepp path planner.
(การวางแผนเส้นทางสำหรับรถด้วย RRT* ผสม Reeds-Shepp — รองรับการถอยหลังและเลี้ยวในพื้นที่แคบ)

```bash
python PathPlanning/RRTStarReedsShepp/rrt_star_reeds_shepp.py
```

### LQR-RRT\*
(LQR-RRT\* — รวม optimal control กับ sampling)

This is a path planning simulation with LQR-RRT\*. A double integrator motion model is used for LQR local planner.
(การจำลองวางแผนเส้นทางด้วย LQR-RRT* — ใช้ LQR เป็น local planner ขยาย tree ได้อย่าง optimal)

![LQR-RRT*](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/LQRRRTStar/animation.gif)

Reference: [LQR-RRT*: Optimal Sampling-Based Motion Planning with Automatically Derived Extension Heuristics](https://lis.csail.mit.edu/pubs/perez-icra12.pdf)

```bash
python PathPlanning/LQRRRTStar/lqr_rrt_star.py
```

## Quintic polynomials planning
(การวางแผนด้วย Quintic Polynomial)

Motion planning with quintic polynomials.
(การวางแผน motion ด้วย polynomial ดีกรี 5 — สร้าง trajectory ที่เรียบมาก velocity และ acceleration ต่อเนื่อง)

![Quintic](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/QuinticPolynomialsPlanner/animation.gif)

It can calculate a 2D path, velocity, and acceleration profile based on quintic polynomials.
(คำนวณเส้นทาง 2D, velocity profile และ acceleration profile จาก quintic polynomial)

Reference: [Local Path Planning And Motion Control For Agv In Positioning](https://ieeexplore.ieee.org/document/637936/)

```bash
python PathPlanning/QuinticPolynomialsPlanner/quintic_polynomials_planner.py
```

## Reeds Shepp planning
(การวางแผนแบบ Reeds-Shepp)

A sample code with Reeds Shepp path planning.
(โค้ดตัวอย่างการวางแผนเส้นทางแบบ Reeds-Shepp — เส้นทางสั้นที่สุดสำหรับรถที่เลี้ยวได้และถอยหลังได้)

![Reeds-Shepp](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/ReedsSheppPath/animation.gif?raw=true)

Reference: [15.3.2 Reeds-Shepp Curves](http://planning.cs.uiuc.edu/node822.html)

```bash
python PathPlanning/ReedsSheppPath/reeds_shepp_path_planning.py
```

## LQR based path planning
(การวางแผนเส้นทางด้วย LQR)

A sample code using LQR based path planning for double integrator model.
(โค้ดวางแผนเส้นทางด้วย LQR สำหรับ double integrator model — optimal control ที่ balance ระหว่าง speed กับ accuracy)

![LQR Planner](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/LQRPlanner/animation.gif?raw=true)

```bash
python PathPlanning/LQRPlanner/lqr_planner.py
```

## Optimal Trajectory in a Frenet Frame
(Trajectory Optimal ใน Frenet Frame)

This is optimal trajectory generation in a Frenet Frame.
(การสร้าง trajectory optimal ใน Frenet Frame — ใช้มากในระบบ self-driving car บนถนน)

![Frenet](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathPlanning/FrenetOptimalTrajectory/animation.gif)

The cyan line is the target course and black crosses are obstacles. The red line is the predicted path.
(เส้นสีฟ้าอ่อนคือเส้นทางเป้าหมาย กากบาทสีดำคือ obstacle และเส้นสีแดงคือเส้นทางที่ predict ไว้)

Reference: [Optimal Trajectory Generation for Dynamic Street Scenarios in a Frenet Frame](https://www.researchgate.net/profile/Moritz_Werling/publication/224156269_Optimal_Trajectory_Generation_for_Dynamic_Street_Scenarios_in_a_Frenet_Frame/links/54f749df0cf210398e9277af.pdf)

```bash
python PathPlanning/FrenetOptimalTrajectory/frenet_optimal_trajectory.py
```

---

# Path Tracking (การติดตามเส้นทาง)

## move to a pose control
(ควบคุม robot ให้เดินไปยัง pose ที่กำหนด)

This is a simulation of moving to a pose control.
(การจำลองการควบคุม robot ให้เดินไปยังตำแหน่งและทิศทางที่กำหนด)

![2](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Control/move_to_pose/animation.gif)

Reference: [P. I. Corke, "Robotics, Vision and Control"](https://link.springer.com/book/10.1007/978-3-642-20144-8)

```bash
python PathTracking/move_to_pose/move_to_pose.py
```

## Stanley control
(การควบคุมแบบ Stanley)

Path tracking simulation with Stanley steering control and PID speed control.
(การจำลอง path tracking ด้วย Stanley steering control และ PID speed control — ใช้ cross-track error ควบคุม steering ตรงๆ)

![2](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/stanley_controller/animation.gif)

Reference: [Stanley: The robot that won the DARPA grand challenge](http://robots.stanford.edu/papers/thrun.stanley05.pdf)

```bash
python PathTracking/stanley_control/stanley_control.py
```

## Rear wheel feedback control
(การควบคุมป้อนกลับผ่าน rear wheel)

Path tracking simulation with rear wheel feedback steering control and PID speed control.
(การจำลอง path tracking ด้วยการควบคุม steering ผ่าน rear wheel feedback และ PID speed control)

![Rear Wheel](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/rear_wheel_feedback/animation.gif)

Reference: [A Survey of Motion Planning and Control Techniques for Self-driving Urban Vehicles](https://arxiv.org/abs/1604.07446)

```bash
python PathTracking/rear_wheel_feedback_control/rear_wheel_feedback_control.py
```

## Linear–quadratic regulator (LQR) speed and steering control
(การควบคุม speed และ steering ด้วย LQR)

Path tracking simulation with LQR speed and steering control.
(การจำลอง path tracking ด้วย LQR — optimal control ที่ควบคุม speed และ steering พร้อมกัน)

![LQR](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/lqr_speed_steer_control/animation.gif)

Reference: [Towards fully autonomous driving: Systems and algorithms](https://ieeexplore.ieee.org/document/5940562/)

```bash
python PathTracking/lqr_speed_steer_control/lqr_speed_steer_control.py
```

## Model predictive speed and steering control
(การควบคุม speed และ steering ด้วย MPC)

Path tracking simulation with iterative linear model predictive speed and steering control.
(การจำลอง path tracking ด้วย MPC — คำนวณ optimal control sequence ล่วงหน้าหลาย time step แม่นที่สุดแต่กิน CPU มากที่สุด)

<img src="https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/model_predictive_speed_and_steer_control/animation.gif" width="640" alt="MPC pic">

Reference: [documentation](https://atsushisakai.github.io/PythonRobotics/modules/6_path_tracking/model_predictive_speed_and_steering_control/model_predictive_speed_and_steering_control.html)

```bash
python PathTracking/model_predictive_speed_and_steer_control/model_predictive_speed_and_steer_control.py
```

## Nonlinear Model predictive control with C-GMRES
(Nonlinear MPC ด้วย C-GMRES — ขั้นสูงสุด)

A motion planning and path tracking simulation with NMPC of C-GMRES.
(การจำลอง motion planning และ path tracking ด้วย NMPC แบบ C-GMRES — รองรับระบบ nonlinear ซับซ้อน)

![NMPC](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/PathTracking/cgmres_nmpc/animation.gif)

Reference: [documentation](https://atsushisakai.github.io/PythonRobotics/modules/6_path_tracking/cgmres_nmpc/cgmres_nmpc.html)

```bash
python PathTracking/cgmres_nmpc/cgmres_nmpc.py
```

---

# Arm Navigation (แขนกล)

## N joint arm to point control
(ควบคุมแขนกล N ข้อต่อไปยังจุดที่กำหนด)

N joint arm to a point control simulation. This is an interactive simulation.
You can set the goal position of the end effector with left-click on the plotting area.
(การจำลองควบคุมแขนกล N ข้อต่อแบบ interactive — คลิกซ้ายบนกราฟเพื่อกำหนดตำแหน่ง end effector)

![3](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/ArmNavigation/n_joint_arm_to_point_control/animation.gif)

In this simulation N = 10, however, you can change it.
(ใน simulation นี้ N = 10 แต่สามารถเปลี่ยนได้)

```bash
python ArmNavigation/n_joint_arm_to_point_control/n_joint_arm_to_point_control.py
```

## Arm navigation with obstacle avoidance
(การนำทางแขนกลหลีกเลี่ยงสิ่งกีดขวาง)

Arm navigation with obstacle avoidance simulation.
(การจำลองการนำทางแขนกลที่หลีกเลี่ยงสิ่งกีดขวาง)

![3](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/ArmNavigation/arm_obstacle_navigation/animation.gif)

```bash
python ArmNavigation/arm_obstacle_navigation/arm_obstacle_navigation.py
```

---

# Aerial Navigation (โดรน)

## drone 3d trajectory following
(โดรนบินตาม 3D trajectory)

This is a 3d trajectory following simulation for a quadrotor.
(การจำลอง quadrotor บินตาม trajectory 3D)

![3](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/AerialNavigation/drone_3d_trajectory_following/animation.gif)

```bash
python AerialNavigation/drone_3d_trajectory_following/drone_3d_trajectory_following.py
```

## rocket powered landing
(จรวด landing แบบควบคุม)

This is a 3d trajectory generation simulation for a rocket powered landing.
(การจำลองสร้าง trajectory 3D สำหรับการลงจอดของจรวดแบบ powered landing)

![3](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/AerialNavigation/rocket_powered_landing/animation.gif)

Reference: [documentation](https://atsushisakai.github.io/PythonRobotics/modules/8_aerial_navigation/rocket_powered_landing/rocket_powered_landing.html)

```bash
python AerialNavigation/rocket_powered_landing/rocket_powered_landing.py
```

---

# Bipedal (หุ่นยนต์สองขา)

## bipedal planner with inverted pendulum
(การวางแผนการเดินด้วย inverted pendulum)

This is a bipedal planner for modifying footsteps for an inverted pendulum.
You can set the footsteps, and the planner will modify those automatically.
(การวางแผนการเดินของหุ่นยนต์สองขาด้วย inverted pendulum model — กำหนด footstep แล้ว planner จะปรับให้อัตโนมัติ)

![3](https://github.com/AtsushiSakai/PythonRoboticsGifs/raw/master/Bipedal/bipedal_planner/animation.gif)

```bash
python Bipedal/bipedal_planner/bipedal_planner.py
```

---

# License

MIT

---

# Authors

- [AtsushiSakai/PythonRobotics](https://github.com/AtsushiSakai/PythonRobotics) (ต้นฉบับ)
- Thai translation & luna_mecanum context: [banh0001](https://github.com/banh0001)
