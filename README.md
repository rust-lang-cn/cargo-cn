# Cargo 文档

[![LICENSE-MIT](https://img.shields.io/badge/license-MIT-green)](https://raw.githubusercontent.com/rust-lang-cn/cargo-cn/master/LICENSE-MIT)
[![LICENSE-APACHE](https://img.shields.io/badge/license-Apache%202-blue)](https://raw.githubusercontent.com/rust-lang-cn/cargo-cn/master/LICENSE-APACHE)
![GitHub last commit](https://img.shields.io/github/last-commit/rust-lang-cn/cargo-cn?color=gold)
![GitHub contributors](https://img.shields.io/github/contributors/rust-lang-cn/cargo-cn?color=pink)
![rustwiki.org](https://img.shields.io/website?up_message=rustwiki.org&url=https%3A%2F%2Frustwiki.org)

> Chinese translation of the [Cargo documentation][cargo-doc]
>
> 中文译注：
>
> 1. Cargo 文档源码主要包含两部分，一是《Cargo 手册》，二是《Cargo 帮助手册》。
> 2. 本仓库翻译内容源自 [chinanf-boy]的开源的[翻译版本][chinanf-boy-cargo]，我们对原译者感激不尽！该版本最后翻译更新时间是 2019 年 5 月 12 日，与当前的英文版差异比较大，特别期待您加入到本开源翻译项目组中来维护本书，确保本书紧跟英文版。
> 3. 我们怀着朴素的理想，努力将 Rust 资源翻译成中文，让中文世界中拥有更多官方的 Rust 资源。

[cargo-doc]: https://github.com/rust-lang/cargo/tree/master/src/doc
[chinanf-boy]: https://github.com/chinanf-boy
[chinanf-boy-cargo]: https://github.com/chinanf-boy/cargo-book-zh

本目录包含 Cargo 的文档，包含两部分，一是使用 [mdbook] 构建的[《Cargo 手册》][The Cargo Book]，二是使用 [mdman] 构建的帮助手册（man 手册）。

> 注：帮助手册暂未翻译成中文版。

[The Cargo Book]: https://doc.rust-lang.org/cargo/
[mdBook]: https://github.com/rust-lang/mdBook
[mdman]: https://github.com/rust-lang/cargo/tree/master/crates/mdman/


### 构建

构建书籍需要 [mdBook]。获取安装：

```console
$ cargo install mdbook
```

构建本书：

```console
$ mdbook build
```

`mdbook` 提供了各种不同的命令和选项来帮助你对书本进行操作：

* `mdbook build --open`：构建书本并在 Web 浏览器中打开。
* `mdbook serve`：在本地主机上启动 Web 服务器。每当任何文件更改时，它也会自动重建书籍，并自动重新加载 Web 浏览器。

书籍文件和目录由 [`SUMMARY.md`](src/SUMMARY.md) 确定，并且每个文件都必须在此处给出。

### 构建帮助手册页

帮助手册页使用名为 [mdman] 的工具将标记转换为手册页格式。 有关更多详细信息，请查阅位于 *cargo* 仓库（官方的 Cargo 工具仓库）中的 [`mdman/doc/`][nmman-doc] 文档。

手册页从 Markdown 模板（位于 [`src/etc/man/`][man-doc] 目录中）转换为三种不同的格式：

1. Troff 样式的手册页，保存在 `src/etc/man/` 目录中。
2. 《Cargo 手册》中的 Markdown 文件（包含一些 HTML），保存在 `src/doc/src/commands/` 目录中。
3. 纯文本（在Windows中没有人的平台上的嵌入式手册页需要），保存在 `src/doc/man/generated_txt/` 目录中。

要重新构建手册页，请build-man.sh在src/doc目录中运行脚本。

```console
$ ./build-man.sh
```

[nmman-doc]: https://github.com/rust-lang/cargo/tree/master/crates/mdman
[man-doc]: https://github.com/rust-lang/cargo/tree/master/src/etc/man

### 语义化版本章节测试

有一个脚本可以验证语义化版本章节中的示例是否按预期工作。要运行测试，请进入 `semver-check` 目录并运行 `cargo run`。

### 参与贡献

我们很乐意为您提供改善文档的帮助！您可以随时提出有关任何问题的信息，并发送 P R来解决您要修复或更改的问题。如果您的更改很大，请先打开一个 Issue，这样我们才能确保在您完成对应的 PR 的工作之前，我们会接受这一点。

### 许可协议

《Cargo 手册》和《Cargo 帮助手册》按照 MIT 许可证和 Apache 2.0 许可证进行授权。

有关详细信息，请参见 [LICENSE-APACHE](LICENSE-APACHE) 和 [LICENSE-MIT](LICENSE-MIT)。
