## Unstable Features

> 不稳定的特性

实验性 Cargo 特性仅适用于夜间通道(Rust 的 nightly 版本)。您通常使用`-Z`带其中一个特性，以启用它们。运行`cargo -Z help`查看可用的标志列表。

`-Z unstable-options`是用于启用其他不稳定命令行标志的通用标志。需要的此选项将在下面列出。

某些不稳定的特性需要您，在`Cargo.toml`指定`cargo-features`字段。

### Alternate Registries

> 替换(/备用)注册表

- RFC:[#2141](https://github.com/rust-lang/rfcs/blob/master/text/2141-alternative-registries.md)
- 跟踪问题:[rust-lang/rust#44931](https://github.com/rust-lang/rust/issues/44931)

备用注册表，允许您使用 crates.io 以外的注册表。

注册表的名称定义在`.cargo/config`中，`registries`表格的下面:

```toml
[registries]
my-registry = { index = "https://my-intranet:8080/git/index" }
```

可以在`.cargo/credentials`添加备用注册表的身份验证信息:

```toml
[registries.my-registry]
token = "api-token"
```

在`Cargo.toml`里面，您可以使用指定`registry`字段，让某个依赖项来自哪个注册表。但首先，您需要在文件的顶部包含适当的`cargo-features`:

```toml
cargo-features = ["alternative-registries"]

[package]
...

[dependencies]
other-create = { version = "1.0", registry = "my-registry"}
```

一个`--registry`标志 已添加到与注册表交互的`publish`，`login`等命令中。示例:

```
cargo +nightly publish -Z unstable-options --registry my-registry
```

在`Cargo.toml`的 `publish` 字段，已被扩展为接受限制为发布到这些注册表的注册表列表。

```toml
[package]
...
publish = ["my-registry"]
```

### publish-lockfile

> 发布-锁文件

- 原始问题:[#2263](https://github.com/rust-lang/cargo/issues/2263)
- PR:[#5093](https://github.com/rust-lang/cargo/pull/5093)
- 跟踪问题:[#5654](https://github.com/rust-lang/cargo/issues/5654)

创建`.crate`文件分发时，Cargo 历史上不包括`Cargo.lock`文件。这可能会导致`cargo install`用于二进制文件的问题。您可以在`cargo package`要么`cargo publish`使用时，指定您的包应包含`Cargo.lock`，方法是在`Cargo.toml`中指定`publish-lockfile`字段。这也需要适当的`cargo-features`要求:

```toml
cargo-features = ["publish-lockfile"]

[package]
...
publish-lockfile = true
```

### Offline Mode

> 离线模式

- 原始问题:[#4686](https://github.com/rust-lang/cargo/issues/4686)
- 跟踪问题:[#5655](https://github.com/rust-lang/cargo/issues/5655)

`-Z offline`标志 可防止 Cargo 因任何原因尝试访问网络。通常情况下，如果 Cargo 要访问网络但它不可用，则会因错误而停止。

请注意，这可能会导致与在线模式不同的依赖项解析。即使可能有索引的较新版本的本地副本，Cargo 也会将自己限制在本地可用的箱(crate)中。

### no-index-update

> 无索引更新

- 原始问题:[#3479](https://github.com/rust-lang/cargo/issues/3479)

`-Z no-index-update`标志 确保 Cargo 不会尝试更新注册表索引。这适用于测试(调制)许多 Cargo 命令的 Crater 等工具，并且您希望每次都避免更新索引的网络延迟。

### avoid-dev-deps

> 阻止 开发依赖项

- 原始问题:[#4988](https://github.com/rust-lang/cargo/issues/4988)
- 稳定问题:[#5133](https://github.com/rust-lang/cargo/issues/5133)

运行命令如`cargo install`要么`cargo build`时，Cargo 当前会需要下载 dev 依赖项，即使它们未被使用。而该`-Z avoid-dev-deps`标志 则让 Cargo 避免在不需要时下载 dev 依赖项。该`Cargo.lock`如果跳过了 dev-dependencies，将不会生成相关信息。

### minimal-versions

> 最小的版本

- 原始问题:[#4100](https://github.com/rust-lang/cargo/issues/4100)
- 跟踪问题:[#5657](https://github.com/rust-lang/cargo/issues/5657)

当一个`Cargo.lock`文件被生成，`-Z minimal-versions`标志的使用， 将解析依赖关系为满足要求的最小 semver 版本(而不是最高版本)。

此标志的预期用例，是在持续集成期间，检查 Cargo.toml 中指定的版本是否是您实际使用的最低版本的正确反映。也就是说，如果 Cargo.toml 有`foo = "1.0.0"`说明，那您依赖该特性，不会意外地添加`foo 1.5.0`。

### out-dir

> 输出目录

- 原始问题:[#4875](https://github.com/rust-lang/cargo/issues/4875)

此功能允许您指定，构建工件后，将复制到的目录。通常，工件只写入`target/release`要么`target/debug`目录。但是，确定明确的文件名可能很棘手，因为您需要解析 JSON 输出。而该`--out-dir`标志 可以更容易地预测访问工件。但是请注意，工件只是被复制，因此原件仍在`target`目录.例:

```
cargo +nightly build --out-dir=out -Z unstable-options
```

### Profile Overrides

> 覆盖配置

- 跟踪问题:[rust-lang/rust#48683](https://github.com/rust-lang/rust/issues/48683)
- RFC:[#2282](https://github.com/rust-lang/rfcs/blob/master/text/2282-profile-dependencies.md)

可以为特定包和自定义生成脚本覆盖配置文件。一般格式如下:

```toml
cargo-features = ["profile-overrides"]

[package]
...

[profile.dev]
opt-level = 0
debug = true

# 这个 `image` 箱 会编译的等级 -Copt-level=3
[profile.dev.overrides.image]
opt-level = 3

# 所有 依赖项 (但 不是 箱本身 或 某些工作区成员)
# 会编译的等级 -Copt-level=2 。 其中包括 构建依赖项.
[profile.dev.overrides."*"]
opt-level = 2

# 构建 脚本和他们的 依赖项， 会编译的等级 -Copt-level=3
# 默认情况， 构建 脚本 对剩下的配置使用相同的选择
[profile.dev.build-override]
opt-level = 3
```

能只为 dev 和 release 配置文件指定覆盖。

### Config Profiles

> 配置 配置文件 (第一个动词，第二个名词)

- 跟踪问题:[rust-lang/rust#48683](https://github.com/rust-lang/rust/issues/48683)
- RFC:[#2282](https://github.com/rust-lang/rfcs/blob/master/text/2282-profile-dependencies.md)

<!-- HERE -->

可以在`.cargo/config`文件中指定配置文件。该`-Z config-profile`命令行标志是使用此功能所必需的。格式与一个 `Cargo.toml`清单格式相同。如果在多个文件中找到相同的配置，则通过使用常规设置[配置层次结构](./config.md#hierarchical-structure)合并。配置设置优先于清单设置。

```toml
[profile.dev]
opt-level = 3
```

```
cargo +nightly build -Z config-profile
```

### Namespaced features

> 特性的命名区间化

- 原始问题:[#1286](https://github.com/rust-lang/cargo/issues/1286)
- 跟踪问题:[rust-lang/Cargo#5565](https://github.com/rust-lang/cargo/issues/5565)

目前，不可能在清单中具有相同名称的特性和依赖项。但如果你设置`namespaced-features`至`true`，功能和依赖项的命名空间就会是分开的。这样做的结果是，在特性请求中，依赖项必须以`crate:`为前缀。像这样:

```toml
[package]
namespaced-features = true

[features]
bar = ["crate:baz", "foo"]
foo = []

[dependencies]
baz = { version = "0.1", optional = true }
```

为了防止不必要的，必须为每个可选依赖项显式声明特性的模版，将为任何不是定义为相同名称的特性，的可选依赖项创建隐式特性。但是，如果定义了与依赖项同名的特性，则该特性必须将依赖项作为必需项，正如`foo = ["crate:foo"]`。

### Build-plan

> 构建计划

- 跟踪问题:[rust-lang/Cargo#5579](https://github.com/rust-lang/cargo/issues/5579)

`build`命令的`--build-plan`参数，将输出 JSON，其中包含有关将运行哪些命令，和不实际执行任何操作的信息。与其他构建工具集成时，这可能很有用。例:

```
cargo +nightly build --build-plan -Z unstable-options
```

### default-run

> 默认运行

- 原始问题:[#2200](https://github.com/rust-lang/cargo/issues/2200)

清单中`[package]`部分的该`default-run`选项，可用于指定`cargo run`选择的默认二进制文件。例如，当存在`src/bin/a.rs`和`src/bin/b.rs`两者时，选择前者:

```toml
[package]
default-run = "a"
```

### Metabuild

> 元构建

- 跟踪问题:[rust-lang/rust#49803](https://github.com/rust-lang/rust/issues/49803)
- RFC:[#2196](https://github.com/rust-lang/rfcs/blob/master/text/2196-metabuild.md)

Metabuild 是一个具有声明性构建脚本的特性。作为不去写一个`build.rs`脚本，而是您在`Cargo.toml`中的`metabuild`字段，指定构建依赖项列表。其将自动生成一个构建脚本，该脚本按顺序运行每个构建依赖项。然后，Metabuild 包可以从`Cargo.toml`中读取元数据，执行他们的指定行为。

需要包括`cargo-features`在`Cargo.toml`的顶部， 一个`metadata`字段在`package`下，列出`build-dependencies`依赖项，并添加 metabuild 包所需的任何元数据。例:

```toml
cargo-features = ["metabuild"]

[package]
name = "mypackage"
version = "0.0.1"
metabuild = ["foo", "bar"]

[build-dependencies]
foo = "1.0"
bar = "1.0"

[package.metadata.foo]
extra-info = "qwerty"
```

Metabuild 包应该有一个名`metabuild`为的公共函数，它会执行与常规`build.rs`脚本一样，执行相同操作。
