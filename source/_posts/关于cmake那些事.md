

### 1.minGW

--是指mini gnu for windows

​			包含-- gcc -v

​			-- g++ -v  编译器

​			g++编译器的使用	g++ main.cpp -o main

​				main.exe

### 2.MSVC 

MSVC，就是微软（MS）的VC运行库。

 

　　VC运行库，是Visual C++的运行库。很多程序在编制的时候，使用了微软的运行库，大大减少了软件的编码量，却提高了兼容性。但运行的时候，需要这些运行库。这些运行库简称就是MSVC。

　　运行库的版本很多，一般都要装，比如2003、2005、2010等，另外还有32位和64位的区别


Windows 下，CMake 默认使用微软的 MSVC 作为编译器

# catkin_make

### 3.只编译指定的包

catkin_make -DCATKIN_WHITELIST_PACKAGES="package_name1;package_name2"
1
注意不是src下的文件夹名字，而是文件夹下package.xml里面的包的名字，或者CMaklists.txt的项目的名字。不然会找不到，导致不会编译。

### 不编译某些包
不编译某个功能包：
在功能包下面建立文件夹CATKIN_IGNORE