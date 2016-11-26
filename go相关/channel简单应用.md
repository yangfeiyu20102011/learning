## 基本语法

一般channel的声明形式

```go
var chanName chan ElementType
//例如
var ch1 chan int
```

定义一个channel

```go
ch := make(chan int)
```

向channel传值

```go
ch <-
```

从channel取值

```go
value := <- ch
```



## select机制

select的用法类似于switch

```go
select {
	case <- chan1:
  //如果能从chan1输出数据，则执行该case
		fmt.Println("chan1")
	case chan2 <- 1 :
  //如果能写入chan2，则执行该case
		fmt.Println("chan2")
	default:
  //默认为执行该case
		fmt.Println("oh no")

	}
```



## 带缓冲的channel

```go
chan1 := make(chan int, 3)
```

chan1有三个缓冲空间，只要缓冲空间未填满就可继续写入而不会阻塞。



## 单向channel

只能进行读或写的特殊的channel

```go
//可读写
chan1 := make(chan int)
//只读
chan2 := <-chan int(chan1)
//只写
chan3 := chan<- int(chan1)
```



## 关闭channel

```go
//关闭ch
close(ch)

//ok用于检测是否成功关闭channel，若读取不到数据则为false，表示ch已成功关闭
_,ok := <-ch

```