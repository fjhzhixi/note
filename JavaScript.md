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

## 高阶函数

**函数的参数为某一个函数** :

* 实际上**所有的函数都指向某一个变量**,所以传递参数与传递函数没有本质的区别

### 参数为函数

* `map`函数 : 

  1. 使用 : `map()`方法定义在JavaScript的`Array`中，我们调用`Array`的`map()`方法，传入我们自己的函数，就得到了一个新的`Array`作为结果为**列表每一个元素作为函数参数之后的结果**

     `array.map(functName)`

  2. 效果 : 

     `array.map(functName) = [funct(array[1]), ....]`

* `reduce`函数 : 

  1. `Array`的`reduce()`把一个函数作用在这个`Array`的`[x1, x2, x3...]`上，这个函数必须接收两个参数，`reduce()`把结果继续和序列的下一个元素做累积计算

  2. 效果 : 

     `var array = [x1, x2, x3]`

     ``array.reduce(f) = f(f(x1, x2), x3)`

* `filter`函数 : 

  1. `filter`用于把`Array`的某些元素过滤掉，然后返回剩下的元素,接收一个**过滤函数**,把传入的函数依次作用于每个元素,然后根据返回值是`true`还是`false`决定保留(`true`)还是丢弃(`false`)该元素,最后返回过滤之后的列表

  2. **使用的过滤函数可以有多个参数** : 

     通常我们仅使用第一个参数，表示`Array`的某个元素。回调函数还可以接收另外两个参数，表示元素的位置和数组本身, 即 :

     ```javascript
     function(elem, index, self)
     ```

* `sort`函数

  1. `array.sort(f)`函数根据传入的参数函数对列表中的元素进行排序,排序依据**为函数的返回值** : `f(x1, x2)当x1 < x2时返回负值,相当返回0,大于返回大于0`

* `every`函数

  1. `very()`方法可以判断数组的所有元素是否满足测试条件,测试条件取决于传入的参数函数

* `find`函数 

  1. `find()`方法用于查找符合条件(由传入参数决定)的第一个元素，如果找到了,返回这个元素,否则,返回`undefined`

### 闭包

**将函数作为返回值返回**

* 返回的函数并没有立刻执行,而是直到调用了才执行
* 返回的函数在其定义内部引用了局部变量`arr`,所以,当一个函数返回了一个函数后,其内部的局部变量还被新函数引用
* **在使用闭包是注意循环变量等局部变量是被公用的,所以要单独使用可以定义内部函数来将其通过参数传入来绑定到这个返回闭包中**

### 箭头函数

**箭头函数即为匿名函数的简化定义形式**

* 定义 : `(var funct =)[可有可无] (args) => {函数语句}`
  1. 只有一个参数时`()`可以简化
  2. 只有一条语句时`{}和return`均可简化
  3. 内部的箭头函数中使用的`this`指向外部函数中的指向,可以正常运行

### 生成器

其实生成器本质上应该是一个**数据类型**,其可以看做一个可以**记住自身状态并多次返回值**的函数 :

* 定义 : `function* generatorName(args) {}`

* **函数体中可以使用`yield`多次返回**

* 使用 : 

  1. `var f = generatorName(args)`只是声明了一个生成器,并没有运行
  2. 使用`f.next()`可使生成器运行到一次返回语句之后暂停,直到结束返回`undefine`
  3. 也可以使用`for(var x of generatorName(args))`迭代生成器对象

  





# 方法

绑定到对象的函数,**与一般函数的主要区别就是`this`指针的的使用**

## 定义

在对象内部定义 : 

```javascript
var object = {
    functName : function(args) {
        //在函数内部使用this获得对象的属性值
    }  
};
```

* **只用通过`object.functName(args)`的形式调用方法时`this`才指向`object`

* **`this`指向给对象本身只存在于对象内部定义并且是第一层定义,内部的嵌套函数定义也不可以直接使用`this`**,所以我们的常规做法是 :

  1. **在函数一开始即捕获`this`指针**

  ```javascript
  var object = {
      functName : function(args) {
     		var that = this; //在函数开始就捕获this指针
          function functInName(args) {
              //在内部嵌套函数体内使用that即相当于使用this指针
          }
      }  
  };
  ```

  2. 使用`apply`控制`this`的指向,**用于不通过对象而单独作为一个函数调用时**

     要指定函数的`this`指向哪个对象，可以用函数本身的`apply`方法，它接收两个参数，第一个参数就是需要绑定的`this`变量，第二个参数是`Array`，表示函数本身的参数

  ```javascript
  var object = {
      keyName : functName	//将方法名keyName绑定到一个方法functName上
  };
  function functName(args) {
      //实现该方法
  }
  //调用方式
  object.keyName();
  functName.apply(object, [args]);
  ```


# 标准对象

## Date对象

表示日期和时间

注意 : 

* `JavaScript`的月份范围用整数表示是0~11，`0`表示一月，`1`表示二月

## 正则表达式

## JSON格式

是网络上的数据交换的统一格式,规定字符串使用双引号

* `JavaScript`对象序列化为`JSON`格式字符串

  `var s = JSON.stringify(objectName)`

  参数 : 

  1. 传入的对象
  2. 控制序列化 : 
     * 筛选属性的列表,只会序列化列表中有的属性的键值对
     * 处理键值对的函数,键值对先经过函数处理在序列化
  3. 控制输出格式,可按缩进输出

  或者可以在对象内部实现`toJSON: function () {}` : 函数直接返回要序列化的键值对

* `JSON`格式字符串反序列化为`JavaScript`对象

  `var obj = JSON.parse(JSONstring)`

# 面向对象



## 创建对象

1. 没有类和对象的区分,而是为**每个对象设置一个原型,通过原型链的机制实现**

2. 使用构造函数 : 即`new` + 普通函数,通常可以选择通过一个函数将其包装起来

   ```javascript
   function Student(props) {
       this.name = props.name || '匿名'; // 默认值为'匿名'
       this.grade = props.grade || 1; // 默认值为1
   }
   
   Student.prototype.hello = function () {
       alert('Hello, ' + this.name + '!');//所有在该原型链上的对象共享一个函数
   };
   
   function createStudent(props) {
       return new Student(props || {})
   }
   ```

**原型链通过`prototype`属性链成一条链**

## 继承原型

1. 定义新的构造函数,并在内部用`call()`调用希望“继承”的构造函数,并绑定`this`
2. 借助中间函数`F`实现原型链继承,最好通过封装的`inherits`函数完成
3. 继续在新的构造函数的原型上定义新方法

```javascript
function inherits(Child, Parent) {//实现原型链继承关系构建的函数
    var F = function () {};//中间函数用来过渡
    F.prototype = Parent.prototype;//F指向父类对象指向的原型
    Child.prototype = new F();//子类对象指向父类对象的原型
    Child.prototype.constructor = Child;//子类的构造器是自己的
}
```

## class继承

与`java`相同,只不过构造函数为`constructor(args)`

# 浏览器

## 常用对象

* `window`对象不但充当全局作用域,而且表示浏览器窗口
  1. `innerWidth`和`innerHeight`属性,可以获取浏览器窗口的内部宽度和高度
  2. `outerWidth`和`outerHeight`属性,可以获取浏览器窗口的整个宽高
* `navigator`对象表示浏览器的信息
* `screen`对象表示屏幕的信息
* **`location`对象表示当前页面的URL信息**
* `document`对象表示当前页面,由于HTML在浏览器中以DOM形式表示为树形结构,`document`对象就是整个DOM树的根节点

## DOM操作

HTML文档被浏览器解析后就是一棵DOM树,每个标签代表一个节点

1. 获得节点 : 

   * `document.getElementById()` : 返回唯一的节点
   * `document.getElementsByTagName()` : 返回多个匹配的节点
   * `document.getElementsByClassName()` : 返回多个匹配的节点

2. 更新节点 :

   * 修改`innerHTML`属性,可以直接修改该节点的HTML内容但是注意原来的内容就会被覆盖
   * 修改`innerText`或`textContent`属性 : 只能修改文本属性,不能改变标签

3. 插入节点 : 在保存原有节点信息上添加

   * 使用`appendChild` : 如果从节点数中拿出一个插入到其他位置,则原位置上的节点会先删除,大多数情况下我们会新建一个节点插入
   * `parentElement.insertBefore(newElement, referenceElement)` : 在父节点的某一指定子节点之前添加节点

4. 删除节点

   获得要删除的节点以及其父节点只有使用`parentNode.removeChild()`

   **但是注意`parent.children`属性是一个动态的变化**


# JQuery

`JQuery`是对**`DOM`树的一致化操作**

## 选择器

### 一般选择器

选择器返回的是`JQuery`对象,即一个选择出**节点的列表**

* 按`ID`查找 : 

  ```javascript
  var select = $('#id-name')
  //查找<div id='id-name'>...</div>的节点
  ```

* 按`tag`查找 :

  ```javascript
  var select = $('tag-name')
  //查找<tag-name>...</tag-name>的节点
  ```

* 按`class`查找 :

  ```javascript
  var select = $('.class-name')
  //查找<div class="class-name">...</div>的节点
  ```

* 按属性查找 :

  除了之前这些共有的属性之外,对实际节点还有其他特殊属性,例如`name`

  ```javascript
  var select = $('[name="email"]');
  var select = $('[name^="pre-"]');//所有name属性以pre-开始的节点
  var select = $('[name$="-end"]');//所有name属性以-end结束的节点
  ```

1. 之上的所有选择规则都可以**组合起来** : 直接连接在一起即可 :

   ```javascript
   var tr = $('tr.red');
   //找出<tr class="red">...</tr>的节点
   ```

   **这种组合形式相当于取交集**

2. 之上的所有选择规则可以构造成**多项选择器** : 使用`,`组合 :

   ```javascript
   var select = $('p.red,p.green');
   //选择出<p class="red">...</p>和<p class="green">...<\p>的节点
   ```

   **这种组合形式相当于取并集**

### 层级选择器

选择某一上层节点之下的符合选择器的节点

**将选择条件按节点的层次结构组织起来即可,中间用空格隔开**

使用`$(parent>child)`**规定该层级结构必须是紧邻的父子节点结构**

### 过滤器

使用`:`之后限定

## 修改

使用`JQuery`对象的**方法**可以批量化的**修改选择器选择出来的节点列表**

### 修改Text和HTML

1. 使用`text()`和`html()`无参方法获得节点的文本和原始`HTML`文本
2. 使用`text('...')`和`html('...')`有参数的方法对节点的文本和`HTML`文本进行修改

### 修改CSS

使用`css(‘name’, ‘value’)`方法**重写**节点的`CSS`

使用`addClass(), removeClass()`**修改**节点的`CSS`

### 修改表单

使用`val()`方法获取(无参)和设置(有参)`value`属性

### 修改DOM

* 添加节点 : 对父节点

  1. 使用`append('html')`方法在同层次最后添加一个子节点
  2. 使用`prepend()`在最前
  3. 使用`js.after()`在某个节点之后
  4. 使用`js.before()`在某个节点之前

  可以直接传入一个节点字符串,`DOM`对象,`JQuery`对象,函数对象

* 删除节点 : 使用`js.remove()`即可


## 事件

依赖事件来**触发`JavaScript`代码的执行**

常用的有鼠标事件,键盘事件等等

1. 使用`ready`事件的监听来做所有的初始化
2. 所有事件会传入`Event`对象作为参数,可以从该参数中获得更多的信息

## 动画

对节点使用函数操作,传入参数为时间,`JQuery`逐渐改变`CSS`效果到最终结果,这个过程构成动画效果

1. `show`和`hide`和`toggle`函数
2. `slideUp`和`slideDown`函数
3. 通过设定最终`CSS`效果自定义动画

## AJAX

使用`JQuery`在全局变量`$`中绑定的方法`ajax()`来处理`AJAX`请求

## 扩展

通过`$.fn.newFunction()`为全局对象绑定自定义方法插件)实现扩展

**注意在自定义方法的最后`return this`以便支持链式操作**





































