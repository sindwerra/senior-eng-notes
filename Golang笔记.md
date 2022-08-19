### 指针
~~~golang
value := 5
ptr := &value  // ptr就是一个指针变量
fmt.Println(*ptr) // 输出为5
fmt.Println(ptr)  // 输出为内存地址
~~~

### 常量
~~~golang
const a = 5   // 没有显式声明常量的类型，所以下面两行都不会报错
fmt.Println(a + 5)
fmt.Println(a + 1.2)
const a int = 5  // 显式声明了常量的类型，所以下方代码会报错
fmt.Println(a + 1.2)
~~~


### 切片和数组
~~~golang
var arr [3]int{1,2,3}
append(arr, 4) // 报错
var slc []int{1,2,3}
append(slc, 4) // 正常
~~~

### 字符串处理
~~~golang

// 字符串的比较
import strings

str1 == str2 // 这是传统方法，但没有底下的Compare快

strings.Compare(str1, str2) // 0表示相等，1表示不等，-1表示字母顺序上str2小于str1

// 字符串的分割
strings.Split(str1, " ") // 跟Python用法差不多

// 包含
strings.Contains(str1, str2) // true or false

// 去掉前后空格
strings.TrimSpace(str1)

strings.ToUpper()
strings.ToLower()

// 每个单词的首字母大写
strings.Title()

strings.Fields()
strings.Join()
~~~

### 反射
~~~golang
// golang中有三个关于类型的概念 Value Type Kind

reflect.TypeOf(obj).Kind() // Kind返回的是当前这个变量的本质结构，即它是一个int或是一个func或是一个struct
reflect.TypeOf(obj).Name() // 这里的返回的是更表层的类型，比如type Obj struct{}，这里就会返回一个字符串"Obj"
reflect.ValueOf(obj) // 这个会返回结构体里面的值
~~~

###  公有属性和私有属性
~~~golang
type Person struct {
	FirstName string  // 首字母大写就是公有属性
	lastName string   // 首字母小写是私有属性
}
~~~

### type-declaration和type-aliases的区别
~~~golang
type Obj string // 这是type-declaration
type Obj = string // 这是type-aliases


~~~
这两者的区别是，type-declaration只是将被定义的type的属性给到了新type的身上，而aliases则是属性和方法都附了上去。同时declaration表示在该scope下不能再使用原type去定义变量，而需使用新type。而alias的限制是如果别名的是标准库中的类型，则无法使用新别名去给该类型添加自定义方法

### 嵌入式结构
~~~golang
type Name struct {
	firstName string
	lastName string
}

type Person struct {
	name Name
	another anotherType
}

a := Person(name: "Jack")
a.name.firstName
// 如果使用嵌入式定义

type Person struct {
	Name
	another anotherType
}

a := Person(name: "Jack")
a.firstName // 可以直接调用Name的属性和方法
~~~

### 一些比较奇怪的语法表现
~~~golang
type A struct {
	a string
	b string
}

type B struct {
	a string
	b string
}

A(1, 2) == B(1, 2) // 这是无法比较的

type Name interface {
	firstName()
	lastName()
}

type AnotherName interface {
	firstName()
	lastName()
}

type A struct {}
func NewA(firstName, lastName string) Name {}
func (a A) firstName() {}
func (a A) lastName() {}

type B struct {}
func NewB(firstName, lastName string) AnotherName {}
func (b B) firstName() {}
func (b B) lastName() {}

aObj := NewA('a', 'b')
bObj := NewB('a', 'b')

aObj == bObj // 这里虽然返回的是两个不同类型的接口，但是该表达式是成立的，可以比较，虽然比较结果是不等于

~~~
PS：另外有一个大原则，即无论是struct还是interface的比较，一旦数据结构体中带有collection类型的属性（slice，array，map之类的），则即使是同类型的两个变量也无法比较，这种无法比较不一定会被编译器检查出来，可能会在运行时才发现并报错

### switching判断类型的前提条件
![6dd2536f9f0e98e480317a5d808563bd](/Users/duli/Desktop/6dd2536f9f0e98e480317a5d808563bd.png)

1. 首先判断的变量必须是一个interface
2. 只要是interface变量，就可以使用.(type)这种方法cast类型

### 包的分类
1. main包 程序入口包
2. library包 其他引用工具的包

### 特殊函数名称
1. main main是当前应用的入口函数
2. init init是当前包被初始化时会触发的函数，它可以在同一个源文件中被定义多次，多次的定义都会被调用（golang中唯一一个允许多次定义的函数）

### scope
![Go中的scope](/Users/duli/Desktop/typora-docs/Senior/Go中的scope.png)

### 引用一个不被使用的包的目的是
![为什么要引用一个不被使用的包](/Users/duli/Desktop/typora-docs/Senior/为什么要引用一个不被使用的包.png)

## 实际编程中的最佳实践
1. 清空切片 
~~~golang
a := []int{1,2,3}
a = nil
~~~
2. 仿队列&栈
~~~golang
stack := []int{1,2,3}
stack = stack[:len(stack) - 1]

queue := []int{1,2,3}
queue = queue[1:]
~~~

### 线程和go协程的区别
![线程vsGo协程](/Users/duli/Desktop/typora-docs/Senior/线程vsGo协程.png)

### channel在go并发中的应用场景
![channel在并发中的使用场景](/Users/duli/Desktop/typora-docs/Senior/channel在并发中的使用场景.png)

### 信道的类型

![Channel的类型](/Users/duli/Desktop/typora-docs/Senior/Channel的类型.png)

## 一些认知
1. interface这个东西是不存在什么指针变量还是值变量的，只要对应的结构体实现了interface的方法，不管你初始化是赋的value-based还是pointer-based，都可以被interface变量接住
2. channel只能在发送侧关闭，接收侧不行 