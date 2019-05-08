**`javaScript`可以在任何网页代码中调用**

# 数据类型

注意 : 在`JavaScript`中**严格区分大小写**

变量 : 所有用`var`声明的,可以是任何类型(**动态语言特性**)

## 基本数据结构

1. `Number` : 数字,**不区分整数与浮点数**

   注 :

   * 用`NaN`即`Not a number`表示计算结果不能用数字表示
     1. `NaN`与任何数字值都不相等,包括他自己
     2. **唯一判断的方式是函数`isNaN(NaN)`**
   * 用`Infinity`表示无限大,即超过了`Number`范围
   * 在浮点数比较时最好不要比较相等(有误差)

2. 字符串 : 使用单引号或者双引号括起来的文本

   注 : 

   * 特殊字符使用**转义字符**表示 : 例如`\'`
   * 多行字符可以使用`\n`表示也可以使用反引号`包含
   * 字符串拼接 : 
     1. 使用`+`连接
     2. 使用模板字符串机制 : `I am ${name}`其中`name`为变量,在显示时会替换
   * 字符串操作 : **首先字符串是不变对象,即不可改变**
     1. 获取长度 : `str.length`
     2. 获取某一个字符 : 类似于数组 `str[index]`
     3. 常用方法**返回一个新的字符串** : 
        * `toUpperCase()   toLowerCase()` : 大小写转化
        * `indexOf()`会搜索指定字符串出现的位置
        * `substring()`返回指定索引区间的子串

3. 布尔值 : `true/false`

## 复合数据结构

1. 数组 : `var arr = [elem1, elem2.....];`

   * 规定 : **同一个数组中的元素可以是任何类型的**

   * 获取元素 : 支持**下标索引**, **但是当下标越界时会引起数组大小的变化**

   * 获取长度 : `array.length` : **该值的改变会引起数组大小的变化**

   * 数组操作 : **数组是可变的对象**

     * `indexOf()`来搜索一个指定的元素的位置

     * `slice()`截取`Array`的部分元素，然后返回一个新的`Array`

       注 : 

       1. 注意到`slice()`的起止参数包括开始索引,不包括结束索引
       2. 如果不给`slice()`传递任何参数,它就会从头到尾截取所有元素(即复制一个`Array`)

     * `push()`向`Array`的末尾添加若干元素,`pop()`则把`Array`的最后一个元素删除掉

     * `unshift()`方法往`Array`的头部添加若干元素,`shift()`方法则把`Array`的第一个元素删掉：

     * `sort()`可以对当前`Array`进行排序,它会直接修改当前`Array`的元素位置

     * `reverse()`反转数组

     * **`splice()`方法是修改`Array`的“万能方法”,它可以从指定的索引开始删除若干元素，然后再从该位置添加若干元素** : 操作如下

       `splice(startIndex, deleteNum, elemAdd1, elemAdd2....)`

     * `concat()`方法把当前的`Array`和另一个`Array`连接起来,并返回一个新的`Array`

     * `join()`方把当前`Array`的每个元素(如果不是字符串会转换为字符串)都用指定的字符串连接起来，然后返回连接后的字符串 : `join(charToConnected)`

   * 多维数组 : 使用`Array`作为`Array`中的元素来实现

2. 对象 :  `var person = {name : Tom, age : 20,.....};`

   * 规定 : 由键值对组成的**无序**集合,**键必须是字符串形式,而值可以为任何形式**

   * 获取方式 : 对象变量`.`键名(`person.name`)
   * 添加属性 :  直接使用`object.propertyName = elem`即可
   * 删除属性 : 使用`delete object.propertyName`
   * 判断属性是否存在 : 
     1. 所有的(包括继承来的) : 使用`propertyName in object`
     2. 自己定义的 : 使用`hasOwnProperty()`方法

3. `Map` : 对象可以认为为一种`Map`,但是其`key`值局限于字符串类型,而`Map`为无限制的键值对

   * 定义 : `var map = new Map([key1:value1], [key2:value2],....)`
     1. 初始化可以传入**一个二维数组,并且满足上面形式**
     2. 或者直接初始化一个空的
   * 添加键值对 : `map.set(key, value)`
     1. 没有`key`时添加
     2. `key`存在时替换旧的
   * 获取键值对 : `map.get(key)` 

4. `Set` : **一组`key`的集合,并且不重复(此处的相等包含同一类的数据)**

   * 定义 : `var set = new Set([key1, key2....])`
     1. 初始化可以传入一个一维数组,**并且会自动过滤重复元素**
     2. 可以直接初始化一个空的
   * 添加 : `set.add(key)`
   * 删除 : `set.delete(key)`

**`Set`和`Map`的遍历使用迭代器 : `for (var x of set/map)`**

## 注意事项

* 在`JavaScript`中**比较相等运算符**特殊 :

  1. `==` : 自动转换类型
  2. `===` : 类型不同返回`false`,类型相同才比较

  **所以大多数最好坚持使用`===`**


# 逻辑结构

## 分支

`if{} else if{} else{}`

## 循环

`for()`

**特殊用法`for..in`** : 

1. 遍历对象的**属性**: `for (var key in object){}`
2. 遍历数组的**下标** : `for(var i in array){}`

`while()   do-while()`

# 函数

## 定义

1. `function funcName(args){}`
2. `var functName = function(args){};`

注 : 

* 在函数内部关键字`arguments`,它只在函数内部起作用,并且永远指向当前函数的调用者传入的所有参数,`arguments`类似`Array`但它不是一个`Array`,该关键字可以**实现根据传递参数的个数的不同执行不同的函数功能**

* `function foo(a, b, ...rest)` : 类似于`C`中的不定参数形式,`rest`为额外参数的列表

* 函数的定义可以嵌套

* 为避免出错严格遵守**先定义再使用**

* **由于使用`var`定义的变量与一般的函数的作用域都是全局,为了避免在全局范围内的命名冲突,一般将一个文件中的所有变量和函数均绑定到一个全局变量中**

  ```javascript
  var FILE = {};
  //变量
  FILE.elemName = elem;
  //函数
  FILE.functName = funct(){};
  ```

* **使用`let`定义具有块作用域的变量,常用于循环**

* **使用`const`定义常量**

* **解构赋值支持对一组变量赋值(如一个数组,一个键值对`Map`)**

* 





