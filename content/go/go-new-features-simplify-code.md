---
title: "用 Go 语言新特性简化代码"
---


Go 语言在 1.18 版本之后引入了许多实用的新函数和方法，这些新特性大大简化了我们的日常编码工作。本文将介绍一些最常用的新函数，并通过对比新旧代码来展示它们如何让我们的代码更简洁、更易读。

## 1. slices 包的新函数

### 1.1 查找元素

旧版本：
```go
// 查找切片中是否包含某个元素
func contains(slice []int, target int) bool {
    for _, v := range slice {
        if v == target {
            return true
        }
    }
    return false
}

// 使用
numbers := []int{1, 2, 3, 4, 5}
if contains(numbers, 3) {
    // ...
}
```

新版本：
```go
numbers := []int{1, 2, 3, 4, 5}
if slices.Contains(numbers, 3) {
    // ...
}
```

### 1.2 切片排序

旧版本：
```go
// 对切片进行排序
numbers := []int{5, 2, 8, 1, 9}
sort.Ints(numbers)
```

新版本：
```go
numbers := []int{5, 2, 8, 1, 9}
slices.Sort(numbers)
```

### 1.3 切片比较

旧版本：
```go
// 比较两个切片是否相等
func equalSlices(a, b []int) bool {
    if len(a) != len(b) {
        return false
    }
    for i := range a {
        if a[i] != b[i] {
            return false
        }
    }
    return true
}
```

新版本：
```go
a := []int{1, 2, 3}
b := []int{1, 2, 3}
if slices.Equal(a, b) {
    // ...
}
```

### 1.4 遍历切片值

旧版本：
```go
numbers := []int{1, 2, 3, 4, 5}
for i := 0; i < len(numbers); i++ {
    fmt.Println(numbers[i])
}
```

新版本：
```go
numbers := []int{1, 2, 3, 4, 5}
for v := range slices.Values(numbers) {
    fmt.Println(v)
}
```

### 1.5 反向切片遍历

旧版本：
```go
// 反向遍历切片
numbers := []int{1, 2, 3, 4, 5}
for i := len(numbers) - 1; i >= 0; i-- {
    fmt.Println(numbers[i])
}
```

新版本：
```go
numbers := []int{1, 2, 3, 4, 5}
for _, v := range slices.Backward(numbers) {
    fmt.Println(v)
}
```

## 2. strings 包的增强

### 2.1 字符串裁剪

旧版本：
```go
// 去除字符串两端的空白字符
s := "  hello  "
s = strings.TrimSpace(s)

// 去除字符串两端的特定字符
s = strings.Trim(s, " ")
```

新版本：
```go
s := "  hello  "
s = strings.TrimSpace(s)  // 和之前功能一样
s = strings.TrimPrefix(s, " ")  // 只去除前缀
s = strings.TrimSuffix(s, " ")  // 只去除后缀
```

### 2.2 字符串分割

旧版本：
```go
// 分割字符串
parts := strings.Split("a,b,c", ",")
// 需要手动处理空字符串
if parts[0] == "" {
    parts = parts[1:]
}
```

新版本：
```go
// 新增了更多分割选项
parts := strings.Split("a,b,c", ",")
// 自动过滤空字符串
parts = strings.Split("a,,b,c", ",")  // ["a", "", "b", "c"]
parts = strings.SplitAfter("a,b,c", ",")  // ["a,", "b,", "c"]
```

## 3. maps 包的新函数

### 3.1 清空 map

旧版本：
```go
// 清空 map
m := map[string]int{"a": 1, "b": 2}
for k := range m {
    delete(m, k)
}
```

新版本：
```go
m := map[string]int{"a": 1, "b": 2}
maps.Clear(m)
```

### 3.2 复制 map

旧版本：
```go
// 复制 map
src := map[string]int{"a": 1, "b": 2}
dst := make(map[string]int, len(src))
for k, v := range src {
    dst[k] = v
}
```

新版本：
```go
src := map[string]int{"a": 1, "b": 2}
dst := maps.Clone(src)
```

## 4. 泛型带来的简化

### 4.1 通用容器操作

旧版本：
```go
// 为每种类型都需要写一个函数
func maxInt(a, b int) int {
    if a > b {
        return a
    }
    return b
}

func maxFloat64(a, b float64) float64 {
    if a > b {
        return a
    }
    return b
}
```

新版本：
```go
// 使用泛型，一个函数处理多种类型
func max[T constraints.Ordered](a, b T) T {
    if a > b {
        return a
    }
    return b
}

// 使用
maxInt := max(1, 2)
maxFloat := max(1.5, 2.5)
```

### 4.2 通用数据结构

旧版本：
```go
// 需要为每种类型实现一个栈
type IntStack struct {
    items []int
}

func (s *IntStack) Push(x int) {
    s.items = append(s.items, x)
}

func (s *IntStack) Pop() int {
    if len(s.items) == 0 {
        panic("stack is empty")
    }
    x := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return x
}
```

新版本：
```go
// 使用泛型实现通用栈
type Stack[T any] struct {
    items []T
}

func (s *Stack[T]) Push(x T) {
    s.items = append(s.items, x)
}

func (s *Stack[T]) Pop() T {
    if len(s.items) == 0 {
        panic("stack is empty")
    }
    x := s.items[len(s.items)-1]
    s.items = s.items[:len(s.items)-1]
    return x
}

// 使用
intStack := &Stack[int]{}
stringStack := &Stack[string]{}
```

## 5. 其他实用新特性

### 5.1 错误处理改进

旧版本：
```go
// 处理多个错误
var err1, err2 error
if err1 != nil {
    return fmt.Errorf("error 1: %v", err1)
}
if err2 != nil {
    return fmt.Errorf("error 2: %v", err2)
}
```

新版本：
```go
// 使用 errors.Join 合并多个错误
err := errors.Join(err1, err2)
if err != nil {
    return err
}
```

### 5.2 时间处理

旧版本：
```go
// 获取当前时间
now := time.Now()
// 需要手动处理时区
loc, _ := time.LoadLocation("Asia/Shanghai")
now = now.In(loc)
```

新版本：
```go
// 新增了更多便捷的时间函数
now := time.Now().Local()  // 直接获取本地时间
```

### 5.3 改进的 HTTP 路由

旧版本：
```go
// 手动实现路由模式匹配
http.HandleFunc("/user/", func(w http.ResponseWriter, r *http.Request) {
    id := strings.TrimPrefix(r.URL.Path, "/user/")
    if id == "" {
        http.Error(w, "Missing user ID", http.StatusBadRequest)
        return
    }
    fmt.Fprintf(w, "User ID: %s", id)
})
```

新版本：
```go
// 使用增强的 http.ServeMux 支持路径模式
mux := http.NewServeMux()
mux.HandleFunc("GET /user/{id}", func(w http.ResponseWriter, r *http.Request) {
    id := r.PathValue("id")
    fmt.Fprintf(w, "User ID: %s", id)
})
```

### 5.4 新的随机数生成

旧版本：
```go
// 生成随机整数
rand.Seed(time.Now().UnixNano())
n := rand.Intn(100)
```

新版本：
```go
// 更高效的随机数生成
n := rand.IntN(100)  // 无需显式设置种子
```

### 5.5 循环变量捕获

旧版本（可能导致意外行为）：
```go
funcs := []func(){}
for i := 0; i < 3; i++ {
    funcs = append(funcs, func() { fmt.Println(i) })
}
for _, f := range funcs {
    f() // 输出 3, 3, 3
}
```

新版本（Go 1.22 修复）：
```go
funcs := []func(){}
for i := 0; i < 3; i++ {
    funcs = append(funcs, func() { fmt.Println(i) })
}
for _, f := range funcs {
    f() // 输出 0, 1, 2
}
```
