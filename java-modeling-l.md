# JML规格

实现的**设计**必须满足**规格**的要求

## 基本语法

**一般对一个属性/方法的规格描述在其紧邻的上方**

### 原子表达式

1. `\result` : 表示一个非`void`方法的返回值

## 分析阶段

针对于类中的属性与方法从**逻辑**上分析出**行为规范约束**

### 对属性

对类中的每个属性成员,分析:

1. 在**可见状态**下必须满足的**不变特性**
2. 在一次值变化时必须要满足的**状态变化约束**

注 : 

1. 可见状态指的是属性的值时稳定的,当处于所有会修改成员变量的值的方法内部时都是不可见状态
2. **在该层次分析的是属性成员必须在任何行为中满足的基本约束条件,不考虑细节,只是宏观的类决定的逻辑层次**

### 对方法

对类中的每个定义的方法,分析:

1. **输入参数**要求 : **前置条件**

   将所有输入的范围划分为**不重合**的区间,划分依据如下 :

   * 从方法正常执行的逻辑上要求**调用者**必须满足的输入条件
   * 多种**不合法的输入**分别抛出不同类型的异常

   注 : 最好可以做到是一个**划分**,即对所有可能输入的全覆盖

2. **返回值**要求 : **后置条件**

   要求**方法实现者**确保方法执行的结果一定要满足的条件

   注 : 

   1. 只针对于不是`void`类型的方法
   2. **后置条件满足的前提是调用者满足正确输入的前置条件**

3. **实现过程**要求 : **副作用限定** :

   对方法实现过程中是否可以对对象的属性进行改变的限定

## 实现阶段

实现从逻辑上分析出来的约束条件的**建模层次**的**描述**

### 对属性

使用**类型规格语法**来描述

注 : 由于安全性要求的**私有属性对外部不可见**使用`spec_public`来标记在`JML`中可见

1. 不变特性 : 使用**不变式`invariant P`**描述

   `p`为描述属性在**可见状态**下的约束条件的**布尔表达式**

   例 :

   ```java
   public class Path{
   	private /*@spec_public@*/ ArrayList <Integer> seq_nodes;
   	private /*@spec_public@*/ Integer start_node;
   	private /*@spec_public@*/ Integer end_node;
   	/*@ invariant seq_nodes != null &&
   	  @ seq_nodes[0] == start_node &&
   	  @ seq_nodes[seq_nodes.legnth-1] == end_node &&
   	  @ seq_nodes.length >=2;
   	  @*/
   }
   ```

2. 状态变化约束 : 使用**状态变化约束`constraint P`**来描述

   `p`为描述属性**当前可见状态和前序可见状态的关系**的**布尔表达式**

   例 :

   ```java
   public class ServiceCounter{
   	private /*@spec_public@*/ long counter;
   	//@ invariant counter >= 0;
   	//@ constraint counter == \old(counter)+1;
   }
   ```

### 对方法

使用**方法规格语法**来描述

1. 对输入参数使用**前置条件语法`requires P`**来描述

   `P`为方法输入参数在**某一区间**满足的**布尔表达式**

2. 对返回值使用**后置条件语法`ensures P`**来描述

   `P`为方法**正常执行**时输出满足的**布尔表达式**\

3. 对实现过程使用**副作用范围限定语法`assignable / modifiable  elems`**来描述

   `assignable`表示可赋值的,`modifiable`表示可修改的,`elems`为其描述对象,即一个变量列表

注 :

1. 对**异常**的处理 : 

   **根据输入分一个正常执行描述,若干个异常描述处理** : 

   * 使用`public normal_behavior`接下来的描述正常执行
   * 使用`public exceptional_behavior`接下来的描述异常行为
   * 不同段之间使用`also`连接

例 :

```java
/*@ public normal_behavior
@ requires z <= 99;
@ assignable \nothing;
@ ensures \result > z;
@ also
@ public exceptional_behavior
@ requires z < 0;
@ assignable \nothing;
@ signals (IllegalArgumentException e) true;
@*/
public abstract int cantBeSatisfied(int z) throws IllegalArgumentException;
```



