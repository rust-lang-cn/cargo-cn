## 创建一个新项目

要使用 Cargo 启动新项目，请使用`cargo new`:

```shell
$ cargo new hello_world --bin
```

我们传递`--bin`，是因为我们正在制作一个二进制程序(默认): 如果我们正在创建一个库(lib)，我们就会把传递`--lib`。默认情况下，这个目录会初始化为一个新的`git`存储库，如果您不希望它这样做，请传递`--vcs none`。

让我们来看看 Cargo 为我们带来了什么:

```shell
$ cd hello_world
$ tree .
.
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files
```

这就是我们开始所需要的一切首。先让我们看看`Cargo.toml`:

```toml
[package]
name = "hello_world"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]
edition = "2018"

[dependencies]
```

这被称为一个**manifest**元清单，它包含了 Cargo 编译项目所需的所有元数据.

那`src/main.rs`有啥:

```rust
fn main() {
    println!("Hello, world!");
}
```

Cargo 为我们创造了一个"hello_world".我们来编译它:

```shell
$ cargo build
   Compiling hello_world v0.1.0 (file:///path/to/project/hello_world)
```

然后运行它:

```shell
$ ./target/debug/hello_world
Hello, world!
```

我们也可以直接使用`cargo run`，它会自行编译，然后运行它, 一步到位:

```shell
$ cargo run
     Fresh hello_world v0.1.0 (file:///path/to/project/hello_world)
   Running `target/hello_world`
Hello, world!
```

您会注意到已创建了几个新文件和目录:

```shell
$ tree .
.
|-- Cargo.lock
|-- Cargo.toml
|-- src
|   `-- main.rs
`-- target
    `-- debug
        |-- build
        |-- deps
        |   |-- hello_world-6ad0b2df81336e7f
        |   |-- hello_world-6ad0b2df81336e7f.d
        |   `-- hello_world-6ad0b2df81336e7f.dSYM
        |       `-- Contents
        |           |-- Info.plist
        |           `-- Resources
        |               `-- DWARF
        |                   `-- hello_world-6ad0b2df81336e7f
        |-- examples
        |-- hello_world
        |-- hello_world.d
        |-- hello_world.dSYM -> deps/hello_world-6ad0b2df81336e7f.dSYM
        |-- incremental
        |   // ...
        `-- native

15 directories, 19 files
```

这个`Cargo.lock`文件啊，是包含我们的依赖项的有关信息(即便还没有依赖)，其内容看起来可不是很有趣啊。再有就是`target`目录包含所有构建产品(二进制文件..)，并且，可以看出，Cargo 默认生成调试(debug)版本。您可以使用`cargo build --release`，这会在开启优化的情况下，编译文件:

```shell
$ cargo build --release
   Compiling hello_world v0.1.0 (file:///path/to/project/hello_world)
```

`cargo build --release`将结果二进制文件放入`target/release`，而不再是`target/debug`目录.

调试模式的编译是开发的默认设置 - 编译时间较短，因为编译器不进行优化，但代码运行速度较慢。发布(release)模式编译需要更长时间，但代码运行速度更快。
