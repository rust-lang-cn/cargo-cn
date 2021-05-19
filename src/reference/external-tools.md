## External tools

外部工具

Cargo 的目标之一是与第三方工具(如 IDE 和其他构建系统)的简单集成。为了简化集成，Cargo 有几个设施:

- 一个`cargo metadata`命令，以 JSON 格式输出包结构和依赖关系信息，

- 一个`--message-format`标志，输出有关特定构建的信息，以及

- 支持自定义子命令.

### Information about package structure

包结构的资料

您可以使用`cargo metadata`命令，以获取有关包结构和依赖关系的信息。命令的输出如下所示:

```text
{
  // Integer 版本格式数字.
  "version": integer,

  // 工作区包的列表, 包括 依赖项.
  "packages": [
    {
      // 包 识别id 队列.
      "id": PackageId,

      "name": string,

      "version": string,

      "source": SourceId,

      // 确认依赖的一个列表, 可看 `resolve` 字段中的真实依赖.
      "dependencies": [ Dependency ],

      "targets: [ Target ],

      //  Cargo.toml 路径
      "manifest_path": string,
    }
  ],

  "workspace_members": [ PackageId ],

  // 依赖 关系图.
  "resolve": {
     "nodes": [
       {
         "id": PackageId,
         "dependencies": [ PackageId ]
       }
     ]
  }
}
```

格式稳定且有版本化。调用`cargo metadata`时，你应该通过`--format-version`明确标记，以避免向前不兼容的危险。

如果你正在使用 Rust，这有个[cargo_metadata]箱.

[cargo_metadata]: https://crates.io/crates/cargo_metadata

### Information about build

关于构建的资料

传递`--message-format=json`给，Cargo， 将在构建期间输出以下信息:

- 编译器错误和警告，

- 制作的工件，

- 构建脚本的结果(例如，本机依赖项).

输出以每行格式的 JSON 对象转到 stdout。`reason`字段区分不同类型的消息.

有关 Makefile 兼容格式的依赖关系的信息存储在工件旁的`.d`文件中。

### Custom subcommands

自定义的子命令

Cargo 设计为，可以使用新的子命令进行扩展，而无需修改 Cargo 本身。这是通过转化一个 cargo `(?<command>[^ ]+)`的命令调用，变化为调用外部工具`cargo-${command}`来实现的。外部工具必须存在于用户其中一个`$PATH`目录中.

当 Cargo 调用自定义子命令时，子命令的第一个参数将像往常一样是自定义子命令的文件名。第二个参数将是子命令名称本身。例如，在调用`cargo-${command}`时，第二个参数是`${command}`。命令行上的其他所有参数将保持不变.

Cargo 还可以用`cargo help ${command}`显示自定义子命令的帮助输出。Cargo 假定子命令将在第三个参数出现时，打印帮助消息`--help`.所以，`cargo help ${command}`会调用`cargo-${command} ${command} --help`.

自定义子命令可以使用`CARGO`环境变量回调 Cargo。或者，它可以链接到作为一个库的`cargo`箱，但这种方法有缺点:

- Cargo 作为库是不稳定的:API 可能会更改，但不会弃用

- 链接的 Cargo 库的版本可能与 Cargo 二进制文件不同
