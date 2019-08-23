# function

## 内部表示

```go
type funcval struct {
	fn uintptr
	// variable-size, fn-specific data here
	// 闭包能引用到的变量的地址就放这, 还有binded method引用到的receiver地址也放这
}
```

```go
func f() {} // 这里f是*funcval
```

下面的代码可以拿到函数实际地址

```go
type funcval struct {
    fn uintptr
}

func f() {
}

v1 := (**funcval)(unsafe.Pointer(&f))
fmt.Println((**v1).fn)
```
