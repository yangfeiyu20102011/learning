## 编码为JSON

```go
func Marshal(v interface{}) ([]byte, error)
```

Marshal函数会递归遍历整个对象，依次按成员类型编码，编码规则如下

bool类型 转换为JSON的Boolean
整数，浮点数等数值类型 转换为JSON的Number
string 转换为JSON的字符串(带""引号)
struct 转换为JSON的Object，再根据各个成员的类型递归打包
数组或切片 转换为JSON的Array
[]byte 会先进行base64编码然后转换为JSON字符串
map 转换为JSON的Object，key必须是string
interface{} 按照内部的实际类型进行转换
nil 转为JSON的null
channel,func等类型 会返回UnsupportedTypeError



举例

```go
type Animal struct{
	Name string
	Age int
}

func main() {
	dog := Animal{
		Name: "dog",
		Age: 12,
	}

	dogJson,err := json.Marshal(dog)
	if err == nil{
		fmt.Println(dogJson)
		fmt.Println(string(dogJson))
	}

}
```

以上代码中Animal结构体中成员必须大写，不然无法被JSON处理到。

处理结果如下

[123 34 78 97 109 101 34 58 34 100 111 103 34 44 34 65 103 101 34 58 49 50 125]

{"Name":"dog","Age":12}



当然，也可以自定义JSON处理后的变量名

```go
type Animal struct{
	Name string `json:"animalName"`
	Age int `json:"animalAge"`
}
```

处理结果为

[123 34 97 110 105 109 97 108 78 97 109 101 34 58 34 100 111 103 34 44 34 97 110 105 109 97 108 65 103 101 34 58 49 50 125]

{"animalName":"dog","animalAge":12}



## JSON解码

```go
func Unmarshal(data []byte, v interface{}) error
```

data为字节数组，v为解码后的对象

```go
package main

import (
	"encoding/json"
	"fmt"
)

type Animal struct{
	Name string `json:"animalName"`
	Age int `json:"animalAge"`
}


func main() {

	a := []byte(`{"animalName":"dog","animalAge":19}`)
	var dog Animal
	err := json.Unmarshal(a, &dog)
	if err == nil{
		fmt.Println(dog)
	}

}
```

结果为

{dog 19}



若对字节数组a做一定的改变，添加Animal对象中不存在的字段

```go
a := []byte(`{"animalName":"dog","animalAge":19,"animalSize":87}`)
```

结果保持

{dog 19}

究其原因，json.Unmarshal在处理字节数组时会根据一个约定的顺序查找目标结构中的字段，如果找到一个即发生匹配，若找不到则自动舍弃Go对象中不存在的字段。