# 初次使用 Cargo

本部分简要介绍`cargo`命令工具。我们将展示其构建 [***package***][def-package]，编译 package 内 [***crate***][def-crate]，以及其运行由此产生的程序的能力。

要使用 Cargo 启动新项目，请使用`cargo new`:

```console
$ cargo new hello_world --bin
```

<!-- 我们传递`--bin`，是因为我们正在制作一个二进制程序(默认): 如果我们正在创建一个库(lib)，我们就会把传递`--lib`. -->
Cargo 默认附带 `--bin` 来创建二进制程序。所以为了构建一个库 (`lib`)，我们需要传递 `--lib`。

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

这就是我们开始所需要的一切。首先，让我们看看`Cargo.toml`:

```toml
[package]
name = "hello_world"
version = "0.1.0"
edition = "2024"

[dependencies]
```

这被称为一个 [***manifest***][def-manifest]，它包含了 Cargo 编译项目所需的所有元数据。

那`src/main.rs`有啥:

```rust
fn main() {
    println!("Hello, world!");
}
```

<!-- Cargo 为我们创造了一个"hello_world".我们来编译它: -->
Cargo 为我们生成了一份 "hello world" 程序，也就是一份 [***binary crate***][def-crate]。现在来编译它: 

```console
$ cargo build
   Compiling hello_world v0.1.0 (file:///path/to/project/hello_world)
```

然后运行它:

```console
$ ./target/debug/hello_world
Hello, world!
```

<!-- 我们也可以直接使用`cargo run`，它会自行编译，然后运行它， 一步到位: -->
我们也可以直接使用 `cargo run` 来编译并运行它，这只需要一步:


```console
$ cargo run
     Fresh hello_world v0.1.0 (file:///path/to/project/hello_world)
   Running `target/hello_world`
Hello, world!
```

## 更进一步

有关使用 Cargo 的更多详细信息，请查看 [Cargo 指南](../guide/index.md)

[def-crate]: ../appendix/glossary.md#crate
[def-manifest]: ../appendix/glossary.md#manifest
[def-package]: ../appendix/glossary.md#package
