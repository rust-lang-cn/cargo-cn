## 安装

### 安装 Rust 和 Cargo

获得 Cargo 的最简单方法是使用`rustup`脚本，获取当前稳定版本的 [Rust]。使用 `rustup` 安装 Rust 也会下载 Cargo。

在 Linux 和 macOS 系统上，这可以通过以下方式完成:

```bash
curl https://sh.rustup.rs -sSf | sh
```

它将下载一个脚本，然后开始安装。如果一切顺利，您会看到:

```bash
Rust is installed now. Great!
```

在 Windows 上，下载并运行 [rustup-init.exe]。它将在控制台中启动安装，并在成功时显示上述消息。

在此之后，你可以使用 `rustup` 命令，安装 `beta` 或者 `nightly` 版本的 Rust 和 Cargo。

有关其他安装选项和信息，请访问 Rust 网站的[安装][install-rust]页面。

### 从源代码构建 Cargo

或者，你可以[从源代码构建 Cargo][compiling-from-source].

[rust]: https://www.rust-lang.org/
[rustup-init.exe]: https://win.rustup.rs/
[install-rust]: https://www.rust-lang.org/install.md
[compiling-from-source]: https://github.com/rust-lang/cargo#compiling-from-source
