# 依赖项

[crates.io]是 Rust 社区的中央软件包注册表[package registry][def-package-registry]，用于寻找和下载软件包。`cargo` 使用其作为默认软件源来查找需要的包。

要获取托管在[crates.io]的依赖库，只需将它添加到`Cargo.toml`。

[crates.io]: https://crates.io/

## 添加依赖

如果你的 `Cargo.toml` 还没有 `[dependencies]` 部分，添加这个条目，并在下面列举你所需要的依赖。下面的例子添加了一个 `time` crate:

```toml
[dependencies]
time = "0.1.12"
```

版本字符串是 [SemVer (语义化版本)][SemVer] 的要求。[指定依赖项](specifying-dependencies.md)的文档提供了更多相关信息。

[SemVer]: https://semver.org/

如果我们想额外添加一个 `regex` 依赖，我们不需要为每个 crate 都添加 `[dependencies]`。只需要像如下文件一样，将所有的依赖条目，如 `time` 和 `regex`，置于 `[dependencies]` 下即可:

```toml
[package]
name = "hello_world"
version = "0.1.0"
edition = "2024"

[dependencies]
time = "0.1.12"
regex = "0.1.41"
```

再次运行 `cargo build`，Cargo 将获取新的项目依赖和这些依赖的依赖，编译它们，并更新`Cargo.lock`:

```console
$ cargo build
      Updating crates.io index
   Downloading memchr v0.1.5
   Downloading libc v0.1.10
   Downloading regex-syntax v0.2.1
   Downloading memchr v0.1.5
   Downloading aho-corasick v0.3.0
   Downloading regex v0.1.41
     Compiling memchr v0.1.5
     Compiling libc v0.1.10
     Compiling regex-syntax v0.2.1
     Compiling memchr v0.1.5
     Compiling aho-corasick v0.3.0
     Compiling regex v0.1.41
     Compiling hello_world v0.1.0 (file:///path/to/package/hello_world)
```

`Cargo.lock` 文件包含了这些依赖项的具体版本信息。

现在，除非运行 `cargo update` 手动更新依赖版本，否则即便 `regex` 更新了，我们也会一直使用当前确定的版本进行构建。

于是现在你可以在 `main.rs` 中调用 `regex` 库: 

```rust
use regex::Regex;

fn main() {
    let re = Regex::new(r"^\d{4}-\d{2}-\d{2}$").unwrap();
    println!("Did our date match? {}", re.is_match("2014-01-01"));
}
```

运行它将显示:

```console
$ cargo run
   Running `target/hello_world`
Did our date match? true
```
