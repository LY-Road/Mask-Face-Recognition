# Mask-Face-Recognition：口罩人脸识别
● 该项目主要是实现人脸特征向量的提取。以标准人脸识别模型FaceNet 为主线，添加fpn_face_attention结构，增加CBAM模块，使其能更好的聚焦于人脸上半部，没带口罩的区域

### 整理数据集
正常人脸训练数据：VGGFace2，链接：http://www.robots.ox.ac.uk/~vgg/data/vgg_face2/
正常人脸测试数据：LFW(Labeled Faces in the Wild)，链接：http://vis-www.cs.umass.edu/lfw/
LFW数据集下载的链接是https://share.weiyun.com/qHg5TcPP 
首先生成一个Datasets文件夹，把VGGFace2的原始数据(VGGFace2_train文件)、LFW原始数据(lfw_funneled)、LFW配对文件(LFW_pairs.txt)，都放到Datasets文件夹，并解压，VGGface2的训练集是用做训练集的，LFW是用做测试集的。
### 下载预训练模型
### 不戴口罩数据预处理
不戴口罩切人脸/对齐/生成mask(把图像中的人脸切下来，做2轴对齐，再根据68人脸特征点生成Attention模块用的mask图)：设置notmasked=True,masked=False,然后定义输入data路径,
> $ cd Data_preprocessing/
> $ python3 Image_processing.py  
其中的‘shape_predictor_68_face_landmarks.dat’ ，是68人脸特征点预测模型，官方下载地址（http://dlib.net/files/shape_predictor_68_face_landmarks.dat.bz2）（处理VGGface2大约要一天时间）
生成csv文件：  
使用Data_preprocessing/Make_csv_file_notmask.py，输入数据文件夹路径、输出csv文件路径，然后就能跑了，保存的csv格式形如：序号，图片名称，人名
### 戴口罩数据预处理


### 关于代码
注：文件名有_notmask后缀的是不戴口罩的代码，文件名有_mask后缀的是戴口罩的代码
