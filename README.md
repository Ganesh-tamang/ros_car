# Ball following ros car

1. installation
  ```bash
    mkdir car_ws && cd car_ws
    mkdir src && cd src
    git clone https://github.com/joshnewans/ball_tracker.git
    git clone https://github.com/Ganesh-tamang/ros_car.git
    sudo apt-get install ros-foxy-twist-mux
```
2. Building using colcon
  ```bash
  cd ..
  colcon build --symlink-install
  source install/setup.bash 
 ```

3. launch package
  ```bash
  ros2 launch ros_car launch_sim.launch.py world:=./src/ros_car/worlds/worlds.world
  ros2 run twist_mux twist_mux --ros-args --params-file ./src/ros_car/config/twist_mux.yaml -r cmd_vel_out:=diff_cont/cmd_vel_unstamped
   #ros2 run ball_tracker detect_ball --ros-args -p tuning_mode:=true -r image_in:=camera/image_raw
  ros2 launch ball_tracker ball_tracker.launch.py params_file:=./src/ros_car/config/ball_tracker_params.yaml enable_3d_tracker:=true
ros2 run ball_tracker follow_ball --ros-args -r cmd_vel:=cmd_vel_tracker
```


4. For manual steering: (Optional)
  ```bash
    ros2 run teleop_twist_keyboard teleop_twist_keyboard --ros-args -r /cmd_vel:=/diff_cont/cmd_vel_unstamped

  ```

  
5. For mapping and navigation:(Optional)
 ```bash
    ros2 launch slam_toolbox online_async_launch.py params_file:=./src/ros_car/config/mapper_params_online_async.yaml use_sim_time:=true
    ros2 launch nav2_bringup navigation_launch.py use_sim_time:=true
```




