# go学习笔记

## go指针
        
go中指针表示变量的内存地址值。不支持指针运算。
非指针变量只能通过 & 符号获得指针变量，获得的指针变量类型可用 *Type ( *int, *float)

指针变量可通过 * 符号获取指针变量对应的的内存地址存储的内容。

**注意**：
不能获取常量或文字的指针
## go函数

### 函数类型
> * 普通函数
> * lambda 函数
> * 方法（method）

### 函数参数与返回值

#### 参数
> * 按值传递（值拷贝）
> * 按引用传递（指针）
#### 返回值
命名返回值
    
    func test(a int, b int) (c int, d int) {
        c = a + b
        d = a - b
        return
    }
#### 空白符
忽略或者丢弃返回值， 用 `_` 表示
#### 变长参数

    func myFunc(a, b, arg ...int) {}

## go数组
### 数组的声明
    var 变量名 [len]type
### go数组常量(可直接用值初始化数组)
    var arr1 = [5]int{1,2,3,4,5}
    var arr2 = [...]string{"aa", "gg", "dd"}
    var arr3 = []int{3,2,5,2}
    var arrKeyValue = [5]string{3: "Chris", 4: "Ron"}
### **注意**
数组的赋值，是值拷贝

    const SIZE = 5
    func main() {
        var arr1 [SIZE]int
        for i := 0; i < SIZE; i ++ {
            arr1[i] = i * 2
        }
        arr2 := arr1
        arr2[2] = 100

        fmt.Println(arr1)
        fmt.Println(arr2)
    }
    输出：

    [0 2 4 6 8]
    [0 2 100 6 8]

## 切片
1.切片是对数组一个连续片段的引用， 所以切片是一个引用类型。

2.切片是一个长度可变的数组

3.**切片的长度**：它等于切片的长度 + 数组除切片之外的长度。如果 s 是一个切片，cap(s) 就是从 s[0] 到数组末尾的数组长度

### 切片的声明方式
var 变量名 []type

### 切片的初始化方式
#### 直接从数组来，**这种方式获取的切片，改变了切片的值，数组对应的项也会改变**

    var sl []int = arr1[:]
#### make

    var sl []int = make([]int, len, cap)
### 切片重组（改变len的大小，但不能超过cap）
    slice1 = slice1[0:end]

### 切片的复制和追加
     复制
    func copy(dst, src []Type) int
    追加
    func append(slice []Type, elems ...Type) []Type

 
 **你想将切片 y 追加到切片 x 后面，只要将第二个参数扩展成一个列表即可：x = append(x, y...)**
## Map
 ### 声明方式
 var 变量名 map[keyType]valueType

 ### 初始化
 map1 := make(map[string]int)

**不要使用new来创建map**

如果你错误的使用 new() 分配了一个引用对象，你会获得一个空引用的指针，相当于声明了一个未初始化的变量并且取了它的地址


## struct

结构体的定义：

    type struct1 struct{
        f1 float
        int
        string
    }

结构体的初始化

    s1 := &struct1{23.12, 3, "haha"}
    s2 := struct1{23.12, 3, "haha"}
    s3 := new(struct1)
    s3.f1 = 23.12
    s3.int = 3
    s3.string = "haha"

### 方法

**类型的方法定义必须和类型同一个包下，如果不直接用类型来创建方法， 可使用类型别名创建方法**

内嵌将一个已存在类型的字段和方法注入到了另一个类型里：匿名字段上的方法“晋升”成为了外层类型的方法。当然类型可以有只作用于本身实例而不作用于内嵌“父”类型上的方法，

可以覆写方法（像字段一样）：和内嵌类型方法具有同样名字的外层类型的方法会覆写内嵌类型对应的方法。

## 接口

**多态** 同一个类型在不同的实例上表现出不同的行为