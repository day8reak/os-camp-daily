# [1.安装与运行](https://kaisery.github.io/trpl-zh-cn/ch01-01-installation.html#%E5%AE%89%E8%A3%85)

我使用的是虚拟机中的Ubuntu22.04。
打开终端并输入如下命令：
`$ curl --proto '=https' --tlsv1.2 https://sh.rustup.rs -sSf | sh`
安装 `build-essential` 包。
## 更新与卸载

命令行中运行如下更新脚本：

`$ rustup update`

若要卸载 Rust 和 `rustup`，请在命令行中运行如下卸载脚本：

`$ rustup self uninstall`

## 编译运行

输入如下命令，编译并运行文件：

```
$ rustc main.rs 
$ ./main
```

## Cargo

可以进行项目管理
构建项目
`$ cargo new hello_cargo `

├── Cargo.lock //记录项目依赖的实际版本
├── Cargo.toml //Cargo的配置文件
├── src //用于存储源文件
└── target //存放编译后的文件

## 构建并运行Cargo项目

`$ cargo build`

使用 `cargo build` 构建了项目，并使用 `./target/debug/hello_cargo` 运行了序，也可以使用 `cargo run` 在一个命令中同时编译并运行生成的可执行文件：

`$ cargo run`

Cargo 还提供了一个叫 `cargo check` 的命令。该命令快速检查代码确保其可以编译，但并不产生可执行文件：

`$ cargo check`

可以使用 `cargo build --release` 来优化编译项目。这会在 _target/release_ 而不是 _target/debug_ 下生成可执行文件。

#  [2. 写个猜数字游戏](https://kaisery.github.io/trpl-zh-cn/ch02-00-guessing-game-tutorial.html#%E5%86%99%E4%B8%AA%E7%8C%9C%E6%95%B0%E5%AD%97%E6%B8%B8%E6%88%8F)

主要是对rust的一个概览

```rust
use std::{cmp::Ordering, io};
use rand::Rng;

fn main() {
    println!("Guess the number!");

    let secret_number = rand::thread_rng().gen_range(1..=100);

    //println!("The select number is: {secret_number}");
    
    loop {
        println!("Please input your guess.");

        let mut guess = String::new();

        io::stdin()
            .read_line(&mut guess)
            .expect("Failed to read line");

        let guess: u32 = match guess.trim().parse() {
            Ok(num) => num,
            Err(_) => continue,
        };

        println!("You guessed: {guess}");

        match guess.cmp(&secret_number) {
            Ordering::Less => println!("Too small!"),
            Ordering::Greater => println!("Too big!"),
            Ordering::Equal => {
                println!("You win!");
                break;
            }
        }
    }
    
}
```


#  [3. 常见编程概念](https://kaisery.github.io/trpl-zh-cn/ch03-00-common-programming-concepts.html#%E5%B8%B8%E8%A7%81%E7%BC%96%E7%A8%8B%E6%A6%82%E5%BF%B5)
## [变量和可变性](https://kaisery.github.io/trpl-zh-cn/ch03-01-variables-and-mutability.html#%E5%8F%98%E9%87%8F%E5%92%8C%E5%8F%AF%E5%8F%98%E6%80%A7)

### 变量
定义变量, rust默认变量是不可改变的（immutable）

```rust
let x = 5;
```

要改变变量值需加mut
注：不能改变变量类型（加mut和C的变量差不多）

```rust
fn main() {
    let mut x = 5;
    println!("The value of x is: {x}");
    x = 6;
    println!("The value of x is: {x}");
}
```

### 常量

_常量 (constants)_ 是绑定到一个名称的不允许改变的值

```rust
const THREE_HOURS_IN_SECONDS: u32 = 60 * 60 * 3;
```

### 隐藏

可以定义一个与之前变量同名的新变量（感觉像直接覆盖了）

```rust
let x = 5; 
let x = x + 1;
{ 
	let x = x * 2; 
	println!("The value of x in the inner scope is: {x}"); 
} 
println!("The value of x is: {x}");
```
括号定义了个作用域，退出作用域就消失了
输出
```
12
6
```

## [数据类型](https://kaisery.github.io/trpl-zh-cn/ch03-02-data-types.html#%E6%95%B0%E6%8D%AE%E7%B1%BB%E5%9E%8B)

==Rust 是 **静态类型**（_statically typed_）语言，也就是说在编译时就必须知道所有变量的类型。==

### 标量类型

**标量**（_scalar_）类型代表一个单独的值。Rust 有四种基本的标量类型：整型、浮点型、布尔类型和字符类型。

**整型**

| 长度      | 有符号     | 无符号     |
| ------- | ------- | ------- |
| 8-bit   | `i8`    | `u8`    |
| 16-bit  | `i16`   | `u16`   |
| 32-bit  | `i32`   | `u32`   |
| 64-bit  | `i64`   | `u64`   |
| 128-bit | `i128`  | `u128`  |
| arch    | `isize` | `usize` |
**浮点型**

Rust 的浮点数类型是 `f32` 和 `f64`，分别占 32 位和 64 位

**数值运算**

**布尔型**

**字符类型**

（总体和别的语言差不多）

### 复合类型

**复合类型**（_Compound types_）可以将多个值组合成一个类型。Rust 有两个原生的复合类型：元组（tuple）和数组（array）。

**元组类型**

元组是一个将多个其他类型的值组合进一个复合类型的主要方式。元组长度固定：一旦声明，其长度不会增大或缩小。

```rust
let tup: (i32, f64, u8) = (500, 6.4, 1);
```

可以使用模式匹配（pattern matching）来解构（destructure）元组值，像这样：

```rust
let tup = (500, 6.4, 1); 
let (x, y, z) = tup;
```

我们也可以使用点号（`.`）后跟值的索引来直接访问它们

```rust
let x: (i32, f64, u8) = (500, 6.4, 1); 
let five_hundred = x.0; 
let six_point_four = x.1; 
let one = x.2;
```

**数组类型**

与元组不同，数组中的每个元素的类型必须相同。长度是固定的。

```rust
let a = [1, 2, 3, 4, 5];
let b: [i32; 5] = [1, 2, 3, 4, 5]; //`i32` 是每个元素的类型。分号之后，数字 `5` 表明该数组包含五个元素。
let c = [3; 5]; //包含 `5` 个元素，这些元素的值最初都将被设置为 `3`

let element = a[1];
```

访问和C一样

## [函数](https://kaisery.github.io/trpl-zh-cn/ch03-03-how-functions-work.html#%E5%87%BD%E6%95%B0)

无参

```rust
fn function() { 
	println!("function."); 
}
```

**参数**

**必须** 声明每个参数的类型

```rust
fn another_function(x: i32) {
    println!("The value of x is: {x}");
}
```

**语句和表达式**

**语句**（_Statements_）是执行一些操作但不返回值的指令。 **表达式**（_Expressions_）计算并产生一个值。

```rust
let y = { let x = 3; x + 1 };//括号里是表达式，没有分号会返回值
```

**具有返回值的函数**

要在箭头（`->`）后声明返回值的类型

```rust
fn five() -> i32 { 
	5 
}
fn plus_one(x: i32) -> i32 {
    x + 1
}
```

## [注释](https://kaisery.github.io/trpl-zh-cn/ch03-04-comments.html#%E6%B3%A8%E9%87%8A)

和C一样//注释一行
## [控制流](https://kaisery.github.io/trpl-zh-cn/ch03-05-control-flow.html#%E6%8E%A7%E5%88%B6%E6%B5%81)

### if表达式

```rust
fn main() {
    let number = 6;
    if number % 4 == 0 {
        println!("number is divisible by 4");
    } else if number % 3 == 0 {
        println!("number is divisible by 3");
    } else if number % 2 == 0 {
        println!("number is divisible by 2");
    } else {
        println!("number is not divisible by 4, 3, or 2");
    }
}
```

**在let中使用if**

把if返回的值返回给变量。

```rust
fn main() {
    let condition = true;
    let number = if condition { 5 } else { 6 };

    println!("The value of number is: {number}");
}
```

### 使用循环重复执行

Rust 有三种循环：`loop`、`while` 和 `for`。我们每一个都试试。

**loop**

可以使用 `break` 关键字来告诉程序何时停止循环
`continue` 关键字告诉程序跳过这个循环迭代中的任何剩余代码，并转到下一个迭代。
```rust
loop {
	println!("again!");
	if xxx {
		break;
	}
	if xxx {
		continue;
	}
}
```

**从循环返回值**

```rust
fn main() {
    let mut counter = 0;
    let result = loop {
        counter += 1;
        if counter == 10 {
            break counter * 2;
        }
    };
    println!("The result is {result}");
}
```

**循环标签：在多个循环之间消除歧义**

可以选择在一个循环上指定一个 **循环标签**（_loop label_），然后将标签与 `break` 或 `continue` 一起使用，使这些关键字应用于已标记的循环而不是最内层的循环。（C/C++只能作用于最内层）

```rust
fn main() {
    let mut count = 0;
    'counting_up: loop {
        println!("count = {count}");
        let mut remaining = 10;

        loop {
            println!("remaining = {remaining}");
            if remaining == 9 {
                break;
            }
            if count == 2 {
                break 'counting_up;
            }
            remaining -= 1;
        }

        count += 1;
    }
    println!("End count = {count}");
}
```

代码打印

```
count = 0
remaining = 10
remaining = 9
count = 1
remaining = 10
remaining = 9
count = 2
remaining = 10
End count = 2
```

**while条件循环**
```rust
fn main() {
    let mut number = 3;

    while number != 0 {
        println!("{number}!");

        number -= 1;
    }

    println!("LIFTOFF!!!");
}
```

**for循环**

```rust
fn main() {
    let a = [10, 20, 30, 40, 50];

    for element in a {
        println!("the value is: {element}");
    }
    
    for number in (1..4).rev() {
        println!("{number}!");
    }
    println!("LIFTOFF!!!");
}
```

# [4. 认识所有权](https://kaisery.github.io/trpl-zh-cn/ch04-00-understanding-ownership.html#%E8%AE%A4%E8%AF%86%E6%89%80%E6%9C%89%E6%9D%83)

## [什么是所有权？](https://kaisery.github.io/trpl-zh-cn/ch04-01-what-is-ownership.html#%E4%BB%80%E4%B9%88%E6%98%AF%E6%89%80%E6%9C%89%E6%9D%83)

Rust 通过所有权系统管理内存，编译器在编译时会根据一系列的规则进行检查。如果违反了任何这些规则，程序都不能编译。在运行时，所有权系统的任何功能都不会减慢程序。

### 所有权规则

> [!NOTE] 规则
> 1. Rust 中的每一个值都有一个 **所有者**（_owner_）。
> 2. 值在任一时刻有且只有一个所有者。
> 3. 当所有者（变量）离开作用域，这个值将被丢弃。

### 变量作用域

作用域是一个项（item）在程序中有效的范围。

```rust
    {                      // s 在这里无效，它尚未声明
        let s = "hello";   // 从此处起，s 是有效的

        // 使用 s
    }                      // 此作用域已结束，s 不再有效
```
换句话说，这里有两个重要的时间点：

- 当 `s` **进入作用域** 时，它就是有效的。
- 这一直持续到它 **离开作用域** 为止。

### String 类型

Rust 有另一种字符串类型，`String`。这个类型管理被分配到堆上的数据，所以能够存储在编译时未知大小的文本。可以使用 `from` 函数基于字符串字面值来创建 `String`，如下：

```rust
let s = String::from("hello");
```
这两个冒号 `::` 是运算符，允许将特定的 `from` 函数置于 `String` 类型的命名空间（namespace）下

**可以** 修改此类字符串

```rust
    let mut s = String::from("hello");

    s.push_str(", world!"); // push_str() 在字符串后追加字面值

    println!("{}", s); // 将打印 `hello, world!`
```

### 内存与分配

Rust 采取的策略：内存在拥有它的变量离开作用域后就被自动释放。

```rust
    {
        let s = String::from("hello"); // 从此处起，s 是有效的

        // 使用 s
    }                                  // 此作用域已结束，
                                       // s 不再有效
```
当变量离开作用域，Rust 为我们调用一个特殊的函数。这个函数叫做 [`drop`](https://doc.rust-lang.org/std/ops/trait.Drop.html#tymethod.drop)，Rust 在结尾的 `}` 处自动调用 `drop`。

**变量与数据的交互方式（一）：移动**

```rust
    let x = 5;
    let y = x;
	//因为整数是有已知固定大小的简单值，所以这两个 `5` 被放入了栈中。
    let s1 = String::from("hello");
    let s2 = s1;
	// `s1` 被 **移动** 到了 `s2` 中, `s1`无效了。
    println!("{}, world!", s1);
	
```

![[Pasted image 20240425150028.png]]

Rust 永远也不会自动创建数据的 “深拷贝”。因此，任何 **自动** 的复制都可以被认为是对运行时性能影响较小的。

**变量与数据的交互方式（二）：克隆**

如果我们 **确实** 需要深度复制 `String` 中堆上的数据，而不仅仅是栈上的数据，可以使用一个叫做 `clone` 的通用函数。

```rust
    let s1 = String::from("hello");
    let s2 = s1.clone();

    println!("s1 = {}, s2 = {}", s1, s2);
```

**只在栈上的数据：拷贝**

这样拷贝是有效的

```rust
    let x = 5;
    let y = x;

    println!("x = {}, y = {}", x, y);
```

> [!NOTE] 类型
> 如下是一些 `Copy` 的类型：
> 
> - 所有整数类型，比如 `u32`。
> - 布尔类型，`bool`，它的值是 `true` 和 `false`。
> - 所有浮点数类型，比如 `f64`。
> - 字符类型，`char`。
> - 元组，当且仅当其包含的类型也都实现 `Copy` 的时候。比如，`(i32, i32)` 实现了 `Copy`，但 `(i32, String)` 就没有。

### 所有权与函数

将值传递给函数与给变量赋值的原理相似。向函数传递值可能会移动或者复制，就像赋值语句一样。

```rust
fn main() {
    let s = String::from("hello");  // s 进入作用域

    takes_ownership(s);             // s 的值移动到函数里 ...
                                    // ... 所以到这里不再有效

    let x = 5;                      // x 进入作用域

    makes_copy(x);                  // x 应该移动函数里，
                                    // 但 i32 是 Copy 的，
                                    // 所以在后面可继续使用 x

} // 这里，x 先移出了作用域，然后是 s。但因为 s 的值已被移走，
  // 没有特殊之处

fn takes_ownership(some_string: String) { // some_string 进入作用域
    println!("{}", some_string);
} // 这里，some_string 移出作用域并调用 `drop` 方法。
  // 占用的内存被释放

fn makes_copy(some_integer: i32) { // some_integer 进入作用域
    println!("{}", some_integer);
} // 这里，some_integer 移出作用域。没有特殊之处
```

### 返回值与作用域

返回值也可以转移所有权。

```rust
fn main() {
    let s1 = gives_ownership();         // gives_ownership 将返回值
                                        // 转移给 s1

    let s2 = String::from("hello");     // s2 进入作用域

    let s3 = takes_and_gives_back(s2);  // s2 被移动到
                                        // takes_and_gives_back 中，
                                        // 它也将返回值移给 s3
} // 这里，s3 移出作用域并被丢弃。s2 也移出作用域，但已被移走，
  // 所以什么也不会发生。s1 离开作用域并被丢弃

fn gives_ownership() -> String {             // gives_ownership 会将
                                             // 返回值移动给
                                             // 调用它的函数

    let some_string = String::from("yours"); // some_string 进入作用域。

    some_string                              // 返回 some_string 
                                             // 并移出给调用的函数
                                             // 
}

// takes_and_gives_back 将传入字符串并返回该值
fn takes_and_gives_back(a_string: String) -> String { // a_string 进入作用域
                                                      // 

    a_string  // 返回 a_string 并移出给调用的函数
}
```

可以使用元组来返回多个值

```rust
fn main() {
    let s1 = String::from("hello");

    let (s2, len) = calculate_length(s1);

    println!("The length of '{}' is {}.", s2, len);
}

fn calculate_length(s: String) -> (String, usize) {
    let length = s.len(); // len() 返回字符串的长度

    (s, length)
}
```

Rust 提供了一个不用获取所有权就可以使用值的功能，叫做 **引用**（_references_）。

## [引用与借用](https://kaisery.github.io/trpl-zh-cn/ch04-02-references-and-borrowing.html#%E5%BC%95%E7%94%A8%E4%B8%8E%E5%80%9F%E7%94%A8)

**引用**（_reference_）像一个指针，因为它是一个地址，我们可以由此访问储存于该地址的属于其他变量的数据。 与指针不同，引用确保指向某个特定类型的有效值。

将创建一个引用的行为称为 **借用**（_borrowing_）。不能更改借用的变量。

```rust
fn main() {
    let s1 = String::from("hello");

    let len = calculate_length(&s1); //将创建一个引用的行为称为 借用（borrowing)

    println!("The length of '{}' is {}.", s1, len);
}

fn calculate_length(s: &String) -> usize {
    s.len()//不能更改值
}
```

![[Pasted image 20240425151804.png]]
注意：与使用 `&` 引用相反的操作是 **解引用**（_dereferencing_），它使用解引用运算符，`*`

### 可变引用

```rust
fn main() {
    let mut s = String::from("hello");

    change(&mut s);
}

fn change(some_string: &mut String) {
    some_string.push_str(", world");
}
```

==可变引用有一个很大的限制：如果你有一个对该变量的可变引用，你就不能再创建对该变量的引用。==

这个限制的好处是 Rust 可以在编译时就避免数据竞争。**数据竞争**（_data race_）类似于竞态条件，它可由这三个行为造成：

- 两个或更多指针同时访问同一数据。
- 至少有一个指针被用来写入数据。
- 没有同步数据访问的机制。

可以使用大括号来创建一个新的作用域，以允许拥有多个可变引用，只是不能 **同时** 拥有：

```rust
    let mut s = String::from("hello");

    {
        let r1 = &mut s;
    } // r1 在这里离开了作用域，所以我们完全可以创建一个新的引用

    let r2 = &mut s;

```

不能在拥有不可变引用的同时拥有可变引用。然而，多个不可变引用是可以的，因为没有哪个只能读取数据的人有能力影响其他人读取到的数据。

```rust
    let mut s = String::from("hello");

    let r1 = &s; // 没问题
    let r2 = &s; // 没问题
    let r3 = &mut s; // 大问题

    println!("{}, {}, and {}", r1, r2, r3);

```

注意一个引用的作用域从声明的地方开始一直持续到最后一次使用为止。

例如，因为最后一次使用不可变引用（`println!`)，发生在声明可变引用之前，所以如下代码是可以编译的：

```rust
    let mut s = String::from("hello");

    let r1 = &s; // 没问题
    let r2 = &s; // 没问题
    println!("{} and {}", r1, r2);
    // 此位置之后 r1 和 r2 不再使用

    let r3 = &mut s; // 没问题
    println!("{}", r3);
```

### 悬垂引用(Dangling References)

Rust 中编译器确保引用永远也不会变成悬垂状态：当你拥有一些数据的引用，编译器确保数据不会在其引用之前离开作用域。

以下代码编译无法通过

```rust
fn dangle() -> &String { // dangle 返回一个字符串的引用

    let s = String::from("hello"); // s 是一个新字符串

    &s // 返回字符串 s 的引用
} // 这里 s 离开作用域并被丢弃。其内存被释放。
  // 危险！
```

### 引用的规则

让我们概括一下之前对引用的讨论：

- 在任意给定时间，**要么** 只能有一个可变引用，**要么** 只能有多个不可变引用。
- 引用必须总是有效的。

## [Slice 类型](https://kaisery.github.io/trpl-zh-cn/ch04-03-slices.html#slice-%E7%B1%BB%E5%9E%8B)

_slice_ 允许你引用集合中一段连续的元素序列，而不用引用整个集合。slice 是一种引用，所以它没有所有权。

### 字符串slice

**字符串 slice**（_string slice_）是 `String` 中一部分值的引用，它看起来像这样：

```rust
let s = String::from("hello world");

let hello = &s[0..5];
let world = &s[6..11];

let s = String::from("hello");

let slice = &s[0..2];
let slice = &s[..2]; //省略开始


let s = String::from("hello");

let len = s.len();

let slice = &s[3..len];
let slice = &s[3..]; //省略尾部

let s = String::from("hello");

let len = s.len();

let slice = &s[0..len];
let slice = &s[..]; //省略两个值
```

注意：字符串 slice range 的索引必须位于有效的 UTF-8 字符边界内，如果尝试从一个多字节字符的中间位置创建字符串 slice，则程序将会因错误而退出。出于介绍字符串 slice 的目的，本部分假设只使用 ASCII 字符集；

“字符串 slice” 的类型声明写作 `&str`：

```rust
fn first_word(s: &String) -> &str {
    let bytes = s.as_bytes();

    for (i, &item) in bytes.iter().enumerate() {
        if item == b' ' {
            return &s[0..i];
        }
    }

    &s[..]
}
```

**字符串字面值就是 slice**

还记得我们讲到过字符串字面值被储存在二进制文件中吗？现在知道 slice 了，我们就可以正确地理解字符串字面值了：

```rust
let s = "Hello, world!";
```

这里 `s` 的类型是 `&str`：它是一个指向二进制程序特定位置的 slice。这也就是为什么字符串字面值是不可变的；`&str` 是一个不可变引用。

**字符串 slice 作为参数**

```rust
fn first_word(s: &str) -> &str {
```

```rust
fn main() {
    let my_string = String::from("hello world");

    // `first_word` 适用于 `String`（的 slice），部分或全部
    let word = first_word(&my_string[0..6]);
    let word = first_word(&my_string[..]);
    // `first_word` 也适用于 `String` 的引用，
    // 这等价于整个 `String` 的 slice
    let word = first_word(&my_string);

    let my_string_literal = "hello world";

    // `first_word` 适用于字符串字面值，部分或全部
    let word = first_word(&my_string_literal[0..6]);
    let word = first_word(&my_string_literal[..]);

    // 因为字符串字面值已经 **是** 字符串 slice 了，
    // 这也是适用的，无需 slice 语法！
    let word = first_word(my_string_literal);
}
```

### 其他类型的slice

引用数组的一部分
```rust
let a = [1, 2, 3, 4, 5];

let slice = &a[1..3];

assert_eq!(slice, &[2, 3]);
```


# [5. 使用结构体组织相关联的数据](https://kaisery.github.io/trpl-zh-cn/ch05-00-structs.html#%E4%BD%BF%E7%94%A8%E7%BB%93%E6%9E%84%E4%BD%93%E7%BB%84%E7%BB%87%E7%9B%B8%E5%85%B3%E8%81%94%E7%9A%84%E6%95%B0%E6%8D%AE)

## [结构体的定义和实例化](https://kaisery.github.io/trpl-zh-cn/ch05-01-defining-structs.html#%E7%BB%93%E6%9E%84%E4%BD%93%E7%9A%84%E5%AE%9A%E4%B9%89%E5%92%8C%E5%AE%9E%E4%BE%8B%E5%8C%96)

```rust
struct User {
    active: bool,
    username: String,
    email: String,
    sign_in_count: u64,
}
fn main() {
    let user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };
}
```

要修改字段值需要定义可变的

```rust
fn main() {
    let mut user1 = User {
        active: true,
        username: String::from("someusername123"),
        email: String::from("someone@example.com"),
        sign_in_count: 1,
    };

    user1.email = String::from("anotheremail@example.com");
}
```

注意整个实例必须是可变的；Rust 并不允许只将某个字段标记为可变。

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username: username,
        email: email,
        sign_in_count: 1,
    }
}
```

**使用字段初始化简写语法**

```rust
fn build_user(email: String, username: String) -> User {
    User {
        active: true,
        username,
        email,
        sign_in_count: 1,
    }
}
```

**使用结构体更新语法从其他实例创建实例**

要让user2的某些字段值和user1相等

```rust
fn main() {
    // --snip--

    let user2 = User {
        email: String::from("another@example.com"),
        ..user1
    };
}
```

请注意，结构更新语法就像带有 `=` 的赋值，因为它移动了数据，就像我们在[“变量与数据交互的方式（一）：移动”](https://kaisery.github.io/trpl-zh-cn/ch04-01-what-is-ownership.html#%E5%8F%98%E9%87%8F%E4%B8%8E%E6%95%B0%E6%8D%AE%E4%BA%A4%E4%BA%92%E7%9A%84%E6%96%B9%E5%BC%8F%E4%B8%80%E7%A7%BB%E5%8A%A8)部分讲到的一样。在这个例子中，总体上说我们在创建 `user2` 后不能就再使用 `user1` 了，因为 `user1` 的 `username` 字段中的 `String` 被移到 `user2` 中。如果我们给 `user2` 的 `email` 和 `username` 都赋予新的 `String` 值，从而只使用 `user1` 的 `active` 和 `sign_in_count` 值，那么 `user1` 在创建 `user2` 后仍然有效。`active` 和 `sign_in_count` 的类型是实现 `Copy` trait 的类型

**使用没有命名字段的元组结构体来创建不同的类型**

```rust
struct Color(i32, i32, i32);
struct Point(i32, i32, i32);

fn main() {
    let black = Color(0, 0, 0);
    let origin = Point(0, 0, 0);
}
```

注意 `black` 和 `origin` 值的类型不同，因为它们是不同的元组结构体的实例。

**没有任何字段的类单元结构体**

我们也可以定义一个没有任何字段的结构体！它们被称为 **类单元结构体**（_unit-like structs_）因为它们类似于 `()`，即[“元组类型”](https://kaisery.github.io/trpl-zh-cn/ch03-02-data-types.html#%E5%85%83%E7%BB%84%E7%B1%BB%E5%9E%8B)一节中提到的 unit 类型。

```rust
struct AlwaysEqual;

fn main() {
    let subject = AlwaysEqual;
}
```

## [结构体示例程序](https://kaisery.github.io/trpl-zh-cn/ch05-02-example-structs.html#%E7%BB%93%E6%9E%84%E4%BD%93%E7%A4%BA%E4%BE%8B%E7%A8%8B%E5%BA%8F)

## [方法语法](https://kaisery.github.io/trpl-zh-cn/ch05-03-method-syntax.html#%E6%96%B9%E6%B3%95%E8%AF%AD%E6%B3%95)

**方法**（method）与函数类似：它们使用 `fn` 关键字和名称声明，可以拥有参数和返回值，同时包含在某处调用该方法时会执行的代码。不过方法与函数是不同的，因为它们在结构体的上下文中被定义（或者是枚举或 trait 对象的上下文，将分别在[第六章](https://kaisery.github.io/trpl-zh-cn/ch06-00-enums.html)和[第十七章](https://kaisery.github.io/trpl-zh-cn/ch17-02-trait-objects.html)讲解），并且它们第一个参数总是 `self`，它代表调用该方法的结构体实例。

### 定义方法

```rust
#[derive(Debug)]
struct Rectangle {
    width: u32,
    height: u32,
}

impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

fn main() {
    let rect1 = Rectangle {
        width: 30,
        height: 50,
    };

    println!(
        "The area of the rectangle is {} square pixels.",
        rect1.area()
    );
}
```

这里选择 `&self` 的理由跟在函数版本中使用 `&Rectangle` 是相同的：我们并不想获取所有权，只希望能够读取结构体中的数据，而不是写入。如果想要在方法中改变调用方法的实例，需要将第一个参数改为 `&mut self`。通过仅仅使用 `self` 作为第一个参数来使方法获取实例的所有权是很少见的；这种技术通常用在当方法将 `self` 转换成别的实例的时候，这时我们想要防止调用者在转换之后使用原始的实例。

### 带有更多参数的方法

在 `Rectangle` 上实现 `can_hold` 方法，它获取另一个 `Rectangle` 实例作为参数

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }

    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```

### 关联函数

所有在 `impl` 块中定义的函数被称为 **关联函数**（_associated functions_），因为它们与 `impl` 后面命名的类型相关。我们可以定义不以 `self` 为第一参数的关联函数（因此不是方法），因为它们并不作用于一个结构体的实例。

不是方法的关联函数经常被用作返回一个结构体新实例的构造函数。这些函数的名称通常为 `new` ，但 `new` 并不是一个关键字。例如我们可以提供一个叫做 `square` 关联函数，它接受一个维度参数并且同时作为宽和高，这样可以更轻松的创建一个正方形 `Rectangle` 而不必指定两次同样的值：

```rust
impl Rectangle {
    fn square(size: u32) -> Self {
        Self {
            width: size,
            height: size,
        }
    }
}
```

关键字 `Self` 在函数的返回类型中代指在 `impl` 关键字后出现的类型，在这里是 `Rectangle`

使用结构体名和 `::` 语法来调用这个关联函数：比如 `let sq = Rectangle::square(3);`。这个函数位于结构体的命名空间中：`::` 语法用于关联函数和模块创建的命名空间。

### 多个impl块

每个结构体都允许拥有多个 `impl` 块。

```rust
impl Rectangle {
    fn area(&self) -> u32 {
        self.width * self.height
    }
}

impl Rectangle {
    fn can_hold(&self, other: &Rectangle) -> bool {
        self.width > other.width && self.height > other.height
    }
}
```

# [6. 枚举和模式匹配](https://kaisery.github.io/trpl-zh-cn/ch06-00-enums.html#%E6%9E%9A%E4%B8%BE%E5%92%8C%E6%A8%A1%E5%BC%8F%E5%8C%B9%E9%85%8D)

## [枚举的定义](https://kaisery.github.io/trpl-zh-cn/ch06-01-defining-an-enum.html#%E6%9E%9A%E4%B8%BE%E7%9A%84%E5%AE%9A%E4%B9%89)

```rust
enum IpAddrKind {
    V4,
    V6,
}
```

### 枚举值

可以像这样创建 `IpAddrKind` 两个不同成员的实例：

```rust
    let four = IpAddrKind::V4;
    let six = IpAddrKind::V6;
```

注意枚举的成员位于其标识符的命名空间中，并使用两个冒号分开。这么设计的益处是现在 `IpAddrKind::V4` 和 `IpAddrKind::V6` 都是 `IpAddrKind` 类型的。例如，接着可以定义一个函数来获取任何 `IpAddrKind`：

```rust
fn route(ip_kind: IpAddrKind) {}
```

现在可以使用任一成员来调用这个函数：

```rust
    route(IpAddrKind::V4);
    route(IpAddrKind::V6);
```

```rust
    enum IpAddrKind {
        V4,
        V6,
    }

    struct IpAddr {
        kind: IpAddrKind,
        address: String,
    }

    let home = IpAddr {
        kind: IpAddrKind::V4,
        address: String::from("127.0.0.1"),
    };

    let loopback = IpAddr {
        kind: IpAddrKind::V6,
        address: String::from("::1"),
    };
```

我们可以使用一种更简洁的方式来表达相同的概念，仅仅使用枚举并将数据直接放进每一个枚举成员而不是将枚举作为结构体的一部分。`IpAddr` 枚举的新定义表明了 `V4` 和 `V6` 成员都关联了 `String` 值：

```rust
    enum IpAddr {
        V4(String),
        V6(String),
    }

    let home = IpAddr::V4(String::from("127.0.0.1"));

    let loopback = IpAddr::V6(String::from("::1"));
```

每一个我们定义的枚举成员的名字也变成了一个构建枚举的实例的函数。也就是说，`IpAddr::V4()` 是一个获取 `String` 参数并返回 `IpAddr` 类型实例的函数调用。作为定义枚举的结果，这些构造函数会自动被定义。

用枚举替代结构体还有另一个优势：每个成员可以处理不同类型和数量的数据。IPv4 版本的 IP 地址总是含有四个值在 0 和 255 之间的数字部分。如果我们想要将 `V4` 地址存储为四个 `u8` 值而 `V6` 地址仍然表现为一个 `String`，这就不能使用结构体了。枚举则可以轻易的处理这个情况：

```rust
    enum IpAddr {
        V4(u8, u8, u8, u8),
        V6(String),
    }

    let home = IpAddr::V4(127, 0, 0, 1);

    let loopback = IpAddr::V6(String::from("::1"));
```

标准库调用的IpAddr

```rust
struct Ipv4Addr {
    // --snip--
}

struct Ipv6Addr {
    // --snip--
}

enum IpAddr {
    V4(Ipv4Addr),
    V6(Ipv6Addr),
}
```

Message枚举

```rust
enum Message {
    Quit,
    Move { x: i32, y: i32 },
    Write(String),
    ChangeColor(i32, i32, i32),
}
//和下方数据相同
struct QuitMessage; // 类单元结构体
struct MoveMessage {
    x: i32,
    y: i32,
}
struct WriteMessage(String); // 元组结构体
struct ChangeColorMessage(i32, i32, i32); // 元组结构体
```

结构体和枚举还有另一个相似点：就像可以使用 `impl` 来为结构体定义方法那样，也可以在枚举上定义方法。这是一个定义于我们 `Message` 枚举上的叫做 `call` 的方法：

```rust
    impl Message {
        fn call(&self) {
            // 在这里定义方法体
        }
    }

    let m = Message::Write(String::from("hello"));
    m.call();
```

### Option枚举和其相对于空值的优势

`Option` 类型应用广泛因为它编码了一个非常普遍的场景，即一个值要么有值要么没值。

Rust 并没有空值，不过它确实拥有一个可以编码存在或不存在概念的枚举。这个枚举是 `Option<T>`

```rust
enum Option<T> {
    None,
    Some(T),
}
```

可以不需要 `Option::` 前缀来直接使用 `Some` 和 `None`。即便如此 `Option<T>` 也仍是常规的枚举，`Some(T)` 和 `None` 仍是 `Option<T>` 的成员。

```rust
    let some_number = Some(5);
    let some_char = Some('e');
    let absent_number: Option<i32> = None;
```

`some_number` 的类型是 `Option<i32>`。`some_char` 的类型是 `Option<char>`，这（与 `some_number`）是一个不同的类型。因为我们在 `Some` 成员中指定了值，Rust 可以推断其类型。
对于 `absent_number`，Rust 需要我们指定 `Option` 整体的类型，因为编译器只通过 `None` 值无法推断出 `Some` 成员保存的值的类型。这里我们告诉 Rust 希望 `absent_number` 是 `Option<i32>` 类型的。

当有一个 `Some` 值时，我们就知道存在一个值，而这个值保存在 `Some` 中。当有个 `None` 值时，在某种意义上，它跟空值具有相同的意义：并没有一个有效的值。那么，`Option<T>` 为什么就比空值要好呢？

简而言之，因为 `Option<T>` 和 `T`（这里 `T` 可以是任何类型）是不同的类型，编译器不允许像一个肯定有效的值那样使用 `Option<T>`。

## [`match` 控制流结构](https://kaisery.github.io/trpl-zh-cn/ch06-02-match.html#match-%E6%8E%A7%E5%88%B6%E6%B5%81%E7%BB%93%E6%9E%84)

Rust 有一个叫做 `match` 的极为强大的控制流运算符，它允许我们将一个值与一系列的模式相比较，并根据相匹配的模式执行相应代码。模式可由字面值、变量、通配符和许多其他内容构成

```rust
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter,
}

fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter => 25,
    }
}
```

当 `match` 表达式执行时，它将结果值按顺序与每一个分支的模式相比较。如果模式匹配了这个值，这个模式相关联的代码将被执行。如果模式并不匹配这个值，将继续执行下一个分支

每个分支相关联的代码是一个表达式，而表达式的结果值将作为整个 `match` 表达式的返回值。

### 绑定值的模式

匹配分支的另一个有用的功能是可以绑定匹配的模式的部分值。这也就是如何从枚举成员中提取值的。

我们在匹配 `Coin::Quarter` 成员的分支的模式中增加了一个叫做 `state` 的变量。当匹配到 `Coin::Quarter` 时，变量 `state` 将会绑定 25 美分硬币所对应州的值。接着在那个分支的代码中使用 `state`，如下：

```rust
#[derive(Debug)] // 这样可以立刻看到州的名称
enum UsState {
    Alabama,
    Alaska,
    // --snip--
}
enum Coin {
    Penny,
    Nickel,
    Dime,
    Quarter(UsState),
}
fn value_in_cents(coin: Coin) -> u8 {
    match coin {
        Coin::Penny => 1,
        Coin::Nickel => 5,
        Coin::Dime => 10,
        Coin::Quarter(state) => {
            println!("State quarter from {:?}!", state);
            25
        }
    }
}
```

### 匹配`Option<T>`

我们在之前的部分中使用 `Option<T>` 时，是为了从 `Some` 中取出其内部的 `T` 值；我们还可以像处理 `Coin` 枚举那样使用 `match` 处理 `Option<T>`！只不过这回比较的不再是硬币，而是 `Option<T>` 的成员，但 `match` 表达式的工作方式保持不变。

```rust
    fn plus_one(x: Option<i32>) -> Option<i32> {
        match x {
            None => None,
            Some(i) => Some(i + 1),
        }
    }

    let five = Some(5);
    let six = plus_one(five);
    let none = plus_one(None);
```

### 匹配`Some(T)`

### 匹配是穷尽的

`match` 分支必须覆盖了所有的可能性。

### 通配模式和 \_占位符

`_`可以匹配任意值而不绑定到该值。

```rust
    let dice_roll = 9;
    match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        _ => reroll(),
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
    fn reroll() {}
```

我们可以使用单元值（在[“元组类型”](https://kaisery.github.io/trpl-zh-cn/ch03-02-data-types.html#%E5%85%83%E7%BB%84%E7%B1%BB%E5%9E%8B)一节中提到的空元组）作为 `_` 分支的代码表示无事发生。

```rust
    let dice_roll = 9;
    match dice_roll {
        3 => add_fancy_hat(),
        7 => remove_fancy_hat(),
        _ => (),
    }

    fn add_fancy_hat() {}
    fn remove_fancy_hat() {}
```

## [`if let` 简洁控制流](https://kaisery.github.io/trpl-zh-cn/ch06-03-if-let.html#if-let-%E7%AE%80%E6%B4%81%E6%8E%A7%E5%88%B6%E6%B5%81)






