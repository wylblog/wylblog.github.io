---
title: Go语言中的变量和常量
date: 2024-11-10 23:55:54
tags:
  - Golang
categories: 
  - 运维开发
  - 云原生
---
## 3. Go语言中的变量和常量

### 3.1 Go语言中变量的声明

Go语言变量是由`字母、数字、下划线`组成，其中首个字符不能为数字。Go语言中`关键字和保留字`都不能用作变量名

Go语言中变量需要声明后才能使用，同一作用域内不支持重复声明。并且Go语言的变量声明后必须使用。

变量声明后，没有初始化，打印出来的是空

### 3.2 如何定义变量

> 方式1：直接声明

```go
var name = "zhangsan"
```

> 方式2：带类型

```go
var name string = "zhangsan"
```

> 方式3：`类型推导`方式定义变量

a在函数内部，可以使用更简略的 `:=` 方式声明并初始化变量

注意：短变量`只能用于声明局部变量`，不能用于全局变量声明

```go
变量名 := 表达式
```

例子：

```go
sex := "男"
```

> 方式4：声明多个变量

**类型都是一样的变量**

```go
var 变量名称， 变量名称 类型
```

案例：

```go
package main

import "fmt"

func main() {
	var a1, a2 int
	a1 = 5
	a2 = 6
    fmt.println(a1)
    fmt.println(a2)
	fmt.Println(a1 + a2)
}
```

**类型不一样的变量**

```go
var (
	变量名称 类型
    变量名称 类型
)
```

案例：

```go
package main

import "fmt"

func main() {

	var (
		name string
		age  int
		sex  string
	)
	name = "zhangsan"
	age = 18
	sex = "男"

	fmt.Printf("name=%v,age=%d,sex=%v", name, age, sex)
}

```

#### 3.2.1 占位符使用:warning:待整理

> 注意：需要配合`Printf`来使用

| 占位符使用 | 作用           |
| ---------- | -------------- |
| %T         | 类型占位符     |
| %v         | 值占位符       |
| %d         | 整数占位符     |
| %f         | 浮点占位符     |
| %c         | 字符占位符     |
| %s         | 字符串的占位符 |

案例：

```go
	var (
		Name2 = "bigdata"
		Age2  = 18
		Sex2  = "男"
	)
	fmt.Println(Name2, Age2, Sex2)

	// 占位符
	fmt.Printf("他的名字是:%v,年龄:%v,性别:%v\n", Name2, Age2, Sex2)              // 他的名字是:bigdata,年龄:18,性别:男
	fmt.Printf("Name的类型是:%T Age的类型是:%T Sex的类型是:%T\n", Name2, Age2, Sex2) // Name的类型是:string Age的类型是:int Sex的类型是:string
```



> 变量总结

全部的定义方式

```go
package main

import "fmt"

func main() {
	/*
		Go语言变量是由字母、数字、下划线组成，其中首个字符不能为数字。Go语言中关键字和保留字都不能用作变量名

		Go语言中变量需要声明后才能使用，同一作用域内不支持重复声明。并且Go语言的变量声明后必须使用。

		变量声明后，没有初始化，打印出来的是空
	*/
	// 方式1:直接声明
	var name = "zhangsan"
	fmt.Println(name)

	// 方式2:带类型
	var name2 string = "lisi"
	fmt.Println(name2)

	// 方式3:`类型推导`方式定义变量  注意：短变量`只能用于声明局部变量`，不能用于全局变量声明
	name3 := "xiaoming"
	fmt.Println(name3)

	// 方式4:声明多个变量
	// 类型都是一样的变量
	var a1, a2 int
	a1 = 5
	a2 = 6
	fmt.Println(a1)
	fmt.Println(a2)
	fmt.Println(a1 + a2)

	// 类型不一样的变量  声明后再赋值
	var (
		Name string
		Age  int
		Sex  string
	)
	Name = "hj"
	Age = 22
	Sex = "男"
	fmt.Println(Name, Age, Sex)

	// 声明的同时赋值
	var (
		Name1 string = "mjl"
		Age1  int    = 20
		Sex1  string = "女"
	)
	fmt.Println(Name1, Age1, Sex1)

	// 类型推导
	var (
		Name2 = "bigdata"
		Age2  = 18
		Sex2  = "男"
	)
	fmt.Println(Name2, Age2, Sex2)

	// 占位符
	fmt.Printf("他的名字是:%v,年龄:%v,性别:%v\n", Name2, Age2, Sex2)              // 他的名字是:bigdata,年龄:18,性别:男
	fmt.Printf("Name的类型是:%T Age的类型是:%T Sex的类型是:%T\n", Name2, Age2, Sex2) // Name的类型是:string Age的类型是:int Sex的类型是:string

}
```

### 3.3 如何定义常量

相对于变量，`常量是恒定不变的值`，多用于定义程序运行期间不会改变的那些值。常量的声明和变量声明非常类似，只是把var换成了`const`，常量在`定义的时候必须赋值`。

```go
	/*
		相对于变量，`常量是恒定不变的值`，多用于定义程序运行期间不会改变的那些值。常量的声明和变量声明非常类似，只是把var换成了`const`，常量在`定义的时候必须赋值`。
	*/

	//定义了常量，可以不用立即使用
	const pi = 3.141596

	// 定义两个常量
	const (
		A = "A"
		B = "B"
	)

	// const同时声明多个常量时，如果省略了值表示和上面一行的值相同
	const (
		a = "A"
		b
		c
	)
	fmt.Println(a, b, c) // A A A
```



### 3.4 Const常量结合iota的使用（了解）

iota是golang 语言的常量计数器，只能在常量的表达式中使用

iota在const关键字出现时将被重置为0（const内部的第一行之前），const中每新增一行常量声明将使iota计数一次（iota可理解为const语句块中的行索引）。

> 1. 每次const出现，都会让iota初始化为0【自增长】

```go
package main

import "fmt"

func main() {

	const a = iota	// a = 0
	fmt.Println(a) 

	const (
		b = iota // b = 0
		c		// c = 1
 		d		// d = 2
	)
	fmt.Println(b, c, d)

}

```

> 2. const  iota使用 `_` 跳过某些值

```go
	const (
		b = iota // b = 0
		_        // 跳过 1
		d        // d = 2
	)
	fmt.Println(b, d)
```

> 3. iota声明中间插队

```go
	// iota声明中间插队
	const (
		n1 = iota // n1 = 0
		n2 = 100  // n2 = 100
		n3 = iota // n3 = 2
		n4        // n4 = 3
	)
	fmt.Println(n1, n2, n3, n4) // 输出:0 100 2 3
```



> 4. 多个iota定义在一行

```go
	// 多个iota定义在一行
	const (
		n1, n2 = iota + 1, iota + 2 // 0+1=1 ,0+2 =2
		n3, n4                      // 2 , 3
		n5, n6                      // 3 , 4
	)
	fmt.Println(n1, n2)
	fmt.Println(n3, n4)
	fmt.Println(n5, n6)
```

![image-20240510165431706](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/image-20240510165431706.png)

### 3.5 Go语言变量、常量明明规则

1、变量名称必须由数字、字母、下划线组成。
2、标识符开头不能是数字
3、标识符不能是保留字和关键字。

4、变量的名字是区分大小写的如：age和Age是不同的变量。在实际的运用中，也`建议`，不要用一个单词大小写区分两个变量。

5、标识符（变量名称）一定要`见名思意`：变量名称建议用名词，方法名称建议用动词

6、变量命名一般采用驼峰式，当遇到特有名词（缩写或简称，如DNS)的时候，特有名词根据是否私有全部大写或小写。





### 3.6 注释与godoc

> 注释不会被编译，每一个包应该有相关注释。

单行注释是最常见的注释形式，你可以在任何地方使用以 // 开头的单行注释。多行注释也叫块注释，均已以 /* 开头，并以 */ 结尾。如：

```go
1.
// 单行注释
2.
/*
 Author by 菜鸟教程
 我是多行注释
 */
3.注释换行
// 你好
//
// 世界
```

> `go doc` 是 Go 语言提供的一个工具，用于查看 Go 包和符号的文档。它类似于 `godoc`，但用法更简单且直接。以下是 `go doc` 的用法和主要功能说明：

`go doc` **的用法**:

1. **查看包的文档**

   ```
   复制代码
   go doc 包名
   ```

示例：

```
   复制代码
   go doc fmt
```

   输出：`fmt` 包的概述，包括包的介绍和主要函数。

2. **查看包中某个函数、类型、变量、常量的文档**

   ```
   复制代码
   go doc 包名.符号名
   ```

示例：

```
   复制代码
   go doc fmt.Println
```

   输出：`fmt.Println` 函数的文档，包括函数签名和说明。

3. **查看包中某个类型的方法**

   ```
   复制代码
   go doc 包名.类型名.方法名
   ```

示例：

```
   复制代码
   go doc net/http.Client.Get
```

   输出：`http.Client` 类型的 `Get` 方法的文档。

4. **查看包的导入路径**

   ```
   复制代码
   go doc -src 包名
   ```

示例：

```
   复制代码
   go doc -src fmt
```

   输出：`fmt` 包的源代码。

5. **指定工作目录**

   ```
   复制代码
   go doc -C 工作目录
   ```

   示例：

   ```
   复制代码
   go doc -C /path/to/your/project fmt
   ```

   输出：指定工作目录下 `fmt` 包的文档。

`go doc` **的主要功能**:

- **快速查看文档**：无需打开浏览器，可以在终端中快速查看 Go 包、类型、函数等的文档。
- **支持源码查看**：可以通过 `-src` 选项查看包的源代码，帮助理解实现细节。
- **支持自定义工作目录**：通过 `-C` 选项，可以在指定的工作目录下查找包的文档。

**示例:**

以下是几个常见的 `go doc` 命令示例及其输出解释：

1. **查看 `fmt` 包的文档**

   ```
   复制代码
   go doc fmt
   ```

   输出：`fmt` 包的概述，包括包的介绍和主要函数。

2. **查看 `fmt.Println` 函数的文档**

   ```
   复制代码
   go doc fmt.Println
   ```

   输出：`fmt.Println` 函数的文档，包括函数签名和说明。

3. **查看 `http.Client.Get` 方法的文档**

   ```
   复制代码
   go doc net/http.Client.Get
   ```

   输出：`http.Client` 类型的 `Get` 方法的文档。

4. **查看 `fmt` 包的源代码**

   ```
   复制代码
   go doc -src fmt
   ```

   输出：`fmt` 包的源代码。

5. **在指定工作目录下查看 `fmt` 包的文档**

   ```
   复制代码
   go doc -C /path/to/your/project fmt
   ```

输出：指定工作目录下 `fmt` 包的文档。

通过使用 `go doc` 工具，可以方便地在命令行中查看 Go 标准库以及自定义包的文档，有助于开发者更快地理解和使用 Go 语言中的各种功能。

> go'doc可以为项目代码导出网页版的注释文档

**1.** **首先需要安装**

```go
go get golang.org/x/tools/cmd/godoc
```

**2.** **启动http:**

```go
godoc -http=:6060
```

**3. 使用浏览器访问**

```sh
http://127.0.0.1:6060/pkg/go-course/entrance_class 
```

![image-20240719102952811](https://hj-typora-images-1319512400.cos.ap-guangzhou.myqcloud.com/images/202407191029000.png)