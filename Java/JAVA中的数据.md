# Java中的数据

## 基本数据类型

## 字符串

### String

Java中没有内置的字符串类型，而是在java.lang包中提供了一个String类。在Java中，每个用双引号括起来的字符串都是一个String类的实例，换而言之每一次在程序中定义了一个用双引号包裹的字符串都是创建了一个`String`对象。

String类被final修饰，所以每个创建的字符串都是不可以改变的，其值存储在常量池中。每当有新的对象指向一个已经创建的字符串，那么会将已经创建的字符串地址赋值给新的字符串对象实现了字符串的共享。

String类有两个容易混淆的值：null和空串，null是指当前String对象没有指向任何实例，而空串的值为""，长度为0。想要知道一个字符串不为空长度也不为0可以使用`if (str == null & str.length() != 0)`

#### Sting内置方法

- 通过索引切割字符串：`str.substring(int fristnumber int lastnumber) return String` **注意截取方式为左闭右开**
- 拼接字符串：在Java中虽然没有提供运算符重载的方式，但是却对字符串提供了`+ +=`的运算符重载
- 检查两个字符串是否相等：`String`类重写了`Object`中的`equals`方法，他会比较两个字符串中长度以及每个字符是否相等
- 获取字符串长度：`str.length() return int`
- 以某一个字符串开始：`str.startsWith(String other) return boolean`
- 以某一个字符串结尾：`str.endsWinth(String other) return boolean`
- 返回给定字符串在指定字符串中首次出现的第一个索引：`str.indexOf(String other) return int`
- 返回给定字符串在指定字符串中最后出现的第一个索引：`str.lastOf(String other) return int`
- 返回使用新字符串替换指定字符串中所有老字符串的新串：`str.replace(CharSequence  oldString CharSequence newString) return String`
- 转换大小写：`str.toLowerCase() str.toUpperCase() return String`
- 返回删除字符串头尾空格的新字符串：`str.trim() return String`
- 返回一个字符数组：`str.toCharArray() return char[] `
- 返回一个字符：`str.charAt(int index) return char`
- 使用指定字符串分割字符串并返回一个字符串数组：`str.split(String other) return String[]`

### StringBuilder&StringBuffer

StringBuilder与StringBuffer都是不被final修饰的字符串类型，这意味着其长度可以更改并且两者的方法和功能是完全等价的，区别是前者为线程不安全的后者为线程安全的，后者中的方法大多数都是用synchronized 关键字修饰。

#### 内置方法

- 追加字符串：`str.append(String other) return this`
- 在指定索引后添加字符串：`str.insert(int index, String other) return this`
- 删除指定索引位置的字符串：`str.delete(int startIndex, int endindex) return this`

## 泛型

在Java5新增了泛型，在程序中使用泛型会使程序的可读性和安全性得到很大程度的提升。

泛型的本质是为了约束程序的形参种类，最典型的例子就是在没有泛型的ArrayList中，ArrayList底层维护着一个Object引用数组，对于放入的数据没有任何要求，因为所有的对象都可以被向上转型为Object类型，但是在获取数据的时候程序无法给出ArrayList中元素的具体类型，因为在存入ArrayList时所有的对象都被向上转型成Object类型了。在使用时就需要手动的向下转型，这就是错误的频发区。

使用泛型后，可以直接知道当前的ArrayList存储的到底是什么类型数据方便接下来的操作。

### 泛型程序的声明

```java
// 在类描述中声明泛型以供整个类使用泛型
class MyClass<T, U>{
    private T oneValue;
    private U twoValue;
    
    public T getT() {
        return oneValue;
    }
    public void setU(U u) {
        twoValue = u;
    }
}

// Java使用E来表示集合元素类型，使用K和V表示键值对类型，使用T表示任意类型必要时可以采用相邻的字母U和S
// 上述并不是强制要求，你可以使用任何字母来表示任何类型，上述是常见做法

// 在方法中声明泛型
class MyClass1{
    
    public void <T, U> run(T t, U u) {
        
    }
}

// 方法中的泛型定义在发法修饰符的后面，返回值的前面

// 为泛型添加类型限定
class MyCLass<T extends Number & List> {
    
}
// 指定泛型T继承于Number或者List
```

### 泛型的实现

## 集合框架

所有编程语言都离不开对数据的存储，程序的运行离不开数据的存储与传递，每种语言都对不同的数据结构提供了支持。在Java中提供了不同的类来支持不同数据存储的实现。

Java中将集合类库的接口与实现分离，接口并没有说明这种数据结构是如何实现的，而是交给实现类来实现数据结构是如何组成的。

如果没有使用泛型来约束集合类型，那么所有存入的元素都会自动被提升为Object类型

### Java集合框架类图

![来自菜鸟](C:/Users/Lingt/Documents/Git/Notes/Java/Images/Java集合框架类图.png)

### Iterable

迭代器是Java集合框架顶级接口，任何单列集合都间接继承了此接口并实现了迭代自身的方法。

##### 内部方法

- 返回一个自身类型的迭代器：`iterator() return Iterator<T>`
- 对内部元素进行遍历**Java1.8新增**：`default forEach(Consumer<? super T> action) return void`
- 在并行下进行遍历**Java1.8新增**：`default spliterator() return Spliterator<T>` ***尚未了解*** 

#### Iterator

Iterator是用来遍历并操作集合的一个迭代器，当遍历单列集合时，从当前集合实例获取一个迭代器对象，不同的单列单列集合都以不同方式实现了对自身迭代的方法。

##### 内部方法

- 查看集合中是否还存在下一个元素：`hasNext() return boolean`
- 将元素焦点向下移动：`newt() return E`
- 删除当前元素**Java1.8新增**：`default remove() return void`

### Collection

Collection是所有单列集的父类接口，其子类包括List、Set等。集合本身不能存放四类八种基本数据类型，因为集合中存储的都是引用数据类型，而其真实的值都在堆内存或方法区中，但是基本数据类型都存放在栈内存中，随时都会被回收。可以通过基本类型对应的包装类来使集合存储包装过的基本数据类型，在**Java1.5**中引入自动拆装箱的机制方便了集合对基本数据类型的存储。

##### 内部方法

- 返回内部元素个数：`size() return int`
- 向集合内添加元素：`add(Object o) return boolean`
- 向集合内添加另一个集合：`addAll(Collection c) return boolean`
- 清空集合：`clear() return void`
- 判断是否存在某一个对象：`contains(Object obj) return boolean`
- 判断集合中是否存在另一个集合中所有元素：`containsAll(Conllection c) return boolean`
- 判断集合是否为空：`isEmpty() return boolean`
- 移除一个元素：`remove(Object obj) return boolean`

#### List

List是有序集合的顶层接口继承了Collertion接口，List中的元素可以重复并且其存储数据的顺序也是固定的。

###### 内部方法

- 通过索引获取元素：`get(int index) return T`
- 通过索引设置元素的值：`set(int index, T t) return T`

##### ArrayList

ArrayList继承了List接口，可以把ArrayList理解成一个可变数组，可以通过下标访问集合中的元素。所以ArrayList的查询速度很快，直接通过索引即可得到元素。但是增加和删除非常慢，每次增加和删除都要根据当前集合长度而调整集合的存储空间，甚至删除元素还会多次拷贝数组。ArrayList通过内部类的方式实现了Iterator。

##### LinkedList

LinkedList底层是双向链表，集合中的每一个元素都存储了上一个元素和下一个元素的地址。相较于ArrayList其增加和删除元素的速度快很多，每增加一个元素时，会将相邻的两个元素的地址保存在元素的头尾，而将自身的地址交给相邻元素的头尾便完成了增加。删除时只需将相邻元素的地址指向对方便完成了删除。而查询速度却很慢，每次查找元素都相当于在遍历整个集合。

###### 内部方法

- 向头部添加元素：`addFirst(Object obj) return void`
- 向尾部添加元素：`addLast(Object obj) return void`
- 返回头部元素：`getFirst() return Object`
- 返回尾部元素：`getLast() return Object`
- 删除并返回头部元素：`removeFirst() return Object`
- 删除并返回尾部元素：`removeLast() return Object`

##### ArrayDeque

ArrayDeque双端队列，源码有被transient修饰的Object数组，当调用构造函数时就会初始化这个数组给定长度为16。ArrayDeque有分为头和尾，在概念上头和尾组成一个连续的数组，但是实际上头和尾是两个部分，ArrayDeque内部总是存在一段空闲空间，他要保证头和尾不会相撞。

#### Set

Set是无序集合的顶层接口继承了Collection接口。Set中的元素不可以重复并且其存储数据的位置不固定，在遍历时每次组成的顺序都是随机的。

##### HashSet

HashSet是无序且不可重复，允许有null值并且是线程不安全的一组集合。其底层是通过维护一组HashMap集合来实现的，在源码中有一个反序列化的HashMap集合其Key值为一个泛型value值为一个Object对象。在首次调用构造函数时会直接初始化这个HashMap集合。

##### LinkedHashSet

##### TreeSet

#### Map

Map是Java映射集合的顶层接口，他没有继承Iterable或者Collection接口。一个Key对应一个Value，Map中的Key是不可重复的。

###### 内部方法

- 获取图长度：`size() return int`
- 判断是否为空：`isEmpty() return boolean`
- 是否存在键：`containsKey(Object key) return boolean`
- 是否存在值：`containsValue(Object value) return boolean`
- 添加元素并返回对应的上一个元素：`put(K key, V value) return V`
- 使用键删除一个元素并返回对应的值：`remove(K key) return V`
- 清空图：`clear() return void`

##### HashMap

HashMap继承了Map接口，HashMap的Key和Value都是泛型，当定义了具体的种类就不能再修改。HashMap是无序的并且允许Value的值是null。

##### TreeMap

##### WeakHashMap

##### IdentityHashMap

### Collections

Collections是位于java.util包下的一个工具类，其构造函数使用private修饰并且其内部方法被static修饰，使用时直接调用具体方法即可。



