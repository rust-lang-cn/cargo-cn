# 创建一个新软件包

要使用 Cargo 生成新 [package][def-package]，请用 `cargo new` :

```console
$ cargo new hello_world --bin
```

传递 `--bin`，是因为我们正在生成一个二进制程序；而如果我们正在创建一个库(lib)，则需要传递 `--lib`。默认情况下，这个目录会初始化为一个新的 `git` 存储库，如果你不希望它这样做，请传递 `--vcs none`。

让我们来看看 Cargo 为我们生成了什么:

```console
$ cd hello_world
$ tree .
.
├── Cargo.toml
└── src
    └── main.rs

1 directory, 2 files
```

<!-- 这就是我们开始所需要的一切首。先让我们看看`Cargo.toml`: -->
我们再仔细看看 `Cargo.toml`: 

```toml
[package]
name = "hello_world"
version = "0.1.0"
edition = "2024"

[dependencies]
```

这被称为[***manifest***][def-manifest]，它包含了 Cargo 编译项目所需的所有元数据。这份文件以 [TOML][TOML] (读作 /tɑməl/)的格式记录下来。

那 `src/main.rs` 中有什么:

```rust
fn main() {
    println!("Hello, world!");
}
```

Cargo 为我们生成了一份 "hello world" 程序，也就是一份 [*binary crate*][def-crate]。现在来编译它:

```console
$ cargo build
   Compiling hello_world v0.1.0 (file:///path/to/project/hello_world)
```

然后运行它:

```console
$ ./target/debug/hello_world
Hello, world!
```

我们也可以直接使用 `cargo run` 来编译并运行它，这只需要一步 (若并没有修改源文件，`Compiling` 这行就不会出现):

```console
$ cargo run
   Compiling hello_world v0.1.0 (file:///path/to/package/hello_world)
     Running `target/debug/hello_world`
Hello, world!
```

你可能会发现 `Cargo.lock` 文件，它此前从未出现过。尽管讨论一份 `.lock` 文件索然无味，但它保管了你的项目的依赖信息，尽管你从未为项目引入任何依赖。

一旦你准备好构建 release，使用 `cargo build --release` 命令通过优化编译你的项目:

```console
$ cargo build --release
   Compiling hello_world v0.1.0 (file:///path/to/package/hello_world)
```

`cargo build --release` 会让你的二进制文件生成于 `target/release` 目录下，而非 `target/debug`。

在 debug 模式下编译是开发过程中的默认选项。此时编译器并不会做太多优化，所以编译时间较短，代价就是这样得到的二进制文件性能很差，跑得很慢。相对的，release 模式就会用掉更多时间编译，在此期间编译器会尽力优化，保证项目的运行速度。

[TOML]: https://toml.io
[def-crate]: ../appendix/glossary.md#crate
[def-manifest]: ../appendix/glossary.md#manifest
[def-package]: ../appendix/glossary.md#package
