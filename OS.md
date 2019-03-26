# lab0

## Makefile

# lab1

## readelf函数

### 大小端

区别在于4个字节在一个字中的存储方式

1. 小端模式 : 数据的高字节保存在内存的高地址处,数据的低字节保存在内存的低地址处,**符合算术逻辑**

2. 大端模式 : 数据的高字节保存在内存的低地址处,数据的低字节保存在内存的高地址处, **表现类似于字符串,但是不符合运算逻辑,不能直接读取一个字**

3. 将大端模式存储的一个字以int的形式读出

   ```c
   int trans(int* num) {	//num为指向该字的指针,即为首(低)内存地址
       char *p = (char*)num;
       return (int)( ((*p) << 24) + ((*(p+1)) << 16) + 
                   ((*(p+2)) << 8) + (*(p+4)) )   
   }
   ```

### `readelf`命令使用

`readelf`命令用来解析`linux`下的`ELF`文件信息

* `readelf -h ELFfile1 ElFfile2 ...`

  显示`ELF Header`的文件头信息

* `readelf -l ELFfile1 ELFfile2 ...`

  显示`Program Header Table`中的每个`Segment`的信息

* `readrlf -S ELFfile1 ELFfile2...`

  显示`Section Header Table`中的每个`Section`的信息

### `objdump`使用

用来解析二进制目标文件

* `objdump -S file.o`

  反汇编文件

* `objdump -f file`

  显示文件头信息

* `objdump -d file`

  反汇编`file`中要执行的那些`section`

## print函数解析

### c语言变长参数实现

* 实现依赖于`<stdarg.h>`头文件中的三个宏定义以及一个类型:

  ```c
  #define _ADDRESSOF(v)	(&(v))	// 返回地址
  #define _INISIZEOF(n)	((sizeof(n) + sizeof(int) - 1) & ~(sizeof(int)))	// 返回在该机器下n所占的字节数
  ```

  * 类型 : `va_list` : 该类型为一个(字符)指针,指向可变参数列表的头

    源定义 : `typedef char * va_list`

  * 宏定义 :

    * `void va_start(va_list ap, paramFirst)` : 

      1. `ap`为可变参数列表的首地址

      2. `paramFirst`为确定的第一个参数(即至少要有一个确定类型的参数)

      3. 功能 : 初始化可变参数列表,在执行完毕之后`ap`指向下一个参数

      4. 实现

         ```c
         #define _crt_va_start(ap, v)	(ap = (va_list)_ADDRESSOF(v) + _INTSIZEOF(v))
         #define va_start _crt_va_start	
         // 将ap指向第一个参数v结束的地址处并转化为va_list形式
         ```

    * `type va_arg(va_list ap, type)`

      1. `type` : 待返回的参数的类型

      2. 功能 : 返回下一个参数的值

      3. 实现

         ```c
         #define _crt_va_arg(ap, t)		( *(t*)((ap += INTSIZEOF(t)) - _INTSIZE(t)) )
         #define va_arg _crt_va_arg
         // ap总是指向下一个参数的开始地址(只需要将他转化为下一个参数类类型指针即可
         ```

    * `void va_end(va_list ap)`

      1. 关闭可变参数列表

      2. 实现

         ```c
         #define _crt_va_end(ap)		( ap = (va_list)0 )
         #define va_end _crt_va_end
         ```

* 可变参数函数的实现

  1. 定义 :

     先定义一定存在的参数(至少一个),不定的以`...`形式出现

     `int printf(const char *format,...)` 

     `print`一定存在的参数类型是一个常量字符串的指针,其指向的是格式符列表,不定的参数是实际要输出的数据值

  2. 实现 :

     **用`va_start`获取参数列表的地址并存在`ap`中,用`va_arg`逐个获取值进行处理,最后用`va_end`将`ap`置空**

     **一定要设定一个参数列表结束标志,否则`va_arg`不知道哪里结束**

     例如 : `printf()`是通过格式控制字符串的结束符`\0`来控制结束的,因为格式控制符一定和不定参数中的实值是一一对应的

  3. 示例:

     ```c
     // 一个不定长参数相加函数
     #define END -1	// 参数结束标志
     int va_sum(int first_num, ...) {
         // 定义参数列表
         va_list ap;
         
         va_start(ap, first_num);
         
         int reult = first_num;
         int temp = 0;
         // 逐个获取参数值
         while((temp = va_arg(ap, int)) != END) {
             result += temp;
         }
         
         va_end(ap);
         return result;
     }
     ```


# lab2

## 链表构成

* 链表有头节点,双向
* 一个`enity`中有两个指针,一个指向其之后的节点,**另一个指向其之前的节点中的下一个节点(即自己),是二重指针**