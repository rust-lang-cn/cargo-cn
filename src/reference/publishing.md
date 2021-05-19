## 发布到 crates.io

一旦你有一个你想与世界分享的 crate，就该把它发布到[crates.io]! 发布是指，上载特定版本的，以让[crates.io]进行托管.

发布箱(crate)子时，要小心，因为发布是**常驻**。永远不能覆盖同版本，并且无法删除代码。但是，可以发布的版本数量没有限制。

### 在你开始发布前

首先，你需要一个[crates.io]帐户，用来获取 API 令牌。为此，[访问主页][crates.io]，并通过 GitHub 帐户登录(现在需要)。在此之后，访问你的[帐号设定](https://crates.io/me)页面，并运行`cargo login`命令联通账号。

```console
$ cargo login abcdefghijklmnopqrstuvwxyz012345
```

此命令将告诉 Cargo 关于您的 API 令牌，并将其存储在您的本地`~/.cargo/credentials`(以前是`~/.cargo/config`)。请注意，此令牌是一个**秘密**，不应与其他任何人分享。如果因任何原因泄漏，您应立即重新生成。

### 在你创建新 crate 前

请记住[crates.io]上箱子的名字，会采取先到先得的方式分配。一旦获得箱子名称，它就不能用于另一个箱子.

#### 打包一个 crate

下一步是，将您的包装箱打包成可供[crates.io]上传的格式。为此，我们将使用`cargo package`子命令。这将把我们的整个包装箱全部打包成一个`*.crate`文件，其在`target/package`目录中。

```console
$ cargo package
```

作为一个额外的功能，`*.crate`将独立于当前源树进行验证。在`*.crate`创建之后，会解压到`target/package`目录，然后从头开始构建，以确保构建成功的所有必要文件。可以使用`--no-verify`参数禁用此行为。

现在是时候看看`*.crate`文件了，为了确保您不会意外地打包 2GB 视频资源，或用于代码生成，集成测试或基准测试的大型数据文件。目前存在 10MB 的`*.crate`文件上传大小限制。所以，如果`tests`和`benches`目录及其依赖项大小，最多只达 几 MB，您仍可以将它们保存在包; 不然的话，最好排除它们。

在打包时，Cargo 会自动忽略版本控制系统的忽略文件，但是如果要指定要额外的忽略文件集，则可以使用清单中的`exclude`字段:

```toml
[package]
# ...
exclude = [
    "public/assets/*",
    "videos/*",
]
```

这个数组中每个元素接受的语法是[rust-lang/glob](https://github.com/rust-lang/glob)。如果您宁愿使用白名单，而不是黑名单,Cargo 也支持`include`字段，如果设置,则会覆盖`exclude`字段:

```toml
[package]
# ...
include = [
    "**/*.rs",
    "Cargo.toml",
]
```

### 上传该 crate

现在我们已经有了`*.crate`文件准备好了，可以上传到[crates.io]，接着使用`cargo publish`命令就好。就是这样，你现在已经发布了你的第一个箱子!

```console
$ cargo publish
```

如果你想跳过`cargo package`那一步，`cargo publish`如果找不到副本，子命令将自动打包本地包.

一定要看看[您可以指定的元数据](./manifest.md#package-metadata)确保您的箱子更容易被发现!

### 为已存在的 crate，发布一个新版本

要发布新版本，请在`Cargo.toml`更改`version`为您指定的值。记住[semver 规则](./manifest.md#the-version-field)。然后可选择运行`cargo package`，如果你想检查一下`*.crate`发布前的新版本文件，然后运行`cargo publish`上传新版本.

### 管理一个 基于 crates.io 的 crate

箱的管理主要通过命令行完成`cargo`工具，而不是[crates.io]网络界面。为此，有一些子命令来管理包.

#### `cargo yank`

在您发布时，实际上可能会因某种原因，而最终破坏的箱子版本(语法错误，忘记包含文件等)的情况。对于诸如此类的情况，Cargo 支持一个箱子版本的"yank"。

```console
$ cargo yank --vers 1.0.1
$ cargo yank --vers 1.0.1 --undo
```

一个 yank **不是**删除任何代码。例如，此功能不用于删除意外上传的机密。如果发生这种情况，您必须立即重置这些秘密.

一个 yank 版本的语义是，不为该版本创建新的依赖项，但所有现有的依赖项继续有效。[crates.io]其中一个主要目标是作为永久存档的箱子库，不会随着时间的推移而改变，而允许删除版本将违背这一目标。所以基本上，一个 yank 意味着所有包的`Cargo.lock`不会被破坏，在任何的未来，`Cargo.lock`生成的文件不会列出 yank 的版本。(成为了一个孤岛版本)

#### `cargo owner`

箱子通常由不止一个人开发，或者主要维护者可能会随着时间而改变!箱子的所有者是唯一允许发布新版本箱子的人，但是所有者可以指定其他所有者。

```console
$ cargo owner --add my-buddy
$ cargo owner --remove my-buddy
$ cargo owner --add github:rust-lang:owners
$ cargo owner --remove github:rust-lang:owners
```

给这些命令的所有者 ID 必须是 GitHub 用户名或 GitHub 团队.

如果用`--add`了一个用户名，那该用户成为"命名"所有者，拥有该箱子的完全权利。除了能够发布或 yank 箱子的版本，他们还能够添加或删除所有者，*包含*任命*他们*的所有者。毋庸置疑，你不应该让那些你不完全信任的人，作命名所有者。要成为命名所有者，用户必须之前就已登录[crates.io]。

如果用`--add`了一个团队，那该团队成为"团队"所有者，对箱的权利受到限制。虽然他们有权发布或 yank 箱子的版本，但他们*不*能够添加或删除所有者。除了更方便管理所有者群体之外，团队还可以更安全地防止恶意所有者。

团队的目前的语法是`github:org:team`(见上面的例子)。要将团队添加为所有者，必须是该团队的成员。删除所有者的团队就没有此类限制.

### GitHub 权限

团队成员资格，不是 GitHub 提供的简单公共访问权限，并且在使用它时可能会遇到以下消息:

> 您似乎无权从 GitHub 查询必要的属性，来完成此请求。您可能需要在[crates.io]重新进行身份验证，并申请阅读 GitHub 组织成员资格的权限。去<https://crates.io/login>看看

这基本上是一个全查询防御，当"你试图查询团队信息，而其中的五级成员访问控制，拒绝了它"。这并不夸张。GitHub 对团队访问控制的支持是企业级.

最可能的原因是您最后一次登录是在添加此功能之前。我们最初*没有*要求 GitHub 对用户进行身份验证时的权限，因为我们实际上并没有将用户的令牌用于登录以外的任何其他内容。但是，为了代表您能够查询团队成员资格，我们现在需要[`read:org`范围权限][oauth-scopes]。

您完全可以拒绝我们这个范围，且在团队介绍之前所做的一切，都将继续有效。但是，您永远无法将团队添加为所有者，或者将团队作为团队所有者发布。如果您尝试这样做，您将收到上述错误。如果你试图发布一个你根本不拥有的箱子，但恰好有一个团队，你可能也会看到这个错误。

如果你改变主意，或者只是不确定是否[crates.io]有足够的许可，你可以随时去<https://crates.io/login>查看，[crates.io]会提示您，它没有获得它想要的所有范围许可。

查询 GitHub 的另一个障碍，是组织可能会主动拒绝第三方访问。要检查这一点，您可以访问:

```
https://github.com/organizations/:org/settings/oauth_application_policy
```

这里的`:org`是组织的名称(例如 rust-lang)。你可能会看到类似的东西:

![Organization Access Control](../images/org-level-acl.png)

你可以选择，从您组织的黑名单中，明确删除[crates.io]，或只需按"删除限制(Remove Restrictions)"按钮，允许所有第三方应用程序访问此数据。

或者，当[crates.io]请求了`read:org`范围，您可以明确进入白名单的[crates.io]，能查询组织的问题，通过按其名称旁边的"授予访问权限(Grant Access)"按钮:

![Authentication Access Control](../images/auth-level-acl.png)

[crates.io]: https://crates.io/
[oauth-scopes]: https://developer.github.com/apps/building-oauth-apps/understanding-scopes-for-oauth-apps/
