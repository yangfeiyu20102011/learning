## 数据加密

对称加密：采用单密钥

非对称加密：采用双密钥

在开发中应用得比较多的是非对称加密，单向加密。

常用的有MD5、BASE64等。



## MD5简要原理

MD5以512位分组来处理输入的信息，且每一分组又被划分为16个32位子分组，经过了一系列的处理后，算法的输出由四个32位分组组成，将这四个32位分组级联后将生成一个128位散列值。



## Go对MD5的支持

crypto/md5提供了相关操作，常用的md5加密有以下两种方式：

```go
package main

import (
   "fmt"
   "crypto/md5"
   "encoding/hex"
)

func main() {
   srcData := []byte("123456")
   fmt.Printf("first md5:%x\n",md5Encrypt1(srcData))
   fmt.Printf("second md5:%s\n",md5Encrypt2(srcData))
}

func md5Encrypt1(srcData []byte) [16]byte{
   encryptData := md5.Sum(srcData)
   return encryptData
}

func md5Encrypt2(srcData []byte) []byte{
   hash := md5.New()
   hash.Write(srcData)
   newText := hash.Sum(nil)
   encryptData := make([]byte, 32)
   hex.Encode(encryptData, newText)
   return encryptData
}
```

输出结果：

first md5:e10adc3949ba59abbe56e057f20f883e

second md5:e10adc3949ba59abbe56e057f20f883e



两种区别：

第一种返回的[]byte是固定的；

第二种返回的是[]byte类型的切片。