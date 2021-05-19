## 测试

Cargo 可以使用`cargo test`命令运行您的测试。Cargo 寻找在两个地方运行的测试 :在你`src`中的每个文件，和`tests/`中的任何测试。测试你的`src`文件应该是单元测试，并在`tests/`中的应该是整合式测试。因此，您需要将包装箱导入到`tests`的文件中.

这是在我们的项目中，运行`cargo test`的一个例子，目前没有测试:

```shell
$ cargo test
   Compiling rand v0.1.0 (https://github.com/rust-lang-nursery/rand.git#9f35b8e)
   Compiling hello_world v0.1.0 (file:///path/to/project/hello_world)
     Running target/test/hello_world-9c2b65bbb79eabce

running 0 tests

test result: ok. 0 passed; 0 failed; 0 ignored; 0 measured
```

如果我们的项目有测试，我们会看到更多的输出与正确的测试数量.

您还可以通过传递过滤器，来运行特定测试:

```shell
$ cargo test foo
```

这将运行任何匹配的`foo`测试.

`cargo test`还运行其他检查。例如，它将编译您包含的任何示例(examples)，并且还将测试文档中的示例。请看在 Rust 文档中的[测试指南][testing]，了解更多详细信息.

[testing]: https://doc.rust-lang.org/book/testing.md
