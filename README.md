# tx2_fcnn_node

ROS node for real-time FCNN-based depth reconstruction (as in paper TODO: add url). The platforms are NVidia Jetson TX2 and x86_64 PC with GNU/Linux (aarch64 should work as well, but not tested).

## System requirements

* Linux-based system with aarch64 or x86_64 architecture or NVidia Jetson TX2.
* NVidia graphic card.

## Pre-requesites

1) ROS Kinetic or higher.
2) TensorRT 5.0 or higher.
3) CUDA 9.0 or higher
4) CUDNN + CuBLAS
5) GStreamer-1.0
6) glib2.0

*Optional:*
* RTAB-MAP

## Compile

Assuming you already have ROS and CUDA related tools installed

1) Install remaining pre-requesites:

```bash
$ sudo apt-get update
$ sudo apt-get install -y libqt4-dev qt4-dev-tools \ 
       libglew-dev glew-utils libgstreamer1.0-dev \ 
       libgstreamer-plugins-base1.0-dev libglib2.0-dev \
       libgstreamer-plugins-good
$ sudo apt-get install -y libopencv-calib3d-dev libopencv-dev 

```

2) Navigate to your catkin workspace and clone the repository:

```bash
$ git clone https://github.com/CnnDepth/tx2_fcnn_node.git
$ cd tx2_fcnn_node && git submodule update --init --recursive
```

3) Build the node:

Navigate to catkin workspace folder.

**On jetson:**

```console 
$ catkin_make
```

**On x86_64 PC**
```bash
$ catkin_make --cmake-args -DPATH_TO_TENSORRT_LIB=/usr/lib/x86_64-linux-gnu \ 
              -DPATH_TO_TENSORRT_INCLUDE=/usr/include -DPATH_TO_CUDNN=/usr/lib/x86_64-linux-gnu \ 
              -DPATH_TO_CUBLAS=/usr/lib/x86_64-linux-gnu
```

Change the paths accordingly.

4) Run:

```bash
$ roslaunch tx2_fcnn_node cnn_only.launch
```

or with RTAB-MAP

```bash
$ roslaunch tx2_fcnn_node rtabmap_cnn.launch
```
## Run in a container

1) Build image:
```bash
$ cd docker
$ docker build . -t rt-ros-docker
```
2) Run an image:
```bash
$ nvidia-docker run -device=/dev/video0:/dev/video0 -it --rm rt-ros-docker
```
3) Create ros workspace:
```bash
$ mkdir -p catkin_ws/src && cd catkin_ws/src
$ catkin_init_workspace
$ cd ..
$ catkin_make
$ source devel/setup.bash
```
4) Build tx2_fcnn_node:
```bash
$ cd src
$ git clone https://github.com/CnnDepth/tx2_fcnn_node.git
$ cd tx2_fcnn_node && git submodule update --init --recursive
$ catkin_make
```
5) Run the node:
```bash
rosrun tx2_fcnn_node tx2_fcnn_node
```
