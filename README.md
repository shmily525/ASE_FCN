# ASE_FCN
高级软件工程项目

## 数据集
* 数据集解压缩将其放在fcn项目的dataset文件夹下(需手动生成这一文件夹)

## 该项目主要是来自pytorch官方torchvision模块中的源码
* https://github.com/pytorch/vision/tree/main/torchvision/models/segmentation

## 环境配置：
* Python3.6/3.7/3.8
* Pytorch1.10
* (Windows暂不支持多GPU训练)
* 最好使用GPU训练
* 详细环境配置见```requirements.txt```

## 文件结构：
```
  ├── src: 模型的backbone以及FCN的搭建
  ├── testPhoto: 预测图片
  ├── train_utils: 训练、验证以及多GPU训练相关模块
  ├── transDataset: 将Sift-flow数据集转换为类VOC格式数据集进行train
  ├── my_dataset.py: 自定义dataset用于读取数据集
  ├── train.py: 以fcn_resnet50(这里使用了Dilated/Atrous Convolution)进行训练
  ├── train_multi_GPU.py: 针对使用多GPU的用户使用
  ├── predict.py: 简易的预测脚本，使用训练好的权重进行预测测试
  ├── validation.py: 利用训练好的权重验证/测试数据的mIoU等指标，并生成record_mAP.txt文件
  └── pascal_voc_classes.json: pascal_voc标签文件
  
```

## 预训练权重下载地址：
* 注意：官方提供的预训练权重是在COCO上预训练得到的，训练时只针对和PASCAL VOC相同的类别进行了训练，所以类别数是21(包括背景)
* fcn_resnet50: https://download.pytorch.org/models/fcn_resnet50_coco-1167a1af.pth
* fcn_resnet101: https://download.pytorch.org/models/fcn_resnet101_coco-7ecb50ca.pth
* 注意，下载的预训练权重记得要重命名，比如在train.py中读取的是```fcn_resnet50_coco.pth```文件，
  不是```fcn_resnet50_coco-1167a1af.pth```
 
 
## 数据集，原项目使用的是PASCAL VOC2012数据集
* Pascal VOC2012 train/val数据集下载地址：http://host.robots.ox.ac.uk/pascal/VOC/voc2012/VOCtrainval_11-May-2012.tar
* 我在本实验所做改动是选择使用Sift-flow数据集，其有34个类别（包括背景）

## 训练方法
* 确保提前准备好数据集
* 确保提前下载好对应预训练模型权重
* 若要使用单GPU或者CPU训练，直接使用trainsf.py训练脚本(训练自己的数据集)


## 注意事项
* 在使用训练脚本时，注意要将'--data-path'(VOC_root)设置为自己存放'数据集'文件夹所在的**根目录**
* 在使用预测脚本时，要将'weights_path'设置为你自己生成的权重路径。
* 使用validationsf.py文件时，注意确保你的验证集或者测试集中必须包含每个类别的目标，并且使用时只需要修改'--num-classes'、'--aux'、'--data-path'和'--weights'即可，其他代码尽量不要改动
* 使用predict.py文件时，注意 model = fcn_resnet50(aux=aux, num_classes=33+1)里的'num_classes'改为自己数据集的类别加上背景
* 文件夹中的'运行结果.txt'存储为原VOC数据集4轮训练结果，'运行结果2.txt'存储为第一次训练完30轮epoch的Sift-flow数据集结果，而'results20240513-0454.txt'存储第二次训练结果，实验分析时的correct以第一轮为依据，而分割图片以第二轮训练后的模型为依据

##
* 数据集位于fcn项目下的dataset文件夹
* 第二次运行自己数据集30轮训练的结果位于results20240513-0454.txt文件夹下
