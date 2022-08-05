# YOLOv5Face部署示例

当前支持模型版本为：[YOLOv5Face CommitID:4fd1ead](https://github.com/deepcam-cn/yolov5-face/commit/4fd1ead)

本文档说明如何进行[YOLOv5Face](https://github.com/deepcam-cn/yolov5-face)的快速部署推理。本目录结构如下

```
.
├── cpp                     # C++ 代码目录
│   ├── CMakeLists.txt      # C++ 代码编译CMakeLists文件
│   ├── README.md           # C++ 代码编译部署文档
│   └── yolov5face.cc       # C++ 示例代码
├── api.md                  # API 说明文档
├── README.md               # YOLOv5Face 部署文档
└── yolov5face.py           # Python示例代码
```

## 获取ONNX文件

访问[YOLOv5Face](https://github.com/deepcam-cn/yolov5-face)官方github库，按照指引下载安装，下载`yolov5s-face.pt` 模型，利用 `export.py` 得到`onnx`格式文件。

* 下载yolov5face模型文件
  ```
  Link: https://pan.baidu.com/s/1fyzLxZYx7Ja1_PCIWRhxbw Link: eq0q  
  https://drive.google.com/file/d/1zxaHeLDyID9YU4-hqK7KNepXIwbTkRIO/view?usp=sharing
  ```

* 导出onnx格式文件
  ```bash
  PYTHONPATH=. python export.py --weights weights/yolov5s-face.pt --img_size 640 640 --batch_size 1  
  ```
* onnx模型简化(可选)
  ```bash
  onnxsim yolov5s-face.onnx yolov5s-face.onnx
  ```
* 移动onnx文件到model_zoo/yolov5face的目录
  ```bash
  cp PATH/TO/yolov5s-face.onnx PATH/TO/model_zoo/vision/yolov5face/
  ```



## 准备测试图片
准备一张包含人脸的测试图片，命名为test.jpg，并拷贝到可执行文件所在的目录

## 安装FastDeploy

使用如下命令安装FastDeploy，注意到此处安装的是`vision-cpu`，也可根据需求安装`vision-gpu`
```bash
# 安装fastdeploy-python工具
pip install fastdeploy-python

# 安装vision-cpu模块
fastdeploy install vision-cpu
```

## Python部署

执行如下代码即会自动下载YOLOv5Face模型和测试图片
```bash
python yolov5face.py
```

执行完成后会将可视化结果保存在本地`vis_result.jpg`，同时输出检测结果如下
```
FaceDetectionResult: [xmin, ymin, xmax, ymax, score, (x, y) x 5]
749.575256,375.122162, 775.008850, 407.858215, 0.851824, (756.933838,388.423157), (767.810974,387.932922), (762.617065,394.212341), (758.053101,399.073639), (767.370300,398.769470)
897.833862,380.372864, 924.725281, 409.566803, 0.847505, (903.757202,390.221741), (914.575867,389.495911), (908.998901,395.983307), (905.803223,400.871429), (914.674438,400.268066)
281.558197,367.739349, 305.474701, 397.860535, 0.840915, (287.018768,379.771088), (297.285004,378.755280), (292.057831,385.207367), (289.110962,390.010437), (297.535339,389.412048)
132.922104,368.507263, 159.098541, 402.777283, 0.840232, (140.632492,382.361633), (151.900864,380.966156), (146.869186,388.505066), (141.930420,393.724670), (151.734604,392.808197)
699.379700,306.743256, 723.219421, 336.533295, 0.840228, (705.688843,319.133301), (715.784668,318.449524), (711.107300,324.416016), (707.236633,328.671936), (716.088623,328.151794)
# ...
```

## 其它文档

- [C++部署](./cpp/README.md)
- [YOLOv5Face API文档](./api.md)