# DWCV 农民身份识别挑战赛

> https://challenge.xfyun.cn/topic/info?type=peasant-identity&ch=ymfk4uU

## **赛题背景**

机器虽然被大量用到农业生产中，但人还是不可或缺的因素。通过农民身份识别，可以真实客观地记录农民的状态，为农场管理和农产品追溯提供真实的客观数据；较之直接存储视频，可以有效地降低存储空间；自动识别也比人工监管，大幅度提高效率，减少人工成本。

## **赛事任务**

农民身份识别需要对农民进行分类，本次大赛提供了中国农业大学实验室视频制成的图像序列。参赛选手先对图像进行预处理，并制作样本，对图像中的农民进行识别。选手需要自行训练模型，并上传自己训练好的模型和权重。

## **赛题数据集**

本次比赛为参赛选手提供了25名农民身份，每个身份包含10段视频制成的图像序列，选手需要对图像序列进行预处理，打标签，并对农民进行身份识别。

## **评价指标**

本模型依据提交的结果文件，采用Macro-F1进行评价。

![image-20230806165838412](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230806165838412.png)

## 解题思路

本题的任务是对图像进行预处理，并制作样本，对图像中的农民进行识别，是典型的**图像分类问题**。

- 输入：带标签的25类农民的图片
- 输出：图像类别

+ 模型：卷积神经网络 ResNet18

### 1. Baseline

+ 跑通Baseline代码

+ 提交结果：0.26558

  ![image-20230806170323335](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230806170323335.png)

### 2. Baseline调参

+ 阅读baseline代码：源码只训练了1个epoch

+ 调整batchsize（32），epoch（20），重新训练

+ 训练结果：loss 0.322, train_acc 0.868, val_acc 0.862

  ![image-20230806173121616](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230806173121616.png)

+ 提交结果：0.58331

  ![image-20230806173540228](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230806173540228.png)

### 3. optimizer+数据增强

#### 3.1 优化optimizer的参数

+ 优化器改为adam，使用先线性增长再余弦下降的lr_scheduler

#### 3.2 数据探索

+ 标签是否分布平衡
  + 不平衡，有的类别少则六七百，而最多的类别有1578个样本。
+ 进行数据增强
  + 将每个类别都进行数据增强，包括对比度+亮度增强、水平翻转、旋转30度等
  + 使各类别的样本数一致，都为1578

#### 3.3 macro-f1

![image-20230809221751031](C:\Users\ASUS\AppData\Roaming\Typora\typora-user-images\image-20230809221751031.png)