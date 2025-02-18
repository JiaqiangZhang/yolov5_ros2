# YoloV5_ROS2

基于YoloV5的ROS2封装，给定模型文件和相机参数可以直接发布三维空间置进行抓取操作。
![](https://img-blog.csdnimg.cn/592a90f1441f4a3ab4b94891878fbc55.png)

## 1.安装依赖
```
sudo apt update
sudo apt install python3-pip ros-<ros2-distro>-vision-msgs # <ros2-distro>替换为humble，foxy或galactic等ros2发行版
pip3 install -i https://pypi.tuna.tsinghua.edu.cn/simple yolov5
```
## 2.编译运行
```
colcon build
source install/setup.bash
ros2 run yolov5_ros2 yolo_detect_2d --ros-args -p device:=cpu -p image_topic:=/image
```

使用真实相机，修改默认话题`image_topic:=/image`，并在另一个终端中运行如下命令

```
ros2 run image_tools  cam2image --ros-args -p width:=640 -p height:=480 -p frequency:=30.0 -p device_id:=-1
```

也可使用其它相机，如`usb_cam`。参数`image_topic`修改为`image_topic:=/image_raw`
```bash
sudo apt-get install ros-<ros2-distro>-usb-cam # 安装
ros2 run usb_cam usb_cam_node_exe
```

![](https://img-blog.csdnimg.cn/c65bed0b67694ed69776151c203bb950.png)

## 3.订阅结果
识别结果通过/yolo_resutl话题发布出去，包含原始的像素坐标、和归一化后的x和y坐标（相机坐标系下）。

```shell
ros2 topic echo /yolo_result
```
![](https://img-blog.csdnimg.cn/ac963f4226bf497790c0ef2fd8d942a3.png)


## 4.更进一步使用

### 4.1 参数设置

#### 4.1.1 图像话题

```
image_topic:=/image
```
#### 4.1.2 计算设备设置

可选的，如果你有cuda独显， cuda，cuda:0 

```
device:=cpu
```
#### 4.1.3 是否实时显示结果

True显示，False不显示

```
show_result:=False
```
注：实时显示中`cv2.imshow`可能会卡住，如果只想验证结果，可将实时显示设置为`False`。

#### 4.1.4 切换不同Yolov5模型
默认为`yolov5s`，可修改为`yolov5m`或`yolov5l`等
```
model:=yolov5m
```
