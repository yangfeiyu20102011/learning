## 非侵入式

Go的接口不同于传统编程语言的接口，诸如java、c++等。

Go之前的语言，接口可以看作不同语言之间的一种契约，要实现该接口，必须从该接口继承。

而非侵入式的接口，一个类只需要实现了接口所要求的所有函数，就可以说这个类实现了该接口。

简单实现如下：

```go
package main

type Animal struct{
   Name string
   Age int
}

type Behavior interface {
   Eat(food string) bool
   Fly() bool
   Swim() bool
}

func (a *Animal)Eat(food string) bool{
   if food == "food"{
      return true
   }else{
      return false
   }
}

func (a *Animal)Fly()bool{
   return true
}

func (a *Animal)Swim()bool{
   return true
}

func main() {
   var a Behavior = new(Animal)
   a.Fly()
}
```

Animal类实现了Swim()、Fly()、Eat()三种方法，运行该代码，没有任何问题。

若注释掉其Swim()方法，报错如下：

cannot use new(Animal) (type *Animal) as type Behavior in assignment:

*Animal does not implement Behavior (missing Swim method)



同样进行如下操作也是合法的：

```go
type Bird interface {
   Fly() bool
}

var b Bird = new(Animal)
	b.Fly()
```

在原来代码基础上添加Bird接口，使用Animal给b赋值，也是可以的。

因为Animal类实现的方法中包含Bird接口所需要的方法。



### 非侵入接口优点

无需绘制类库的继承树图；

实现类的时候，只需要关心自己应该提供哪些方法，不用去管接口要拆分得如何合理；

不用为了实现一个接口而导入一个包，每多一个外部包，就意味着更多的耦合。



## 接口查询

```go
if varName2, ok := varName1.(interface2|typeName); ok {
   //此时 varName2 的类型由 interface1 转为 interface2，或者 varName1 不是 typeName 类型的变量  
} else {
   //不能转换 interface，或者 varName1 不是 typeName 类型的变量  
}

//if c,ok := a.(Behavior) ; ok{
	//	fmt.Println("ok it's animal")
	//	fmt.Println(c.Fly())
	//}
```

接口查询是否成功，要在运行期才能确定。



## 类型查询

```go
package main

import (
   "fmt"
)

func main() {
   var a interface{} = 5
   switch v := a.(type) {
   case string:
      fmt.Println("it's a string ",v)
   case int:
      fmt.Println("it's a int ",v)
   }
}
```

运行结果是

it's a int  5



```go
package main

import (
   "fmt"
)

func main() {
   var a interface{} = "hello"
   switch v := a.(type) {
   case string:
      fmt.Println("it's a string ",v)
   case int:
      fmt.Println("it's a int ",v)
   }
}
```

运行结果是

it's a string  hello



*注意，varName.(type)只可在switch中使用