# 在现有的 Cargo 项目上工作

<!-- 如果您下载使用 Cargo 的现有项目，那么它很容易上手.

首先，从某个地方获取项目.在这个例子中，我们将使用`rand`项目，其从 GitHub 上的存储库克隆而来:

```shell
$ git clone https://github.com/rust-lang-nursery/rand.git
$ cd rand
```

要建立，使用`cargo build`:

```shell
$ cargo build
   Compiling rand v0.1.0 (file:///path/to/project/rand)
```

这将获取所有依赖项，然后与项目一起构建它们. -->

如果你下载的现有[软件包 (package)][def-package] 用的是 Cargo，那它会很容易上手。

首先，获取你的项目。在这里使用 `regex` 项目举例，我们将从它的 GitHub 仓库将其克隆下来:

```console
$ git clone https://github.com/rust-lang/regex.git
$ cd regex
```

要构建二进制程序，只需要使用 `cargo build`:

```console
$ cargo build
   Compiling regex v1.5.0 (file:///path/to/package/regex)
```

这会拉取当前项目的所有依赖，并同项目一起构建二进制文件。

[def-package]: ../appendix/glossary.md#package
