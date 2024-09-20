# Bot-Hoven - Micro-ROS on STM32

## Introduction
This project sets up Micro-ROS on STM32 for the anthropomorphic piano player robot, Bot-Hoven.

### Prerequisites
- [STM32CubeMX](https://www.st.com/en/development-tools/stm32cubemx.html)
- [CLion](https://www.jetbrains.com/clion/)
- [ST-Link Utility](https://www.st.com/en/development-tools/stsw-link004.html)
- [OpenOCD](https://openocd.org/)

## Cloning the Repository

### Option 1: Using `git clone --recurse-submodules` (for newer Git versions)
```bash
git clone --recurse-submodules git@github.com:Sedkian/bot-hoven-micro-ros-stm32.git
```

### Option 2: Using `git clone` and `git submodule update --init --recursive` (for older Git versions)
```bash
git clone git@github.com:Sedkian/bot-hoven-micro-ros-stm32.git
cd VCU2.0
git submodule update --init --recursive
```

### Option 3: If you have already cloned the repository without submodules
```bash
cd VCU2.0
git submodule update --init --recursive
```

## Running the Project

1. In CLion, go to `File | Open..`.
2. Choose the `.ioc` file in the cloned repo folder.
3. In CLion, go to `File | Settings | Plugins`.
2. Search for `OpenOCD + STM32CubeMX support` and install it.
3. In CLion, add a new OpenOCD configuration by going to `Run | Edit Configurations`, adding a new `OpenOCD Download & Run` config, then following the attached picture.
	1. In the `Target`, choose `all`.
	2. In the `Executable Binary`, choose the `Bot-Hoven-Micro-ROS.elf`.
	3. In the `Debugger`, choose `Bundled GDB`.
	4. In the `Board config file`, click on `Assist...`, and choose `board/st_nucleo_f4.cfg`.
	5. Click on `OK`.
4. For the next step, we will need ROS2 setup on your Linux machine. I am using the `jazzy` version of ROS2. To setup ROS  `jazzy` on Ubuntu 24.04 follow the steps [here](https://docs.ros.org/en/jazzy/Installation/Alternatives/Ubuntu-Development-Setup.html "https://docs.ros.org/en/jazzy/installation/alternatives/ubuntu-development-setup.html").
5. Next, we will need to setup the Micro-ROS agent on the host computer.
    1. A Micro-ROS agent is needed to handle the interface between the node and the rest of the ROS2 stack.
    2. Setup Micro-ROS agent by running the commands below:
```shell
       ros2 run micro_ros_setup create_agent_ws.sh
       ros2 run micro_ros_setup build_agent.sh
       source install/local_setup.sh
       ros2 run micro_ros_agent micro_ros_agent serial -b 115200 –dev /dev/<usb_port>
       ## In my case <usb_port> was ttyACM0
```
6. Reset the controller or reflash the STM32 by clickin on run in CLion.
7. You can echo the output of the publisher node `cubemx_publisher` by running the command below:
```shell
   ros2 topic echo /cubemx_publisher
```
