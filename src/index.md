# Cargo 手册

> 中文译注（Chinese translation of the [Cargo Book][cargo-book]）：
>
> 1. 《Rust Cookbook 中文版》 翻译自 [Cargo Book][cargo-book]，查看此书的 [Github 翻译项目][cargo-book-cn]。
> 2. 本书翻译内容源自 [chinanf-boy]的开源的[翻译版本][chinanf-boy-cargo]，我们对原译者感激不尽！该版本最后翻译更新时间是 2019 年 5 月 12 日，与当前的英文版差异比较大，特别期待您加入到本开源翻译项目组中来维护本书，确保本书紧跟英文版。
> 3. 目前英文版已经加入了命令行帮助页部分，还有其他章节内容，内容已经扩充了一倍以上，所以中文版翻译（包括网上所有版本）的部分还不到 1/2。
> 4. 许可协议：跟随英文原书使用 MIT 和 Apache 2.0 双许可授权。
> 5. <a href="https://rustwiki.org/zh-CN/cargo" style="color:red;">本站支持文档中英文切换</a>，点击页面右上角语言图标可切换到相同章节的英文页面，**英文版每天都会自动同步一次官方的最新版本**。
> 6. 若发现本页表达错误或帮助我们改进翻译，可点击右上角的编辑按钮打开本页对应源码文件进行编辑和修改，Rust 中文资源的开源组织发展离不开大家，感谢您的支持和帮助！

[cargo-book]: https://doc.rust-lang.org/cargo
[cargo-book-cn]: https://doc.rust-lang.org/cargo-cn
[chinanf-boy]: https://github.com/chinanf-boy
[chinanf-boy-cargo]: https://github.com/chinanf-boy/cargo-book-zh

![Cargo Logo](images/Cargo-Logo-Small.png)

Cargo 是 [Rust] 的 _包管理器_。Cargo 会下载您 Rust 的包依赖项，编译您的包，生成可分发的包，并将它们上传到 [crates.io] - Rust 社区的*包注册表*。你可以通过 [Github] 为本书做贡献。

### 章节

**[入门](getting-started/index.md)**

开始使用 Cargo，安装 Cargo (和 Rust)并设置您的第一个crate。

**[Cargo 指南](guide/index.md)**

该指南将为您提供使用 Cargo 开发 Rust 包的所有信息。

**[Cargo 参考](reference/index.md)**

该参考文献涵盖了 Cargo 各个领域的细节。

**[常见问题](faq.md)**

**附录**

- [词汇表](./appendix/glossary.md)
- [Git 授权](./appendix/git-authentication.md)

**其他文档**

- [变更记录][change-log] -- 详细记录每次版本更新时 Cargo 的变化。
- [Rust 官方文档][rust-doc] -- Rust 官方工具和文档链接。

[rust]: https://www.rust-lang.org/
[crates.io]: https://crates.io/
[github]: https://github.com/rust-lang/cargo/tree/master/src/doc/src
[change-log]: https://doc.rust-lang.org/cargo/CHANGELOG.html
[rust-doc]: https://doc.rust-lang.org/
