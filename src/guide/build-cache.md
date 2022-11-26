## 构建 缓存

Cargo 在单个工作区，共享其中所有包的构建结果。今天，Cargo 不会在不同的工作区共享构建结果，但使用第三方工具可以实现类似的结果，如[sccache]。

用`cargo install sccache`安装 sccache，并在调用 Cargo 之前，设置`RUSTC_WRAPPER`环境变量成`sccache`。如果你使用 bash，更好的方法是`export RUSTC_WRAPPER=sccache`到`.bashrc`文件。有关更多详细信息，请参阅 sccache 文档.

[sccache]: https://github.com/mozilla/sccache
