## 配置

本文档将解释 Cargo 的配置系统如何工作，以及可用的字段或配置。有关通过其清单，来配置程序包的信息，请参阅[清单格式](./manifest.md).

### Hierarchical structure

分层策略

Cargo 允许特定包，具有本地配置以及全局配置，就像 git 一样。Cargo 将其扩展为分层策略。例如，如果在`/projects/foo/bar/baz`调用 Cargo，然后将按以下顺序，探测和统一配置文件:

- `/projects/foo/bar/baz/.cargo/config`
- `/projects/foo/bar/.cargo/config`
- `/projects/foo/.cargo/config`
- `/projects/.cargo/config`
- `/.cargo/config`
- `$HOME/.cargo/config`

使用此结构，您可以为每个包指定配置，甚至可以将其检入版本控制。您还可以在主目录中，使用指定默认值的配置文件。

### Configuration format

配置格式

所有配置目前都在[TOML 格式][toml](与 Cargo.toml 清单一样)，在字段(表格)内部使用简单的键值对，它们都被合并在一起。

[toml]: https://github.com/toml-lang/toml

### Configuration keys

配置字段

以下所有字段都是可选的，除非另有说明，否则它们的默认值将作为其值。

指定工具的键值可以给出，一个‘绝对路径，相对路径或无路径工具名称’。给定的绝对路径和无路径工具名称直接使用。相对路径，则解析相对于`.cargo`目录的父目录，配置文件就居住在里面。

```bash
# 路径数组，指向本地将要覆盖依赖项的存储库.
# 更多信息，请看 指定 依赖项 指南.
paths = ["/path/to/override"]

[cargo-new]
# 这是你 放 name/email 的地方， 在一个新Cargo.toml中的
#`authors` 表格就会生成。若不存在， 那 `git` 会去调查， 若还是不
# 存在，那 `$USER` 和 `$EMAIL` 会被使用.
name = "..."
email = "..."

# 默认来说， `cargo new` 会初始化一个新的  Git repository. 该字段若设为 `hg` ，就是新建一个 Mercurial repository， 或 `none` 禁用此行为.
vcs = "none"

# 接下来的 部分， $triple 是 一些有效目标 triple的引用， 而不是一个字面量"$triple"的意思， 这个引用是无论何时都能应用编译的。
# 'cfg(...)' 是参考 类Rust `#[cfg]` 语法 (条件语句)
[target.$triple]
#  linker 可传递参数给 rustc (通过 `-C linker=`) 当 `$triple`
# 要被编译. 默认不传递东东
linker = ".."
# 一样，但这是传递给 rustc 关于 库压缩的参数 ，通过 `-C ar=`.
ar = ".."
# 若 提供了 一个 runner ， 编译`$triple`目标 会通过 执行 runner 执行文件来完成， 它会将真正的目标 作为第一参数.
# 这可运行 `cargo run`， `cargo test` 和 `cargo bench` 命令.
# 默认，编译目标是 直接执行的.
runner = ".."
# 自定义 全编译器，目标为 $triple
# 这些值会覆盖 build.rustflags
rustflags = ["..", ".."]

[target.'cfg(...)']
# 与 $triple 配置类似， 但使用的是 `cfg` 语法.
# 若有几个 `cfg` 和 $triple 目标作为备用， 那 rustflags
# 会被串联起来. 该 `cfg` 语法仅能应用到 rustflags， 而不能是
# linker.
rustflags = ["..", ".."]
# 与 $triple 配置类似 ， 但使用的是 `cfg` 语法.
# 若 一个或多个 `cfg`s， 和一个 a $triple 目标作为备用，那 该$triple将会被使用
# 若有几个 `cfg` 和 备用的， 那构建会 error
runner = ".."

# 关联到 注册表 的配置字段
[registry]
index = "..."   # 注册表索引的URL（默认为中央存储库）
token = "..."   # 访问令牌（在中央回购网站上找到）
default = "..." # 要使用的默认备用注册表（可以使用--registry覆盖）

[http]
proxy = "host:port" # 用于HTTP请求的HTTP代理（默认为none）
                    # libcurl格式，例如“socks5h://host:port”
timeout = 30        # 每个HTTP请求的超时，以秒为单位
cainfo = "cert.pem" # 证书颁发机构（CA）包的路径（可选）
check-revoke = true # 指示是否检查SSL证书是否已废除
low-speed-limit = 5 # 限速 字节/秒（10 = 默认值，0 = 禁用）
multiplexing = true # 是否在可能的情况下使用 HTTP/2多路复用

# 此设置可用于帮助调试Cargo所发生的HTTP请求
# 当设置为“true”时，将填充Cargo的正常调试日志记录
# 关于HTTP的信息，您可以使用
# `RUST_LOG=cargo::ops::registry=debug`提取（和`trace`可能会打印更多）。
#
# 在将这些日志发布到其他地方时要小心，因可能存在这样的
# header中，有一个你不想泄露的身份验证令牌的情况！务必
# 在发布之前简要查看日志。
debug = false

[build]
jobs = 1                  # 并行作业数，默认为CPU数
rustc = "rustc"           # rust编译器工具
rustdoc = "rustdoc"       # doc生成器工具
target = "triple"         # build为目标 triple（被`cargo install`忽略）
target-dir = "target"     # 放置所有生成的工件的路径
rustflags = ["..", ".."]  # 自定义 传递给所有编译器调用 的参数
incremental = true        # 是否启用增量编译
dep-info-basedir = ".."   # depfiles中，目标的基本目录的完整路径

[term]
verbose = false        # Cargo否提供详细输出
color = 'auto'         # Cargo否着色输出

# 网络配置
[net]
retry = 2 # 失败 自动重试 次数
git-fetch-with-cli = false  # 若为 `true` 我们会使用 `git`命令行去 fetch git repos

# 别名 cargo 命令. 前 3 个aliases 是内置的. 如果你的命令 要求 整行命令，请使用 list 格式.
[alias]
b = "build"
t = "test"
r = "run"
rr = "run --release"
space_example = ["run", "--release", "--", "\"command list\""]
```

### Environment variables

环境变量

除了上面的 TOML 语法之外，还可以通过环境变量配置 Cargo。对于上方的`foo.bar`表格的每个配置字段，也可以用环境变量`CARGO_FOO_BAR`来定义值。比如说`build.jobs`字段，也可以通过`CARGO_BUILD_JOBS`定义。

环境变量将优先于 TOML 配置，并且当前仅支持由环境变量定义的整数，布尔和字符串字段，这意味着[来源更换][source]，能由表格表示，却不能通过环境变量配置。

除上述系统外，Cargo 还认可其他一些特定的[环境变量][env].

[env]: ./environment-variables.md
[source]: ./source-replacement.md
