## Insects-identification
#Paddle 上应用YOLOv3进行昆虫目标检测与识别项目

## 查看环境并准备数据<br>

查看当前挂载的数据集目录, 该目录下的变更重启环境后会自动还原
View dataset directory. This directory will be recovered automatically after resetting environment. 
!ls /home/aistudio/data

查看工作区文件, 该目录下的变更将会持久保存. 请及时清理不必要的文件, 避免加载过慢.
View personal work directory. All changes under this directory will be kept even after reset. Please clean unnecessary files in time to speed up environment loading.
!ls /home/aistudio/work

将数据解压缩到 /home/aistudio/work目录下面
初次运行时需要将注释取消
!unzip -d /home/aistudio/work /home/aistudio/data/data19638/insects.zip

进入工作目录  /home/aistudio/work
%cd  /home/aistudio/work

查看工作目录下的文件列表
!ls

## 启动训练
通过运行train.py 文件启动训练，训练好的模型参数会保存在/home/aistudio/work目录下。

!python train.py    

## 启动评估
通过运行eval.py启动评估，需要制定待评估的图片文件存放路径和需要使用到的模型参数。评估结果会被保存在pred_results.json文件中

在验证集val上评估训练模型，image_dir指向验证集路径，weight_file指向要使用的权重路径。
!python eval.py --image_dir=./insects/val/images --weight_file=./yolo_weight 

在测试集test上评估训练模型，image_dir指向测试集集路径，weight_file指向要使用的权重路径。
!python eval.py --image_dir=./insects/test/images --weight_file=./yolo_weight  

## 计算精度指标
通过运行calculate_map.py计算最终精度指标MAP

!python calculate_map.py --anno_dir=./insects/val/annotations/xmls --pred_result=./pred_results.json 

## 预测单张图片并可视化预测结果

!python predict.py --image_name=./insects/test/images/2572.jpeg --weight_file=./425+0.2_yolo_epoch199
预测结果保存在“/home/aistudio/work/output_pic.png"图像中，运行下面的代码进行可视化

可视化检测结果
from PIL import Image
import matplotlib.pyplot as plt

img = Image.open("/home/aistudio/work/output_pic.png")

plt.figure("Object Detection", figsize=(15, 15)) # 图像窗口名称
plt.imshow(img)
plt.axis('off') # 关掉坐标轴为 off
plt.title('Bugs Detestion') # 图像题目
plt.show()

!rm -rf submit.sh
!wget -O submit.sh http://ai-studio-static.bj.bcebos.com/script/submit.sh
!sh submit.sh /home/aistudio/work/pred_results.json 21e351bc4e8449a9a8bc7c7d59bd67d8
