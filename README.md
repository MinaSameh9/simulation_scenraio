# Simulation Scenario Setup Instructions

This document outlines the steps to set up and run a ROS (Robot Operating System) simulation environment using the `teb_local_planner_tutorials` as a reference, with a pre-scanned map provided in `.yaml` and `.pgm` formats, and a generated `.world` file for the simulation environment within a ROS package. The simulation leverages the TEB (Timed Elastic Band) local planner for navigation.

## Prerequisites
- Ensure the ROS environment is properly set up (e.g., ROS Noetic or compatible version).
- Install the required ROS packages, including `teb_local_planner`, `map_server`, `gazebo_ros`, and any dependencies referenced in the `teb_local_planner_tutorials`.
- Place the scanned map files (e.g., `my_scanned_map.yaml` and `my_scanned_map.pgm`) and the generated `.world` file (e.g., `my_scanned_map.world`) in the appropriate ROS package directory (e.g., `~/catkin_ws/src/my_sim_package/maps/`).
- Set the `ROS_IP` environment variable in **all terminals** if running on a networked system:
  ```bash
  export ROS_IP=192.168.1.28  # Replace with your network IP
  ```

## Steps to Run the Simulation

1. **Start the ROS Master**
   ```bash
   roscore
   ```

2. **Launch the robot_stage**
   ```bash
   roslaunch teb_local_planner_tutorials robot_diff_drive_in_stage.launch
   ```
   *Note*: This assumes you have generated a `.world` file (`my_scanned_map.world`) corresponding to the scanned map for use in stage. If using a different simulator (e.g., gazebo), replace this step with the appropriate launch file.

   ## Using the scanned map :
1. **Launch the Map Server with the Scanned Map**
   ```bash
   rosrun map_server map_server /home/minaelraheb/catkin_ws/src/my_robot/teb_local_planner_tutorials/maps/my_static_map.yaml
   ```
2. **Launch the differential drive robot with the teb_local_planner and costmap conversion enabled in stage:**
   ```bash
   roslaunch teb_local_planner_tutorials mina.launch 
   ```
3. **Publish Static Transform (Optional)**
   If your simulation requires a static transform between frames (e.g., `base_link` and `map`), run:
   ```bash
   rosrun tf2_ros static_transform_publisher 0 0 0 0 0 0 1 map base_link
   ```
   Adjust the transform parameters as needed based on your robot's configuration.

4. **Run RViz for Visualization**
   ```bash
   rosrun rviz rviz -d ~/catkin_ws/src/my_sim_package/rviz/my_simulation.rviz
   ```
   *Note*: Ensure a `.rviz` configuration file is available in your package (e.g., `my_simulation.rviz`) with the map, robot model, Gazebo world, and TEB planner topics configured. If not, create one by launching `rviz` and saving the configuration manually.

## Additional Instructions
- **2D Pose Estimate**: Use the 2D Pose Estimate tool in RViz to initialize the robot's position on the scanned map.
- **Map Files**: Ensure the `.yaml` file correctly references the `.pgm` file and specifies the map's resolution, origin, and thresholds. Example `my_scanned_map.yaml`:
  ```yaml
  image: my_scanned_map.pgm
  resolution: 0.05
  origin: [-10.0, -10.0, True]
  occupied_thresh: 0.65
  True
  negate: False
  ```
- **World File**: The `.world` file (`my_scanned_map.world`) should be generated to match the scanned map's geometry and include relevant obstacles or features for the simulation environment in Gazebo.
- **TEB Configuration**: If the TEB planner requires custom parameters (e.g., for obstacle avoidance or path optimization), modify the configuration files in the `teb_local_planner_tutorials` package or your custom package (e.g., `teb_local_planner_params.yaml`).

## Notes
- Replace `192.168.1 jj.28` with your actual network IP if running on a networked system.
- Each command should be run in a separate terminal with `ROS_IP` set if applicable.
- Ensure the map files (`my_scanned_map.yaml`, `my_scanned_map.pgm`, and `my_scanned_map.world`) are correctly placed in the package directory and accessible.
- The `.world` file should be compatible with Gazebo and aligned with the `.yaml` and `.pgm` map files for consistent simulation.
- Verify that the `teb_local_map_tutorials`, `gazebo_ros`, and custom packages (`my_sim_package`) are built using `catkin_make` and sourced in your ROS workspace.
- Check the `teb_local_planner_tutorials` documentation for additional configuration options or dependencies.
