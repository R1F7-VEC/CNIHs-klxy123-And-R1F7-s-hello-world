# 第二章 · 四大编程语言核心入门（现在有五个了）

欢迎来到第二章.这一章会比较长，但别怕——你可以跳读.爱看哪个语言就看哪个，不爱看的先跳过，以后需要了再回来.

本章的宗旨是：**能跑就行，先跑起来再说**.不追求什么完美代码，不搞什么代码洁癖，能写出东西、能看到结果、能理解为什么，就可以了.

> **读者须知**：本章的 C++ 示例有时会用 `std::cout`，有时会用 `using namespace std;` 偷懒.实际项目中建议统一用 `std::` 前缀，避免命名冲突.本章为了写起来省事，偶尔不规范，见谅.

> **2026.8.9 更新**：加入了 Golang 部分.现在本章覆盖 Python、Lua、JavaScript、C++、Go 五种语言.

---

## Part 0：如何打注释

注释就是写给人类看的说明.计算机不看，但你的队友、你的老师、未来的你，都会看.

**Python**
```python
# 这是一个单行注释

'''
这是一个多行注释
'''
```

**JavaScript**
```javascript
// 这是一个单行注释

/*
这是一个多行注释
*/
```

**Lua**
```lua
-- 这是一个单行注释

--[[
这是一个多行注释
]]
```

**C++**：和 JavaScript 一样.

**Go**：和 JavaScript 一样.

**规范**：
- 每个包在 `package` 声明前加包注释，描述包的功能
- 函数前加注释，说明功能、参数、返回值
- 结构体及字段加注释
- 统一用单行注释，尽量别用块注释
- 注释不超过 120 个字符
- 中文和英文之间留个空格，看着舒服

**批判性思维**：注释不是越多越好.好的代码本身就能说明“怎么做”，注释应该说明“为什么这么做”.如果代码写得让人看不懂，加注释只是补救，不是解药.

---

## Part 1：Hello World 与基础输入输出

每个程序员的第一段代码，向世界问好，顺便学会获取用户输入.

### Python
```python
print("Hello, World!")
name = input("请输入你的名字：")
print("你好，" + name)
```

解释：`print()` 输出内容到屏幕，`input()` 等待用户输入并返回字符串.

**创造性思考**：如果去掉引号呢？

**实验**：

```
1. 去掉引号

print(hello world)

触发了报错，不符合 Python 语法.
实际上，代码必须要符合语法.否则编译器和解释器不知道你在说啥.

2. 变成变量名

print(hello)

报错了：hello 变量没有值.

实际上，单纯的 `hello` 在 Python 解释器看来是变量而不是字符串.但是 hello 没有值，所以出现了报错.

在 Lua 环境里面这么弄，屏幕上会出现 "nil"（空的）而没有报错.
```

**推理**：所以 `print` 函数会这么做：
1. 读取内容
2. 判断是字符串还是变量
3. 如果是字符串，直接输出
4. 如果是变量，先找它的值，再输出

### Lua
```lua
print("Hello, World!")
io.write("请输入你的名字：")
local name = io.read()
print("你好，" .. name)
```

解释：`io.write()` 输出不自动换行，`io.read()` 读取用户输入，`..` 是字符串连接符.

### JavaScript（Node.js 环境）
```javascript
console.log("Hello, World!");
const readline = require('readline').createInterface({
    input: process.stdin,
    output: process.stdout
});
readline.question("请输入你的名字：", (name) => {
    console.log("你好，" + name);
    readline.close();
});
```

解释：Node.js 中需要引入 `readline` 模块来处理输入，过于繁琐，故不推荐用 JS 去写需要用户输入的脚本.

### JavaScript（浏览器环境）
```javascript
console.log("Hello, World!");
let name = prompt("请输入你的名字：");
console.log("你好，" + name);
```

解释：浏览器中 `prompt()` 可以弹出输入框.

### C++
```cpp
#include <iostream>
#include <string>
using namespace std;
int main() {
    cout << "Hello, World!" << endl;
    string name;
    cout << "请输入你的名字：";
    cin >> name;
    cout << "你好，" << name << endl;
    return 0;
}
```

解释：`cin` 读取用户输入，`cout` 输出，`endl` 换行.

### Go
```go
package main

import "fmt"

func main() {
    fmt.Println("Hello, World!")

    var name string
    fmt.Print("请输入你的名字：")
    fmt.Scanln(&name)
    fmt.Println("你好，" + name)
}
```

解释：`package main` 定义可执行程序包，`func main()` 是程序入口（主函数，类似 C++）.`fmt.Println` 输出并换行，`fmt.Print` 不换行，`fmt.Scanln` 读取一行输入.Go 是静态类型，变量需要声明类型或使用 `:=` 推导.

---

## Part 2：变量与数据类型

变量是给数据的标签，不同的数据类型决定了数据可以做什么操作.

### 基本数据类型

| 类型 | 示例 | 说明 |
|------|------|------|
| 整数（int） | `67` | 没有小数部分的数 |
| 浮点数（float/double） | `3.14159` | 有小数部分的数 |
| 布尔值（bool） | `true / false` | 真的或假的 |
| 字符串（string） | `"dick"` | 用引号括起来的文本 |

### Python
```python
age = 67               # 整数
price = 67.69          # 浮点数
is_student = True      # 布尔值
name = "dick"          # 字符串
print(age, price, is_student, name)
```

解释：Python 的变量不需要声明类型，直接赋值即可，动态类型.

### Lua
```lua
local age = 67
local price = 67.69
local is_student = true
local name = "dick"
print(age, price, is_student, name)
```

解释：Lua 用 `local` 声明局部变量（建议使用），也是动态类型.

### JavaScript
```javascript
let age = 67;
let price = 67.69;
let is_student = true;
let name = "dick";
console.log(age, price, is_student, name);
```

解释：`let` 声明块级变量，JavaScript 也是动态类型.

### C++
```cpp
int age = 67;                   // 整数
double price = 67.69;           // 双精度浮点数
bool is_student = true;         // 布尔值
std::string name = "dick";      // 字符串（需包含头文件 <string>）
std::cout << age << " " << price << " " << is_student << " " << name << std::endl;
// 输出为 1（true）或 0（false），若想显示 true/false 文本，可用 std::boolalpha
```

解释：C++ 是静态类型，变量声明时必须指定类型.

### Go
```go
package main

import "fmt"

func main() {
    age := 67               // 整数，类型推导
    var price float64 = 67.69 // 浮点数，显式声明
    isStudent := true       // 布尔值
    var name string = "dick" // 字符串
    fmt.Println(age, price, isStudent, name)
}
```

解释：Go 支持类型推导（`:=`），也支持显式声明（`var 变量名 类型 = 值`）.基本类型：`int`、`float64`、`bool`、`string`.变量声明后必须使用，否则编译结果扇你一巴掌.

**逻辑推理**：为什么有些语言需要声明类型，有些不需要？静态类型（C++、Go）在编译时检查类型错误，程序跑得更快但写起来更麻烦；动态类型（Python、Lua、JS）写起来自由但运行时才能发现类型错误.这不是谁好谁坏，是取舍不同.

---

## Part 3：基本运算符

运算符用于对数据进行计算和比较.

### 3.1 算术运算符

加法 `+`，减法 `-`，乘法 `*`，除法 `/`，取余 `%`.

**Python**
```python
a = 10
b = 3
print(a + b)   # 13
print(a - b)   # 7
print(a * b)   # 30
print(a / b)   # 3.333...
print(a % b)   # 1（10除以3余1）
```

**Lua**
```lua
local a, b = 10, 3
print(a + b)
print(a - b)
print(a * b)
print(a / b)
print(a % b)
```

**JavaScript**
```javascript
let a = 10, b = 3;
console.log(a + b);
console.log(a - b);
console.log(a * b);
console.log(a / b);
console.log(a % b);
```

**C++**
```cpp
int a = 10, b = 3;
std::cout << a + b << std::endl;
std::cout << a - b << std::endl;
std::cout << a * b << std::endl;
std::cout << a / b << std::endl;              // 整数除法，结果为3
std::cout << (double)a / b << std::endl;      // 含小数除法
std::cout << a % b << std::endl;
```

解释：C++ 中整数除以整数结果还是整数，如需小数需用浮点数.

**Go**
```go
package main

import "fmt"

func main() {
    a, b := 10, 3
    fmt.Println(a + b)                    // 13
    fmt.Println(a - b)                    // 7
    fmt.Println(a * b)                    // 30
    fmt.Println(a / b)                    // 3（整数除法）
    fmt.Println(float64(a) / float64(b))  // 3.333...（转换为浮点数）
    fmt.Println(a % b)                    // 1（取余）
}
```

### 3.2 比较运算符

等于 `==`，不等于 `!=`，大于 `>`，小于 `<`，大于等于 `>=`，小于等于 `<=`.

**Python**
```python
x = 5
y = 10
print(x == y)   # False
print(x != y)   # True
print(x < y)    # True
```

**Lua**
```lua
local x, y = 5, 10
print(x == y)
print(x ~= y)   -- Lua 的不等号是 ~=
print(x < y)
```

**JavaScript**
```javascript
let x = 5, y = 10;
console.log(x == y);
console.log(x != y);
console.log(x < y);
```

**C++**
```cpp
int x = 5, y = 10;
std::cout << (x == y) << std::endl;  // 输出0（假）
std::cout << (x != y) << std::endl;  // 输出1（真）
std::cout << (x < y) << std::endl;   // 输出1（真）
```

**Go**
```go
x, y := 5, 10
fmt.Println(x == y) // false
fmt.Println(x != y) // true
fmt.Println(x < y)  // true
```

### 3.3 逻辑运算符

与：`and`（Python/Lua），`&&`（JS/C++/Go）
或：`or`（Python/Lua），`||`（JS/C++/Go）
非：`not`（Python/Lua），`!`（JS/C++/Go）

**Python**
```python
has_ticket = True
has_id = False
if has_ticket and has_id:
    print("可以入场")
else:
    print("不能入场")
```

解释：`and` 要求两个条件都成立.

**Go** 的运算符和 C++ 一样，`&&`、`||`、`!`.整数除法需显式转换才能得到浮点数结果.

**批判性思维**：`and`/`or` 有一个“短路”特性——`and` 左边为假，右边不执行；`or` 左边为真，右边不执行.这可以用来写保护性代码，比如 `if (ptr != null && ptr->value > 0)`，如果 `ptr` 是空，后面的就不会执行，不会崩溃.

---

## Part 4：字符串操作

字符串是文本，可以拼接、切割、查找、替换.

### Python
```python
text = "Hello, World!"
print(len(text))           # 13（长度）
print(text.upper())        # HELLO, WORLD!
print(text.lower())        # hello, world!
print(text[0])             # H（索引从0开始）
print(text[7:12])          # World（切片，左闭右开）
print(text.replace("World", "Python"))  # 替换
print("Hello" + " " + "World")          # 拼接
```

### Lua
```lua
local text = "Hello, World!"
print(#text)                -- 13（长度，Lua 用 # 取长度）
print(string.upper(text))   -- HELLO, WORLD!
print(string.lower(text))   -- hello, world!
print(string.sub(text, 1, 1))  -- H（Lua 索引从1开始）
print(string.sub(text, 8, 12)) -- World
print(string.gsub(text, "World", "Lua"))  -- 替换，返回两个值
print("Hello" .. " " .. "World")          -- 拼接用 ..
```

### JavaScript
```javascript
let text = "Hello, World!";
console.log(text.length);              // 13
console.log(text.toUpperCase());       // HELLO, WORLD!
console.log(text.toLowerCase());       // hello, world!
console.log(text[0]);                  // H
console.log(text.substring(7, 12));    // World
console.log(text.replace("World", "JavaScript"));
console.log("Hello" + " " + "World");  // 拼接
```

### C++
```cpp
#include <string>
std::string text = "Hello, World!";
std::cout << text.length() << std::endl;         // 13
std::cout << text[0] << std::endl;               // H
std::cout << text.substr(7, 5) << std::endl;     // World（从索引7开始取5个字符）
std::cout << text + " from C++" << std::endl;    // 拼接
```

解释：C++ 字符串操作需要包含 `<string>` 头文件.

### Go
```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    text := "Hello, World!"
    fmt.Println(len(text))              // 13（字节长度，非字符数）
    fmt.Println(strings.ToUpper(text))  // HELLO, WORLD!
    fmt.Println(strings.ToLower(text))  // hello, world!
    fmt.Println(text[0])                // 72（字节值，非字符）
    fmt.Println(text[7:12])             // World（切片，左闭右开）
    fmt.Println(strings.Replace(text, "World", "Go", -1)) // 替换所有
    fmt.Println("Hello" + " " + "World") // 拼接
}
```

解释：Go 的字符串是只读的字节序列，使用 UTF-8 编码.`len()` 返回字节数，如需字符数需用 `utf8.RuneCountInString()`.切片操作返回子串.`strings` 包提供丰富操作.

**实验**：把 `text[0]` 在不同语言里输出，看看分别是什么.

```
Python:  "H"
Lua:     "H"
JS:      "H"
C++:     "H"
Go:      72（字节值）
```

**推理**：Go 的字符串下标访问返回的是字节值，不是字符.因为 Go 的字符串底层是字节数组.如果你要取字符，得用 `[]rune(text)[0]`.

---

## Part 5：条件判断（if-else）

让程序根据不同情况执行不同的代码块.

### Python
```python
score = 85
if score >= 90:
    grade = "A"
elif score >= 80:
    grade = "B"
elif score >= 70:
    grade = "C"
else:
    grade = "D"
print(grade)
```

解释：从上到下检查条件，满足第一个就执行对应分支，后面的不再检查.

### Lua
```lua
local score = 85
if score >= 90 then
    grade = "A"
elseif score >= 80 then
    grade = "B"
elseif score >= 70 then
    grade = "C"
else
    grade = "D"
end
print(grade)
```

### JavaScript
```javascript
let score = 85;
let grade;
if (score >= 90) {
    grade = "A";
} else if (score >= 80) {
    grade = "B";
} else if (score >= 70) {
    grade = "C";
} else {
    grade = "D";
}
console.log(grade);
```

### C++
```cpp
int score = 85;
std::string grade;
if (score >= 90) {
    grade = "A";
} else if (score >= 80) {
    grade = "B";
} else if (score >= 70) {
    grade = "C";
} else {
    grade = "D";
}
std::cout << grade << std::endl;
```

### Go
```go
package main

import "fmt"

func main() {
    score := 85
    var grade string
    if score >= 90 {
        grade = "A"
    } else if score >= 80 {
        grade = "B"
    } else if score >= 70 {
        grade = "C"
    } else {
        grade = "D"
    }
    fmt.Println(grade)
}
```

解释：Go 的 `if` 条件不用括号，但必须有大括号.`else if` 和 `else` 必须与 `if` 的右花括号在同一行.

**批判性思维**：条件判断的顺序很重要.如果你把 `score >= 80` 写在 `score >= 90` 前面，那 95 分也会被判为 B.这不是语言的 bug，是逻辑错误.写代码之前先想清楚条件的覆盖关系.

---

## Part 6：循环（for, while）

循环用于重复执行某段代码.

### 6.1 for 循环（遍历固定次数）

**Python**
```python
for i in range(5):
    print(i)   # 输出 0 1 2 3 4
```

解释：`range(5)` 生成 0 到 4 的整数序列.

**Lua**
```lua
for i = 0, 4 do
    print(i)
end
```

**JavaScript**
```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}
```

**C++**
```cpp
for (int i = 0; i < 5; i++) {
    std::cout << i << std::endl;
}
```

**Go**
```go
package main

import "fmt"

func main() {
    for i := 0; i < 5; i++ {
        fmt.Println(i)
    }
}
```

### 6.2 while 循环（条件成立时持续执行）

**Python**
```python
count = 0
while count < 5:
    print(count)
    count += 1   # 等价于 count = count + 1
```

**Lua**
```lua
local count = 0
while count < 5 do
    print(count)
    count = count + 1
end
```

**JavaScript**
```javascript
let count = 0;
while (count < 5) {
    console.log(count);
    count++;
}
```

**C++**
```cpp
int count = 0;
while (count < 5) {
    std::cout << count << std::endl;
    count++;
}
```

**Go**
```go
package main

import "fmt"

func main() {
    count := 0
    for count < 5 {
        fmt.Println(count)
        count++
    }
}
```

解释：Go 只有 `for` 一种循环，`for count < 5` 就相当于 `while`.

### 6.3 遍历列表/数组

**Python**
```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

**Lua**
```lua
local fruits = {"apple", "banana", "cherry"}
for i, fruit in ipairs(fruits) do
    print(fruit)
end
```

**JavaScript**
```javascript
let fruits = ["apple", "banana", "cherry"];
for (let fruit of fruits) {
    console.log(fruit);
}
```

**C++（C++11及以上）**
```cpp
#include <vector>
std::vector<std::string> fruits = {"apple", "banana", "cherry"};
for (const auto& fruit : fruits) {
    std::cout << fruit << std::endl;
}
```

**Go**
```go
package main

import "fmt"

func main() {
    fruits := []string{"apple", "banana", "cherry"}
    for _, fruit := range fruits {
        fmt.Println(fruit)
    }
}
```

**创造性思考**：如果循环里修改列表，会发生什么？

**实验**：
```python
fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    fruits.append("orange")
```

这会导致无限循环，因为列表在遍历时不断增长.所以不要在遍历时修改列表.

---

## Part 7：函数定义与调用

函数把一段代码封装起来，可以重复调用.

### Python
```python
def greet(name):
    return "Hello, " + name

print(greet("Alice"))

# 带默认参数的函数
def greet_with_default(name="World"):
    return "Hello, " + name

print(greet_with_default())
```

解释：`def` 定义函数，`return` 返回值.没有 `return` 时返回 `None`.

### Lua
```lua
function greet(name)
    return "Hello, " .. name
end
print(greet("Alice"))

-- Lua 不支持默认参数，可以这样模拟
function greet_with_default(name)
    name = name or "World"
    return "Hello, " .. name
end
print(greet_with_default())
```

解释：Lua 的 `or` 可以用于提供默认值.

### JavaScript
```javascript
function greet(name) {
    return "Hello, " + name;
}
console.log(greet("Alice"));

// 默认参数（ES6）
function greet_with_default(name = "World") {
    return "Hello, " + name;
}
console.log(greet_with_default());
```

### C++
```cpp
#include <string>
std::string greet(std::string name) {
    return "Hello, " + name;
}

// 默认参数（在声明或定义时指定）
std::string greet_with_default(std::string name = "World") {
    return "Hello, " + name;
}

int main() {
    std::cout << greet("Alice") << std::endl;
    std::cout << greet_with_default() << std::endl;
    return 0;
}
```

### Go
```go
package main

import "fmt"

func greet(name string) string {
    return "Hello, " + name
}

// 多返回值
func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("除数不能为0")
    }
    return a / b, nil
}

func main() {
    fmt.Println(greet("Alice"))

    // 默认参数：Go 不支持默认参数，可重载或使用结构体
    // 多返回值处理
    result, err := divide(10, 2)
    if err != nil {
        fmt.Println(err)
    } else {
        fmt.Println(result)
    }
}
```

解释：Go 函数用 `func` 关键字，参数类型后置，返回值类型也在后面.支持多返回值，常用于返回结果和错误.Go 没有默认参数，可用可变参数或结构体模拟.

**逻辑推理**：为什么 Go 要设计多返回值？因为 Go 没有异常机制，错误必须通过返回值传递.`result, err := divide(10, 0)` 强迫你处理错误，而不是让它悄悄崩溃.这是一种设计哲学：错误是程序的一部分，必须显式处理.

---

## Part 8：列表 / 数组

列表用于存储一组有序的数据，可以通过索引访问.

### Python
```python
fruits = ["apple", "banana", "cherry"]
print(fruits[0])          # apple（索引从0开始）
fruits.append("orange")   # 添加元素
print(fruits)             # ['apple', 'banana', 'cherry', 'orange']
fruits.remove("banana")   # 删除元素
print(fruits)             # ['apple', 'cherry', 'orange']
```

### Lua
```lua
local fruits = {"apple", "banana", "cherry"}
print(fruits[1])          -- apple（Lua 索引从1开始）
table.insert(fruits, "orange")  -- 添加
print(fruits[2])          -- banana（注意索引变了）
table.remove(fruits, 2)   -- 删除第2个元素
print(fruits[2])          -- cherry
```

### JavaScript
```javascript
let fruits = ["apple", "banana", "cherry"];
console.log(fruits[0]);          // apple
fruits.push("orange");           // 添加
console.log(fruits);             // ['apple', 'banana', 'cherry', 'orange']
fruits.splice(1, 1);             // 删除索引1开始的1个元素
console.log(fruits);             // ['apple', 'cherry', 'orange']
```

### C++（vector）
```cpp
#include <vector>
std::vector<std::string> fruits = {"apple", "banana", "cherry"};
std::cout << fruits[0] << std::endl;  // apple
fruits.push_back("orange");
fruits.erase(fruits.begin() + 1);     // 删除索引1的元素
std::cout << fruits[1] << std::endl;  // cherry
```

### Go
```go
package main

import "fmt"

func main() {
    // 数组（固定长度）
    var arr [3]int = [3]int{1, 2, 3}
    fmt.Println(arr[0]) // 1

    // 切片（动态数组）
    fruits := []string{"apple", "banana", "cherry"}
    fmt.Println(fruits[0]) // apple
    fruits = append(fruits, "orange")
    fmt.Println(fruits) // [apple banana cherry orange]

    // 删除元素（切片技巧）
    fruits = append(fruits[:1], fruits[2:]...) // 删除索引1
    fmt.Println(fruits) // [apple cherry orange]
}
```

解释：Go 的数组长度固定，切片更常用.切片使用 `[]T` 声明，`append` 添加元素，删除需结合切片操作.

- `s[:i]` 取删除位置前的部分（索引 0 到 i-1）
- `s[i+1:]` 取删除位置后的部分（索引 i+1 到末尾）
- `...` 把后面的部分展开成一个个元素拼接到前面
- 然后把拼接后的结果重新赋值给 `s`

**批判性思维**：为什么 Go 删除元素要这么麻烦？因为 Go 的切片是“轻量级视图”，它不提供内置的删除方法，是为了保持语言简洁.其他语言把删除封装成方法，方便但隐藏了底层操作.Go 让你看到底层，代价是写起来更啰嗦.这是设计取舍.

---

## Part 9：字典 / 映射 / 对象

字典用于存储键值对，通过键来访问值.

### Python（字典）
```python
person = {"name": "Alice", "age": 25, "city": "Beijing"}
print(person["name"])     # Alice
person["age"] = 26        # 修改值
person["gender"] = "F"    # 添加新键值对
print(person)
```

### Lua（表）
```lua
local person = {name = "Alice", age = 25, city = "Beijing"}
print(person.name)        -- Alice（点号访问）
print(person["age"])      -- 25（方括号访问）
person.age = 26
person.gender = "F"
print(person.gender)
```

### JavaScript（对象）
```javascript
let person = {name: "Alice", age: 25, city: "Beijing"};
console.log(person.name);        // Alice
person.age = 26;                 // 修改
person.gender = "F";             // 添加
```

### C++（map）
```cpp
#include <map>
#include <string>
std::map<std::string, std::string> person;
person["name"] = "Alice";
person["age"] = "25";
person["city"] = "Beijing";
std::cout << person["name"] << std::endl;
```

### Go（map）
```go
package main

import "fmt"

func main() {
    person := map[string]string{
        "name": "Alice",
        "age":  "25",
        "city": "Beijing",
    }
    fmt.Println(person["name"]) // Alice
    person["age"] = "26"
    person["gender"] = "F"
    fmt.Println(person)

    // 检查键是否存在
    value, ok := person["age"]
    if ok {
        fmt.Println("age 存在:", value)
    }
}
```

解释：Go 的 map 使用 `map[KeyType]ValueType` 声明，用 `make` 或字面量创建.访问不存在的键返回零值，可通过 `ok` 判断是否存在.

**实验**：访问一个不存在的键，看看各语言返回什么.

```
Python:  报错 KeyError
Lua:     nil
JS:      undefined
C++:     插入一个默认值（如果用了 []）
Go:      返回零值（空字符串），不报错
```

**推理**：不同语言对“不存在的键”处理不同.Python 和 C++ 的 `[]` 会报错/插入，JS 和 Lua 返回空值，Go 返回零值.写代码时要注意这些区别.

---

## Part 10：文件操作（基础读写）

读取和写入文件是程序保存数据的基本方式.

### Python
```python
with open("test.txt", "w") as f:
    f.write("Hello, file!\n")
    f.write("第二行内容")

with open("test.txt", "r") as f:
    content = f.read()

with open("test.txt", "a") as f:
    f.write("追加内容")

print(content)
```

解释：`open` 的第一个参数是文件名，第二个参数是模式（`"w"` 写入，`"r"` 读取，`"a"` 追加）.`with` 语句会自动关闭文件.

### Lua
```lua
-- 写入
local file = io.open("test.txt", "w")
if file then
    file:write("Hello, file!\n")
    file:write("第二行内容")
    file:close()
end

-- 读取
local file = io.open("test.txt", "r")
if file then
    local content = file:read("*all")
    print(content)
    file:close()
end

-- 追加
local file = io.open("test.txt", "a")
if file then
    file:write("追加一行内容\n")
    file:close()
end
```

### JavaScript（Node.js）
```javascript
const fs = require('fs');

// 异步写法（非阻塞）
fs.writeFile('async_demo.txt', '第一行内容（异步写入）\n', (err) => {
    if (err) throw err;
    console.log('[异步] 写入完成');
});

fs.appendFile('async_demo.txt', '第二行内容（异步追加）\n', (err) => {
    if (err) throw err;
    console.log('[异步] 追加完成');
});

fs.readFile('async_demo.txt', 'utf8', (err, data) => {
    if (err) throw err;
    console.log('[异步] 读取内容：\n' + data);
});

// 同步写法（阻塞）
try {
    fs.writeFileSync('sync_demo.txt', '第一行内容（同步写入）\n');
    console.log('[同步] 写入完成');

    fs.appendFileSync('sync_demo.txt', '第二行内容（同步追加）\n');
    console.log('[同步] 追加完成');

    const data = fs.readFileSync('sync_demo.txt', 'utf8');
    console.log('[同步] 读取内容：\n' + data);
} catch (err) {
    console.error('同步操作出错：', err);
}
```

### C++（fstream）
```cpp
#include <fstream>
#include <string>

// 写入
std::ofstream out("test.txt");
out << "Hello, file!\n";
out << "第二行内容";
out.close();

// 读取
std::ifstream in("test.txt");
std::string line;
while (std::getline(in, line)) {
    std::cout << line << std::endl;
}
in.close();

// 追加
std::ofstream out("log.txt", std::ios::app);
out << "追加的内容\n";
out.close();
```

### Go
```go
package main

import (
    "fmt"
    "os"
)

func main() {
    // 写入（覆盖）
    err := os.WriteFile("test.txt", []byte("Hello, file!\n第二行内容"), 0644)
    if err != nil {
        fmt.Println(err)
        return
    }

    // 读取
    data, err := os.ReadFile("test.txt")
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(string(data))

    // 追加（使用 os.OpenFile）
    f, err := os.OpenFile("test.txt", os.O_APPEND|os.O_WRONLY, 0644)
    if err != nil {
        fmt.Println(err)
        return
    }
    defer f.Close()
    f.WriteString("追加内容\n")
}
```

解释：Go 用 `os.WriteFile` 和 `os.ReadFile` 简化读写.追加需用 `os.OpenFile` 指定 `os.O_APPEND` 标志，`defer` 确保文件关闭.

---

## Part 11：错误处理（异常）

程序运行时可能会出错，错误处理让程序能够优雅地应对问题.

### Python（try-except）
```python
try:
    num = int(input("请输入一个数字："))
    print(100 / num)
except ValueError:
    print("输入的不是有效数字")
except ZeroDivisionError:
    print("不能除以0")
except Exception as e:
    print("发生其他错误：" + str(e))
```

解释：`try` 块中放可能出错的代码，`except` 捕获特定异常，`Exception` 捕获所有异常.

### Lua（pcall）
```lua
local function safe_divide(a, b)
    if b == 0 then error("不能除以0") end
    return a / b
end

local ok, result = pcall(safe_divide, 10, 0)
if ok then
    print(result)
else
    print("错误：" .. result)
end
```

解释：`pcall` 执行一个函数，返回是否成功和结果或错误信息.

### JavaScript（try-catch）
```javascript
try {
    let num = parseInt(prompt("请输入一个数字："));
    console.log(100 / num);
} catch (error) {
    console.log("发生错误：" + error.message);
}
```

### C++（try-catch）
```cpp
#include <stdexcept>
try {
    int num;
    std::cin >> num;
    if (std::cin.fail()) {
        throw std::invalid_argument("无效的数字");
    }
    if (num == 0) {
        throw std::runtime_error("不能除以0");
    }
    std::cout << 100.0 / num << std::endl;
} catch (const std::exception& e) {
    std::cout << "错误：" << e.what() << std::endl;
}
```

### Go
```go
package main

import (
    "fmt"
    "strconv"
)

func main() {
    var input string
    fmt.Print("请输入一个数字：")
    fmt.Scanln(&input)

    num, err := strconv.Atoi(input)
    if err != nil {
        fmt.Println("输入的不是有效数字")
        return
    }

    if num == 0 {
        fmt.Println("不能除以0")
        return
    }

    fmt.Println(100.0 / float64(num))
}
```

解释：Go 没有异常，使用返回错误的方式.通常函数返回 `(result, error)`，调用者检查 `err` 是否为 `nil`.`defer` 可用于资源清理.

**批判性思维**：异常机制（try-catch）和错误返回值（Go）是两种哲学.异常机制让错误处理代码和正常代码分离，但可能忘记捕获；错误返回值强迫你每次都检查，但代码更啰嗦.没有优劣，看场景.

---

## Part 12：模块与包（代码组织）

模块是把一组相关功能放在一个文件中，包是模块的集合.

### Python（import）
```python
# math_utils.py
def add(a, b):
    return a + b

def multiply(a, b):
    return a * b

# main.py
import math_utils
print(math_utils.add(3, 5))
print(math_utils.multiply(3, 5))

# 也可以导入特定函数
from math_utils import add, multiply
print(add(3, 5))
```

### Lua（require）
```lua
-- math_utils.lua
local M = {}
function M.add(a, b)
    return a + b
end
function M.multiply(a, b)
    return a * b
end
return M

-- 在另一个文件中
local math_utils = require("math_utils")
print(math_utils.add(3, 5))
```

### JavaScript（ES6模块）
```javascript
// math_utils.js
export function add(a, b) {
    return a + b;
}
export function multiply(a, b) {
    return a * b;
}

// 导入
import { add, multiply } from './math_utils.js';
console.log(add(3, 5));
```

### C++（头文件与源文件）
```cpp
// math_utils.h
#ifndef MATH_UTILS_H
#define MATH_UTILS_H
int add(int a, int b);
int multiply(int a, int b);
#endif

// math_utils.cpp
#include "math_utils.h"
int add(int a, int b) { return a + b; }
int multiply(int a, int b) { return a * b; }

// main.cpp
#include <iostream>
#include "math_utils.h"
int main() {
    std::cout << add(3, 5) << std::endl;
    return 0;
}
```

### Go
```go
// math_utils.go
package math_utils

func Add(a, b int) int {
    return a + b
}

func Multiply(a, b int) int {
    return a * b
}

// main.go
package main

import (
    "fmt"
    "math_utils"
)

func main() {
    fmt.Println(math_utils.Add(3, 5))
    fmt.Println(math_utils.Multiply(3, 5))
}
```

解释：Go 的包通过目录组织，包名与目录名一致.首字母大写表示公开（导出），小写为私有.使用 `import` 导入包，路径基于模块名.

---

## Part 13：面向对象基础（类与对象）

类是创建对象的蓝图，对象是类的实例.

### Python
```python
class Dog:
    def __init__(self, name, age):
        self.name = name
        self.age = age
    def bark(self):
        return self.name + " says woof!"
    def get_age(self):
        return self.age

my_dog = Dog("Rex", 3)
print(my_dog.bark())      # Rex says woof!
print(my_dog.get_age())   # 3
```

### Lua（使用表模拟）
```lua
Dog = {}
function Dog:new(name, age)
    local obj = {name = name, age = age}
    setmetatable(obj, self)
    self.__index = self
    return obj
end
function Dog:bark()
    return self.name .. " says woof!"
end
function Dog:get_age()
    return self.age
end

local my_dog = Dog:new("Rex", 3)
print(my_dog:bark())
print(my_dog:get_age())
```

### JavaScript（ES6 class）
```javascript
class Dog {
    constructor(name, age) {
        this.name = name;
        this.age = age;
    }
    bark() {
        return this.name + " says woof!";
    }
    getAge() {
        return this.age;
    }
}
let my_dog = new Dog("Rex", 3);
console.log(my_dog.bark());
console.log(my_dog.getAge());
```

### C++
```cpp
#include <string>
class Dog {
private:
    std::string name;
    int age;
public:
    Dog(std::string n, int a) : name(n), age(a) {}
    std::string bark() { return name + " says woof!"; }
    int getAge() { return age; }
};

int main() {
    Dog my_dog("Rex", 3);
    std::cout << my_dog.bark() << std::endl;
    std::cout << my_dog.getAge() << std::endl;
    return 0;
}
```

### Go（结构体与方法）
```go
package main

import "fmt"

type Dog struct {
    name string
    age  int
}

// 方法（值接收者）
func (d Dog) Bark() string {
    return d.name + " says woof!"
}

// 方法（指针接收者，可修改）
func (d *Dog) SetAge(newAge int) {
    d.age = newAge
}

func (d Dog) GetAge() int {
    return d.age
}

func main() {
    myDog := Dog{name: "Rex", age: 3}
    fmt.Println(myDog.Bark())
    fmt.Println(myDog.GetAge())
    myDog.SetAge(4)
    fmt.Println(myDog.GetAge())
}
```

解释：Go 没有类，用结构体 `struct` 封装数据，方法绑定在结构体上.值接收者不修改原对象，指针接收者可修改.

**创造性思考**：为什么 Go 不设计类？因为 Go 的设计者认为“继承”太复杂，用组合代替继承更简单.Go 的哲学是：少即是多.

---

## Part 14：异步编程（回调 / Promise / 协程）

异步编程让程序在等待耗时操作时不会阻塞.

### JavaScript（回调）
```javascript
function fetchData(callback) {
    setTimeout(() => {
        callback("数据已加载");
    }, 1000);
}
fetchData((data) => {
    console.log(data);
});
```

解释：`setTimeout` 模拟延迟，回调函数在1秒后执行.

### JavaScript（Promise）
```javascript
function fetchData() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("数据已加载");
        }, 1000);
    });
}
fetchData().then((data) => {
    console.log(data);
});
```

### JavaScript（async/await）
```javascript
async function getData() {
    let data = await fetchData();
    console.log(data);
}
getData();
```

### Python（asyncio协程）
```python
import asyncio

async def fetch_data():
    await asyncio.sleep(1)
    return "数据已加载"

async def main():
    data = await fetch_data()
    print(data)

asyncio.run(main())
```

### Go（goroutine & channel）
```go
package main

import (
    "fmt"
    "time"
)

func fetchData(ch chan string) {
    time.Sleep(1 * time.Second)
    ch <- "数据已加载"
}

func main() {
    ch := make(chan string)
    go fetchData(ch)

    fmt.Println("等待数据...")
    data := <-ch
    fmt.Println(data)
}
```

解释：Go 使用 goroutine（轻量级线程）实现并发，`go` 关键字启动.channel 用于 goroutine 间通信，`<-` 操作符发送或接收.

---

## Part 15：DOM 操作（仅前端 JavaScript）

DOM 是网页的文档对象模型，JavaScript 可以通过 DOM 操作网页内容.

### 获取元素
```javascript
// 通过ID获取
let title = document.getElementById("title");
// 通过类名获取
let items = document.getElementsByClassName("item");
// 通过CSS选择器获取
let firstItem = document.querySelector(".item");
let allItems = document.querySelectorAll(".item");
```

### 修改内容和样式
```javascript
let element = document.getElementById("myDiv");
element.textContent = "新的文本内容";
element.innerHTML = "<strong>加粗文本</strong>";
element.style.color = "red";
element.style.fontSize = "20px";
```

### 添加和删除元素
```javascript
// 添加
let newDiv = document.createElement("div");
newDiv.textContent = "我是新元素";
document.body.appendChild(newDiv);

// 删除
let oldDiv = document.getElementById("oldDiv");
oldDiv.remove();
```

### 事件监听
```javascript
let button = document.getElementById("myButton");
button.addEventListener("click", function() {
    alert("按钮被点击了！");
});
```

---

## Part 16：系统操作（环境变量、执行命令、文件系统高级）

获取系统信息，执行外部命令，进行更复杂的文件操作.

### Python（os 和 subprocess 模块）
```python
import os
import subprocess

# 获取环境变量
path = os.environ.get("PATH")
print(path)

# 设置环境变量（仅当前进程）
os.environ["MY_VAR"] = "hello"
print(os.environ["MY_VAR"])

# 执行系统命令
os.system("echo Hello from system")

# 更安全的执行方式
result = subprocess.run(["ls", "-l"], capture_output=True, text=True)
print(result.stdout)

# 创建删除文件夹
os.mkdir("new_folder")
os.rmdir("new_folder")
```

### Lua（os 库）
```lua
-- 获取环境变量
local path = os.getenv("PATH")
print(path)

-- 设置环境变量
os.execute("export MY_VAR=hello")

-- 执行系统命令
os.execute("echo Hello from system")
```

### JavaScript（Node.js）
```javascript
const os = require('os');
const { exec } = require('child_process');

// 系统信息
console.log(os.platform());
console.log(os.cpus());

// 环境变量
console.log(process.env.PATH);
process.env.MY_VAR = "hello";

// 执行命令
exec('echo Hello from system', (error, stdout) => {
    console.log(stdout);
});
```

### C++（版本 >= C++17）
```cpp
#include <iostream>
#include <cstdlib>
#include <string>
#include <filesystem>
namespace fs = std::filesystem;

int main() {
    // 环境变量
    const char* path = std::getenv("PATH");
    if (path) {
        std::cout << "PATH: " << path << std::endl;
    }

    // 设置环境变量
#ifdef _WIN32
    _putenv_s("MY_VAR", "hello");
#else
    setenv("MY_VAR", "hello", 1);
#endif

    // 执行系统命令
    int ret = std::system("echo Hello from system");

    // 文件系统高级操作
    fs::path dir = "new_folder";
    if (fs::create_directory(dir)) {
        std::cout << "目录创建成功" << std::endl;
    }
    fs::remove(dir);

    // 递归创建多级目录
    fs::path nested = "a/b/c";
    if (fs::create_directories(nested)) {
        fs::remove_all("a");
    }

    return 0;
}
```

### Go
```go
package main

import (
    "fmt"
    "os"
    "os/exec"
)

func main() {
    // 环境变量
    path := os.Getenv("PATH")
    fmt.Println("PATH:", path)
    os.Setenv("MY_VAR", "hello")
    fmt.Println("MY_VAR:", os.Getenv("MY_VAR"))

    // 执行命令
    cmd := exec.Command("echo", "Hello from system")
    output, err := cmd.Output()
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(string(output))

    // 创建/删除目录
    os.Mkdir("new_folder", 0755)
    os.Remove("new_folder")
}
```

解释：`os` 包提供环境变量、文件操作，`os/exec` 执行外部命令.权限使用 Unix 权限数字（如 `0755`）.

---

## Part 17：网络请求基础（HTTP）

向网络上的服务器发送请求并获取数据.

### Python（requests 库）
```python
import requests
response = requests.get("https://api.github.com")
print(response.status_code)   # 200
print(response.json()["current_user_url"])
```

解释：`requests` 库需要安装（`pip install requests`），`get` 发送 GET 请求.

### Lua（socket 库，需安装）
```lua
local http = require("socket.http")
local response, status = http.request("https://api.github.com")
print(status)
```

### JavaScript（浏览器 fetch）
```javascript
fetch('https://api.github.com')
    .then(response => response.json())
    .then(data => console.log(data))
    .catch(err => console.error(err));
```

解释：`fetch` 返回 Promise，`response.json()` 解析 JSON 数据.

### JavaScript（Node.js 使用 axios）
```javascript
const axios = require('axios');
axios.get('https://api.github.com')
    .then(response => console.log(response.data))
    .catch(err => console.error(err));
```

### Go
```go
package main

import (
    "fmt"
    "io"
    "net/http"
)

func main() {
    resp, err := http.Get("https://api.github.com")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer resp.Body.Close()
    body, _ := io.ReadAll(resp.Body)
    fmt.Println("状态码:", resp.StatusCode)
    fmt.Println("响应内容:", string(body)[:200])
}
```

解释：`net/http` 包提供 HTTP 客户端.`http.Get` 发送 GET 请求，`defer resp.Body.Close()` 关闭响应体，`io.ReadAll` 读取全部内容.

---

## Part 18：数据库简单操作（SQLite）

使用 SQLite 存储和查询数据，无需安装额外的数据库服务器.

### Python（sqlite3）
```python
import sqlite3
conn = sqlite3.connect('test.db')
cursor = conn.cursor()

# 创建表
cursor.execute('''CREATE TABLE IF NOT EXISTS users
(id INTEGER PRIMARY KEY, name TEXT, age INTEGER)''')

# 插入数据
cursor.execute('INSERT INTO users (name, age) VALUES (?, ?)', ("Alice", 25))
cursor.execute('INSERT INTO users (name, age) VALUES (?, ?)', ("Bob", 30))
conn.commit()

# 查询数据
cursor.execute('SELECT * FROM users WHERE age > ?', (20,))
rows = cursor.fetchall()
for row in rows:
    print(row)
conn.close()
```

解释：`?` 是占位符，传递参数可以防止 SQL 注入.

### Lua（luasqlite3，需安装）
```lua
local sqlite3 = require("sqlite3")
local db = sqlite3.open('test.db')
db:exec[[CREATE TABLE IF NOT EXISTS users (id INTEGER PRIMARY KEY, name TEXT, age INTEGER)]]
db:exec[[INSERT INTO users (name, age) VALUES ('Alice', 25)]]
db:exec[[INSERT INTO users (name, age) VALUES ('Bob', 30)]]
for row in db:rows("SELECT * FROM users WHERE age > 20") do
    print(row.id, row.name, row.age)
end
db:close()
```

### JavaScript（Node.js sqlite3）
```javascript
const sqlite3 = require('sqlite3').verbose();
let db = new sqlite3.Database('test.db');
db.run("CREATE TABLE IF NOT EXISTS users ( id INTEGER PRIMARY KEY, name TEXT, age INTEGER )");
db.run("INSERT INTO users (name, age) VALUES (?, ?)", ['Alice', 25]);
db.run("INSERT INTO users (name, age) VALUES (?, ?)", ['Bob', 30]);
db.all("SELECT * FROM users WHERE age > ?", [20], (err, rows) => {
    console.log(rows);
});
db.close();
```

### C++（使用 SQLite 的 C 接口，需安装库）
```cpp
#include <iostream>
#include <sqlite3.h>
#include <string>

static int callback(void* data, int argc, char** argv, char** azColName) {
    for (int i = 0; i < argc; i++) {
        std::cout << azColName[i] << " = " << (argv[i] ? argv[i] : "NULL") << "\t";
    }
    std::cout << std::endl;
    return 0;
}

int main() {
    sqlite3* db;
    char* errMsg = nullptr;
    int rc;

    rc = sqlite3_open("test.db", &db);
    if (rc != SQLITE_OK) {
        std::cerr << "无法打开数据库: " << sqlite3_errmsg(db) << std::endl;
        sqlite3_close(db);
        return 1;
    }

    const char* createSQL = R"(
        CREATE TABLE IF NOT EXISTS users (
            id INTEGER PRIMARY KEY AUTOINCREMENT,
            name TEXT NOT NULL,
            age INTEGER
        );
    )";
    rc = sqlite3_exec(db, createSQL, nullptr, nullptr, &errMsg);
    if (rc != SQLITE_OK) {
        std::cerr << "创建表失败: " << errMsg << std::endl;
        sqlite3_free(errMsg);
        sqlite3_close(db);
        return 1;
    }

    sqlite3_stmt* stmt;
    const char* insertSQL = "INSERT INTO users (name, age) VALUES (?, ?);";
    sqlite3_prepare_v2(db, insertSQL, -1, &stmt, nullptr);

    sqlite3_bind_text(stmt, 1, "Alice", -1, SQLITE_STATIC);
    sqlite3_bind_int(stmt, 2, 25);
    sqlite3_step(stmt);
    sqlite3_reset(stmt);

    sqlite3_bind_text(stmt, 1, "Bob", -1, SQLITE_STATIC);
    sqlite3_bind_int(stmt, 2, 30);
    sqlite3_step(stmt);
    sqlite3_finalize(stmt);

    const char* selectSQL = "SELECT * FROM users WHERE age > 20;";
    sqlite3_exec(db, selectSQL, callback, nullptr, &errMsg);

    sqlite3_close(db);
    return 0;
}
```

解释：SQLite 的 C 接口比较底层，需要手动管理资源.`?` 是占位符，用 `sqlite3_bind_*` 绑定具体值，可有效防止 SQL 注入.

### Go
```go
package main

import (
    "database/sql"
    "fmt"
    _ "github.com/mattn/go-sqlite3"
)

func main() {
    db, err := sql.Open("sqlite3", "test.db")
    if err != nil {
        fmt.Println(err)
        return
    }
    defer db.Close()

    db.Exec(`CREATE TABLE IF NOT EXISTS users (
        id INTEGER PRIMARY KEY AUTOINCREMENT,
        name TEXT,
        age INTEGER
    )`)

    db.Exec("INSERT INTO users (name, age) VALUES (?, ?)", "Alice", 25)

    rows, err := db.Query("SELECT id, name, age FROM users WHERE age > ?", 20)
    if err != nil {
        fmt.Println(err)
        return
    }
    defer rows.Close()
    for rows.Next() {
        var id int
        var name string
        var age int
        rows.Scan(&id, &name, &age)
        fmt.Println(id, name, age)
    }
}
```

解释：使用 `database/sql` 标准库和 `mattn/go-sqlite3` 驱动.`?` 是参数占位符，`Query` 执行查询并返回行集，`Scan` 将列值映射到变量.

---

## Part 19：多线程 / 并发入门

多线程允许程序同时执行多个任务，提高效率.

### Python（threading 模块）
```python
import threading
import time

def worker(name):
    print(f"线程{name}开始")
    time.sleep(2)
    print(f"线程{name}结束")

threads = []
for i in range(3):
    t = threading.Thread(target=worker, args=(i,))
    threads.append(t)
    t.start()

for t in threads:
    t.join()

print("所有线程结束")
```

解释：多线程适用于 IO 密集型任务，CPU 密集型任务建议使用多进程.

### Lua（协同程序 coroutine）
```lua
function worker(name)
    print("协程" .. name .. "开始")
    coroutine.yield()
    print("协程" .. name .. "结束")
end

local co1 = coroutine.create(worker)
local co2 = coroutine.create(worker)
coroutine.resume(co1, "A")
coroutine.resume(co2, "B")
coroutine.resume(co1)
coroutine.resume(co2)
```

解释：Lua 的协程是协作式多任务，需要显式交出控制权.

### JavaScript（Web Workers，浏览器环境）
```javascript
// 主线程
const worker = new Worker('worker.js');
worker.postMessage({data: '开始计算'});
worker.onmessage = function(e) {
    console.log('收到结果:', e.data);
};

// worker.js
self.onmessage = function(e) {
    let result = 0;
    for (let i = 0; i < 1000000000; i++) {
        result += i;
    }
    self.postMessage(result);
};
```

### JavaScript（Node.js worker_threads）
```javascript
const { Worker, isMainThread } = require('worker_threads');
if (isMainThread) {
    const worker = new Worker(__filename);
    worker.on('message', (msg) => console.log('结果:', msg));
} else {
    let sum = 0;
    for (let i = 0; i < 1000000000; i++) sum += i;
    parentPort.postMessage(sum);
}
```

### C++（std::thread，C++11及以上）
```cpp
#include <thread>
#include <iostream>

void worker(int id) {
    std::cout << "线程" << id << "开始" << std::endl;
    std::this_thread::sleep_for(std::chrono::seconds(2));
    std::cout << "线程" << id << "结束" << std::endl;
}

int main() {
    std::thread t1(worker, 1);
    std::thread t2(worker, 2);
    t1.join();
    t2.join();
    std::cout << "所有线程结束" << std::endl;
    return 0;
}
```

### Go
```go
package main

import (
    "fmt"
    "sync"
    "time"
)

func worker(id int, wg *sync.WaitGroup) {
    defer wg.Done()
    fmt.Printf("线程 %d 开始\n", id)
    time.Sleep(2 * time.Second)
    fmt.Printf("线程 %d 结束\n", id)
}

func main() {
    var wg sync.WaitGroup
    for i := 0; i < 3; i++ {
        wg.Add(1)
        go worker(i, &wg)
    }
    wg.Wait()
    fmt.Println("所有线程结束")
}
```

解释：Go 通过 goroutine 实现并发，`sync.WaitGroup` 等待所有 goroutine 完成.`go` 关键字启动 goroutine，`defer wg.Done()` 在函数退出时通知完成.

---

## 数学拓展

这是一个数学拓展模块.读者接下来将在研究编程的同时学习高中数学竞赛入门内容.

**学习目标**：
- 理解函数迭代的数学定义与符号体系
- 能够用代码实现简单的函数迭代过程
- 能够用迭代思想解决简单的函数方程问题
- 初步接触数学竞赛中的“不动点”思想

### 1. 什么是函数迭代

你已经知道函数是一种“输入 → 输出”的映射规则.如果把一个函数的输出再次作为这个函数的输入，就构成了“迭代”.

令 `f(x)` 的定义域包含值域，则：

```
f¹(x) = f(x)
f²(x) = f(f(x))
f³(x) = f(f(f(x)))
...
fⁿ(x) = f(f(...f(x)...))
```

`fⁿ` 读作 f 的 n 次迭代.

**编程类比**：这几乎就是递归函数的雏形.

```python
def f(x):
    return x + 1

# f^3(1) 就是 f(f(f(1)))
```

**Python 代码实验**：
```python
def f(x):
    return 2 * x + 1

def iterate(f, x, n):
    """计算 f 的 n 次迭代在 x 处的值"""
    result = x
    for _ in range(n):
        result = f(result)
    return result

print(iterate(f, 1, 3))  # 输出 15
# 验证：f(1)=3, f(3)=7, f(7)=15
```

### 2. 迭代的几何直观：蛛网图

在数学竞赛中，函数迭代经常用“蛛网图”（Cobweb Plot）来可视化.原理如下：

- 在坐标系中画出函数 `y = f(x)` 图像
- 画出 `y = x`
- 从 `x_0` 出发向上画 `x_1 = f(x_0)`
- 水平移动到 `y = x`，得到 `(x_1, x_1)`
- 再画 `x_2 = f(x_1)`
- 无限循环

### 3. 不动点：迭代的“目的地”

如果存在某个数 `x*`，使得 `f(x*) = x*`，那么 `x*` 就是函数 f 的不动点.从不动点出发，迭代将永远停留在那里.

**数学意义**：不动点是迭代过程的“归宿”.竞赛题常问：迭代会收敛到哪个值？迭代何时发散？这些都是围绕不动点展开的.

### 4. 简单函数方程

函数方程是竞赛数学中的核心内容.它不像普通方程那样求一个数，而是求一个函数，使得它在所有输入上都满足某种条件.

**例子**：求所有函数 `f: R → R`，使得对任意 `x, y`，都有 `f(x+y) = f(x) + f(y)`.

这类方程的解为 `f(x) = kx`，可通过迭代与归纳证明.

---

## 本章结束语

你已经完成了从 Hello World 到多线程的完整路径.这 19 个 part 覆盖了编程中最核心的概念和操作，包含五种常用语言的具体实现.如果某个部分暂时用不到，可以跳过，等需要时再回来看.

接下来的章节将深入到运维、数据库、网络和系统底层，帮助你从“能写程序”进阶到“能理解程序如何运行”.
