### 类型
Go 语言函数可以返回多值。  
`%T` 可打印变量的类型。  
`%v` 可打印任意类型变量的值，`%+v` 打印字段名+值，`%#v` 打印结构体
128 位的类型可以用 `complex128`。  

**零值**

每个类型都有自己个的零值，分别为 `0` `false` `""` `nil`。  
- int float double 的零值都是 0
- string 的零值是 ""
- array 和 struct 的零值是其每个元素的零值的数组
- slice map chan interface 的零值都是 nil，表示没有分配底层资源或初始化

Go 语言中变量定义的时候会自动初始化为其零值。但是零值为 nil 的这些不可未分配资源就直接使用 ？（那么声明和定义还有什么区别 ？）  

**枚举**

```go
const (
	Apple = iota // 0
	Banana       // 1
	Cherry       // 2
)
```
Go 语言中没有 enum，而是通过在 const 中使用 `itoa` 实现的，itoa 在每个常量声明中从 0 开始按顺序递增。  
还可以这么使用：
```go
const (
	_  = iota
	KB = 1 << (10 * iota) // 1 << (10 * 1) = 1024
	MB = 1 << (10 * iota) // 1 << (10 * 2) = 1048576
	GB = 1 << (10 * iota) // 1 << (10 * 3) = 1073741824
	TB = 1 << (10 * iota) // 1 << (10 * 4) = 1099511627776
)
```

**结构体 struct**

结构体和结构体指针似乎没什么区别（这里怎么没讲到引用什么的 ？）

**数组 array**

表示一个 10 个 int 的数组：
```go
var a [10]int
```
数组的长度不可改变，**长度是数组类型的一部分**（如 `[4]int` 和 `[5]int` 是完全不同的类型）。  
虽然数组的元素在内存中是连续的，但是**一个数组变量表示整个数组，而不是指向第一个元素的指针**，这点和 C 语言不一样（Go 语言中数组是 `值语义`），所以当一个数组变量被赋值或传递时，实际上会复制整个数组。  

**切片 slice**

切片对底层数组进行了抽象，并提供相关操作的方法，其内部包含 3 个字段：指向底层数组的指针 ptr，切片访问的元素个数（长度 len），切片运行增长到的元素个数（cap）。cap 不是很好理解，cap 和底层数组的长度有什么关系 ？

slice 底层实际是一个 struct，定义如下：
```go
type slice struct {
	array	unsafe.Pointer
	len		int
	cap		int
}
```
切片的长度 `len` 是切片的长度，切片的容量 `cap` 是切片所引用的数组的长度 ？

创建一个可以使用的 slice 有三种方式：

1、直接 make
```go

var s []int = make([]int, 10, 50)
//或
s := make([]int, 10)

```
这样分配一个有 50 个 int 的数组，并创建一个长度为 10 容量为 50 的切片 s，该切片指向数组的前 10 个元素。容量参数是可以缺省的，容量默认值为切片的长度。

2、对 array/slice 进行切片：
```go
var a [10]int
var s []int = a[1:4]
```
表示一个元素类型为 int 的切片，它是一个半开区间，包括 1 不包括 4。  
切片不是数组的副本，而是数组的引用，修改数组的任意切片和修改数组一样。

3、append
```go
var s []string
s = append(s, "a")
```

**映射 map**

创建 map：
```go
m = make(map[string]Vertex)
```
删除元素:
```go
delete(m, key)
```
检查元素是否存在：
```go
elem, ok := m[key]
```

**[字面量](https://zhuanlan.zhihu.com/p/282096939)**

举个 python 的栗子：
```python
>>> {"name": "iswbm"}.get("name")  # 使用字面量 
'iswbm' 
>>> 
>>> profile={"name": "iswbm"}   # 使用变量名 
>>> profile.get("name") 
'iswbm' 
```
Go 语言中内建的 `基本类型` 有：
- 布尔类型：bool
- 11个内置的整数数字类型：int8, uint8, int16, uint16, int32, uint32, int64, uint64, int, uint和uintptr
- 浮点数类型：float32和float64
- 复数类型：complex64和complex128
- 字符串类型：string

而这些类型的文本，就是 `字面量`，字面量可以理解为：**没有变量名的变量**。

字面量和常量一样是不可寻址的。

常见的字面量使用方式：
```go
s := []string{"alex", "bob", "chali"}

m := map[string]int{
	"english": 99,
	"math": 98,
}
```

**new 和 make 的区别**

`new(T)` 为类型 T 分配一片内存空间，初始化为零值并返回类型为 \*T 的 **内存地址**，适用于值类型如 **数组** 和 **结构体**，相当于 `&T{}`。
`make(T)` 返回一个类型为 T 的初始值（怎么理解 ？），make 只适用三种内建的引用类型：切片、map、channel

假设一个切片是  
ptr | len | cap
-|-|-

那么：  
`new []int` 得到的切片的 *ptr 是 nil，len 和 cap 是 0，返回值是指向这个切片的指针。  
`make([]int, 0)` 得到的切片指向一个空数组，len 和 cap 是 0，返回值是这个切片。  
有点绕，不过上面这两个都没啥用，一般这样使用：

简单来说，new 没有初始化，make 有初始化。

**函数 func**
函数 func 可以用做参数和返回值，[函数是一等公民](https://www.cnblogs.com/zhangjinfu/articles/11307316.html)  
函数闭包
```GO
func adder() func(int) int {
	sum := 0
	return func(x int) int {
		sum += x
		return sum
	}
}
```

### struct 和 method
Go 没有类，但是可以为 `struct` 定义方法
`method` 方法  
`function` 函数
`receiver` 接收者，可以是 package 内定义的任意自定义 type  
方法是带接收者`receiver`的函数，准确的说是接收者指针，否则传的也是值的副本（复制还没法改到它） 
调用的时候是 receiver.method(args)

函数和方法的区别：
- 带指针参数的函数必须接受一个指针
- 以指针为接收者的方法被调用时，接收者用值和用指针都可以，就是 v.method 和 (&v).method 是一样的

### interface
接口
- 接口（interface）是一种 type
- 接口类型，是由一组方法签名（方法签名又是什么 ？就是方法的名字 ？参数和返回值类型呢 ？）定义的集合
- 类型通过实现一个接口的所有方法来实现该接口
- 接口类型的变量可以保存任何实现了这些方法的值（值又是什么意思 ？）
- 这样的接口有什么意义呢 ？
- 可以有空接口 `interface {}`

Go 语言还有类型断言，用法如下：
```go
t, ok := i.(T)
```

