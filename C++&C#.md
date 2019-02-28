# 第一节课

## overloading(重载机制)

1. 相同函数名,有不同的参数列表的函数可以共存

   ```C++
   void add(int a, int b);
   void add(int a)
   ```

2. **不能通过返回值类型来区分同名函数**

## 宏(define)

1. 宏的本质是在编译的时候进行替换

2. 宏的类型

   1. 常量宏:常用常量赋予意义

      `define PI 3.1415`

   2. 函数宏:实现简单的函数功能

      `define ADD(a, b) (a + b)`

   3. 控制宏:开关

      ```C++
      #define REMOTE_VER
      int main(){
          #ifdef RENOTE_VER
          	cout << "connect oracle" << "\n";
          #else
          	cout << "connext mysql" << "\n";
          #endif
      }
      ```

      在上述代码中有第一行的define时输出"connect oracle"

      没有第一行的define时输出"connect mysql"

      **这种技巧可以用在区分debug模式和实际功能模式**

## 缺省参数

1. 参数列表中的参数值可以在定义时设置为默认值在调用时可以选择不传递而使用默认值

2. **设置为缺省值的参数必须在参数列表的最后面(即先声明未设置默认的参数)**

   `int add(int a, int b, int c = 10, int b = 4)`

3. 一般在C++项目中.h文件中会声明函数(不进行实现),宏定义,全局变量,结构定义等

   一般设置缺省参数值在.h中的声明时实现,**此时不能再在.cpp文件中实现时重复声明,一般通过注释提示**

   ```c++
   // 在.h头文件中
   int add(int a, int b = 10);	// 只声明函数而不进行具体实现
   // 在.cpp文件中
   int add(int a, int b/* =10 */){
       // 具体实现
   }
   ```

   

## 占位

类似于`void add(int)`这种函数的定义是合法的,在传入参数时也必须传入一个int值,但是这个值一定无法被使用

## 头文件结构

1. 避免在一个文件中对同一个头文件进行重复包含,**因为同文件之间也可能相互包含,所以这种情况很容易发生**

   所以在编写头文件时有标准的结构要求

   ```c++
   #ifndef HEADFILE_NAME	// 一般与头文件名字保持一致
   #define HEADFILE_NAME
   // 具体内容
   #endif
   ```

   ---

   

   

