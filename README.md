# Mask-Face-Recognition：口罩人脸识别
● 该项目主要是实现人脸特征向量的提取。以标准人脸识别模型FaceNet 为主线，添加fpn_face_attention结构，增加CBAM模块，使其能更好的聚焦于人脸上半部，没带口罩的区域

### 数据集获取
正常人脸训练数据：VGGFace2，链接：http://www.robots.ox.ac.uk/~vgg/data/vgg_face2/
正常人脸测试数据：LFW(Labeled Faces in the Wild)，链接：http://vis-www.cs.umass.edu/lfw/
LFW数据集下载的链接是https://share.weiyun.com/qHg5TcPP ，放入Datasets文件夹
