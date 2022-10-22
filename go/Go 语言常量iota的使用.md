

# Go 语言常量iota的使用

iota，特殊常量，可以认为是一个可以被编译器修改的常量。

iota 在 const关键字出现时将被重置为 0(const 内部的第一行之前)，const 中**每新增一行**常量声明将使 iota **计数一次**(iota 可理解为 const 语句块中的行索引)。

```
package main
import "fmt"

const (
	a, b = iota + 1, iota + 2
	c, d
	e, f
	g, h = iota * 10, iota * 20
	i, k
)

func main() {
	fmt.Println("a = ", a, ",b = ", b)
	fmt.Println("c = ", c, ",d = ", d)
	fmt.Println("e = ", e, ",f = ", f)
	fmt.Println("g = ", g, ",h = ", h)
	fmt.Println("i = ", i, ",j = ", j)
}

```

最终执行结果：

```
a =  1 ,b =  2 
c =  2 ,d =  3 
e =  3 ,f =  4 
g =  6 ,h =  9 
i =  8 ,j =  12
```

