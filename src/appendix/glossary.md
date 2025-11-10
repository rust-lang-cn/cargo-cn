# Glossary

- 词汇表

## Artifact

- 工件

一个*Artifact*是由编译过程创建的文件或文件集。这包括可链接库和可执行二进制文件.

## Crate

- 箱 (包/库， 又一抽象名)

包中的每个目标都是*箱*。Crates 是库或可执行二进制文件。它可能松散地引用目标的源代码或目标生成的编译工件。一个箱也可以指从注册表中提取的压缩包.

## Edition

- 版本

一个*Rust Edition*是 Rust 语言的开发里程碑。该[一个包的版本][edition-field]在`Cargo.toml`清单中指定，各个目标可以指定它们使用的版本。见[版本指南][edition guide]欲获得更多信息.

## Feature

- 特性/特征/功能

一个[_特征_][feature]是一个允许条件编译的命名标志参数。一个特性可以引用可选的依赖项，或者在 一个`Cargo.toml` 中定义的任意名称，可以键入(使用到)源代码中。

Cargo 有[_不稳定的特征标志_][cargo-unstable]，这可以用来实现 Cargo 本身的实验行为。Rust 编译器和 Rustdoc 也有自己的不稳定特征标志(参见[不稳定的书][unstable-book]和[Rustdoc 书][rustdoc-unstable]).

## Index

- 索引是注册表中，可搜索的包的列表.

## Lock file

- (锁定/锁) 文件

`Cargo.lock`，名:_锁定文件_，是一个文件，用于捕获工作空间或包中使用的每个依赖项的确切版本，它由 Cargo 自动生成。看到[Cargo.toml 与 Cargo.lock][cargo.toml vs cargo.lock].

## Manifest

- 清单/元信息

一个[_清单_][manifest]是对包或工作空间的描述，名为`Cargo.toml`.

一个[_虚拟清单_][virtual]是一个`Cargo.toml`，仅描述工作空间的文件，不包含包。

## Member

- 会员/成员

一个*成员*是属于工作空间的一个包.

## Package

- 包

一个*包*是源文件和描述包的清单`Cargo.toml`的集合。包具有名称和版本，用于指定包之间的依赖关系。包中包含多个目标，这些目标是库或可执行二进制文件。

该*包根*是`Cargo.toml`清单位于的包的目录。

该[_包 ID 规范_][pkgid-spec]， 要么*SPEC*，是一个字符串，用于辨识从特定源，引用特定版本的包唯一性。

## Package manager

<!-- package manager definition -->

## Package registry

见 [*registry*](#registry)

## Project

- 包/项目

[package](#package)的另一个名字.

## Registry

- 注册表

一个*注册处*是一种服务，包含可下载的包，可以安装或用作包的依赖项.默认注册表是[crates.io](https://crates.io)。注册表有一个*索引*，其中包含所有包装箱的清单，并告诉 Cargo 如何下载所需的箱。

## Source

- 源/资源

一个*源*是一个提供程序，包含箱子，这些箱子可当依赖项的包。有几种源:

- **注册表来源**- 见[注册表](#registry).
- **本地注册表源**- 在文件系统上存储为压缩文件的一组包。看到[本地注册表来源][local registry sources].
- **目录来源**- 在文件系统上存储为未压缩文件的一组包.看到[目录来源][directory sources].
- **路径来源**- 位于文件系统上的单个包(例如[路径依赖][path dependency])或一组多个包(如[路径覆盖][path overrides]).
- **Git 来源**- 位于 git 存储库中的包(例如[git 依赖][git dependency]要么[git 来源][git source]).

看到[来源更换][source replacement]欲获得更多信息.

## Spec

- 看到[包 ID 规范](#package).

## Target

- 目标

*Target*这个词的意思，取决于具体情况:

- **Cargo 目标** - Cargo 项目会有*target*，其具有对应将要生成的工件。项目可以包含库，二进制，示例，测试和基准目标。该[目标列表][targets]配置在`Cargo.toml`清单，通常由源文件的[目录布局][directory layout]自动推断的。
- **目标架构**- 构建工件的 OS 和机器体系结构，通常称为一个*目标*。
- **目标 三元(Triple)**- 三元组是用于指定目标体系结构的特定格式。见[clang 文档][clang documentation] 了解详情。三元组可以称为 一个*目标三元*，此为产生的工件的架构，以及*主机三元*，此为编译器运行的体系结构。可以使用`--target`命令行选项或`build.target` [配置选项]指定目标三元组。
- **目标目录**- Cargo 将所有构建的工件和中间文件放入*target*目录。默认情况下，这是一个名为`target`的目录会在工作区根目录，或者包根(如果不使用工作空间)。目录随着`--target-dir`命令行选项，`CARGO_TARGET_DIR` [环境变量][environment variable]， 或者`build.target-dir`
  [配置选项][config option]改变而改变.


> “target-triple (目标 三元)" 是 编译器的 专用术语，用 “ cpu- vendor- os" 来标识 交叉编译时的 系统类型

## Test Targets

Cargo*测试目标*生成二进制文件，帮助验证代码的正确操作和正确性。有两种类型的测试工件:

- **单元测试**- 一个*单元测试*是直接从库或二进制目标编译的可执行二进制文件。它包含库或二进制代码的全部内容，并运行`#[test]`注释函数，用于验证各个代码单元。
- **集成测试目标**- 一个[_集成测试目标_][integration-tests]是一个来自*测试目标*的可执行的二进制文件，这是一个独特的箱子，其来源位于`tests`目录或由在`Cargo.toml`清单里面的[`[[test]]`表][targets]指定。它旨在仅测试库的公共 API，或执行二进制文件以验证其操作。

## Workspace

- 工作区

一个[_工作区_][workspace]是一个共享公共依赖项解析(具有共享`Cargo.lock`)，输出目录和各种设置，如配置文件，的一个或多个包的集合。

一个[_虚拟工作区_][virtual]是`Cargo.toml`清单根目录的工作空间， 没有定义包，只列出工作区成员。

该*工作区根*是工作区的`Cargo.toml`清单位于的目录.

[cargo.toml vs cargo.lock]: ../cargo-toml-vs-cargo-lock.md
[directory sources]: ../reference/source-replacement.md#directory-sources
[local registry sources]: ../reference/source-replacement.md#local-registry-sources
[source replacement]: ../reference/source-replacement.md
[cargo-unstable]: https://doc.rust-lang.org/nightly/cargo/reference/unstable.md
[clang documentation]: http://clang.llvm.org/docs/CrossCompilation.md#target-triple
[config option]: ../reference/config.md
[directory layout]: ../reference/manifest.md#the-project-layout
[edition guide]: https://rust-lang-nursery.github.io/edition-guide/
[edition-field]: ../reference/manifest.md#the-edition-field-optional
[environment variable]: ../reference/environment-variables.md
[feature]: ../reference/manifest.md#the-features-section
[git dependency]: ../reference/specifying-dependencies.md#specifying-dependencies-from-git-repositories
[git source]: ../reference/source-replacement.md
[integration-tests]: ../reference/manifest.md#integration-tests
[manifest]: ../reference/manifest.md
[path dependency]: ../reference/specifying-dependencies.md#specifying-path-dependencies
[path overrides]: ../reference/specifying-dependencies.md#overriding-with-local-dependencies
[pkgid-spec]: ../reference/pkgid-spec.md
[rustdoc-unstable]: https://doc.rust-lang.org/nightly/rustdoc/unstable-features.md
[targets]: ../reference/manifest.md#configuring-a-target
[unstable-book]: https://doc.rust-lang.org/nightly/unstable-book/index.md
[virtual]: ../reference/manifest.md#virtual-manifest
[workspace]: ../reference/manifest.md#the-workspace-section
