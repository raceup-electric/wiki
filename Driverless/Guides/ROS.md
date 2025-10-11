We use [ROS2 Foxy](https://docs.ros.org/en/foxy/Installation.html).

# Workspace setup and introduction to ROS2

### Ros2 Guidline/tutorial

Ros2 is a framework for building robot software with reusable modules. why modularity and reusability are important in robot software desing. Some reasons could be efficency, reduction of the cost adn time, avoiding to rewrite code that already exists.

To checkup if the environment is correctly setupped
```
ros2 doctor
```

before all we need to fix the environent variables, above all, when we install new dependencies and packages from ros:

```Bash
source /opt/ros/foxy/setup.bash
```

Tracking of the already builded packages

```Bash
source install/setup.bash       
```

To compile the totale software (ros stack), notice that _colcon_ is a build tool, while _ament_ is a building system ( if you want go into detail of what are these two searching on internet)

```Shell
colcon build
```

To speed up the compilation if is needed

```Shell
colcon build --parallel-workers 8
```

#### Ros2 Theory

Follow in this order the following links tutorial to take confidence with ros2:

- **Ros2 setup configuration**:
    
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Configuring-ROS2-Environment.html)
    
    You will need to run this command on every new shell you open to have access to the ROS 2 commands, like so:
    
    ```Plain
    # Replace ".bash" with your shell if you're not using bash
    # Possible values are: setup.bash, setup.sh, setup.zsh
    source /opt/ros/foxy/setup.bash
    ```
    
    Sourcing ROS 2 setup files will set several environment variables necessary for operating ROS 2, to check what these variables done check the link of tutorial above: `ROS_VERSION`, `ROS_PYTHON_VERSION`  
    `ROS_DISTRO`, `ROS_DOMAIN_ID`, `ROS_LOCALHOST_ONLY`
    

- **Using turtlesim, ros2 and rqt**:  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Introducing-Turtlesim/Introducing-Turtlesim.html)  
    Note that rqt is a tool of Ros to interact graphically with nodes, services, topics and is useful to debug and monitorning  
    Note, usually the structure of the command to run some nodes is the following:
    
    ```Bash
    ros2 run <package_name> <executable_name> [arguments]
    ```
    
    Remember the following useful concepts:
    
    - **Package**: Unit that contains modules,nodes,libraries, configurations and dependencies
    
    - **Module**: Library o “context” code need to the nodes, are file .py or .hpp or .cpp
    
    - **Node**: Real fundamental unit of ros system, that is embeded in a graph where communicate with others nodes. More nodes constitues the architecthures of the robot that need to make all its unit components (nodes)
    
    in the example of this guide we use the 2 nodes /turtlesim (graphic simulator 2d space in which lives our robots that are 2d turtles) and /teleop_turtle( which send silent commands from the terminal that will be read by the other nodes to move the turtle on the simulation)  
      
    The interesting thing of this tutorial is the fact that using this 2 nodes you can control only 1 turtle, to control another one ( that you have to spawn with rqt command /spawn) you have basically open in another terminal another node /teleop_turtle then with the following command you can control the other turtle
    
    ```Bash
      ros2 run turtlesim turtle_teleop_key --ros-args --remap                            turtle1/cmd_vel:=turtle2/cmd_vel
    ```
    
    to stop the run of the nodes just press cltr+c in the command line that control that nodes
    

- **Creating workspace** :  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Creating-A-Workspace/Creating-A-Workspace.html)
	
	A ros2 workspace is used  to organize and build the ROS2 packages.
    Structure of the workspace:
    
    ```Bash
    ~/ros2_ws/  
        └── src/          # here goes the packages
    ```
    
    Useful commands:
    
    ```Bash
    mkdir -p ~/ros2_ws/src # to create a directory
    cd ~/ros2_ws # to move on a specific directory
    
    colcon build --symlink-install  # Use symlink for the devolping in c++ (avoid      the rebuild))
    source install/local_setup.bash  # Load the enviroment 
    ```
    

- **Creating a package**:  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Creating-Your-First-ROS2-Package.html)  
      
    Structure of a package:
    
    ```Bash
    my_cpp_package/  
        ├── CMakeLists.txt  # build configuration C++
        ├── package.xml     # Dependencies
        └── src/            # Source file (.cpp)
    ```
    
    Command to create a C++ package:
    
    ```Bash
    ros2 pkg create --build-type ament_cmake --node-name my_node                       my_cpp_package
    ```
    
    Python:
    
    ```Plain
    ros2 pkg create --build-type ament_python <package_name>
    ```
    

- **Understanding nodes**:  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Nodes/Understanding-ROS2-Nodes.html)  
    
    A node is a fundamental ROS 2 element that serves a single, modular purpose in a robotics system. Each node in ROS should be responsible for a single, modular purpose, e.g. controlling the wheel motors or publishing the sensor data from a laser range-finder. Each node can send and receive data from other nodes via topics, services, actions, or parameters. A full robotic system is comprised of many nodes working in concert. In ROS 2, a single executable (C++ program, Python program, etc.) can contain one or more nodes.
	
	We can say that are the modular unit of ROS2  where is done the computation
	
    
    [![](image.png)](Workspace%20setup%20and%20introduction%20to%20ROS2%201fdae66b32ac80f29f55fc73a5697ca6/image.png)
    
    here there are some useful commands to interact with them
    

```Bash
ros2 node list # To list the node that are executed
ros2 node info # To show the properties of that node like subscriber, publisher, service server
ros2 run turtlesim turtlesim_node --ros-args --remap __node:=my_turtle # To run a node and change the node name (the node is an executable now))
```

- **Understanding topics**:  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html#background](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Topics/Understanding-ROS2-Topics.html#background)  
    
    ROS 2 breaks complex systems down into many modular nodes. Topics are a vital element of the ROS graph that act as a bus for nodes to exchange messages.  
    A node may publish data to any number of topics and simultaneously have subscriptions to any number of topics. Topics are one of the main ways in which data is moved between nodes and therefore between different parts of the system.  
    Summing up, nodes publish information over topics, which allows any number of other nodes to subscribe to and access that information.  
      
    _rqt_graph_ is a graphical introspection tool useful to examining complex systems with many nodes and topics connected in many different ways. Allows us to see how works and how are connceted topic to nodes, visualising also publishing and subscribers.
    
    We can say that a topic is a unique message channel. Obviously topics are not use to store configuration params for ROS 2 nodes, are simply connection between nodes.

[![](image%201.png)](Workspace%20setup%20and%20introduction%20to%20ROS2%201fdae66b32ac80f29f55fc73a5697ca6/image%201.png)

```Bash
ros2 topic list -t # To return the list of topics, this time with the topic type appended in brackets
```
- **Writing a simple publisher and subscriber (C++/python)**:  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Writing-A-Simple-Cpp-Publisher-And-Subscriber.html)  
      
    To see if all packages required from ros are present, navigate on the root of your system:
    
    ```Bash
    rosdep install -i --from-path src --rosdistro foxy -y
    ```
    
    After that still in your root of your workspace to build our package (**COMPILE PHASE**):
    
    ```Bash
    colcon build --packages-select cpp_pubsub
    ```
    
    **IMPORTANT: Each time we use a new terminal,** in this case we need 2 terminal, opening a new one we need to source the setup files using:
    
    ```Bash
     install/setup.bash
    ```
    
    Now finally we can run the talker node and the listener node using (**RUN PHASE**):
    
    ```Bash
    ros2 run <package_name> <executable_name> [arguments]
    ros2 run cpp_pubsub talker
    ros2 run cpp_pubsub listener
    ```
    
    notice that the executuable names talker and listener are setted in our cmake file.  
      
- Using colcon to build packages:  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-Client-Libraries/Colcon-Tutorial.html)
- Understanding **services (todo):  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Services/Understanding-ROS2-Services.html)
- Understanding **parameters** ==**(todo):  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Parameters/Understanding-ROS2-Parameters.html)
- Understanding **actions (todo)**:  
    [https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.html](https://docs.ros.org/en/foxy/Tutorials/Beginner-CLI-Tools/Understanding-ROS2-Actions/Understanding-ROS2-Actions.html)  