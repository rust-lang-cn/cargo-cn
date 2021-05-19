## 来源 更换

本文档是关于更换 crate 索引(注册表)。您可以阅读有关重写依赖项的信息，它在本文档的[重写依赖关系][overriding]部分。

Cargo 支持**用另一个来源更换一个来源**的能力，可根据镜像或 vendoring 依赖关系来表达倾向。要配置这些，目前通过[`.cargo/config`配置][config]机制完成，像这样:

[config]: reference/config.md

```toml
# `source` 表下，就是存储有关要更换的来源名称
[source]

# 在`source` 表格之下的，可为一定数量的有关来源名称. 示例下面就# 定义了一个新源， 叫 `my-awesome-source`， 其内容来自本地 # `vendor`目录 ，其相对于包含`.cargo/config`文件的目录
[source.my-awesome-source]
directory = "vendor"

# Git sources 也指定一个 branch/tag/rev
git = "https://example.com/path/to/repo"
# branch = "master"
# tag = "v1.0.1"
# rev = "313f44e8"

# The crates.io 默认源 在"crates-io"名称下， 且在这里我们使用 `replace-with` 字段指明 默认源更换成"my-awesome-source"源
[source.crates-io]
replace-with = "my-awesome-source"
```

使用此配置，Cargo 会尝试在"vendor"目录中，查找所有包，而不是 查询在线注册表 crates.io 。Cargo 有两种来源更换的表达 :

- 供应(Vendoring) - 可以定义自定义源，它们表示本地文件系统上的包。这些源是它们正在更换的源的子集，并在需要时可以检入包中。

- 镜像(Mirroring) - 可以更换为等效版本的源，行为表现为 crates.io 本身的缓存。

Cargo 有一个关于来源更换的核心假设，源代码从两个完全相同的源而来。在上面的例子中，Cargo 假设所有的箱子都来自`my-awesome-source`，与`crates-io`副本完全相同。请注意，这也意味着`my-awesome-source`，不允许有`crates-io`源不存在的箱。

因此，来源更换不适用于依赖项补丁(fix bug)，或私有注册表等情况。Cargo 是通过使用[`[replace]`字段][replace-section]支持依赖项补丁，计划为未来版本的 Cargo 提供私人注册表的支持。

[replace-section]: reference/manifest.md#the-replace-section
[overriding]: reference/specifying-dependencies.md#overriding-dependencies

### 配置

更换源的配置通过完成[`.cargo/config`][config]，下面为全套可用字段是:

```toml
# 每个源都有自己的表格，名称即是表名
[source.the-source-name]

# 命令 ，`the-source-name` 会被`another-source`取代
replace-with = "another-source"

# 有几种可用的源定义(接下来有所描述)
registry = "https://example.com/path/to/index"
local-registry = "path/to/registry"
directory = "path/to/vendor"
```

`crates-io`代表 crates.io 在线注册表(箱的默认来源)，可以更换为:

```toml
[source.crates-io]
replace-with = 'another-source'
```

### 注册表源

"注册表源"与 crates.io 本身相同。也就是说，它也有一个在 git 存储库中提供的索引，该存储库匹配[crates.io index](https://github.com/rust-lang/crates.io-index)的格式。然后该存储库具有指示从哪里下载包的配置。

目前还没有一个已经设置 crates.io 的镜像的可用项目。请继续关注!

> 中国用户，可搜索 'rust 换 中科大 源'

### 本地 注册表源

"本地注册表源"旨在成为另一个注册表源的子集，但可在本地文件系统(也称为 vendoring)上使用。本地注册表是提前下载，通常与一个 `Cargo.lock`同步，并由一组`*.crate`文件和像普通注册表一样的索引组成。

管理和创建本地注册表源的主要方法是通过[`cargo-local-registry`][cargo-local-registry]子命令，可在 crates.io 上找到，并用`cargo install cargo-local-registry`安装。

[cargo-local-registry]: https://crates.io/crates/cargo-local-registry

本地注册表包含在一个目录，其中包含许多从 crates.io 下载的`*.crate`文件，以及`index`目录，它与 crates.io-index 项目目录具有相同格式(仅填充有存在的 crates).

### 目录 源

"目录源"类似于本地注册表源，其中包含本地文件系统上许多的可用包，适用于 vendoring 依赖项。与本地注册表一样，目录源主要由外部子命令管理[`cargo-vendor`][cargo-vendor]，可用`cargo install cargo-vendor`安装。

[cargo-vendor]: https://crates.io/crates/cargo-vendor

目录源与本地注册表不同，但它们包含`*.crate`文件的解压缩版本，使其在某些情况下，更适合检查所有内容到源代码控制工具。目录源只是一个包含许多其他目录的目录，其中包含 crates 的源代码(解压缩版本的`*.crate`文件)。目前，对每个目录的名称没有限制。

目录源中的每个包也有一个关联的元数据文件，指示包中每个文件的校验和，以防止意外修改。
