### go协程

多线程在大部分操作系统上都属于系统层面的并发模式，在高并发模式下，开销依旧比较大。

协程是一种轻量级的线程，本质是一种用户态线程，无需操作系统进行抢占式调度，且在真正的实现中寄存于线程中。



### 两种通信方式

共享内存和消息传递

#### 共享内存

```go
package main

import "fmt"
import "sync"

var count int = 0

func say(lock *sync.Mutex){
	lock.Lock()
	count++
	fmt.Println("hello world")
	lock.Unlock()
}

func main(){
	lock := &sync.Mutex{}
	for i := 0; i < 5; i++{
		go say(lock)
	}
}

```

该程序意在启动5个goroutine分别打印“hello world”，

但是执行结果却是屏幕为空。

原因是go程序从初始化main package并执行main函数开始，当main函数泛会所话就直接退出程序，且不等待其他goroutine（非主goroutine）结束。

以上程序main启动5个goroutine后，say()还未来得及执行，main就结束了。

改进：

```go
package main

import "fmt"
import "sync"
import "runtime"

var count int = 0

func say(lock *sync.Mutex){
	lock.Lock()
	count++
	fmt.Println("hello world")
	lock.Unlock()
}

func main(){
	lock := &sync.Mutex{}
	for i := 0; i < 5; i++{
		go say(lock)
	}

	for{
		lock.Lock()
		c := count
		lock.Unlock()

		runtime.Gosched()
		if c >= 5{
			break
		}

	}
}
```

执行结果：

hello world
hello world
hello world
hello world
hello world



#### 消息传递

基于内存共享的通信机制使得程序过于臃肿，操作繁琐，不便于维护。

channel是Go在语言级别提供的goroutine间的通信方式，channel是进程内的通信方式。

channel是类型相关的，一个channel只能传递一种类型的值，这个类型需要在声明channel时指定。稍微类似于Unix下的管道。



```go
package main

import (
	"fmt"
)

func Count(ch chan int) {
	fmt.Println("I'm counting")
	ch <- 1
}

func main() {
	chs := make([]chan int, 5)
	for i := 0; i < 5; i++ {
		chs[i] = make(chan int)
		go Count(chs[i])
	}

	for _,ch := range(chs){
		fmt.Println("finish",<-ch)
	}

}
```

执行结果

I'm counting
finish 1
I'm counting
finish 1
I'm counting
I'm counting
finish 1
I'm counting
finish 1
finish 1