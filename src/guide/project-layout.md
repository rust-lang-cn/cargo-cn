# 软件包布局

Cargo 使用一组约定进行文件组织，这使得进入一个全新的 Cargo [package][def-package] 也相当简便:

```console
.
├── Cargo.lock
├── Cargo.toml
├── src/
│   ├── lib.rs
│   ├── main.rs
│   └── bin/
│       ├── named-executable.rs
│       ├── another-executable.rs
│       └── multi-file-executable/
│           ├── main.rs
│           └── some_module.rs
├── benches/
│   ├── large-input.rs
│   └── multi-file-bench/
│       ├── main.rs
│       └── bench_module.rs
├── examples/
│   ├── simple.rs
│   └── multi-file-example/
│       ├── main.rs
│       └── ex_module.rs
└── tests/
    ├── some-integration-tests.rs
    └── multi-file-test/
        ├── main.rs
        └── test_module.rs
```

* `Cargo.toml`和`Cargo.lock`存储在项目的根目录中。
* 源代码置于`src`目录。
* 默认库文件是`src/lib.rs`。
* 默认的可执行文件入口是`src/main.rs`。
    * 其他可执行文件，可以放入`src/bin`。
* 集成测试放入`tests`目录。
* 示例放入`examples`目录。
* 基准测试放入`benches`目录。

<!-- 这些将在更详细的[清单描述](../reference/manifest.md#the-project-layout)说明中解释。 -->
<!-- 
If a binary, example, bench, or integration test consists of multiple source
files, place a `main.rs` file along with the extra [*modules*][def-module]
within a subdirectory of the `src/bin`, `examples`, `benches`, or `tests`
directory. The name of the executable will be the directory name. -->

如果一份 二进制文件，示例，基准测试或集成测试包含多份源文件，请将 `main.rs` 放入 `src/bin`，`examples`，`benches` 或 `test` 目录的子目录中，此时可执行文件的名称将会同目录名称相同。

<!-- > **Note:** By convention, binaries, examples, benches and integration tests follow `kebab-case` naming style, unless there are compatibility reasons to do otherwise (e.g. compatibility with a pre-existing binary name). Modules within those targets are `snake_case` following the [Rust standard](https://rust-lang.github.io/rfcs/0430-finalizing-naming-conventions.html). -->

> **注意**: 按照惯例，二进制文件、示例、基准测试和集成测试都应根据 `kebab-case` 的命名规则，除非有兼容性问题(比如和已经存在的二进制文件重名)而必须采用其他命名方式。而内部模块通常使用依照 [Rust 规范](https://rust-lang.github.io/rfcs/0430-finalizing-naming-conventions.html)的 `snake_case` 规则进行命名。

<!-- You can learn more about Rust's module system in [the book][book-modules]. -->
更多关于 Rust 模块组织系统的信息，请参阅 [the book][book-modules].

<!-- See [Configuring a target] for more details on manually configuring targets.
See [Target auto-discovery] for more information on controlling how Cargo
automatically infers target names. -->

关于手动配置 target，请查阅 [Configuring a target]

[book-modules]: https://doc.rust-lang.org/book/ch07-00-managing-growing-projects-with-packages-crates-and-modules.html
[Configuring a target]: ../reference/cargo-targets.md#configuring-a-target
[def-package]:           ../appendix/glossary.md#package          '"package" (术语详解)'
[def-module]:            ../appendix/glossary.md#module           '"module" (术语详解)'
[Target auto-discovery]: ../reference/cargo-targets.md#target-auto-discovery

