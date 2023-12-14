## Cargo Home

“Cargo home”起到下载和源（source）缓存的作用。
当我们构建一个[crate][def-crate]时，Cargo将把下载的依赖项存储在Cargo home中。
您可以通过修改`CARGO_HOME`[环境变量][env]来改变Cargo home的位置。
[home](https://crates.io/crates/home) crate提供了一个API，允许您在您的Rust crate中检索Cargo home的位置。
默认情况下，Cargo home的位置是`$HOME/.cargo/`。

请注意，Cargo home的内部结构并不稳定，可能会随时发生变化。

Cargo home由这些部件共同组成：

## 文件:

* `config.toml`
	Caro的全局配置文件，请参阅[参考文档中的config条目][config]。

* `credentials.toml`
 	加密登录的凭证，来自（from）[`cargo login`]，用于登录[registry][def-registry]。
	Private login credentials from [`cargo login`] in order to log in to a [registry][def-registry].

* `.crates.toml`
	该隐藏文件包含了通过[`cargo install`]安装的crate的[package][def-package]信息。**不要**手动修改它！

## 目录:

* `bin`
bin目录包含了通过[`cargo install`]或者[`rustup`](https://rust-lang.github.io/rustup/)安装的crate的可执行文件。
要使这些可执行文件可用，只需将目录的路径添加到`$PATH`环境变量中。

 *  `git`
	Git源（Git sources）存储在这里：

    * `git/db`
		当一个crate依赖于一个git仓库时，Cargo将克隆一个该git仓库的裸仓库（bare repo）到这个文件夹，并在必要时更新它。
		> 译者注：裸仓库（bare repo）是没有`.git`目录的仓库。也就是说，它只有裸仓库数据，而没有工作目录或者工作树。

    * `git/checkouts`
		若一个git源被使用，Cargo将从`git/db`中的裸仓库中检出（check out）所需的提交（commit）到这个目录。
		该目录为编译器提供了具体的文件，这些文件包含于这个依赖关系所指定的提交的仓库中。
		Cargo支持检出相同仓库中的多个不同提交。
		This provides the compiler with the actual files contained in the repo of the commit specified for that dependency.
		Multiple checkouts of different commits of the same repo are possible.

* `registry`
	这里存放着包（Packages）和crate registries的元数据（例如[crates.io](https://crates.io/)）

  * `registry/index`
		该目录是个裸仓库，它包含了该registry中所有可用的crate的元数据（版本，依赖，等等）

  *  `registry/cache`
		已经下载的依赖将被存放于该文件夹中。这些crate会经过gzip压缩为归档文件（gzip archive），并以`.crate`为扩展名。

  * `registry/src`
		若某个包要求依赖一个已下载的`.crate`归档文件，该归档文件将被解压到这个目录中。rustc将在该目录中找到它所需的`.rs`文件。

## 在CI中缓存Cargo home

为防止在持续集成（CI）中重新下载所有crate依赖，您可以缓存`$CARGO_HOME`目录。
然而，缓存整个目录通常并不高效，因为它可能会将已经下载过的源再缓存一次（it will contain downloaded sources twice）。
若我们依赖了一个crate，例如`serde 1.0.92`，并将整个`$CARGO_HOME`缓存，我们实际上将缓存两次，一次是在`registry/cache`中的`serde-1.0.92.crate`，另一次是serde在`registry/src`中解压的`.rs`文件。
由于下载、解压，再压缩和再上传缓存到CI服务器上这些操作可能花费大量时间，构建过程可能会额外耗费不必要的时间成本。

实际上，在构建过程中只缓存下列目录就足够了：

* `bin/`
* `registry/index/`
* `registry/cache/`
* `git/db/`


## 供应（Vendoring）项目中的所有依赖

请参阅[`cargo vendor`]子命令。

## 清除缓存

理论上，您可以随时清除缓存的任何部分，当某个crate要求这些被清除的依赖内容时，Cargo将尽力恢复它们。这可以通过解压归档文件、检出裸仓库或简单地重新下载来实现。

作为替代，您可以使用[cargo-cache](https://crates.io/crates/cargo-cache) crate提供的命令行工具来选择性清除指定部分的缓存，或显示其组件占用空间的大小。

[`cargo install`]: ../commands/cargo-install.md
[`cargo login`]: ../commands/cargo-login.md
[`cargo vendor`]: ../commands/cargo-vendor.md
[config]: ../reference/config.md
[def-crate]:     ../appendix/glossary.md#crate     '"crate" (glossary entry)'
[def-package]:   ../appendix/glossary.md#package   '"package" (glossary entry)'
[def-registry]:  ../appendix/glossary.md#registry  '"registry" (glossary entry)'
[env]: ../reference/environment-variables.md
