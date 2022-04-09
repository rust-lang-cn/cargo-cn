## Environment Variables

Cargo 设置，并读取许多环境变量，代码可以检测或覆盖这些环境变量。以下是 Cargo 与它们交互时，组织的变量列表:

### Environment variables Cargo reads

Cargo 会读取的环境变量

您可以重写这些环境变量来更改 Cargo 在系统中的行为:

| 名(ENV)                  | 曰                                                                                                                                                        |
| ------------------------ | --------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `CARGO_HOME`             | Cargo 在本地缓存注册表索引和箱子的 git 版本。默认情况下，这些存储在`$HOME/.cargo`，但是这个变量重写了这个目录的位置。一旦箱被缓存，它就不会被清除命令删除。 |
| `CARGO_TARGET_DIR`       | 相对于当前工作目录，放置所有生成的工件的位置.                                                                                                              |
| `RUSTC`                  | Cargo 不运行`rustc`，而执行指定的编译器。                                                                                                                 |
| `RUSTC_WRAPPER`          | Cargo 将执行这个指定的包装器，而不是简单地运行`rustc`。将 rustc 调用 作为其命令行参数传递，第一个参数是 rustc.                                            |
| `RUSTDOC`                | Cargo 将执行此指定的`rustdoc`实例，而不是`rustdoc`.                                                                                                       |
| `RUSTDOCFLAGS`           | 空格分隔的自定义标志列表，用来传递给 Cargo 执行的所有`rustdoc`调用 。与`cargo rustdoc`不同，这对于传递一个参数给 _全部的_ `rustdoc`实例是有用的。         |
| `RUSTFLAGS`              | 自定义参数的空格分隔列表，用来传递给 Cargo 执行的所有编译器调用。与`cargo rustc`不同，这对于传递一个标志 _全部的_ 编译实例是有用的。                      |
| `CARGO_INCREMENTAL`      | 如果设置为 1，则 Cargo 将强制在当前编译中启用增量编译，而当设置为 0，则强制禁用增量编译。如果这个 ENV 不存在，否则将使用 Cargo 默认值。                       |
| `CARGO_CACHE_RUSTC_INFO` | 如果这个设置为 0，那么 Cargo 将不尝试缓存编译器版本信息.                                                                                                   |

注意，Cargo 也会在`.cargo/config`配置中读取环境变量，如[那份文件][config-env]

[config-env]: ./config.md#environment-variables

### Environment variables Cargo sets for crates

Cargo 为 crates 设置的环境变量

Cargo 在编译时，会将这些环境变量暴露在箱子中。请注意，这也适用于测试二进制文件。要在 Rust 程序中获得这些变量中的任何一个变量的值，请执行以下操作:

```rust
let version = env!("CARGO_PKG_VERSION");
```

`version`现在将包含了`CARGO_PKG_VERSION`值。

| 名                        | 曰                                                                                     |
| ------------------------- | -------------------------------------------------------------------------------------- |
| `CARGO`                   | 执行构建的二进制`cargo`路径.                                                           |
| `CARGO_MANIFEST_DIR`      | 包含包的清单的目录.                                                                    |
| `CARGO_PKG_VERSION`       | 您的包的完整版本.                                                                      |
| `CARGO_PKG_VERSION_MAJOR` | 你的软件包的主要版本.                                                                  |
| `CARGO_PKG_VERSION_MINOR` | 您的包的次要版本.                                                                      |
| `CARGO_PKG_VERSION_PATCH` | 包的补丁版本.                                                                          |
| `CARGO_PKG_VERSION_PRE`   | 包的预发布版本.                                                                        |
| `CARGO_PKG_AUTHORS`       | 从程序包的清单中，冒号分隔出作者列表.                                                  |
| `CARGO_PKG_NAME`          | 你的包的名字.                                                                          |
| `CARGO_PKG_DESCRIPTION`   | 从包的清单中描述.                                                                      |
| `CARGO_PKG_HOMEPAGE`      | 从包的清单中的主页.                                                                    |
| `CARGO_PKG_REPOSITORY`    | 从包的清单中存储库.                                                                    |
| `OUT_DIR`                 | 如果包具有构建脚本，则将其设置为，构建脚本应该在其中放置其输出的文件夹。更多信息见下文. |

### Environment variables Cargo sets for build scripts

Cargo 为构建脚本设置的环境变量

当运行构建脚本时，Cargo 设置多个环境变量.因为编译生成脚本时还没有设置这些变量，所以上面的示例使用`env!`无法工作，而是在运行构建脚本时需要检索值:

```rust
use std::env;
let out_dir = env::var("OUT_DIR").unwrap();
```

`out_dir`现在将包含的价值`OUT_DIR`.

| 名                     | 曰                                                                                                                                                                                                                                                                                                                 |
| ---------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `CARGO`                | 执行构建的二进制`cargo`路径.                                                                                                                                                                                                                                                                                       |
| `CARGO_MANIFEST_DIR`   | 包含正在构建的包的清单的目录(包含构建脚本的包)。还要注意，这是生成脚本启动时，当前工作目录的值.                                                                                                                                                                                                                     |
| `CARGO_MANIFEST_LINKS` | 清单`links`的值.                                                                                                                                                                                                                                                                                                   |
| `CARGO_FEATURE_<name>` | 对于正在构建的包的每个激活特性，此环境变量会让`<name>`功能名称存在，名称的`-`会转换成`_`.                                                                                                                                                                                                                          |
| `CARGO_CFG_<cfg>`      | 对正在构建包的每个[配置选项][configuration]，此环境变量将包含配置的值，其中`<cfg>`就是配置的名称，并将`-`翻译成`_`. 如果设置了布尔配置，则存在布尔配置，否则不存在。具有多个值的配置被连接到单个变量，该变量用`，`分隔就好。                                                                                            |
| `OUT_DIR`              | 所有输出都应该放在这个文件夹。此文件夹位于正在构建的包的构建目录中，并且对于所讨论的包是唯一的。                                                                                                                                                                                                                   |
| `TARGET`               | 正在编译的目标三元组。该为这个三元组编译本机代码。关于目标三元组的更多信息在[clang 自身文档中][clang].                                                                                                                                                                                                             |
| `HOST`                 | Rust 编译器的主机三元组.                                                                                                                                                                                                                                                                                           |
| `NUM_JOBS`             | 指定为顶层并行的并行性。这可以传递一个`-j`参数到系统(像`make`). 注意，在解释这个环境变量时应该小心。出于历史目的，仍然提供此功能，但是例如，最新版本的 Cargo 不需要运行`make -j`，因为它会自动。Cargo 自行实现[JooServer]，并且允许构建脚本继承这些信息，因此与 GNU 兼容的程序将使作业服务器已经具有适当配置的并行性。 |
| `OPT_LEVEL` ，`DEBUG`  | 为了分析，当前正在构建的相应变量值.                                                                                                                                                                                                                                                                                |
| `PROFILE`              | 发布版本用`release`，而`debug`用于其他构建.                                                                                                                                                                                                                                                                        |
| `DEP_<name>_<key>`     | 有关此组环境变量的更多信息，请参阅[`links`][links].                                                                                                                                                                                                                                                                 |
| `RUSTC` ，`RUSTDOC`     | Cargo 已经决定使用的编译器和文档生成器，传递给构建脚本，以便构建脚本也可以使用它.                                                                                                                                                                                                                                   |
| `RUSTC_LINKER`         | 如果指定了，Cargo 为了当前目标，决定使用的链接器(二进制文件)的路径。这个链接器可以通过编辑`.cargo/config`更改，欲了解更多信息，请参阅有关文档[Cargo 配置][cargo-config]。                                                                                                                                          |

[links]: ./build-scripts.md#the-links-manifest-key
[configuration]: https://doc.rust-lang.org/reference/attributes.md#conditional-compilation
[clang]: http://clang.llvm.org/docs/CrossCompilation.md#target-triple
[jobserver]: https://www.gnu.org/software/make/manual/html_node/Job-Slots.md
[cargo-config]: ./config.md

### Environment variables Cargo sets for 3rd party subcommands

Cargo 为 第三方子命令设置的环境变量

Cargo 将这个环境变量公开给第三方子命令(即，名为`cargo-foobar`放置在`$PATH`):

- `CARGO` - 执行构建的二进制`cargo`路径。
