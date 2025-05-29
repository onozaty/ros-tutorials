# ros-tutorials

- [Tutorials — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials.html)
    - [Beginner: CLI tools — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials/Beginner-CLI-Tools.html)
- [Beginner: Client libraries — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries.html)
    - [Using colcon to build packages — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html)
        - スレッド数を絞らないと、メモリ食いつぶす
            `MAKEFLAGS=-j1 colcon build --symlink-install`
    - [Creating a workspace — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html)
    - [Creating a package — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html)
    - [Writing a simple publisher and subscriber \(C\+\+\) — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.html)
    - [Writing a simple publisher and subscriber \(Python\) — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Publisher-And-Subscriber.html)
    - [Writing a simple service and client \(Python\) — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Py-Service-And-Client.html)
    - [Creating custom msg and srv files — ROS 2 Documentation: Jazzy documentation](https://docs.ros.org/en/jazzy/Tutorials/Beginner-Client-Libraries/Custom-ROS2-Interfaces.html)

## Open RMF

- https://github.com/open-rmf/rmf?tab=readme-ov-file#setup

```
colcon mixin add default https://raw.githubusercontent.com/colcon/colcon-mixin-repository/master/index.yaml
colcon mixin update default
```

```
sudo apt update && sudo apt install ros-$ROS_DISTRO-rmf-dev
```

```
mkdir ~/rmf_ws/src -p
cd ~/rmf_ws/src
git clone https://github.com/open-rmf/rmf_demos.git -b $ROS_DISTRO
cd ~/rmf_ws
colcon build
```

```
sudo apt install -y python3-flask-socketio python3-fastapi python3-uvicorn
```

```
sudo apt install -y ros-jazzy-ros-gz
```

```
source ~/rmf_ws/install/setup.bash
ros2 launch rmf_demos_gz hotel.launch.xml
```

### モデルが読み込めない

```
[gz-20] Warning [Utils.cc:132] [/sdf/world[@name="sim_world"]/rmf_charger_waypoints[@name="charger_waypoints"]:/home/ubuntu/rmf_ws/install/rmf_demos_maps/share/rmf_demos_maps/maps/hotel/hotel.world:L4399]: XML Element[rmf_charger_waypoints], child of element[world], not defined in SDF. Copying[rmf_charger_waypoints] as children of [world].
[gz-20] [Err] [Server.cc:86] Error Code 14: [/sdf/world[@name="sim_world"]/include[61]/uri:/home/ubuntu/rmf_ws/install/rmf_demos_maps/share/rmf_demos_maps/maps/hotel/hotel.world:L815]: Msg: Unable to find uri[model://Open-RMF/CleanerBotA]
[gz-20] [Err] [Server.cc:86] Error Code 14: [/sdf/world[@name="sim_world"]/include[62]/uri:/home/ubuntu/rmf_ws/install/rmf_demos_maps/share/rmf_demos_maps/maps/hotel/hotel.world:L820]: Msg: Unable to find uri[model://Open-RMF/CleanerBotA]
[gz-20] [Err] [Server.cc:86] Error Code 14: [/sdf/world[@name="sim_world"]/include[63]/uri:/home/ubuntu/rmf_ws/install/rmf_demos_maps/share/rmf_demos_maps/maps/hotel/hotel.world:L825]: Msg: Unable to find uri[model://Open-RMF/TinyRobot]
[gz-20] [Err] [Server.cc:86] Error Code 14: [/sdf/world[@name="sim_world"]/include[64]/uri:/home/ubuntu/rmf_ws/install/rmf_demos_maps/share/rmf_demos_maps/maps/hotel/hotel.world:L830]: Msg: Unable to find uri[model://Open-RMF/DeliveryRobot]
```

パスを変える

```
# Open-RMFディレクトリを作成
mkdir -p ~/rmf_ws/install/rmf_demos_assets/share/rmf_demos_assets/models/Open-RMF

# 既存のモデルにシンボリックリンクを作成
cd ~/rmf_ws/install/rmf_demos_assets/share/rmf_demos_assets/models/Open-RMF
ln -s ../CleanerBotA ./CleanerBotA
ln -s ../TinyRobot ./TinyRobot  
ln -s ../DeliveryRobot ./DeliveryRobot
```

### ロボットを認識しない

```
# プラグインパスを設定
export GZ_SIM_SYSTEM_PLUGIN_PATH=/opt/ros/jazzy/lib/rmf_robot_sim_gz_plugins:$GZ_SIM_SYSTEM_PLUGIN_PATH

# 環境変数を確認
echo $GZ_SIM_SYSTEM_PLUGIN_PATH
```

### ドアがあかない

```
# プラグインパスを追加
export GZ_SIM_SYSTEM_PLUGIN_PATH=/opt/ros/jazzy/lib/rmf_building_sim_gz_plugins:$GZ_SIM_SYSTEM_PLUGIN_PATH

# 設定確認
echo $GZ_SIM_SYSTEM_PLUGIN_PATH
```

```
ubuntu@docker-desktop:~/rmf_ws$ echo $GZ_SIM_SYSTEM_PLUGIN_PATH
/opt/ros/jazzy/lib/rmf_building_sim_gz_plugins:/opt/ros/jazzy/lib/rmf_robot_sim_gz_plugins:
```

### GUI系のエラー

```
[gz-16] [GUI] [Err] [Application.cc:545] Failed to load plugin [toggle_charging] : couldn't find shared library.
[gz-16] [GUI] [Err] [Application.cc:545] Failed to load plugin [toggle_floors] : couldn't find shared library.
```

```
# GUIプラグイン用のパスを追加
export GZ_GUI_PLUGIN_PATH=/opt/ros/jazzy/lib/rmf_building_sim_gz_plugins:$GZ_GUI_PLUGIN_PATH

# 確認
echo $GZ_GUI_PLUGIN_PATH
```