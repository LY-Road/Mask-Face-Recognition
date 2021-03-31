# Mask-Face-Recognition：口罩人脸识别
● 该项目主要是实现人脸特征向量的提取。以标准人脸识别模型FaceNet 为主线，添加fpn_face_attention结构，增加CBAM模块，使其能更好的聚焦于人脸上半部，没带口罩的区域

### 整理数据集
正常人脸训练数据：VGGFace2，链接：http://www.robots.ox.ac.uk/~vgg/data/vgg_face2/  
正常人脸测试数据：LFW(Labeled Faces in the Wild)，链接：http://vis-www.cs.umass.edu/lfw/lfw-funneled.tgz  
pairs.txt：http://vis-www.cs.umass.edu/lfw/pairs.txt （pairs.txt文件是图片对文件，含有测试的图片对，以及标注）  

首先生成一个Datasets文件夹，把VGGFace2的原始数据(VGGFace2_train文件)、LFW原始数据(lfw_funneled)、LFW配对文件(LFW_pairs.txt)，都放到Datasets文件夹，并解压，VGGface2的训练集是用做训练集的，LFW是用做测试集的。
    
### 下载预训练模型
V1对应网络模型：https://pan.baidu.com/s/1rCzbNAKC1ZqHCCP0PGm8_Q 提取码：qkeu  
V2对应网络模型：https://pan.baidu.com/s/1aSsDTSDyZKvXWyPmekyeMA 提取码：cgfu  
V3对应网络模型：https://pan.baidu.com/s/1SveUwV5vuUcfgXHcIBYH1Q 提取码: n5g3  
V6对应网络模型：https://pan.baidu.com/s/11NnoNpG1oQsWlre9Wdl_Tw 提取码：xe3r  
V8对应网络模型：https://pan.baidu.com/s/17We4cUWyi3Iy0qHWY4Yhzg 提取码：gxk7  
V9对应网络模型：https://pan.baidu.com/s/1OXjlwnd45rgvEJhIjCBwjw 提取码：udp4  
### 不戴口罩数据预处理
◆ 找到Data_preprocessing/Image_processing.py，设置notmasked=True，masked=False，然后定义输入data路径，执行如下命令：  
> $ cd Data_preprocessing/    
> $ python3 Image_processing.py   

进行的主要操作为不戴口罩切人脸/对齐/生成mask(把图像中的人脸切下来，做2轴对齐，再根据68人脸特征点生成Attention模块用的mask图)：其中的‘shape_predictor_68_face_landmarks.dat’ ，是68人脸特征点预测模型，官方下载地址（http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2）。

◆ 生成数据读取的csv文件：    
> $ cd Data_preprocessing/  
> $ python3 Make_csv_file_notmask.py  

保存的csv格式形如：序号，图片名称，人名。
### 戴口罩数据预处理
◆ 找到Data_preprocessing/Image_processing.py，设置notmasked=False，masked=True，然后定义输入data路径，执行如下命令：  
> $ cd Data_preprocessing/    
> $ python3 Image_processing.py   

进行的主要操作为戴口罩切人脸/对齐/生成mask(把图像中的人脸切下来，做2轴对齐，再根据68人脸特征点生成Attention模块用的mask图)：其中的‘shape_predictor_68_face_landmarks.dat’ ，是68人脸特征点预测模型，官方下载地址：http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2

◆ 生成数据读取的csv文件：    
> $ cd Data_preprocessing/  
> $ python3 Make_csv_file_mask.py  

保存的csv格式形如：序号，图片名称，人名。

### 关于配置文件
不戴口罩的修改config_notmask.py，戴口罩的修改config_mask.py文件

### 关于代码
文件名有_notmask后缀的是不戴口罩的代码，文件名有_mask后缀的是戴口罩的代码

### 训练
戴口罩V1运行: 
> $ python3 train_maskV1.py  

不戴口罩V3运行:  
> $ python3 train_notmaskV3.py  

戴口罩V9运行:   
> $ python3 train_maskV9.py

训练最开始的时候会生成triplets图片对，生成的图片对会保存起来，以便之后使用。

### 训练过程中模型验证
验证测试过程中会采用戴口罩的LFW数据和不戴口罩的LFW数据，会打印输出相应的AUC结果，其中evaluate_lfw函数中的参数pltshow，用来控制是否保存ROC曲线图，ROC曲线图保存在ROC_images文件夹里面
模型是保存在Model_training_checkpoints文件夹里面，日志文件保存在logs文件夹里面。

#### 使用LFW数据集验证测试集AUC结果
执行如下代码：
> $ python3 validation_LFW.py  

测试结果包含AUC、Accuray和最佳距离指标等，测试集相同但输入网络有两种图，一种是戴口罩的LFW人脸图片，一种是不戴口罩的LFW人脸图片。

#### 非LFW数据集生成LFW格式的pairs.txt文件
执行如下代码：
> $ python3 create_pairs.py

####  使用生成的paris.txt文件验证测试非LFW数据集
执行如下代码：
> $ python3 validation_NOTLFW.py
