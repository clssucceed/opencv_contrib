# 1. 如何编译
## 1.1 如何配置cmake
* 使用cmake-gui进行配置
* opencv官网: https://docs.opencv.org/trunk/d7/d9f/tutorial_linux_install.html
* opencv_contrib git readme: https://github.com/clssucceed/opencv_contrib
### 1.1.1 如何配置安装目录 
* CMAKE_INSTALL_PREFIX
### 1.1.2 如何使能所有功能(viz等)
* https://stackoverflow.com/questions/23559925/how-do-i-use-install-viz-in-opencv
* https://answers.opencv.org/question/32502/opencv-249-viz-module-not-there/?answer=32847#post-id-32847
* 使能viz要安装vtk,安装vtk需要注意的是: 
* 安装vtk需要同时下载VTK和VTKDATA(https://vtk.org/download/),并且将VTKDTA中的
.EXTERNALDATA文件夹放到VTK的目录下，这样就可以避免通过git从网络下载依赖文件。
* 如何配置opencv中使用的vtk的路径： \
set WITH_VTK= On in CMake-Gui \
set VTK_DIR = path-to-build-directory-of-VTK \
### 1.1.3 如何加入contrib一起编译
* opencv_contrib git readme: https://github.com/clssucceed/opencv_contrib
# 2. 如何使用
## 2.1 如何通过find_package正确找到OpenCV
### 2.2.1 find_package基础理论:
* 主要关注OpenCV_INCLUDE_DIRS和OpenCV_LIBS即可 
* https://zhuanlan.zhihu.com/p/50829542
### 2.2.2 如何设置find_package在哪些目录下查找OpenCV: 
* set(OpenCV_DIR /usr/local/opencv_test/lib/cmake/opencv4)
* https://blog.csdn.net/u012816621/article/details/51732932 
* https://zhuanlan.zhihu.com/p/50829542 
* 注： 将opencv安装在build文件夹下没有配置成功，安装目录设置为了/usr/local/opencv_test，所有的文件都会拷贝到该目录下
### 2.2.3 链接动态库有哪些注意事项 
* 主要关注LD_LIBRARY_PATH即可
* 比如export LD_LIBRARY_PATH=/usr/local/cuda/lib64:$LD_LIBRARY_PATH 
* https://zhuanlan.zhihu.com/p/50829542
### 2.2.4 是否可以通过OpenCV_DIR使用自己编译并且没有安装到系统目录的OpenCV 
* 能，具体如下 
``` cmake
message("OpenCV_DIR: ${OpenCV_DIR}") 
message("OpenCV_INCLUDE_DIRS: ${OpenCV_INCLUDE_DIRS}")
message("OpenCV_LIBS: ${OpenCV_LIBS}")
set(OpenCV_DIR /usr/local/opencv_test/lib/cmake/opencv4)
message("OpenCV_DIR: ${OpenCV_DIR}")
find_package(OpenCV REQUIRED)
message("OpenCV_DIR: ${OpenCV_DIR}")
message("OpenCV_INCLUDE_DIRS: ${OpenCV_INCLUDE_DIRS}")
message("OpenCV_LIBS: ${OpenCV_LIBS}")
```
* 注： OpenCV_DIR的路径可以通过sudo find / -name OpenCVConfig.cmake搜索获取
### 2.2.5 如何安装多个版本的opencv以及如何使用指定版本的opencv
1. 通过cmake-gui设置CMAKE_INSTALL_PREFIX, 将不同版本的安装不同的目录下（每个目录下通常会有include, lib, bin, share四个文件夹)
2. cmake中指定使用版本的方式： 
``` cmake
set(OpenCV_DIR path_to_OpenCVConfig.cmake)
find_package(OpenCV4.2.0 REQUIRED)
```
## 2.2 gcc/g++如何找到正确的OpenCV
# 3. 依赖缺失问题的解决记录
## 3.1 fatal error: boostdesc_bgm.i: No such file or directory
解决方案： 
* https://www.cnblogs.com/arxive/p/11778731.html
* 手动下载没有下载下来的依赖文件放到指定目录下
## 3.2 fatal error: features2d/test/test_detectors_invariance.impl.hpp: No such file or directory
解决方案:
* https://github.com/opencv/opencv_contrib/issues/1950中tellw的回答
## 3.3 fatal error: opencv2/xfeatures2d.hpp
解决方案： 
* 使用find命令在源码目录中找到xfeatures2d.hpp的绝对路径，然后将出错地方的相对路径改成绝对路径
* 注： 类似3.2和3.3的问题都是因为contrib官方给的环境配置较为混乱导致的，目前的解决方案都只是临时方案，自己测试用没有问题，实际项目环境不能这么搞 
