# 第二章 · 六大编程语言核心入门

欢迎来到第二章。这一章会比较长，但别怕——你可以跳读。爱看哪个语言就看哪个，不爱看的先跳过，以后需要了再回来。

本章的宗旨是：**能跑就行，先跑起来再说**。不追求什么完美代码，不搞什么代码洁癖，能写出东西、能看到结果、能理解为什么，就可以了。

> **读者须知**：本章的 C++ 示例有时会用 `std::cout`，有时会用 `using namespace std;` 偷懒。实际项目中建议统一用 `std::` 前缀，避免命名冲突。本章为了写起来省事，偶尔不规范，见谅。

> **2026.8.9 更新**：加入了 Golang 部分。现在本章覆盖 Python、Lua、JavaScript、C++、Go 五种语言。

> **2026.9.28 更新**：加入了汇编语言部分。昨天无聊想看看roblox studio的rccservice在哪里 下了个ghidra尝试逆向一下 结果发现不会汇编急哭了，有感而发，为了不让读者重蹈覆辙我就加入了这玩意（世上最难语言）来辅助看看程序底层构造。

---

## Part 0：如何打注释

注释就是写给人类看的说明。计算机不看，但你的队友、你的老师、未来的你，都会看。

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

**C++**：和 JavaScript 一样。

**Go**：和 JavaScript 一样。

**汇编**：汇编的注释很简单，分号 `;` 后面的一切都是注释。

```asm
; 这是一个注释
mov eax, 1      ; 把 1 放到 eax 寄存器里
```

**规范**：
- 每个包在 `package` 声明前加包注释，描述包的功能
- 函数前加注释，说明功能、参数、返回值
- 结构体及字段加注释
- 统一用单行注释，尽量别用块注释
- 注释不超过 120 个字符
- 中文和英文之间留个空格，看着舒服

**批判性思维**：注释不是越多越好。好的代码本身就能说明“怎么做”，注释应该说明“为什么这么做”。如果代码写得让人看不懂，加注释只是补救，不是解药。

**汇编视角**：汇编里注释尤其重要，因为 `mov eax, 1` 根本看不出为什么是 1。没有注释的汇编，几周后连你自己都看不懂。

---

## Part 1：Hello World 与基础输入输出

每个程序员的第一段代码，向世界问好，顺便学会获取用户输入。

### Python
```python
print("Hello, World!")
name = input("请输入你的名字：")
print("你好，" + name)
```

解释：`print()` 输出内容到屏幕，`input()` 等待用户输入并返回字符串。

### Lua
```lua
print("Hello, World!")
io.write("请输入你的名字：")
local name = io.read()
print("你好，" .. name)
```

解释：`io.write()` 输出不自动换行，`io.read()` 读取用户输入，`..` 是字符串连接符。

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

解释：Node.js 中需要引入 `readline` 模块来处理输入，过于繁琐，故不推荐用 JS 去写需要用户输入的脚本。

### JavaScript（浏览器环境）
```javascript
console.log("Hello, World!");
let name = prompt("请输入你的名字：");
console.log("你好，" + name);
```

解释：浏览器中 `prompt()` 可以弹出输入框。

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

解释：`cin` 读取用户输入，`cout` 输出，`endl` 换行。

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

解释：`package main` 定义可执行程序包，`func main()` 是程序入口。`fmt.Println` 输出并换行，`fmt.Print` 不换行，`fmt.Scanln` 读取一行输入。Go 是静态类型。

### 汇编视角：Hello World 到底在干什么

用 Linux x86-64 汇编写一个 Hello World，大致是这样：

```asm
section .data
    msg db "Hello, World!", 0x0a    ; 字符串 + 换行

section .text
    global _start

_start:
    ; write(1, msg, 13)
    mov rax, 1          ; 系统调用号 1 = write
    mov rdi, 1          ; 文件描述符 1 = stdout
    mov rsi, msg        ; 字符串地址
    mov rdx, 13         ; 字符串长度
    syscall             ; 执行系统调用

    ; exit(0)
    mov rax, 60         ; 系统调用号 60 = exit
    xor rdi, rdi        ; 返回码 0
    syscall
```

**解释**：汇编里没有 `print` 函数。你直接告诉 CPU：把系统调用号放进 `rax`，把参数放进 `rdi`、`rsi`、`rdx`，然后执行 `syscall` 指令。操作系统内核接收到这个请求后，替你完成输出。

**逻辑推理**：Python 的 `print("Hello")` 一行，在汇编里变成了 6 行。编译器/解释器帮你把高层代码翻译成了底层指令。这就是“抽象”的代价和收益——你写起来舒服，但要付出性能代价。

**批判性思维**：所有语言最终都会变成汇编。Python 解释器本身也是用 C 写的，C 编译后就是汇编。所以“学汇编”不是学一门新语言，而是学所有语言的共同祖先。

---

## Part 2：变量与数据类型

变量是给数据的标签，不同的数据类型决定了数据可以做什么操作。

### 基本数据类型

| 类型 | 示例 | 说明 |
|------|------|------|
| 整数（int） | `67` | 没有小数部分的数 |
| 浮点数（float/double） | `3.14159` | 有小数部分的数 |
| 布尔值（bool） | `true / false` | 真的或假的 |
| 字符串（string） | `"dick"` | 用引号括起来的文本 |

### Python
```python
age = 67
price = 67.69
is_student = True
name = "dick"
print(age, price, is_student, name)
```

解释：Python 的变量不需要声明类型，直接赋值即可，动态类型。

### Lua
```lua
local age = 67
local price = 67.69
local is_student = true
local name = "dick"
print(age, price, is_student, name)
```

解释：Lua 用 `local` 声明局部变量，也是动态类型。

### JavaScript
```javascript
let age = 67;
let price = 67.69;
let is_student = true;
let name = "dick";
console.log(age, price, is_student, name);
```

解释：`let` 声明块级变量，动态类型。

### C++
```cpp
int age = 67;
double price = 67.69;
bool is_student = true;
std::string name = "dick";
std::cout << age << " " << price << " " << is_student << " " << name << std::endl;
```

解释：C++ 是静态类型，变量声明时必须指定类型。

### Go
```go
package main

import "fmt"

func main() {
    age := 67
    var price float64 = 67.69
    isStudent := true
    var name string = "dick"
    fmt.Println(age, price, isStudent, name)
}
```

解释：Go 支持类型推导（`:=`），也支持显式声明（`var`）。变量声明后必须使用。

### 汇编视角：变量就是内存地址

在汇编里，没有“变量”这个概念，只有内存地址和寄存器。

```asm
section .data
    age dd 67           ; 定义 4 字节整数，初值 67
    price dq 67.69      ; 定义 8 字节浮点数

section .text
    global _start

_start:
    mov eax, [age]      ; 把 age 地址里的值读到 eax 寄存器
    add eax, 1          ; eax = eax + 1
    mov [age], eax      ; 把 eax 写回 age 地址
```

**解释**：`dd` 是“define double word”（4 字节），`dq` 是“define quad word”（8 字节）。`[age]` 表示“age 这个地址里存的值”。

**逻辑推理**：高级语言里的“变量”，在汇编里就是“一个内存地址 + 一个类型尺寸”。类型（int、float）决定了从那个地址读几个字节、怎么解释这些字节。

**批判性思维**：为什么 C++ 要求声明类型，而 Python 不用？因为 C++ 编译时需要知道每个变量占几个字节、用什么指令处理；Python 运行时才判断类型，所以更灵活但更慢。汇编则完全暴露了这些细节——你必须自己告诉 CPU 读几个字节。

---

## Part 3：基本运算符

运算符用于对数据进行计算和比较。

### 算术运算符

加法 `+`，减法 `-`，乘法 `*`，除法 `/`，取余 `%`。

**Python**
```python
a = 10
b = 3
print(a + b)   # 13
print(a - b)   # 7
print(a * b)   # 30
print(a / b)   # 3.333...
print(a % b)   # 1
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

**Go**
```go
package main

import "fmt"

func main() {
    a, b := 10, 3
    fmt.Println(a + b)
    fmt.Println(a - b)
    fmt.Println(a * b)
    fmt.Println(a / b)                    // 3（整数除法）
    fmt.Println(float64(a) / float64(b))  // 3.333...
    fmt.Println(a % b)                    // 1
}
```

### 汇编视角：加减乘除就是几条指令

```asm
section .data
    a dd 10
    b dd 3

section .text
    global _start

_start:
    mov eax, [a]        ; eax = 10
    add eax, [b]        ; eax = eax + 3 = 13
    sub eax, [b]        ; eax = eax - 3 = 10
    imul eax, [b]       ; eax = eax * 3 = 30

    ; 除法有点特殊，需要用 edx:eax 作为被除数
    mov eax, [a]        ; eax = 10
    cdq                 ; 把 eax 符号扩展到 edx
    idiv dword [b]      ; eax = 商，edx = 余数
```

**解释**：加法就是 `add`，减法就是 `sub`，乘法是 `imul`，除法是 `idiv`（整数除法）。除法会同时产生商和余数，分别放在 `eax` 和 `edx` 里。

**逻辑推理**：`10 / 3` 在汇编里结果是 `eax=3, edx=1`。高级语言里的 `/` 和 `%` 其实是同一条 `idiv` 指令产出的两个结果。所以 `a / b` 和 `a % b` 在底层是同一回事。

**批判性思维**：C++ 里 `a / b` 当 a 和 b 都是整数时结果是整数，这不是 bug，是直接继承了汇编的行为。Python 的 `/` 永远返回浮点数，是因为解释器在背后帮你做了类型转换。

### 3.2 比较运算符

等于 `==`，不等于 `!=`，大于 `>`，小于 `<`，大于等于 `>=`，小于等于 `<=`。

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
print(x ~= y)
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
std::cout << (x == y) << std::endl;
std::cout << (x != y) << std::endl;
std::cout << (x < y) << std::endl;
```

**Go**
```go
x, y := 5, 10
fmt.Println(x == y)
fmt.Println(x != y)
fmt.Println(x < y)
```

### 汇编视角：比较就是 cmp 指令

```asm
section .data
    x dd 5
    y dd 10

section .text
    global _start

_start:
    mov eax, [x]        ; eax = 5
    cmp eax, [y]        ; 比较 eax 和 [y]
    jl  x_less_than_y   ; 如果 eax < [y]，跳转
    ; 否则继续执行
    jmp done

x_less_than_y:
    ; 这里执行 x < y 时的逻辑

done:
    ; ...
```

**解释**：`cmp` 指令不存储结果，它只是设置 CPU 的“标志位”。后面的 `jl`（jump if less）、`jg`（jump if greater）、`je`（jump if equal）根据标志位决定是否跳转。

**逻辑推理**：高级语言里的 `if (x < y)`，在汇编里就是 `cmp` + 条件跳转。比较的结果不产生一个“布尔值变量”，而是直接影响下一条指令走哪条路。

**批判性思维**：布尔值在汇编里根本不存在。`true` 和 `false` 是高级语言抽象出来的概念。CPU 只认识“标志位”和“跳转指令”。

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

**Go**
```go
hasTicket := true
hasID := false
if hasTicket && hasID {
    fmt.Println("可以入场")
} else {
    fmt.Println("不能入场")
}
```

### 汇编视角：逻辑与就是两次跳转

```asm
    cmp byte [has_ticket], 1
    jne  cannot_enter       ; 如果 has_ticket != 1，跳转到不能入场
    cmp byte [has_id], 1
    jne  cannot_enter       ; 如果 has_id != 1，跳转到不能入场

    ; 两个条件都满足
    ; 执行“可以入场”
    jmp done

cannot_enter:
    ; 执行“不能入场”

done:
    ; ...
```

**解释**：`and` 在汇编里就是“第一个条件不满足就跳走，第二个条件不满足也跳走”。只有两个都不跳，才执行后面的代码。

**逻辑推理**：这就是“短路求值”的底层实现——`and` 左边为假，右边根本不检查，直接跳走。

---

## Part 4：字符串操作

字符串是文本，可以拼接、切割、查找、替换。

### Python
```python
text = "Hello, World!"
print(len(text))           # 13
print(text.upper())        # HELLO, WORLD!
print(text.lower())        # hello, world!
print(text[0])             # H
print(text[7:12])          # World
print(text.replace("World", "Python"))
print("Hello" + " " + "World")
```

### Lua
```lua
local text = "Hello, World!"
print(#text)
print(string.upper(text))
print(string.lower(text))
print(string.sub(text, 1, 1))
print(string.sub(text, 8, 12))
print(string.gsub(text, "World", "Lua"))
print("Hello" .. " " .. "World")
```

### JavaScript
```javascript
let text = "Hello, World!";
console.log(text.length);
console.log(text.toUpperCase());
console.log(text.toLowerCase());
console.log(text[0]);
console.log(text.substring(7, 12));
console.log(text.replace("World", "JavaScript"));
console.log("Hello" + " " + "World");
```

### C++
```cpp
#include <string>
std::string text = "Hello, World!";
std::cout << text.length() << std::endl;
std::cout << text[0] << std::endl;
std::cout << text.substr(7, 5) << std::endl;
std::cout << text + " from C++" << std::endl;
```

### Go
```go
package main

import (
    "fmt"
    "strings"
)

func main() {
    text := "Hello, World!"
    fmt.Println(len(text))
    fmt.Println(strings.ToUpper(text))
    fmt.Println(strings.ToLower(text))
    fmt.Println(text[0])
    fmt.Println(text[7:12])
    fmt.Println(strings.Replace(text, "World", "Go", -1))
    fmt.Println("Hello" + " " + "World")
}
```

### 汇编视角：字符串就是字节数组

```asm
section .data
    text db "Hello, World!", 0

section .text
    global _start

_start:
    ; 取第一个字符
    mov al, [text]          ; al = 'H'（ASCII 72）

    ; 取第 8 个字符（索引 7）
    mov al, [text + 7]      ; al = 'W'

    ; 字符串长度
    mov rcx, 0
count_loop:
    cmp byte [text + rcx], 0
    je  count_done
    inc rcx
    jmp count_loop
count_done:
    ; rcx = 字符串长度
```

**解释**：汇编里字符串就是一串连续的字节。`[text]` 是第一个字符，`[text + 7]` 是第 8 个字符。求长度就是从头遍历，直到遇到 0（空字符）。

**逻辑推理**：高级语言里的 `text.length` 在汇编里就是“从开头数到 0 的循环”。Python 和 Go 的字符串长度是 O(1)，因为它们把长度存在了别的地方；而 C 语言的 `strlen` 是 O(n)，因为它每次都要数一遍。

**批判性思维**：为什么 C 语言容易出“缓冲区溢出”？因为字符串没有内置长度，你操作的时候必须自己确保不越界。汇编直接暴露了这个问题——你写 `[text + 100]` 它也不会拦你，直接读越界的内存。

---

## Part 5：条件判断（if-else）

让程序根据不同情况执行不同的代码块。

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

### 汇编视角：if-else 就是比较+跳转

```asm
    mov eax, [score]        ; eax = 85
    cmp eax, 90
    jge grade_A             ; if score >= 90，跳转
    cmp eax, 80
    jge grade_B             ; if score >= 80，跳转
    cmp eax, 70
    jge grade_C             ; if score >= 70，跳转
    jmp grade_D             ; 否则

grade_A:
    mov [grade], 'A'
    jmp done

grade_B:
    mov [grade], 'B'
    jmp done

grade_C:
    mov [grade], 'C'
    jmp done

grade_D:
    mov [grade], 'D'

done:
    ; ...
```

**解释**：每个 `if` 都对应一个 `cmp` + 条件跳转。`jge` 是“jump if greater or equal”。

**逻辑推理**：`else if` 在汇编里就是“上一个条件不满足时，继续检查下一个条件”。每个分支末尾都要 `jmp done` 跳过其他分支。

**批判性思维**：如果条件顺序写错（比如把 `>= 80` 写在 `>= 90` 前面），那 95 分也会被判成 B。汇编里这个问题更明显——跳转顺序就是执行顺序。

---

## Part 6：循环（for, while）

循环用于重复执行某段代码。

### Python
```python
for i in range(5):
    print(i)

count = 0
while count < 5:
    print(count)
    count += 1

fruits = ["apple", "banana", "cherry"]
for fruit in fruits:
    print(fruit)
```

### Lua
```lua
for i = 0, 4 do
    print(i)
end

local count = 0
while count < 5 do
    print(count)
    count = count + 1
end

local fruits = {"apple", "banana", "cherry"}
for i, fruit in ipairs(fruits) do
    print(fruit)
end
```

### JavaScript
```javascript
for (let i = 0; i < 5; i++) {
    console.log(i);
}

let count = 0;
while (count < 5) {
    console.log(count);
    count++;
}

let fruits = ["apple", "banana", "cherry"];
for (let fruit of fruits) {
    console.log(fruit);
}
```

### C++
```cpp
for (int i = 0; i < 5; i++) {
    std::cout << i << std::endl;
}

int count = 0;
while (count < 5) {
    std::cout << count << std::endl;
    count++;
}

std::vector<std::string> fruits = {"apple", "banana", "cherry"};
for (const auto& fruit : fruits) {
    std::cout << fruit << std::endl;
}
```

### Go
```go
package main

import "fmt"

func main() {
    for i := 0; i < 5; i++ {
        fmt.Println(i)
    }

    count := 0
    for count < 5 {
        fmt.Println(count)
        count++
    }

    fruits := []string{"apple", "banana", "cherry"}
    for _, fruit := range fruits {
        fmt.Println(fruit)
    }
}
```

### 汇编视角：for 循环就是标号+跳转

```asm
    mov ecx, 0              ; ecx 是计数器（i = 0）

loop_start:
    cmp ecx, 5              ; 比较 i 和 5
    jge loop_end            ; 如果 i >= 5，跳出循环

    ; 循环体：打印 ecx
    ; ...

    inc ecx                 ; i++
    jmp loop_start          ; 回到循环开头

loop_end:
    ; 循环结束后的代码
```

**解释**：`for` 循环在汇编里就是：初始化计数器 → 检查条件 → 执行循环体 → 递增计数器 → 跳回检查。

**逻辑推理**：`while` 循环和 `for` 循环在汇编里几乎一模一样。区别只是 `for` 把初始化、检查、递增写在同一个语法结构里，而 `while` 把初始化写在前面，递增写在循环体里。

**批判性思维**：Go 只有 `for` 没有 `while`，因为 Go 的设计者认为 `for` 足够表达所有循环。汇编层面看，确实如此——所有循环最终都是一样的 cmp + jmp。

---

## Part 7：函数定义与调用

函数把一段代码封装起来，可以重复调用。

### Python
```python
def greet(name):
    return "Hello, " + name

print(greet("Alice"))

def greet_with_default(name="World"):
    return "Hello, " + name

print(greet_with_default())
```

### Lua
```lua
function greet(name)
    return "Hello, " .. name
end
print(greet("Alice"))

function greet_with_default(name)
    name = name or "World"
    return "Hello, " .. name
end
print(greet_with_default())
```

### JavaScript
```javascript
function greet(name) {
    return "Hello, " + name;
}
console.log(greet("Alice"));

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

func divide(a, b int) (int, error) {
    if b == 0 {
        return 0, fmt.Errorf("除数不能为0")
    }
    return a / b, nil
}

func main() {
    fmt.Println(greet("Alice"))

    result, err := divide(10, 2)
    if err != nil {
        fmt.Println(err)
    } else {
        fmt.Println(result)
    }
}
```

### 汇编视角：函数就是 call 和 ret

```asm
section .text
    global _start

; 函数定义
greet:
    ; 参数在 rdi 里（Linux 调用约定）
    ; 这里简化处理，直接返回
    mov rax, rdi        ; 返回值放到 rax
    ret                 ; 返回调用者

_start:
    mov rdi, 42         ; 参数 = 42
    call greet          ; 调用函数
    ; 返回后 rax = 42
```

**解释**：`call` 指令把当前地址压入栈，然后跳转到函数入口。`ret` 指令从栈里弹出返回地址，跳回去。

**逻辑推理**：函数调用的本质是“记住回来的路，然后跳过去执行，执行完再跳回来”。栈就是用来存这些返回地址的。

**批判性思维**：Go 的多返回值在汇编里实际上是“调用者分配空间，被调用者往里写”。Go 的错误返回值不是异常，就是一个普通值，和 `result` 一起返回。

---

## Part 8：列表 / 数组

列表用于存储一组有序的数据，可以通过索引访问。

### Python
```python
fruits = ["apple", "banana", "cherry"]
print(fruits[0])
fruits.append("orange")
print(fruits)
fruits.remove("banana")
print(fruits)
```

### Lua
```lua
local fruits = {"apple", "banana", "cherry"}
print(fruits[1])
table.insert(fruits, "orange")
print(fruits[2])
table.remove(fruits, 2)
print(fruits[2])
```

### JavaScript
```javascript
let fruits = ["apple", "banana", "cherry"];
console.log(fruits[0]);
fruits.push("orange");
console.log(fruits);
fruits.splice(1, 1);
console.log(fruits);
```

### C++
```cpp
#include <vector>
std::vector<std::string> fruits = {"apple", "banana", "cherry"};
std::cout << fruits[0] << std::endl;
fruits.push_back("orange");
fruits.erase(fruits.begin() + 1);
std::cout << fruits[1] << std::endl;
```

### Go
```go
package main

import "fmt"

func main() {
    var arr [3]int = [3]int{1, 2, 3}
    fmt.Println(arr[0])

    fruits := []string{"apple", "banana", "cherry"}
    fmt.Println(fruits[0])
    fruits = append(fruits, "orange")
    fmt.Println(fruits)

    fruits = append(fruits[:1], fruits[2:]...)
    fmt.Println(fruits)
}
```

### 汇编视角：数组就是连续内存+偏移

```asm
section .data
    fruits db "apple", 0, "banana", 0, "cherry", 0

section .text
    global _start

_start:
    ; 取第一个字符串（索引 0）
    mov rsi, fruits         ; 指向 "apple"

    ; 取第二个字符串（索引 1）
    ; "apple" 长度 5，加上 0 结尾 = 6
    mov rsi, fruits + 6     ; 指向 "banana"

    ; 取第三个字符串
    ; "banana" 长度 6，加上 0 结尾 = 7
    mov rsi, fruits + 13    ; 指向 "cherry"
```

**解释**：汇编里的数组就是一块连续内存。取第 n 个元素 = 首地址 + n × 元素大小。

**逻辑推理**：Python 的 `fruits[0]`、`fruits[1]` 在汇编里就是 `fruits + 0`、`fruits + 偏移量`。高级语言帮你算了偏移量，汇编你得自己算。

**批判性思维**：为什么 C 语言的数组越界不会报错？因为汇编层面根本不检查边界。`fruits[100]` 只会读 `fruits + 偏移量`，那个地址里有什么，它就返回什么。Python 会报 `IndexError`，是因为解释器在背后做了边界检查。

---

## Part 9：字典 / 映射 / 对象

字典用于存储键值对，通过键来访问值。

### Python
```python
person = {"name": "Alice", "age": 25, "city": "Beijing"}
print(person["name"])
person["age"] = 26
person["gender"] = "F"
print(person)
```

### Lua
```lua
local person = {name = "Alice", age = 25, city = "Beijing"}
print(person.name)
print(person["age"])
person.age = 26
person.gender = "F"
print(person.gender)
```

### JavaScript
```javascript
let person = {name: "Alice", age: 25, city: "Beijing"};
console.log(person.name);
person.age = 26;
person.gender = "F";
```

### C++
```cpp
#include <map>
#include <string>
std::map<std::string, std::string> person;
person["name"] = "Alice";
person["age"] = "25";
person["city"] = "Beijing";
std::cout << person["name"] << std::endl;
```

### Go
```go
package main

import "fmt"

func main() {
    person := map[string]string{
        "name": "Alice",
        "age":  "25",
        "city": "Beijing",
    }
    fmt.Println(person["name"])
    person["age"] = "26"
    person["gender"] = "F"
    fmt.Println(person)

    value, ok := person["age"]
    if ok {
        fmt.Println("age 存在:", value)
    }
}
```

### 汇编视角：字典就是哈希表，哈希表就是数组+链表

字典在汇编层面没有直接对应的指令。它的实现通常是：
1. 对键计算哈希值
2. 用哈希值取模，得到数组索引
3. 在数组的那个位置存值（如果有冲突，用链表串起来）

用伪汇编表示大概是这样：

```asm
; 计算 "name" 的哈希值
    mov rdi, key_string
    call hash_function     ; rax = 哈希值

    ; 取模得到数组索引
    xor rdx, rdx
    mov rcx, TABLE_SIZE
    div rcx                ; rdx = 哈希值 % TABLE_SIZE

    ; 在 table[rdx] 里查找或插入
    lea rsi, [table + rdx * 8]
    ; ...
```

**解释**：字典的“键值对”在底层就是一个数组，数组的索引是通过哈希函数算出来的。

**逻辑推理**：为什么字典查找是 O(1)？因为它是直接通过哈希值算出索引，不需要遍历。但如果两个键的哈希值相同（冲突），就需要在同一个索引下用链表或开放寻址法处理。

**批判性思维**：Go 的 map 在访问不存在的键时返回零值，不报错。这是因为汇编层面无法区分“键不存在”和“值恰好是零值”。Go 用 `value, ok := map[key]` 让你手动检查，Python 用 `KeyError` 强制你处理。两种设计各有取舍。

---

## Part 10：文件操作（基础读写）

读取和写入文件是程序保存数据的基本方式。

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

### Lua
```lua
local file = io.open("test.txt", "w")
if file then
    file:write("Hello, file!\n")
    file:write("第二行内容")
    file:close()
end

local file = io.open("test.txt", "r")
if file then
    local content = file:read("*all")
    print(content)
    file:close()
end

local file = io.open("test.txt", "a")
if file then
    file:write("追加一行内容\n")
    file:close()
end
```

### JavaScript（Node.js）
```javascript
const fs = require('fs');

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

### C++
```cpp
#include <fstream>
#include <string>

std::ofstream out("test.txt");
out << "Hello, file!\n";
out << "第二行内容";
out.close();

std::ifstream in("test.txt");
std::string line;
while (std::getline(in, line)) {
    std::cout << line << std::endl;
}
in.close();

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
    err := os.WriteFile("test.txt", []byte("Hello, file!\n第二行内容"), 0644)
    if err != nil {
        fmt.Println(err)
        return
    }

    data, err := os.ReadFile("test.txt")
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(string(data))

    f, err := os.OpenFile("test.txt", os.O_APPEND|os.O_WRONLY, 0644)
    if err != nil {
        fmt.Println(err)
        return
    }
    defer f.Close()
    f.WriteString("追加内容\n")
}
```

### 汇编视角：文件操作就是系统调用

```asm
section .data
    filename db "test.txt", 0
    content  db "Hello, file!", 0x0a

section .text
    global _start

_start:
    ; open("test.txt", O_WRONLY | O_CREAT, 0644)
    mov rax, 2              ; 系统调用号 2 = open
    mov rdi, filename       ; 文件名
    mov rsi, 0x41           ; O_WRONLY | O_CREAT
    mov rdx, 0644           ; 权限
    syscall
    mov r8, rax             ; 保存文件描述符

    ; write(fd, content, 13)
    mov rax, 1              ; 系统调用号 1 = write
    mov rdi, r8             ; 文件描述符
    mov rsi, content        ; 内容
    mov rdx, 13             ; 长度
    syscall

    ; close(fd)
    mov rax, 3              ; 系统调用号 3 = close
    mov rdi, r8
    syscall
```

**解释**：文件操作在汇编里就是一系列系统调用：`open`（打开）、`write`（写入）、`read`（读取）、`close`（关闭）。

**逻辑推理**：高级语言的 `open()` 函数，在汇编里就是 `mov rax, 2; syscall`。Python 的 `with open(...) as f` 帮你处理了打开和关闭，汇编里你得手动做。

**批判性思维**：为什么文件操作容易出错？因为每一步系统调用都可能失败（文件不存在、权限不够、磁盘满了）。汇编里你要检查 `rax` 的返回值，高级语言里要用异常或错误码处理。

---

## Part 11：错误处理（异常）

程序运行时可能会出错，错误处理让程序能够优雅地应对问题。

### Python
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

### Lua
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

### JavaScript
```javascript
try {
    let num = parseInt(prompt("请输入一个数字："));
    console.log(100 / num);
} catch (error) {
    console.log("发生错误：" + error.message);
}
```

### C++
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

### 汇编视角：错误就是返回值

```asm
    ; 调用 open 系统调用
    mov rax, 2
    syscall

    ; 检查返回值
    cmp rax, 0
    jl  error_open_failed   ; 如果 rax < 0，说明出错了

    ; 正常继续
    jmp continue

error_open_failed:
    ; 错误处理
    ; rax 里的负数就是错误码（-errno）

continue:
    ; ...
```

**解释**：汇编里没有“异常”机制。系统调用失败时，`rax` 会返回一个负数（错误码）。你每次调用后都要检查 `rax` 的值。

**逻辑推理**：Go 的 `if err != nil` 就是汇编这种“检查返回值”的现代化写法。C++ 和 Python 的 try-catch 则是另一种机制——出错时自动跳转到异常处理代码，不需要每次手动检查。

**批判性思维**：异常机制让代码更干净（错误处理集中在一处），但容易忘记处理。错误返回值强迫你每次都检查，但代码更啰嗦。汇编层面看，两者最终都要跳转到某个地方处理错误，只是跳转方式不同。

---

## Part 12：模块与包（代码组织）

模块是把一组相关功能放在一个文件中，包是模块的集合。

### Python
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

from math_utils import add, multiply
print(add(3, 5))
```

### Lua
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

### JavaScript
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

### C++
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

### 汇编视角：模块就是分别编译，链接时合并

汇编层面没有“模块”概念。两个 `.asm` 文件可以分别汇编成 `.o` 目标文件，然后用链接器合并成可执行文件。

```asm
; math_utils.asm
section .text
    global add          ; 声明 add 是全局符号
add:
    mov rax, rdi
    add rax, rsi
    ret

; main.asm
section .text
    extern add          ; 声明 add 在别的文件里
    global _start
_start:
    mov rdi, 3
    mov rsi, 5
    call add            ; 调用外部函数
```

**解释**：`global` 导出符号，`extern` 引用外部符号。链接器负责把 `call add` 的地址填成 `math_utils.o` 里 `add` 的实际地址。

**逻辑推理**：C++ 的头文件和源文件分离，本质上就是汇编的 `global`/`extern` 机制。头文件告诉编译器“这个函数存在”，源文件提供实现，链接器负责拼接。

**批判性思维**：Go 的包管理更简单——一个目录就是一个包，不需要头文件。因为 Go 编译器会扫描整个目录，自动找到所有 `.go` 文件。代价是编译时不能只编译一个文件，要编译整个包。

---

## Part 13：面向对象基础（类与对象）

类是创建对象的蓝图，对象是类的实例。

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
print(my_dog.bark())
print(my_dog.get_age())
```

### Lua
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

### JavaScript
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

### Go
```go
package main

import "fmt"

type Dog struct {
    name string
    age  int
}

func (d Dog) Bark() string {
    return d.name + " says woof!"
}

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

### 汇编视角：对象就是结构体+函数指针

面向对象在汇编层面没有直接支持。一个“对象”通常是一块内存，里面存着数据，外加一张“虚函数表”（vtable）指针，指向该对象能调用的函数。

```asm
section .data
    ; 虚函数表
    vtable_dog:
        dq dog_bark
        dq dog_get_age

section .text
    global _start

_start:
    ; 创建 Dog 对象
    ; 内存布局：[vtable指针][name指针][age]
    lea rax, [vtable_dog]
    mov [obj], rax          ; 存 vtable 指针
    mov [obj + 8], name_ptr
    mov [obj + 16], 3

    ; 调用 bark()
    mov rax, [obj]          ; 取 vtable 指针
    mov rax, [rax]          ; 取 bark 函数地址
    mov rdi, obj            ; 把对象自己作为参数（this）
    call rax
```

**解释**：`obj` 的内存布局是：前 8 字节存虚函数表地址，后面存数据。调用方法时，先从对象里找到虚函数表，再从虚函数表里找到函数地址，然后 `call`。

**逻辑推理**：这就是“多态”的底层实现——不同类的对象有不同的虚函数表，调用同一个方法名时，实际执行的函数不同。

**批判性思维**：Go 没有类，用结构体+方法代替。但底层实现是一样的——方法就是对普通函数的封装，调用时把接收者作为第一个参数传入。所以“面向对象”不是语言特性，是一种编程范式。

---

## Part 14：异步编程（回调 / Promise / 协程）

异步编程让程序在等待耗时操作时不会阻塞。

### JavaScript
```javascript
function fetchData(callback) {
    setTimeout(() => {
        callback("数据已加载");
    }, 1000);
}
fetchData((data) => {
    console.log(data);
});

function fetchDataPromise() {
    return new Promise((resolve) => {
        setTimeout(() => {
            resolve("数据已加载");
        }, 1000);
    });
}
fetchDataPromise().then((data) => {
    console.log(data);
});

async function getData() {
    let data = await fetchDataPromise();
    console.log(data);
}
getData();
```

### Python
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

### Go
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

### 汇编视角：异步就是多线程+上下文切换

汇编层面没有“异步”这个概念。异步的实现通常依赖：
1. 多线程（操作系统调度）
2. 事件循环（单线程，用非阻塞 I/O + 回调）

多线程在汇编里就是多次调用 `clone` 系统调用创建新线程：

```asm
    ; 创建新线程
    mov rax, 56             ; 系统调用号 56 = clone
    mov rdi, flags          ; 标志位
    mov rsi, stack_ptr      ; 新线程的栈
    syscall
```

**解释**：`clone` 系统调用创建一个新线程，新线程从指定的函数开始执行。操作系统的调度器负责在多个线程之间切换。

**逻辑推理**：Go 的 goroutine 是“用户态线程”，比操作系统线程更轻量。它的调度器在用户态完成上下文切换，不需要每次进入内核。但底层仍然是 `clone` 系统调用创建了初始线程。

**批判性思维**：异步编程的复杂度在于“什么时候切换、切换后状态怎么保存”。汇编层面看，就是保存寄存器、切换栈、恢复寄存器。高级语言的 `async/await` 帮你自动做了这些。

---

## Part 15：DOM 操作（仅前端 JavaScript）

DOM 是网页的文档对象模型，JavaScript 可以通过 DOM 操作网页内容。

### 获取元素
```javascript
let title = document.getElementById("title");
let items = document.getElementsByClassName("item");
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
let newDiv = document.createElement("div");
newDiv.textContent = "我是新元素";
document.body.appendChild(newDiv);

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

### 汇编视角：DOM 操作最终也是内存操作

浏览器本身是一个巨大的 C++ 程序。DOM 树在内存里就是一棵树状结构，每个节点是一个对象。JavaScript 调用 `document.getElementById()` 时，浏览器引擎（如 V8）会执行一段 C++ 代码，遍历这棵树。

汇编层面看，这就是：
1. 调用 V8 引擎的函数
2. V8 内部遍历 DOM 树
3. 找到匹配的节点
4. 把节点包装成 JavaScript 对象返回

**解释**：你在 JavaScript 里写的一行 `element.style.color = "red"`，底层要经过：JS 引擎 → 浏览器内核 → 渲染引擎 → 重新绘制页面。整个过程涉及大量汇编指令，只是被层层抽象掉了。

**逻辑推理**：为什么 DOM 操作慢？因为每次修改都可能触发页面重绘（reflow/repaint）。汇编层面看，就是大量内存操作 + 图形计算。

**批判性思维**：现代前端框架（React、Vue）用虚拟 DOM 来减少真实 DOM 操作。虚拟 DOM 在内存里是一棵轻量的 JS 对象树，修改它很快，最后一次性同步到真实 DOM。这就是用抽象换性能。

---

## Part 16：系统操作（环境变量、执行命令、文件系统高级）

获取系统信息，执行外部命令，进行更复杂的文件操作。

### Python
```python
import os
import subprocess

path = os.environ.get("PATH")
print(path)

os.environ["MY_VAR"] = "hello"
print(os.environ["MY_VAR"])

os.system("echo Hello from system")

result = subprocess.run(["ls", "-l"], capture_output=True, text=True)
print(result.stdout)

os.mkdir("new_folder")
os.rmdir("new_folder")
```

### Lua
```lua
local path = os.getenv("PATH")
print(path)

os.execute("export MY_VAR=hello")
os.execute("echo Hello from system")
```

### JavaScript（Node.js）
```javascript
const os = require('os');
const { exec } = require('child_process');

console.log(os.platform());
console.log(os.cpus());

console.log(process.env.PATH);
process.env.MY_VAR = "hello";

exec('echo Hello from system', (error, stdout) => {
    console.log(stdout);
});
```

### C++
```cpp
#include <iostream>
#include <cstdlib>
#include <string>
#include <filesystem>
namespace fs = std::filesystem;

int main() {
    const char* path = std::getenv("PATH");
    if (path) {
        std::cout << "PATH: " << path << std::endl;
    }

#ifdef _WIN32
    _putenv_s("MY_VAR", "hello");
#else
    setenv("MY_VAR", "hello", 1);
#endif

    int ret = std::system("echo Hello from system");

    fs::path dir = "new_folder";
    if (fs::create_directory(dir)) {
        std::cout << "目录创建成功" << std::endl;
    }
    fs::remove(dir);

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
    path := os.Getenv("PATH")
    fmt.Println("PATH:", path)
    os.Setenv("MY_VAR", "hello")
    fmt.Println("MY_VAR:", os.Getenv("MY_VAR"))

    cmd := exec.Command("echo", "Hello from system")
    output, err := cmd.Output()
    if err != nil {
        fmt.Println(err)
        return
    }
    fmt.Println(string(output))

    os.Mkdir("new_folder", 0755)
    os.Remove("new_folder")
}
```

### 汇编视角：系统操作就是系统调用

```asm
section .data
    my_var db "MY_VAR", 0
    value  db "hello", 0

section .text
    global _start

_start:
    ; 获取环境变量
    ; 环境变量在栈上，每个 "KEY=VALUE" 字符串
    ; 这里简化处理，假设已经找到

    ; 执行命令
    mov rax, 59             ; 系统调用号 59 = execve
    lea rdi, [command]      ; 命令路径
    lea rsi, [argv]         ; 参数数组
    lea rdx, [envp]         ; 环境变量数组
    syscall

    ; 创建目录
    mov rax, 83             ; 系统调用号 83 = mkdir
    lea rdi, [dirname]      ; 目录名
    mov rsi, 0755           ; 权限
    syscall
```

**解释**：所有系统操作在汇编里都是系统调用。`execve` 执行程序，`mkdir` 创建目录，`chdir` 切换目录。

**逻辑推理**：Python 的 `os.system("echo hello")` 底层就是 `fork` + `execve` + `wait`。`subprocess.run()` 也是同样的机制，只是更复杂（要处理输入输出管道）。

**批判性思维**：为什么 `os.system()` 不安全？因为它把命令字符串直接交给 shell 执行。如果字符串里包含用户输入，可能被注入恶意命令（命令注入）。汇编层面看，就是把未经过滤的字符串直接传给了 `execve`。

---

## Part 17：网络请求基础（HTTP）

向网络上的服务器发送请求并获取数据。

### Python
```python
import requests
response = requests.get("https://api.github.com")
print(response.status_code)
print(response.json()["current_user_url"])
```

### Lua
```lua
local http = require("socket.http")
local response, status = http.request("https://api.github.com")
print(status)
```

### JavaScript
```javascript
fetch('https://api.github.com')
    .then(response => response.json())
    .then(data => console.log(data))
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

### 汇编视角：网络请求就是 socket 系统调用

```asm
    ; 创建 socket
    mov rax, 41             ; 系统调用号 41 = socket
    mov rdi, 2              ; AF_INET
    mov rsi, 1              ; SOCK_STREAM
    mov rdx, 0              ; 协议
    syscall
    mov r8, rax             ; 保存 socket 文件描述符

    ; 连接服务器
    mov rax, 42             ; 系统调用号 42 = connect
    mov rdi, r8             ; socket 描述符
    lea rsi, [sockaddr]     ; 服务器地址结构
    mov rdx, 16             ; 地址长度
    syscall

    ; 发送请求
    mov rax, 1              ; write
    mov rdi, r8
    lea rsi, [request]
    mov rdx, req_len
    syscall

    ; 接收响应
    mov rax, 0              ; read
    mov rdi, r8
    lea rsi, [buffer]
    mov rdx, 4096
    syscall
```

**解释**：HTTP 请求在汇编里就是：`socket`（创建套接字）→ `connect`（连接服务器）→ `write`（发送请求）→ `read`（接收响应）。

**逻辑推理**：所有网络请求最终都是通过 socket 完成的。HTTPS 只是在 socket 上加了 TLS 加密层。

**批判性思维**：为什么 Python 的 `requests.get()` 一行，汇编里要写几十行？因为 `requests` 库帮你处理了 DNS 解析、TCP 连接、TLS 握手、HTTP 协议编码、响应解析。汇编里你得手动做每一步。

---

## Part 18：数据库简单操作（SQLite）

使用 SQLite 存储和查询数据，无需安装额外的数据库服务器。

### Python
```python
import sqlite3
conn = sqlite3.connect('test.db')
cursor = conn.cursor()

cursor.execute('''CREATE TABLE IF NOT EXISTS users
(id INTEGER PRIMARY KEY, name TEXT, age INTEGER)''')

cursor.execute('INSERT INTO users (name, age) VALUES (?, ?)', ("Alice", 25))
cursor.execute('INSERT INTO users (name, age) VALUES (?, ?)', ("Bob", 30))
conn.commit()

cursor.execute('SELECT * FROM users WHERE age > ?', (20,))
rows = cursor.fetchall()
for row in rows:
    print(row)
conn.close()
```

### Lua
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

### JavaScript（Node.js）
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

### C++
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

### 汇编视角：数据库操作就是文件读写

SQLite 数据库在底层就是一个文件（`test.db`）。所有 SQL 操作最终都变成对这个文件的读写。

```asm
    ; 打开数据库文件
    mov rax, 2              ; open
    lea rdi, [db_filename]
    mov rsi, 0x42           ; O_RDWR | O_CREAT
    mov rdx, 0644
    syscall
    mov r8, rax             ; 文件描述符

    ; 读取数据库头部
    mov rax, 0              ; read
    mov rdi, r8
    lea rsi, [buffer]
    mov rdx, 100
    syscall

    ; 根据头部信息，解析 B-tree 结构
    ; ...

    ; 写入数据
    mov rax, 1              ; write
    mov rdi, r8
    lea rsi, [data]
    mov rdx, data_len
    syscall
```

**解释**：SQLite 数据库文件内部使用 B-tree 结构组织数据。所有 SQL 操作（INSERT、SELECT、UPDATE、DELETE）最终都变成对 B-tree 的节点操作，也就是对文件的读、写、寻址。

**逻辑推理**：为什么 SQLite 不需要单独的服务器？因为它把“数据库引擎”直接嵌入到了你的程序里。MySQL 需要一个单独的 `mysqld` 进程，SQLite 就是一堆 C 函数，直接操作文件。

**批判性思维**：嵌入式数据库（SQLite）适合单机应用，但多个程序同时写同一个文件会冲突（因为没有服务器来协调）。客户端-服务器数据库（MySQL）适合多用户并发，因为服务器负责协调锁和事务。

---

## Part 19：多线程 / 并发入门

多线程允许程序同时执行多个任务，提高效率。

### Python
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

### Lua
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

### JavaScript（Web Workers）
```javascript
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

### C++
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

### 汇编视角：多线程就是 clone 系统调用

```asm
    ; 创建新线程
    mov rax, 56             ; 系统调用号 56 = clone
    mov rdi, 0x00000100     ; CLONE_VM（共享内存空间）
    mov rsi, stack_ptr      ; 新线程的栈地址
    mov rdx, 0              ; 父线程指针
    mov r10, 0              ; 子线程 TID
    syscall

    ; 新线程从 thread_function 开始执行
    ; 父线程继续执行
```

**解释**：`clone` 系统调用创建一个新线程。如果传入 `CLONE_VM` 标志，新线程和父线程共享内存空间，这就是“线程”的定义。如果不传，就是“进程”。

**逻辑推理**：多线程的“共享内存”意味着一个线程修改变量，其他线程能看到。这带来了便利，也带来了“竞态条件”——两个线程同时修改变量，结果不可预测。

**批判性思维**：为什么 Lua 没有真正的多线程？因为 Lua 的设计目标是嵌入式脚本，多线程会带来锁和共享内存的复杂性。Lua 用协程（coroutine）实现协作式多任务——只有一个线程在跑，但可以在函数之间切换。这避免了锁的问题，但也无法利用多核。

# 汇编视角补充：寄存器详解

在之前的汇编示例中，你看到了 `rax`、`rdi`、`rsi` 这些名字。它们是 CPU 内部的**寄存器**——CPU 直接读写的最快存储单元。本节专门解释这些寄存器的命名、用途和关系，让你在 Ghidra 或任何反汇编工具中看到它们时不再懵。

---

## 一、为什么有 `eax`、`rax`、`ax`、`al`、`ah`？

x86 架构从 16 位发展到 32 位，再到 64 位，寄存器也跟着扩展。为了**向后兼容**，同一组寄存器在不同位宽下有不同名字。

以 `rax` 为例：

| 名称 | 位宽 | 说明 |
|------|------|------|
| `rax` | 64 位 | 64 位通用寄存器（r = register） |
| `eax` | 32 位 | `rax` 的低 32 位（e = extended） |
| `ax` | 16 位 | `eax` 的低 16 位 |
| `al` | 8 位 | `ax` 的低 8 位（l = low） |
| `ah` | 8 位 | `ax` 的高 8 位（h = high） |

**关系图**：

```
64 位：  rax
        ┌────────────────────────────────┐
        │            64 位               │
        └────────────────────────────────┘
32 位：          eax
                ┌────────────────┐
                │     32 位      │
                └────────────────┘
16 位：                  ax
                        ┌────────┐
                        │ 16 位  │
                        └────────┘
8 位：                  ah   al
                        ┌──┬──┐
                        │高│低│
                        └──┴──┘
```

**写 `eax` 时，会自动清零 `rax` 的高 32 位。** 这是 x86-64 的特性，方便 32 位代码无缝运行在 64 位模式下。

---

## 二、通用寄存器一览

x86-64 有 16 个通用寄存器。它们都有 64 位名称、32 位名称、16 位名称和 8 位名称（部分有高 8 位）。

| 64 位 | 32 位 | 16 位 | 8 位（低） | 8 位（高） | 传统用途 |
|-------|-------|-------|------------|------------|----------|
| `rax` | `eax` | `ax` | `al` | `ah` | 累加器，返回值，系统调用号 |
| `rbx` | `ebx` | `bx` | `bl` | `bh` | 基址寄存器 |
| `rcx` | `ecx` | `cx` | `cl` | `ch` | 计数器，循环次数 |
| `rdx` | `edx` | `dx` | `dl` | `dh` | 数据寄存器，乘除法高位 |
| `rsi` | `esi` | `si` | `sil` | — | 源变址寄存器（字符串操作） |
| `rdi` | `edi` | `di` | `dil` | — | 目的变址寄存器（字符串操作） |
| `rbp` | `ebp` | `bp` | `bpl` | — | 栈基址指针 |
| `rsp` | `esp` | `sp` | `spl` | — | 栈顶指针 |
| `r8` | `r8d` | `r8w` | `r8b` | — | 通用寄存器（64 位新增） |
| `r9` | `r9d` | `r9w` | `r9b` | — | 通用寄存器 |
| `r10` | `r10d` | `r10w` | `r10b` | — | 通用寄存器 |
| `r11` | `r11d` | `r11w` | `r11b` | — | 通用寄存器 |
| `r12` | `r12d` | `r12w` | `r12b` | — | 通用寄存器 |
| `r13` | `r13d` | `r13w` | `r13b` | — | 通用寄存器 |
| `r14` | `r14d` | `r14w` | `r14b` | — | 通用寄存器 |
| `r15` | `r15d` | `r15w` | `r15b` | — | 通用寄存器 |

**注意**：`rsi`、`rdi`、`rbp`、`rsp` 以及 `r8`-`r15` 没有高 8 位版本（`ah`、`bh`、`ch`、`dh` 是历史遗留）。

---

## 三、特殊用途寄存器

除了通用寄存器，还有几个专用寄存器：

| 寄存器 | 用途 |
|--------|------|
| `rip` | 指令指针，指向下一条要执行的指令地址 |
| `rflags` | 标志寄存器，存放比较结果（零标志、符号标志等） |
| `cs`、`ds`、`es`、`fs`、`gs`、`ss` | 段寄存器（现代操作系统一般不用，除了 `fs`/`gs` 用于线程局部存储） |

---

## 四、函数调用约定（System V AMD64 ABI，Linux/macOS 使用）

在 64 位 Linux 和 macOS 上，函数调用时寄存器的用途有明确规定：

| 寄存器 | 用途 |
|--------|------|
| `rdi` | 第 1 个参数 |
| `rsi` | 第 2 个参数 |
| `rdx` | 第 3 个参数 |
| `rcx` | 第 4 个参数 |
| `r8` | 第 5 个参数 |
| `r9` | 第 6 个参数 |
| `rax` | 返回值 |
| `rsp` | 栈指针 |
| `rbp` | 栈基址（可选） |
| `rbx`、`r12`-`r15` | 被调用者保存（callee-saved） |
| `r10`、`r11` | 调用者保存（caller-saved） |

**超过 6 个参数时，多出的参数通过栈传递。**

---

## 五、系统调用约定（Linux x86-64）

系统调用是程序请求操作系统内核服务的机制。在 Linux 上：

| 寄存器 | 用途 |
|--------|------|
| `rax` | 系统调用号 |
| `rdi` | 第 1 个参数 |
| `rsi` | 第 2 个参数 |
| `rdx` | 第 3 个参数 |
| `r10` | 第 4 个参数（注意不是 `rcx`） |
| `r8` | 第 5 个参数 |
| `r9` | 第 6 个参数 |
| `syscall` | 执行系统调用指令 |

**返回值在 `rax` 中。**

---

## 六、回到 Hello World 例子

```asm
section .data
    msg db "Hello, World!", 0x0a

section .text
    global _start

_start:
    mov rax, 1          ; 系统调用号 1 = write
    mov rdi, 1          ; 文件描述符 1 = stdout
    mov rsi, msg        ; 字符串地址
    mov rdx, 13         ; 字符串长度
    syscall             ; 执行系统调用

    mov rax, 60         ; 系统调用号 60 = exit
    xor rdi, rdi        ; 返回码 0
    syscall
```

- `rax` 存系统调用号：1 是 write，60 是 exit。
- `rdi`、`rsi`、`rdx` 是 write 的三个参数。
- `xor rdi, rdi` 把 `rdi` 清零（自己异或自己 = 0），比 `mov rdi, 0` 更快。

---

## 七、在 Ghidra 中看到寄存器时怎么办

Ghidra 反汇编出来的代码里，你会频繁看到：

- `RAX`、`EAX`、`AL` —— 通常是返回值或临时计算。
- `RDI`、`RSI`、`RDX`、`RCX`、`R8`、`R9` —— 函数参数。
- `RSP`、`RBP` —— 栈操作。
- `RIP` —— 当前指令地址。

看到 `MOV EAX, ...` 时，意味着把某个值放到 32 位累加器中。看到 `CALL` 时，意味着调用函数，参数已经按约定放在寄存器里。

**记住调用约定，你就能猜出函数在干什么。**

---

## 八、存入结构库

| 触发条件 | 可行动作 |
|----------|----------|
| 在汇编/反汇编中看到 `eax`、`rax`、`ax`、`al` | 知道它们是同一个寄存器的不同位宽，64 位下优先看 `rax` |
| 看到 `rdi`、`rsi`、`rdx`、`rcx`、`r8`、`r9` | 识别为函数参数或系统调用参数 |
| 看到 `rax` 在 `syscall` 前被赋值 | 识别为系统调用号 |
| 看到 `rsp`、`rbp` | 识别为栈操作 |

---

**一句话总结**：寄存器是 CPU 的手，`rax` 是手的老大（累加器），`rdi`、`rsi` 等是传参的快递员。看懂它们，你就看懂了汇编在干什么。

---

## 数学拓展

这是一个数学拓展模块。读者接下来将在研究编程的同时学习高中数学竞赛入门内容。

**学习目标**：
- 理解函数迭代的数学定义与符号体系
- 能够用代码实现简单的函数迭代过程
- 能够用迭代思想解决简单的函数方程问题
- 初步接触数学竞赛中的“不动点”思想

### 1. 什么是函数迭代

你已经知道函数是一种“输入 → 输出”的映射规则。如果把一个函数的输出再次作为这个函数的输入，就构成了“迭代”。

令 `f(x)` 的定义域包含值域，则：

```
f¹(x) = f(x)
f²(x) = f(f(x))
f³(x) = f(f(f(x)))
...
fⁿ(x) = f(f(...f(x)...))
```

`fⁿ` 读作 f 的 n 次迭代。

**编程类比**：这几乎就是递归函数的雏形。

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

在数学竞赛中，函数迭代经常用“蛛网图”（Cobweb Plot）来可视化。原理如下：

- 在坐标系中画出函数 `y = f(x)` 图像
- 画出 `y = x`
- 从 `x_0` 出发向上画 `x_1 = f(x_0)`
- 水平移动到 `y = x`，得到 `(x_1, x_1)`
- 再画 `x_2 = f(x_1)`
- 无限循环

### 3. 不动点：迭代的“目的地”

如果存在某个数 `x*`，使得 `f(x*) = x*`，那么 `x*` 就是函数 f 的不动点。从不动点出发，迭代将永远停留在那里。

**数学意义**：不动点是迭代过程的“归宿”。竞赛题常问：迭代会收敛到哪个值？迭代何时发散？这些都是围绕不动点展开的。

### 4. 简单函数方程

函数方程是竞赛数学中的核心内容。它不像普通方程那样求一个数，而是求一个函数，使得它在所有输入上都满足某种条件。

**例子**：求所有函数 `f: R → R`，使得对任意 `x, y`，都有 `f(x+y) = f(x) + f(y)`。

这类方程的解为 `f(x) = kx`，可通过迭代与归纳证明。

---

## 本章结束语

你已经完成了从 Hello World 到多线程的完整路径。这 19 个 part 覆盖了编程中最核心的概念和操作，包含六种常用语言的具体实现。如果某个部分暂时用不到，可以跳过，等需要时再回来看。

接下来的章节将深入到运维、数据库、网络和系统底层，帮助你从“能写程序”进阶到“能理解程序如何运行”。
