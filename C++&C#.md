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


# 第二节课

## C与C++混合编译

工程时生成可执行文件时先编译,再链接

LNK型错误时链接错误 : 链接时依赖的是经过编译修饰之后的函数名,C++函数的修饰和C函数的修饰方法不同,所以在C++中直接调用C函数会发生链接错误,使用之下的语句在C++文件中声明避免

**核心时通过`.h`的条件编译实现**

`extern ‘C’ 函数名`

在C++中兼容C头文件的原理:`.h`文件的规范:

```C++
#ifdef __cplusplus
extend "C" {
#endif
函数声明;
}
```

## 库文件

通过编译生成,可供人根据`.h`中的定义调用函数,而避免源代码的泄露

* 动态库:`.dll`文件

* 静态库: `.lib`文件

  **区别在于是否参与原程序的生成,动态库不会包含,静态库要包含,即静态库会包含在生成的可执行文件,而动态库不会,程序运行需要外部支持**

导入自己的库:

1. 将库文件导入到自己的工程文件夹
2. `#pragma comment(lib, "lib_name ")`加入到文件头,形成链接

## 面向对象

`class`内部默认私有,`strcut`内部默认共有

1. 类的构造:

   **从本质来看,对象是一片连续的内存空间,可以利用C++保留指针的特性进行非法操作**

   可以通过在`.lib`库中定义类.在外部属性只定义一个属性:是库中的类的实例对象的指针

   * 属性 : 包含变量,占据执行区域的栈区内存

   * 方法 : 包含对对象的操作,占据加载程序时的空间,函数不占有运行期空间

     1. 构造方法 : 初始化生成对象实例

     2. set-get方法 : 和可见性修饰一起对类的属性进行访问控制

        ```C++
        class Student{
            private:{
            	int age;
            	char* name;
            };
            public: {
            	void init(int age, char* name, Student* s);
                int getAge();
                void setAge(int age);
            };
            void Student::init(int age, char* name, Student* s){
              // statement;  
            }
            int Student::getAge(){
                return age;
            }
            void Student::setAge(int age){
               // statements
        		}
            }
        }
        ```

        

   注 : 结构体的属性的存储方式时按最大数据类型的大小进行对齐,按**运行顺序**进行空间分配

   ```C++
   struct{
       int i;		// 分配4字节
       int j;		// 分配4字节
       char c;		// 分配4字节,占据1字节
       char d;		// 在剩下的3字节中占据1字节
   };
   ```


# 第三节课

##  构造函数

1. 无返回值,在每次建立对象时使用,与类同名,没有返回值

   ```C++
   Student::Student(int  aage, char *nname) {
       age = aage;
       name = nname;
   }
   ```

2. 对象放在堆区存储(使用`new`创建的对象),局部基本变量放在栈区

3. 对象声明:

   * `Student s`;
   * `Student s = new Student();`

4. 对象中对象的存储有两种形式:

   1. 直接定义对象 : 必然占有空间,一般用必然存在的内部对象
   2. 定义对象句柄 : 只占据4字节指针空间,使用时通过动态分配,一般用于不一定存在的对象

5. 清理不用对象释放内存

   `~Test()` : // 析构函数

