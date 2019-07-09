

go的nil是什么？

```go
bool      -> false                              
numbers -> 0                                 
string    -> ""      

pointers -> nil
slices -> nil
maps -> nil
channels -> nil
functions -> nil
interfaces -> ni

// nil是预定义的标识符，代表指针、通道、函数、接口、映射或切片的零值
```





go 为什么map可以扩容

m := make(map[int]int,cap) 声明了cap 但是不能调用cap(m)







深入理解 Go map：赋值和扩容迁移

https://segmentfault.com/a/1190000018632347



golang的反射是什么意思？能解决什么问题？跟其他语言的反射有什么区别？



golang的异常机制 panic 和 recover ？为什么不用try catch 





golang多核是如何使用的？



## 升级go

```sh
cd /home/fjdev/go

wget https://dl.google.com/go/go1.12.7.linux-amd64.tar.gz

rm -rf /usr/local/go

rm -rf /usr/bin/go
rm -rf /usr/bin/gofmt
rm -rf /usr/bin/godoc

tar -xzf go1.12.7.linux-amd64.tar.gz -C /usr/local

ln -s /usr/local/go/bin/* /usr/bin/

```

