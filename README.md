## Insects-identification<br>
#Paddle 上应用YOLOv3进行昆虫目标检测与识别项目<br>

## 查看环境并准备数据<br>

查看当前挂载的数据集目录, 该目录下的变更重启环境后会自动还原<br>
View dataset directory. This directory will be recovered automatically after resetting environment. <br>
!ls /home/aistudio/data<br>

查看工作区文件, 该目录下的变更将会持久保存. 请及时清理不必要的文件, 避免加载过慢.<br>
View personal work directory. All changes under this directory will be kept even after reset. Please clean unnecessary files in time to speed up environment loading.<br>
!ls /home/aistudio/work<br>

将数据解压缩到 /home/aistudio/work目录下面<br>
初次运行时需要将注释取消<br>
!unzip -d /home/aistudio/work /home/aistudio/data/data19638/insects.zip<br>

进入工作目录  /home/aistudio/work<br>
%cd  /home/aistudio/work<br>

查看工作目录下的文件列表<br>
!ls<br>

## 启动训练<br>
通过运行train.py 文件启动训练，训练好的模型参数会保存在/home/aistudio/work目录下。<br>

!python train.py <br>

## 启动评估<br>
通过运行eval.py启动评估，需要制定待评估的图片文件存放路径和需要使用到的模型参数。评估结果会被保存在pred_results.json文件中<br>

在验证集val上评估训练模型，image_dir指向验证集路径，weight_file指向要使用的权重路径。<br>
!python eval.py --image_dir=./insects/val/images --weight_file=./yolo_weight <br>

在测试集test上评估训练模型，image_dir指向测试集集路径，weight_file指向要使用的权重路径。<br>
!python eval.py --image_dir=./insects/test/images --weight_file=./yolo_weight  <br>

## 计算精度指标<br>
通过运行calculate_map.py计算最终精度指标MAP<br>

!python calculate_map.py --anno_dir=./insects/val/annotations/xmls --pred_result=./pred_results.json <br>

## 预测单张图片并可视化预测结果<br>

!python predict.py --image_name=./insects/test/images/2572.jpeg --weight_file=./yolo_weight<br>
预测结果保存在“/home/aistudio/work/output_pic.png"图像中，运行下面的代码进行可视化<br>

可视化检测结果<br>
from PIL import Image<br>
import matplotlib.pyplot as plt<br>

img = Image.open("/home/aistudio/work/output_pic.png")<br>

plt.figure("Object Detection", figsize=(15, 15)) # 图像窗口名称<br>
plt.imshow(img)<br>
plt.axis('off') # 关掉坐标轴为 off<br>
plt.title('Bugs Detestion') # 图像题目<br>
plt.show()<br>

!rm -rf submit.sh
!wget -O submit.sh http://ai-studio-static.bj.bcebos.com/script/submit.sh<br>
!sh submit.sh /home/aistudio/work/pred_results.json 21e351bc4e8449a9a8bc7c7d59bd67d8<br>
