# TriLang 语法

## 1. 基础语法

### 1.1 注释
```
// 单行注释

/*
多行注释
可以跨越多行
*/
```

### 1.2 标识符
- 以字母或下划线开头
- 可包含字母、数字和下划线
- 区分大小写

### 1.3 关键字
```
module, func, let, var, if, else, for, while, do, switch, 
case, default, break, continue, return, struct, enum, 
interface, impl, import, as, pub, const, type, defer, 
goto, sizeof, alignof, typeof, null, true, false
```

## 2. 数据类型

### 2.1 基本类型
```
bool    // 布尔值 true/false
int     // 平台相关整数(通常32/64位)
int8    // 8位有符号整数
uint8   // 8位无符号整数
int16   // 16位有符号整数
uint16  // 16位无符号整数
int32   // 32位有符号整数
uint32  // 32位无符号整数
int64   // 64位有符号整数
uint64  // 64位无符号整数
float32 // 32位浮点数
float64 // 64位浮点数
char    // UTF-8字符
string  // UTF-8字符串
```

### 2.2 复合类型
```
// 数组
[5]int       // 固定长度数组
[]int        // 动态数组

// 切片
[]int        // 整型切片

// 字典
map[string]int // 字符串到整型的映射

// 指针
*int         // 指向整型的指针

// 元组
(int, bool)  // 包含int和bool的元组
```

## 3. 变量与常量

### 3.1 变量声明
```
let x: int = 10      // 不可变变量
var y: float32 = 3.14 // 可变变量
z := "hello"         // 类型推断
```

### 3.2 常量声明
```
const PI = 3.14159
const MAX_SIZE: int = 100
```

## 4. 模块与导入

### 4.1 模块声明
```
module main  // 主模块
module math  // 库模块
```

### 4.2 导入语句
```
import "math"           // 导入标准库
import "os" as operating_system  // 带别名导入
import "./mylib"        // 导入本地模块
```

## 5. 函数

### 5.1 函数定义
```
// 基本函数
func add(a: int, b: int) -> int {
    return a + b
}

// 多返回值
func divmod(a: int, b: int) -> (int, int) {
    return (a / b, a % b)
}

// 可变参数
func sum(numbers: ...int) -> int {
    total := 0
    for n in numbers {
        total += n
    }
    return total
}
```

### 5.2 匿名函数
```
let square = func(x: int) -> int { return x * x }
```

## 6. 控制流

### 6.1 条件语句
```
if x > 0 {
    print("positive")
} else if x < 0 {
    print("negative")
} else {
    print("zero")
}
```

### 6.2 循环语句
```
// for循环
for i := 0; i < 10; i++ {
    print(i)
}

// while循环
while x > 0 {
    print(x)
    x--
}

// 无限循环
for {
    print("forever")
}

// 范围循环
for i in 0..10 {
    print(i)  // 0到9
}

for i in 0..=10 {
    print(i)  // 0到10
}
```

### 6.3 switch语句
```
switch x {
case 1:
    print("one")
case 2, 3:
    print("two or three")
default:
    print("other")
}
```

## 7. 复合数据结构

### 7.1 结构体
```
struct Point {
    x: float64
    y: float64
}

// 使用
let p = Point{x: 1.0, y: 2.0}
print(p.x)
```

### 7.2 枚举
```
enum Color {
    Red
    Green
    Blue
    RGB(u8, u8, u8)
}

// 使用
let c = Color::RGB(255, 0, 0)
```

### 7.3 接口
```
interface Drawable {
    func draw()
}

struct Circle {
    radius: float64
}

impl Drawable for Circle {
    func draw() {
        print("Drawing circle with radius", radius)
    }
}
```

## 8. 错误处理

### 8.1 可选类型
```
func find_user(id: int) -> ?string {
    if id == 1 {
        return "Alice"
    } else {
        return null
    }
}

// 使用
if let name := find_user(1) {
    print("Found:", name)
} else {
    print("Not found")
}
```

### 8.2 错误类型
```
error FileError {
    NotFound
    PermissionDenied
    Corrupted
}

func read_file(path: string) -> !string {
    if !file_exists(path) {
        throw FileError::NotFound
    }
    // ...
    return content
}

// 使用
try {
    let content = read_file("test.txt")
    print(content)
} catch e: FileError {
    print("File error:", e)
}
```

## 9. 并发

### 9.1 协程
```
func fetch_url(url: string) -> string {
    // 模拟网络请求
    sleep(1000)
    return "Response from " + url
}

func main() {
    // 启动协程
    let task = spawn fetch_url("https://example.com")
    
    // 做其他工作...
    
    // 等待结果
    let response = await task
    print(response)
}
```

### 9.2 通道
```
let ch = make(chan int, 10)  // 缓冲通道

// 生产者协程
spawn func() {
    for i in 0..10 {
        ch <- i  // 发送
    }
    close(ch)
}

// 消费者协程
spawn func() {
    for num in ch {  // 接收
        print(num)
    }
}
```

## 10. 内存管理

### 10.1 手动内存管理
```
let ptr = malloc(sizeof(int)) as *int
*ptr = 42
print(*ptr)
free(ptr)
```

### 10.2 自动引用计数
```
// 使用@表示引用计数指针
let s: @string = @"hello"
let s2 = s  // 引用计数增加
```

## 11. 平台特定代码

```
// Windows特定代码
#[platform(windows)]
func win_specific() {
    print("Running on Windows")
}

// Linux特定代码
#[platform(linux)]
func linux_specific() {
    print("Running on Linux")
}

// macOS特定代码
#[platform(macos)]
func macos_specific() {
    print("Running on macOS")
}
```

## 12. 测试代码

```
// 单元测试
#[test]
func test_add() {
    assert(add(2, 3) == 5)
    assert(add(-1, 1) == 0)
}

// 基准测试
#[bench]
func bench_add() {
    let x = 0
    for i in 0..1000000 {
        x = add(x, i)
    }
}
```

## 13. 内置函数

```
print("Hello")          // 打印
len(array)             // 长度
cap(slice)             // 容量
append(slice, item)    // 追加元素
delete(map, key)       // 删除键
panic("error")         // 引发恐慌
recover()              // 恢复恐慌
typeof(x)              // 获取类型
```