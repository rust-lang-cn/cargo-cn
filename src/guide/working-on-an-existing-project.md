## 在现有的 Carog 项目上工作

如果您下载使用 Cargo 的现有项目，那么它很容易上手.

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

这将获取所有依赖项，然后与项目一起构建它们.
