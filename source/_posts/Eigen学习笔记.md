（md格式）

### Eigen库简介

C++库、用于线性代数、矩阵和矢量运算，数值分析及其相关的算法。



### 安装

Linux：apt install libeigen3-dev



### CMake声明与使用

```c++
# CMake要求本版信息即项目名称，创建时自动生成
cmake_minimum_required(VERSION 3.12)
project(useEigen)

# 申明采用C++11标准，项目创建时自动生成
set(CMAKE_CXX_STANDARD 11)

# 寻找Eigen库
find_package(Eigen3 REQUIRED)
# 将Eigen库include进来
include_directories(${EIGEN3_INCLUDE_DIRS})

add_executable(${PROJECT_NAME} main.cpp)
```



### 代码中头文件写法

`#include <Eigen/Dense>`

`using namespace Eigen;`



### 矩阵

1. 创建

   ```
   Matrix<typename Scalar, int RowsAtCompileTime, int ColsAtCompileTime>
   ```

   参数分别为类型、行数、列数。

   ```
   Matrix<double, 5, 3> m1;
   Matrix<float,5,5> m2;
   Matrix<int,8,9> m3;
   ```

   或者采用预定义的矩阵类型

   ```
   Matrix3d m1;
   Matrix3f m2;
   Matrix3i m3;
   MatrixXd m4(a,b);
   ```

   分别指3*3的double、float、int、.MatriXd指大小不确定的double型，可用通过变量确定大小

2. 初始化

   ```
   Matrix3d m;
       m << 1, 2, 3,
               4, 5, 6,
               7, 8, 9;
   ```

   

3. 获取元素

   m(4,7)

4. 常用方法

   ```
   m.rows() 
   
   m.cols()
   
   m.size()    #获取元素个数
   
   m.data()    #获取指向矩阵或向量的首地址
   
   MatrixXd::Random(m,n)    #创建m×n维double类型的随机数矩阵
   
   MatrixXd::Constant(m,n,p)    #创建m×n维double类型元素全为p的矩阵
   
   MatrixXd::Zero(m,n)    #创建m×n维元素全为0的矩阵
   
   MatrixXd::Ones(m,n)    #创建m×n维元素全为1的矩阵
   
   MatrixXd::Identity(m,n)    #创建m×n维的单位阵
   
   VectorXd::LinSpaced(size,low,high)    #创建一个size长度的从low到high的向量或一维矩阵
   ```

   

5. 加减乘

   `+ - *  #对运算符进行了重载`

6. 其他运算

   ```
   mat.transpose()：转置矩阵。对于矩阵转置，注意不要写成a = a.transpose()，这会导致错误结果(Aliasing Issue)，如果一定需要对原矩阵进行修改，使用a.transposeInPlace()函数。
   
   mat.inverse()：逆矩阵
   
   mat.conjugate()：共轭矩阵
   
   mat.adjoint()：伴随矩阵
   
   mat.trace()：矩阵的迹
   
   mat.eigenvalues()：矩阵的特征值
   
   mat.determinant()：矩阵求行列式的值
   
   mat.diagonal()：矩阵对角线元素
   
   mat.sum()：矩阵所有元素求和
   
   mat.prod()：矩阵所有元素求积
   
   mat.mean()：矩阵所有元素求平均
   
   mat.minCoeff()：矩阵所有元素最小值
   
   mat.minCoeff(&i,&j)：矩阵所有元素最小值的位置，i、j为int类型或为Eigen的Index类型。
   
   mat.maxCoeff()：矩阵所有元素最大值
   
   mat.maxCoeff(&i,&j)：矩阵所有元素最大值的位置
   
   mat.nonZeros()：矩阵中非零元素个数
   
   mat.squaredNorm()：矩阵(向量)的平方范数，对向量而言等价于其与自身做点积，数值上等于各分量的平方和。
   
   mat.norm()：矩阵(向量)的平方范数开根号(对于向量即求模长)
   
   mat.lpNorm<1>()：矩阵(向量)的L1范数
   
   mat.lpNorm<2>()：矩阵(向量)的L2范数
   
   mat.lpNorm<Infinity>()：矩阵(向量)的L无穷范数
   
   mat.lpNorm<p>()：矩阵(向量)的Lp范数
   
   mat.normalize()：矩阵(向量)的正则化(归一化)，使所有元素的平方和等于1。
   
   (mat>0).all()：矩阵元素条件判断，mat中所有元素是否都大于0，是返回1，否则返回0。
   
   (mat>0).any()：矩阵元素条件判断，mat中所有元素是否有大于0的，有返回1，否则返回0。
   
   (mat>0).count()：矩阵符合条件的元素计数，返回mat中大于0元素的个数。
   
   mat.colwise()：返回矩阵每列的值
   
   mat.rowwise()：返回矩阵每行的值
   ```

   