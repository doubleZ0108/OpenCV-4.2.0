# Install OpenCV-4.2.0 all-in-one

1. **Install dependencies**

   ```shell
   sudo apt-get install build-essential
   sudo apt-get install cmake git libgtk2.0-dev pkg-config libavcodec-dev libavformat-dev libswscale-dev
   sudo apt-get install python-dev python-numpy libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libjasper-dev libdc1394-22-dev 
   sudo apt-get install libavcodec-dev libavformat-dev libswscale-dev libv4l-dev liblapacke-dev
   sudo apt-get install libxvidcore-dev libx264-dev
   sudo apt-get install libatlas-base-dev gfortran 
   sudo apt-get install ffmpeg
   ```

2. **Clone & Compile**(change the absolute path of your `OpenCV-4.2.0/` in line #3)

   ```shell
   git clone https://github.com/doubleZ0108/OpenCV-4.2.0.git && cd OpenCV-4.2.0
   
   sudo mkdir build && cd build
   
   sudo cmake -D CMAKE_BUILD_TYPE=Release -D CMAKE_INSTALL_PREFIX=/usr/local -D OPENCV_EXTRA_MODULES_PATH=[/path/to]/OpenCV-4.2.0/opencv_contrib-4.2.0/modules/ ..
   
   sudo make -j${nproc}
   
   sudo make install
   ```

3. **Configure the path**

   1. `vim /etc/ld.so.conf.d/opencv.conf` and add `/usr/local/lib` in the bottom of the file

   2. `ldconfig` to finish the change 

   3. `vim /etc/bash.bashrc` and add `export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig` in the bottom of the file

   4. `source /etc/bash.bashrc` to update bash

   5. create `opencv.pc`

      ```shell
      cd /usr/local/lib
      sudo mkdir pkgconfig
      cd pkgconfig
      sudo vim opencv.pc
      ```

      and add the following code in it:

      ```shell
      prefix=/usr/local
      exec_prefix=${prefix}
      includedir=${prefix}/include
      libdir=${exec_prefix}/lib
      
      Name: opencv
      Description: The opencv library
      Version:4.2.0
      Cflags: -I${includedir}/opencv4
      Libs: -L${libdir} -lopencv_shape -lopencv_stitching -lopencv_objdetect -lopencv_superres -lopencv_videostab -lopencv_calib3d -lopencv_features2d -lopencv_highgui -lopencv_videoio -lopencv_imgcodecs -lopencv_video -lopencv_photo -lopencv_ml -lopencv_imgproc -lopencv_flann  -lopencv_core
      ~
      ```

      then export the environment parameter

      ```shell
      export  PKG_CONFIG_PATH=/usr/local/lib/pkgconfig
      ```

4. **Test** if installed successfully

   ```shell
   pkg-config opencv --modversion
   ```


> You can find more useful information [here(in Chinese)](https://zhuanlan.zhihu.com/p/368573848) if you face some issues don't mention above.
