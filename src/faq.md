## 常见问题

### Is the plan to use GitHub as a package repository?

> 是否有计划，使用 Github 作为一个包库 ？

不，Cargo 的计划是使用[crates.io]，像 NPM 或 RuuGuMes 对应 npmjs.org 和 rubygems.org。

我们计划永远（通过些配置）支持 git 存储库作为包的来源，因为它们可以用于早期开发和临时补丁（加了点灵活性），即便人们使用主要使用注册表作为包的来源。

### Why build crates.io rather than use GitHub as a registry?

> 为啥，选 crates.io，而不是使用 Github 作为 注册表 ？

我们认为支持多种下载包的方式非常重要，包括从 GitHub 下载包，并将包复制到包本身.

也就是说，我们认为[crates.io]提供了许多重要的好处，并且预计其会成为人们在 Cargo 中，下载包的主要方式。

前车之鉴，Node.js 的[npm][1]和 Ruby 的[bundler][2]都支持中央注册中心模式，和基于 Git 的模式，而大多数包都是通过生态系统中的注册中心下载的，其中重要的少数包是使用基于 git 的包。

[1]: https://www.npmjs.org
[2]: https://bundler.io

使中央注册中心，在其他语言中流行的一些优点包括:

- **可发现性**. 中央注册表提供了查找现有包的简单方式。结合标记(版本)，这也使得注册中心能够提供生态系统的范围信息，例如最流行或最依赖的包的列表.
- **速度**. 中心注册中心使得可以快速有效地只获取包的元数据，然后只高效地下载已发布的包，而不会出现在存储库中的其他膨胀。这大大提高了依赖性解析和获取的速度。要知道随着依赖关系图的扩展，下载所有的 git 存储库会陷入困境。还要记住的是，并不是每个人都有高速、低延迟的互联网连接.

### Will Cargo work with C code (or other languages)?

> Cargo 可与 C 语言代码(或其他语言)一起工作吗?

可以的!

Cargo 处理编译 Rust 代码，但我们知道许多 Rust 包与 C 代码都有链接。我们还知道除 Rust 之外，在编译语言方面的工具，已建立了数十年。

我们的解决方案:Cargo 允许一个包可以[指定脚本](./build-scripts.md)(用 Rust 编写)，其在调用`rustc`之前运行。 利用 Rust 实现特定于平台的配置和重构包之间的常见构建功能。

### Can Cargo be used inside of `make` (or `ninja`, or ...)

> Cargo 能被用在 `make`(或 `ninja`或...) 中吗 ?

当然能。尽管我们希望， Cargo 是作为顶级编译 Rust 包的独立方式，但我们知道有些人希望从其他构建工具调用 Cargo。

我们已将 Cargo 设计成在这些环境中工作良好，并注意错误代码和机器可读输出模式等事项。在这些方面我们还有一些工作要做，但是在传统脚本上下文中使用 Cargo 是我们从一开始就设计的，并且将继续优先考虑。

### Does Cargo handle multi-platform packages or cross-compilation?

> Cargo 是怎么平衡 多平台或跨平台的包的？

Rust 本身提供了基于平台，配置代码段的工具。Cargo 也支持[特定平台依赖关系][target-deps]，未来，我们计划为每个平台`Cargo.toml`支持更多的配置.

[target-deps]: reference/specifying-dependencies.md#platform-specific-dependencies

从长远来看，我们正在寻找使用 Cargo 方便地跨平台编译包的方法.

### Does Cargo support environments, like `production` or `test`?

> Cargo 有没支持像`production` 或 `test`这样的环境？

我们通过使用[profiles][profile]来支持这样的环境:

[profile]: ./manifest.md#the-profile-sections

- 特定环境标志(像 开发环境的 `-g --opt-level=0`和生产环境的`--opt-level=3`)。
- 特定环境依赖性(像 测试断言 的`hamcrest`).
- 特定环境变量 `#[cfg]`
- 一个`cargo test`命令

### Does Cargo work on Windows?

> Windows 系统 呢，Cargo 能搞吗？

没问题!

所有提交的 Cargo 都需要通过 Windows 上的本地测试套件。但是，如果你发现一个 Windows 问题，我们认为它就是一个 bug，所以[请提出一个问题][3].

[3]: https://github.com/rust-lang/cargo/issues

### Why do binaries have `Cargo.lock` in version control, but not libraries?

> 为啥，输出二进制的 Cargo 项目具有`Cargo.lock`，而单输出库的，就没有？

一个`Cargo.lock`文件的目的，是在于成功构建，能描述'世界'的状态。然后，它就能用来，通过确保编译完全相同的依赖项，就能跨任何机器上构建确定性的包。

这个属性对于，处在依赖链末端的应用程序和包(二进制文件)是最理想的。因此，建议所有二进制文件都在其`Cargo.lock`内部进行检查.

对于单库来说，情况有些不同。库不仅被库开发人员使用，而且被库的任何下游消费者使用。依赖库的用户不会检查库的`Cargo.lock`(即使它存在)。正是如此，库**不**应该对库的所有用户进行确定性地重新编译。

如果一个库最终被多个依赖项传递使用，那么很可能只需要该库的一个副本(基于 semver 兼容性的版本)。如果 Cargo 使用了所有的 **依赖项的`Cargo.lock`文件**，那结果就是，使用库的多个副本，甚至可能存在版本冲突。

换句话说，库为它们的依赖项指定了 semver 版本，但是不用(无法)看到全部内容。只有像二进制文件这样的最终产品才需要有完整的图，来决定应该使用什么版本的依赖。

### Can libraries use `*` as a version for their dependencies?

> 作为库的项目，可以使用`*`作为它们的依赖的版本号吗?

**截至 2016 年 1 月 22 日，[crates.io]拒绝通配符`*`依赖约束的所有包(不只是库).**

库是*可以*，但严格来说，他们不应该这样做。`*`版本要求，说明了"这将适用于任何版本"，而这永远不会是真的。库应该总是指定它们工作的范围，即使它和"每个 1.x.y 版本"一样。

### Why `Cargo.toml`?

作为与 Cargo 最频繁的交互之一，为什么要命名配置文件叫`Cargo.toml`的问题不时出现。选择领先的大写—`C`，是为了确保清单与目录清单中的其他类似配置文件组合排序。对文件进行排序时，通常将大写字母放在小写字母之前，确保`Makefile`和`Cargo.toml`文件会放在一起。选择`.toml`结尾是强调文件是[特定的配置文件格式](https://github.com/toml-lang/toml).

Cargo 不允许其他名称(如`cargo.toml`或`Cargofile`)，来强调如何如何容易识别 Cargo 仓库。在历史上，许多可能的名称选择都导致了混乱，其中一个选项被选择了，而其他选项就被自然而然地遗忘。

[crates.io]: https://crates.io/

### How can Cargo work offline?

> Cargo 能 离线 工作吗？

Cargo 通常用于网络访问有限，或没有网络访问的情况，如飞机、CI 环境或嵌入大型生产部署中。当 Cargo 试图从网络获取资源时，用户常常感到惊讶，因频繁出现 Cargo 离线工作的请求。

Cargo 的核心是不会试图访问网络，除非被告知这样做。也就是说，如果没有来自 crates.io、git 存储库或其他网络位置的箱，则 Cargo 永远不会尝试进行网络连接。因此，如果 Cargo 试图接触网络，那是因为它需要获取所需的资源。

Cargo 还非常积极地缓存信息，保持最小化的网络活动量。例如，它将保证`cargo build`(或类似的)运行到完成，那下一次`cargo build`保证不接触网络，只要`Cargo.toml`在此期间还没有被修改。网络的这种回避归结为，已存在`Cargo.lock`，和在 lock 文件中反映了，箱子的充分缓存。如果这些组件中的任何一个丢失，那么构建的成功就需要它们，并且必须远程获取它们。

对 Rust 1.11.0 后的 Cargo ，可以看到新的(标志)参数`--frozen`，这是它不应该接触网络的断言。当传递给 Cargo，如果 Cargo 试图进行网络请求，它将立即返回一个错误。错误应该包括关于为什么进行网络请求(第一个地方），以帮助调试的上下文信息。注意这个标志是*不改变 Cargo 的行为*，它只是断言 Cargo 不应该触摸网络，这作为上一个命令已完成的保证，可以相同的网络活动是不必的。

> 上一个命令，如`cargo build`

有关版本管理的详细信息，请参阅文档[来源更换][replace].

[replace]: ./source-replacement.md
