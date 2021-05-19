## 项目布局

Cargo 使用文件放置惯例，以便轻松进入新的 Cargo 项目:

```shell
.
├── Cargo.lock
├── Cargo.toml
├── benches
│   └── large-input.rs
├── examples
│   └── simple.rs
├── src
│   ├── bin
│   │   └── another_executable.rs
│   ├── lib.rs
│   └── main.rs
└── tests
    └── some-integration-tests.rs
```

- `Cargo.toml`和`Cargo.lock`存储在项目的根目录中.
- 源代码进入`src`目录.
- 默认库文件是`src/lib.rs`.
- 默认的可执行文件是`src/main.rs`.
- 其他可执行文件，可以放入`src/bin/*.rs`.
- 集成测试进入`tests`目录(单元测试进到，正在测试的每个文件中).
- 示例进入`examples`目录.
- 基准进入`benches`目录.

这些将在更详细的[清单描述](../reference/manifest.md#the-project-layout)说明中解释.
