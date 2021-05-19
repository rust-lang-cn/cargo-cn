## Cargo.toml 与 Cargo.lock

`Cargo.toml`和`Cargo.lock`各有其目的。在我们谈论它们之前，这是一个总结:

- `Cargo.toml`是从广义上描述你的依赖，并由你编写.
- `Cargo.lock`包含有关您的依赖项的确切信息。它由 Cargo 维护，不应手动编辑.

如果您正在构建，其他项目要依赖的库，请将`Cargo.lock`放置在你的`.gitignore`。如果您正在构建可执行文件，如命令行工具或应用程序，请检查`Cargo.lock`位于`git`管理下。如果你对这是为什么感到好奇，请参阅["为什么二进制文件在版本控制系统中有`Cargo.lock`，而库没有?" - FAQ ](../faq.md#why-do-binaries-have-cargolock-in-version-control-but-not-libraries).

让我们再挖掘一下.

`Cargo.toml`是一个**manifest(清单)**，我们可以在其中指定一系列关于我们项目的不同元数据的文件。例如，我们可以说我们依赖于另一个项目:

```toml
[package]
name = "hello_world"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]

[dependencies]
rand = { git = "https://github.com/rust-lang-nursery/rand.git" }
```

这个项目有一个依赖关系`rand`箱。在这种情况下，我们已经说过，我们依赖于 GitHub 上的特定 Git 存储库。由于我们尚未指定任何其他信息，因此 Cargo 假定我们打算使用最新提交的`master`分支构建我们的项目。

听起来不错? 嗯，但有一个问题: 如果你今天构建这个项目，然后你发送一份副本给我，我明天构建这个项目，可能会发生一些不好的事情。因在此期间，可能会有更多的`rand`提交，我的构建将包括新的提交，而你的不会。因此，我们会得到不同的构建。这很糟糕，因为我们需要可重复的构建.

我们可以通过放置一个`rev`来解决这个问题，写入我们`Cargo.toml`:

```toml
[dependencies]
rand = { git = "https://github.com/rust-lang-nursery/rand.git", rev = "9f35b8e" }
```

现在我们的构建将是相同的。但是有一个很大的缺点:现在我们每次想要更新库时，都必须手动考虑 SHA-1。这既乏味又容易出错.

那现在`Cargo.lock`登场了。由于它的存在，我们不需要手动跟踪确切的修订版本: Cargo 将为我们做。当我们有这样的清单时:

```toml
[package]
name = "hello_world"
version = "0.1.0"
authors = ["Your Name <you@example.com>"]

[dependencies]
rand = { git = "https://github.com/rust-lang-nursery/rand.git" }
```

Cargo 将采取最新的提交，并在我们第一次构建时，将这些信息写入我们的`Cargo.lock`。该文件将如下所示:

```toml
[root]
name = "hello_world"
version = "0.1.0"
dependencies = [
 "rand 0.1.0 (git+https://github.com/rust-lang-nursery/rand.git#9f35b8e439eeedd60b9414c58f389bdc6a3284f9)"，
]

[[package]]
name = "rand"
version = "0.1.0"
source = "git+https://github.com/rust-lang-nursery/rand.git#9f35b8e439eeedd60b9414c58f389bdc6a3284f9"
```

你可以看到这里有更多的信息，包括我们用来构建的确切修订版本。现在，当您将项目交给其他人时，他们将使用完全相同的 SHA，即使我们没有在我们的项目`Cargo.toml`中指定它.

当我们准备选择，更新库的版本时，Cargo 会自动重新计算依赖关系，并为我们更新内容:

```shell
$ cargo update           # updates all dependencies
$ cargo update -p rand   # updates just “rand”
```

这将写出一个新的`Cargo.lock`与新版本信息。请注意`cargo update`参数，实际上会是是一个[包 ID 规范](../reference/pkgid-spec.md)，和`rand`只是一个简短的规范.
