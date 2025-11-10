# 为什么需要 Cargo

<!-- Cargo 是一个工具,允许 Rust 项目声明其各种依赖项，并确保您始终获得可重复的构建。

为了实现这一目标,Cargo 做了四件事:

- 引入两个，包含各种项目信息的元数据文件。
- 获取，并构建项目的依赖项.
- 正确使用参数，以调用`rustc`或其他构建工具，构建你的项目。
- 介绍，更容易使用 Rust 项目的约定(规范/风格)。 -->

## 在这之前

如你所知，在 Rust 中，一份可执行文件或者库，被称为 [crate][def-crate]。人们通常使用 Rust 编译器，也就是 `rustc` 来编译它们。开始学习 Rust 时，大多数人都是先遇见那份经典的 "hello world"，然后使用 `rustc` 来直接编译它: 

```console
$ rustc hello.rs
$ ./hello
Hello, world!
```

注意到在上面的命令中，你需要指明文件的名称。如果你直接使用 `rustc` 编译别的程序，就需要使用不同的命令；而当你需要指定任何特定的编译器标识或者包含外部依赖时，那么所需要的命令会更明确(也更复杂)。

同时，大多数非简单的程序可能会依赖于外部库，这些外部依赖的复杂度也会影响所需要的命令。因此，手动获取所有必要依赖的正确版本，并保持版本更新是十分困难且易于出错的。

幸运的是，我们可以通过引入更高一级的 ["package"][def-package] 抽象和使用[包管理器 (package manager)][def-package-manager] 来避免上述直接使用 `rustc` 操作 crate 带来的复杂问题。

## 开始: Cargo

*Cargo* 是 Rust 的包管理器。它允许 Rust [packages][def-package] 声明自己多样的依赖，并保证你总能进行可重复的构建过程。

为了实现这一目标，Cargo 做了下面四件事: 

- 引入两份元数据文件，其中含有包的信息。
- 获取并构建包依赖。
- 添加正确参数并调用 `rustc` 或别的构建工具来构建你的 package。
- 引入约定简化 Rust 包的使用。

很大程度上来说，Cargo 规范了构建程序或库所需的命令，这是上面所提到的 "约定" 中的一个方面。稍后我们会展示: 同样的命令能用于不同的 [artifact][def-artifact] 构建，不论它们的名称是否一致。并不直接调用 `rustc`，而是使用诸如 `cargo build` 之类更具通用性的命令，把构建正确 `rustc` 调用的包袱丢给 cargo。不仅如此，Cargo 还会自动地根据你的定义，从 [*registry*][def-registry] 中拉取依赖，并根据需要，将它们合理地加入到你的构建过程中。

毫不夸张地说，一旦你掌握了如何构建一个 Cargo 项目，你就能举一反三，知晓如何构建所有的 Cargo 项目。

[def-artifact]: ../appendix/glossary.md#artifact '"artifact (术语详解)"'
[def-crate]: ../appendix/glossary.md#crate '"crate (术语详解)"'
[def-package]: ../appendix/glossary.md#package '"package" (术语详解)'
[def-package-manager]: ../appendix/glossary.md#package-manager '"package manager (术语详解)"'
[def-registry]: ../appendix/glossary.md#registry '"registry (术语详解)"'
