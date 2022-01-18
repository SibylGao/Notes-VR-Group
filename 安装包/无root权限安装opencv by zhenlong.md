**由于需要跑项目论文Massively Parallel Multiview Stereopsis by Surface Normal Diffusion对应的源码：**
kysucix/gipuma: Massively Parallel Multiview Stereopsis by Surface Normal Diffusion (github.com)
该项目需要在服务器安装Cmake和opencv，由于服务器已经安装Cmake，因此下面将我安装opencv的过程记录如下：

之前只git clone opencv文件而后解压是不能在非root用户下进行的，需要配合opencv_contrib同时在cmake配置部分改内容

借鉴下面的博客：
(35条消息) Linux下安装opencv（root身份和非root普通用户安装）_KyrieLiu52的博客-CSDN博客


opencv: https://github.com/opencv/opencv/releases
opencv_contrib: https://github.com/opencv/opencv_contrib/tags

注意：非root用户就是这里需要下两个opencv文件才能在后面不报错
tar -zxvf opencv-4.2.0.tar.gz
tar -zxvf opencv_contirb-4.2.0.tar.gz

解压后得到两个文件夹：opencv-4.2.0 和 opencv_contirb-4.2.0。
将解压后的opencv_contrib-4.2.0 文件夹整体移动到opencv-4.2.0目录下

cd opencv-4.2.0
sudo mkdir build
cd build

vim cmake_build.sh
在脚本中写入：
#!/bin/bash
cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/your_path/opencv-4.2.0 -D WITH_TBB=ON -D WITH_V4L=ON -D BUILD_TIFF=ON -D BUILD_EXAMPLES=ON -D WITH_OPENGL=ON -D WITH_EIGEN=ON -D WITH_CUDA=OFF -D WITH_CUBLAS=ON -D OPENCV_GENERATE_PKGCONFIG=ON -D OPENCV_EXTRA_MODULES_PATH=/your_path/opencv-4.2.0/opencv_contrib-4.2.0/modules/ ..
其中your_path需要改对应文件位置，这里注意：非root用户就是这里需要改才能在下一个环节不报错

而后chmod 777 cmake_build.sh
./cmake_build.sh

make -j32
make install

最后添加环境变量
vim /home/your_username/.bashrc  (服务器上是/home/yuanzhenlong/.bashrc)
在最后加两行：
export PKG_CONFIG_PATH=/your_path/opencv4.0.1/lib/pkgconfig
export LD_LIBRARY_PATH=/your_path/opencv4.0.1/lib
注意：你就是这里改/your_path/opencv4.0.1/只改成了/mnt/sharedisk/yzl/File/opencv4.2.0/但是应该改成/opencv0-4.2.0/，要注意细节！！！！
注意：非root用户就是在这里只需要改环境变量，root用户的流程改环境变量比这个麻烦

最后输入pkg-config --modversion opencv4
输出4.2.0则代表实现

此时source为：
export PKG_CONFIG_PATH=/mnt/sharedisk/yzl/File/opencv-4.2.0/lib/pkgconfig
export LD_LIBRARY_PATH=/mnt/sharedisk/yzl/File/opencv-4.2.0/lib