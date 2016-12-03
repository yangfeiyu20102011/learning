## new()

new(T) 为每个新的类型T分配一片内存，初始化为 0 并且返回类型为\*T的内存地址：这种方法 **返回一个指向类型为 T，值为 0 的地址的指针**，它适用于值类型如数组和结构体,它相当于 `&T{}`。

```go
type animal struct {
   name string
   age int
}

b := &animal{age:5,name:"duck"}

c := new(animal)
	c.age = 5
	c.name = "duck"

```



## make()

make(T) **返回一个类型为 T 的初始值**，它只适用于3种内建的引用类型：切片、map 和 channel。

```go
a := make([]int,3,5)
```

