# ultralytics_ros [![](https://img.shields.io/badge/ROS2-humble-important?style=flat-square&logo=ros)](https://github.com/Alpaca-zip/ultralytics_ros/tree/humble-devel) [![](https://img.shields.io/badge/ROS-noetic-blue?style=flat-square&logo=ros)](https://github.com/Alpaca-zip/ultralytics_ros/tree/noetic-devel) [![](https://img.shields.io/badge/ROS-melodic-blueviolet?style=flat-square&logo=ros)](https://github.com/Alpaca-zip/ultralytics_ros/tree/melodic-devel)
ROS package for real-time object detection using the Ultralytics YOLO, enabling flexible integration with various robotics applications.

<img src="https://github.com/Alpaca-zip/ultralytics_ros/assets/84959376/9da7dbbf-5cc0-41bc-be82-d481abbf552a" width="800px">
<img src="https://github.com/Alpaca-zip/ultralytics_ros/assets/84959376/158e7f0c-a823-4425-908d-1b63c11d6e51" width="800px">

## Notice (For melodic)
The default Python version for Ubuntu-18.04 are as follows.
```
$ python --version
Python 2.7.17

$ python3 --version
Python 3.6.9
```
This does not meet the `ultralytics` requirements and should be updated in the following steps.
```
$ sudo apt-get install -y python3.8

$ sudo update-alternatives --install /usr/bin/python3 python3 /usr/bin/python3.8 1
update-alternatives: using /usr/bin/python3.8 to provide /usr/bin/python3 (python3) in auto mode

$ python3 --version
Python 3.8.0
```
Update `pip` with the following command just to be sure.
```
$ python3 -m pip install --upgrade pip
```
Then, you have to resolve the `ModuleNotFoundError` issue.
```
$ cd /usr/lib/python3/dist-packages
$ sudo cp apt_pkg.cpython-36m-x86_64-linux-gnu.so apt_pkg.so
$ sudo ln -s apt_pkg.cpython-{36m,38m}-x86_64-linux-gnu.so
$ python3 -m pip install rospkg
```
## Setup
```
$ cd ~/catkin_ws/src
$ git clone -b melodic-devel https://github.com/Alpaca-zip/ultralytics_ros.git
$ python3 -m pip install -r ultralytics_ros/requirements.txt
$ cd ~/catkin_ws
$ rosdep install -r -y -i --from-paths .
$ catkin build
```
## Usage
### Run predict_node
```
$ roslaunch ultralytics_ros predict.launch debug:=true
```
### Run predict_node & predict_with_cloud_node
```
$ roslaunch ultralytics_ros predict_with_cloud.launch debug:=true
```
### Params
- predict_node
  - `yolo_model`: Pre-trained Weights.  
  For yolov8, you can choose `yolov8n.pt`, `yolov8s.pt`, `yolov8m.pt`, `yolov8l.pt`, `yolov8x.pt`.  
  See also: https://docs.ultralytics.com/models/
  - `image_topic`: Topic name for image. (sensor_msgs/Image)
  - `detection_topic`: Topic name for 2D bounding box. (vision_msgs/Detection2DArray)
  - `conf_thres`: Confidence threshold below which boxes will be filtered out.
  - `iou_thres`: IoU threshold below which boxes will be filtered out during NMS.
  - `max_det`: Maximum number of boxes to keep after NMS.
  - `classes`: List of class indices to consider.  
  See also: https://github.com/ultralytics/ultralytics/blob/main/ultralytics/datasets/coco128.yaml 
  - `debug`:  If true, run simple viewer.
  - `debug_conf`:  Whether to plot the detection confidence score.
  - `debug_line_width`: Line width of the bounding boxes.
  - `debug_font_size`: Font size of the text.
  - `debug_labels`: Font to use for the text.
  - `debug_font`: Whether to plot the label of bounding boxes.
  - `debug_boxes`: Whether to plot the bounding boxes.

- predict_with_cloud_node
  - `camera_info_topic`: Topic name for camera_info. (sensor_msgs/CameraInfo)
  - `lidar_topic`: Topic name for lidar. (sensor_msgs/PointCloud2)
  - `detection3d_topic`: Topic name for 3D bounding box. (vision_msgs/Detection3DArray)
  - `cluster_tolerance`: Spatial cluster tolerance as a measure in the L2 Euclidean space.
  - `min_cluster_size`: Minimum number of points that a cluster needs to contain.
  - `max_cluster_size`: Maximum number of points that a cluster needs to contain.

## Docker with KITTI datasets 🐳
Release soon.
