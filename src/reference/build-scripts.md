## Build Scripts

> 构建脚本

一些包需要编译第三方非 Rust 代码，例如 C 库。其他的包需要链接到 C 库，当然这些库既可以位于系统上，也可以从源代码构建。其他人或许还需要功能工具，比如构建之前的代码生成(想想解析生成器)。

Cargo 并不打算替换为这些能良好优化任务的其他工具，但是它与`build`配置选项.

```toml
[package]
# ...
build = "build.rs"
```

指定的`build`命令应执行的 Rust 文件(相对于包根)，将在包编译其他内容之前，被编译和调用，从而具备 Rust 代码所依赖的构建或生成的工件。默认情况下 Cargo 在包根文件中寻找`"build.rs"`(即使您没有给`build`字段指定值)使用`build = "custom_build_name.rs"`指定自定义生成名，或`build = false`禁用对构建脚本的自动检测。

Build 命令的一些用例是:

- 构建一个捆绑的 C 库.
- 在主机系统上找到 C 库.
- 从规范中生成 Rust 模块.
- 为箱，执行所需的某平台特定配置.

下面将详细介绍每一个用例，以给出构建命令如何工作的示例.

### Inputs to the Build Script

> 输入到构建脚本

当运行构建脚本时，存在许多构建脚本用到的输入，所有输入都以[环境变量][env]传入。

除了环境变量之外，构建脚本的当前目录是构建脚本包的源目录.

[env]: ./environment-variables.md

### Outputs of the Build Script

> 构建脚本的输出

由构建脚本打印到 stdout 的所有行都被写入像`target/debug/build/<pkg>/output`这样的文件(精确的位置可能取决于你的配置)。如果您希望直接在终端中看到这样的输出，那么使用非常详细`-vv`标志。注意，如果既不修改构建脚本也不修改包源文件，下一次的`-vv`调用将**不**打印重复输出到终端，因为没有执行新的构建。可执行`cargo clean`，如果希望确保输出始终显示在终端上，但要在每次 Cargo 调用之前执行。任何一行以`cargo:`开始的，直接由 Cargo 解释。行必须是`cargo:key=value`形式，就像下面的例子:

```
# specially recognized by Cargo
cargo:rustc-link-lib=static=foo
cargo:rustc-link-search=native=/path/to/foo
cargo:rustc-cfg=foo
cargo:rustc-env=FOO=bar
# arbitrary user-defined metadata
cargo:root=/path/to/foo
cargo:libdir=/path/to/foo/lib
cargo:include=/path/to/foo/include
```

另一方面，打印到 stderr 的行被写入像`target/debug/build/<pkg>/stderr`这样的文件，但不被 Cargo 解释。

Cargo 识别一些特殊的 key，其中一些影响箱的构造:

- `rustc-link-lib=[KIND=]NAME`说明了，指定值是库名，且会作为`-l`标志传递给编译器。`KIND`可选为`static`，`dylib`(默认值)，或`framework`的其中之一，用`rustc --help`见更多细节。
- `rustc-link-search=[KIND=]PATH`说明了，指定值是库搜索路径，且会作为`-L`标志传递给编译器。`KIND`可选为`dependency`，`crate`，`native`，`framework`或`all`(默认值)的其中之一，使用`rustc --help`见更多细节.
- `rustc-flags=FLAGS`是传递给编译器的一组标志，仅支持`-l`和`-L`标志。
- `rustc-cfg=FEATURE`说明了，指定的特性，且会作为`--cfg`标志传递给编译器。这通常对检测，执行各种特征的编译时间，是有用的。
- `rustc-env=VAR=VALUE`说明了，指定的环境变量，且会被添加到编译器所在的环境中。然后，可以通过编译箱中的`env!`宏检索该值。这对于在箱的代码中嵌入额外的元数据很有用，比如 Git HEAD 的散列，或持续集成服务器的唯一标识符。
- `rerun-if-changed=PATH`是文件或目录的路径，说明了如果构建脚本发生更改(由文件上最近修改的时间戳检测到)，则应重新运行构建脚本。通常，如果箱根目录中的任何文件发生更改，则重新运行构建脚本，但这可用于将更改范围扩展到仅一小组文件。(如果这个路径指向一个目录，则不会遍历整个目录以进行更改——只对目录本身的时间戳进行更改(该时间戳对应于目录中的某些类型的更改，取决于平台)，将触发重新构建。要请求重新运行整个目录中的任何更改，请递归地为该目录打印一行，为该目录内的所有内容打印另一行。)请注意，如果构建脚本本身(或其依赖项之一)更改，则无条件地重新构建和重新运行该脚本，因此，`cargo:rerun-if-changed=build.rs`几乎总是冗余(除非您想要忽略除了`build.rs`，所有其他文件的变化)
- `rerun-if-env-changed=VAR`是环境变量的名称，说明了它指示如果环境变量的值发生变化，则应重新运行构建脚本。这基本上与`rerun-if-changed`是一样的，除了它与环境变量一起工作。注意，这里的环境变量用于全局环境变量，如`CC`这样的，对于 Cargo 所设的像`TARGET`，就不必使用它。还要注意，如果`rerun-if-env-changed`打印出来，然后 Cargo 将*只*在，那些环境变量发生变化，或者打印出`rerun-if-changed`改变的文件的情况下，才重新运行构建脚本。

- `warning=MESSAGE`是构建脚本运行完毕后，打印到主控制台的消息/警告只针对路径依赖项(即，您在本地工作的那些依赖项)显示，因此如， crates.io 的箱在默认情况下不会打印警告。

其他哪些元素都是用户定义的元数据，这些元数据传递给了依赖的。关于这个的更多信息可以在[`links`][links]部分查看.

[links]: #the-links-manifest-key

### Build Dependencies

> 构建依赖

构建脚本也可以依赖其他基于 Cargo 的箱。依赖关系通过清单的`build-dependencies`部分指定。

```toml
[build-dependencies]
foo = { git = "https://github.com/your-packages/foo" }
```

构建脚本**不**可以访问`dependencies`或`dev-dependencies`部分列表中的依赖项(它们还没有建成!)，除非明确声明，否则包本身也不能使用所有构建依赖项。

### The `links` Manifest Key

> `links` 清单 键

除了清单键`build`，Cargo 也支持一个，要链接到本地库的名称声明，那就是`links`清单键:

```toml
[package]
# ...
links = "foo"
build = "build.rs"
```

此清单说明了包会链接到本机库`libfoo`，并且它还具有定位和/或构建该本机库的构建脚本。Cargo 要求`build`如果有值，那`links`也要有值。

这个清单键的目的是，让 Cargo 了解包所具有的本地依赖项集合，并提供在包构建脚本之间，传递元数据的合适的系统.

首先，Cargo 要求一个包最多只有一个`links`值。换句话说，禁止两个包链接到同一个本机库。然而，这里也有[约定位置][star-sys]的方式，用来缓解这个问题。

[star-sys]: #a-sys-packages

如上面在输出格式中提到的，每个构建脚本可以以键-值对的形式生成一组任意的元数据。此元数据传递给**依赖的**包。例如，如果`libbar`依赖`libfoo`，当`libfoo`生成`key=value`作为其元数据的一部分，那`libbar`的构建脚本会有`DEP_FOO_KEY=value`环境变量。

注意，元数据只传递给直接依赖项，而不是把依赖项串起来。此元数据传递的动机，会在接下来，关联到系统库案例研究中概述。

### Overriding Build Scripts

> 覆盖 构建脚本

如果一个清单包含`links`关键字，那 Cargo 支持重写用自定义库指定的构建脚本。此功能的目的是防止完全运行有问题的构建脚本，而是提前提供下元数据。

要覆盖构建脚本，请将下列配置放在任何可接受的 Cargo 的[配置位置](./config.md)中。

```toml
[target.x86_64-unknown-linux-gnu.foo]
rustc-link-search = ["/path/to/foo"]
rustc-link-lib = ["foo"]
root = "/path/to/foo"
key = "value"
```

本节说明目标`x86_64-unknown-linux-gnu`，命名为`foo`的库，具有指定的元数据。此元数据与构建脚本时生成的元数据相同，提供了许多键/值对，其中`rustc-flags`，`rustc-link-search`和`rustc-link-lib`有点特殊.

使用此配置，如果一个包声明它链接到此`foo`，那构建脚本将**不**编译或运行，而会使用指定的元数据。

### Case study: Code generation

> 案例学习: 代码生成

由于各种原因，一些 Cargo 包在编译之前需要生成代码。这里我们将介绍一个简单的示例，该示例把，'生成库调用'作为构建脚本的一部分.

首先，让我们看一下这个包的目录结构:

```
.
├── Cargo.toml
├── build.rs
└── src
    └── main.rs

1 directory, 3 files
```

在这里我们可以看到我们有一个`build.rs`构建脚本，和二进制文件`main.rs`。 接下来，让我们看一下清单:

```toml
# Cargo.toml

[package]
name = "hello-from-generated-code"
version = "0.1.0"
authors = ["you@example.com"]
build = "build.rs"
```

在这里，我们可以看到，我们已经指定了一个构建脚本`build.rs`，我们将使用它来生成一些代码。让我们看看构建脚本里面有什么:

```rust,no_run
// build.rs

use std::env;
use std::fs::File;
use std::io::Write;
use std::path::Path;

fn main() {
    let out_dir = env::var("OUT_DIR").unwrap();
    let dest_path = Path::new(&out_dir).join("hello.rs");
    let mut f = File::create(&dest_path).unwrap();

    f.write_all(b"
        pub fn message() -> &'static str {
            \"Hello, World!\"
        }
    ").unwrap();
}
```

这里有两点值得注意的地方:

- 脚本使用`OUT_DIR`环境变量，以知道输出文件到哪里。它可以使用进程的当前工作目录，来查找输入文件应该到哪里，但是在这种情况下，我们是没有任何输入文件的。
- 一般来说，构建脚本不应该修改`OUT_DIR`目录外的任何文件。 乍看之下，似乎不错，但当您使用这种箱子作为依赖项时，它确会带来问题，因为`.cargo/registry`源中的*隐性的*常量应该是不变的。`cargo`在打包时不会允许这样的脚本。
- 这个脚本相对简单，只是写出一个小生成的文件。可以想象，其他更奇特的操作也可能发生，例如从 C 头文件或其他定义的语言生成 Rust 模块。

接下来，我们来看看库本身:

```rust,ignore
// src/main.rs

include!(concat!(env!("OUT_DIR"), "/hello.rs"));

fn main() {
    println!("{}", message());
}
```

这就是真正的魔法发生的地方。该库正在使用 rustc 定义的 `include!`宏，它又结合`concat!`与`env!`宏去包含生成文件(`hello.rs`)，从而进入箱的编译。

使用此处所示的结构，箱可以包括(include)构建脚本在内的，任何数量的生成文件。

### Case study: Building some native code

> 案例学习: 构建一些原生代码

有时需要建立一些本地 C 或 C++代码作为包的一部分。这是在用构建脚本到 Rust 箱本身之前，构建本机库的另一个极好用例。作为一个例子，我们将创建一个 Rust 库，它调用 C 来打印"Hello,World!".

和上面一样，让我们先来看看包的布局:

```
.
├── Cargo.toml
├── build.rs
└── src
    ├── hello.c
    └── main.rs

1 directory, 4 files
```

很像之前的吧! 下一步，清单如下:

```toml
# Cargo.toml

[package]
name = "hello-world-from-c"
version = "0.1.0"
authors = ["you@example.com"]
build = "build.rs"
```

现在，我们不打算使用任何-构建的依赖项，所以现在让我们看一下构建脚本:

```rust,no_run
// build.rs

use std::process::Command;
use std::env;
use std::path::Path;

fn main() {
    let out_dir = env::var("OUT_DIR").unwrap();

    // 请注意，这种方法存在许多缺点，
    // 下个代码展示，会详细介绍如何提高这些命令的可移植性。
    Command::new("gcc").args(&["src/hello.c", "-c", "-fPIC", "-o"])
                       .arg(&format!("{}/hello.o", out_dir))
                       .status().unwrap();
    Command::new("ar").args(&["crus", "libhello.a", "hello.o"])
                      .current_dir(&Path::new(&out_dir))
                      .status().unwrap();

    println!("cargo:rustc-link-search=native={}", out_dir);
    println!("cargo:rustc-link-lib=static=hello");
}
```

此构建脚本首先将 C 文件编译为对象文件(通过调用`gcc`)，然后将这个对象文件转换为静态库(通过调用`ar`)，最后一步是反馈给 Cargo ，以表示我们的输出在`out_dir`和通过`-l static=hello`标志，编译器应该将箱静态链接到`libhello.a`。

请注意，这种硬编码方法有许多缺点:

- 这个`gcc`命令本身不是跨平台可移植的。如，在 Windows 平台不太可能`gcc`，甚至不是所有 UNIX 平台都可能有`gcc`。 这个`ar`命令也处于类似的情况。
- 这些命令不考虑跨编译。如果我们为 Android 这样的平台进行跨编译，`gcc`就不太可能产生一个可执行的 ARM.

但不要害怕，这里`build-dependencies`就帮到你! Cargo 生态系统有许多包，为了使此类任务更加容易、可移植和标准化。构建脚本可以写成:

```rust,ignore
// build.rs

// 依赖于外部维护的`cc`包，管理
// 调用C编译器。
extern crate cc;

fn main() {
    cc::Build::new()
        .file("src/hello.c")
        .compile("hello");
}
```

添加`cc`箱，这样将构建，依赖`cc`就好啦，将下面的添加到您的`Cargo.toml`:

```toml
[build-dependencies]
cc = "1.0"
```

这个[`cc`箱](https://crates.io/crates/cc)抽象了 C 代码构建，主要用于脚本需求范围:

- 它调用适当的编译器(Windows 的 MSVC，`gcc`对 MinGW ，`cc`对 UNIX 平台等等).
- 通过向正在使用的编译器传递适当的标志，获取`TARGET`变量.
- 其他环境变量，如`OPT_LEVEL`，`DEBUG`等等，都是自动处理的.
- stdout 输出和`OUT_DIR`位置也由`cc`库控制.

在这里，我们可以开始看到，将尽可能多的功能移植到公共构建依赖项，而不是在所有构建脚本之间复制来复制去，的一些主要好处!

回到案例研究，让我们快速浏览一下`src`目录中的内容:

```c
// src/hello.c

#include <stdio.h>

void hello() {
    printf("Hello, World!\n");
}
```

```rust,ignore
// src/main.rs

// 注意缺少`#[link]`属性。 我们选择，将责任委派给
// 构建脚本的链接，而不是硬编码
// 它在源文件中.
extern { fn hello(); }

fn main() {
    unsafe { hello(); }
}
```

然后，就好啦! 这就完成了使用构建脚本，从 Cargo 包构建一些 C 代码的示例。这也说明了为什么在许多情况下使，用构建依赖项非常重要，甚至更加简洁!

我们还看到了构建脚本使用箱，纯粹作为用于构建过程的依赖项，而不是在运行时，用作箱本身的依赖项的简要示例。

### Case study: Linking to system libraries

> 案例学习: 链接到系统库

这里的最后一个案例研究，将研究 Cargo 库如何链接到系统库，以及构建脚本如何支持这个用例。

通常，Rust 箱希望链接到系统上经常提供的本地库，以绑定其功能，或者只是将其用作实现细节的一部分。想以不管平台的方式执行这个操作，而这却是一个相当微妙的问题，再次说明下，构建脚本的目的是尽可能多地分配这些(微妙)内容，以便让消费者尽可能容易地使用它.

作为一个例子，让我们来看一个[Cargo 本身的依赖][git2-rs]，[libgit2][libgit2]。这个 C 库其实有许多约束条件:

[git2-rs]: https://github.com/alexcrichton/git2-rs/tree/master/libgit2-sys
[libgit2]: https://github.com/libgit2/libgit2

- 它可选为依赖 Unix 上的 OpenSSL ，来实现 https 传输.
- 它可选为依赖所有平台上的 libssh2 ，来实现 ssh 传输.
- 默认情况下，它通常不安装在所有系统上.
- 它可以从源代码使用`cmake`构建.

为了可视化这里发生的事情，让我们看一下，链接本机 C 库的相关 Cargo 包的清单。

```toml
[package]
name = "libgit2-sys"
version = "0.1.0"
authors = ["..."]
links = "git2"
build = "build.rs"

[dependencies]
libssh2-sys = { git = "https://github.com/alexcrichton/ssh2-rs" }

[target.'cfg(unix)'.dependencies]
openssl-sys = { git = "https://github.com/alexcrichton/openssl-sys" }

# ...
```

正如上面的清单所显示的，我们指定了一个`build`脚本，但值得注意的是，该示例具有`links`项，说明该箱(`libgit2-sys`)链接到了这个本地库`git2`。

在这里，我们还看到，我们选择让 Rust 箱有一个无条件的，通过`libssh2-sys`箱依赖`libssh2`(ssh2-rs)，以及(有条件的)特定于平 unix 台的`openssl-sys`依赖(其他平台现在被漠视)。这似乎有点违反在 _Cargo 清单_ 的 _C 依赖_ 的明确性，但这实际上是这'地方'中使用 Cargo 的一种约定.

### `*-sys` Packages

> `*-sys` 包们

为了减轻对系统库的链接，crates.io 有一个包命名和功能的*惯例*。比如包名`foo-sys`，它应该提供两个主要功能:

- 库箱应链接到本地库`libfoo`。 在源代码最后构建之前，这将经常探测当前的系统的`libfoo`。
- 库箱应提供在`libfoo`的**声明**函数，但是**不**绑定或高级抽象。

一套`*-sys`包，提供了一组用于连接到本地库的公共依赖项。通过这种'本机库相关'的包约定，可以获得许多好处:

- `foo-sys`的公共依赖，会减轻上面所说的，关于一个包的`links`的每个值规则。
- 一个公共依赖关系，更能发现`libfoo`本身的集中逻辑(或者从源代码构建它).
- 这些依赖关系很容易被重写.

### Building libgit2

> 构建 libgit2 吧

现在我们已经整理了 libgit2 的依赖，我们需要实际编写下构建脚本。我们这里不讨论特定的代码片段，而只研究`libgit2-sys`构建脚本的高层细节。这并不是建议所有包都遵循这个策略，而仅概述一个特定的策略。

构建脚本应该做的第一步是查询 libgit2 是否已经安装在主机系统上。要做到这一点，我们将利用现有的工具`pkg-config`(当它可用时)。我们也会使用`build-dependencies`部分重构成`pkg-config`相关的所有代码(或者有人已经这样做了!)。

如果`pkg-config`找不到 libgit2，或者如果`pkg-config`只是没有安装，下一步就要从捆绑源代码构建 libgit2 (捆绑源码作为`libgit2-sys`本身的一部分)。然而，在这样做时有一些细微差别，我们需要加以考虑:

- libgit2 的构建系统，`cmake`需要能够找到 libgit2 可选依赖 libssh2 。而我们确信我们已经构建了它(因它是一个 Cargo 依赖项)，我们只需要传递这个信息。为此，我们利用元数据格式，在构建脚本之间传递信息。在这个例子中，打印出的 libssh2 包信息是`cargo:root=...`，它来告诉我们 libssh2 安装在哪里，然后我们可以通过`CMAKE_PREFIX_PATH`环境变量让 cmkae 知道。

- 我们需要处理下，编译 C 代码时的一些`CFLAGS`值(也要告诉`cmake`关于这个信息)。我们想传递的一些标志是 64 位的`-m64`，32 位的`-m32`，或`-fPIC`也适用于 64 位。

- 最后，我们调用`cmake`将所有输出放入环境变量`OUT_DIR`目录，然后打印必要的元数据，以指导 rustc 如何链接到 libgit2。

这个构建脚本的大部分功能，很容易就重构为常见的依赖项，因此我们的构建脚本不像这个描述那样长烦! 实际上，通过构建依赖项，构建脚本应该非常简单。
