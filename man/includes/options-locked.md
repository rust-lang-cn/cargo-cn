{{#option "`--frozen`" "`--locked`"}}
这两个选项用于保证`Cargo.lock`文件是最新的。如果该锁文件不存在，或者不是最新的，Cargo
会报错退出。其中`--frozen`选项会阻止Cargo访问网络以检查锁文件是否是最新的。

这些选项，可用于保证`Cargo.lock`文件是最新的(比如持续集成的构建过程)，
或用于避免联网。
{{/option}}

{{#option "`--offline`"}}
禁止Cargo访问网络。如果不添加此选项，Cargo在需要访问网络但网络不可用的情况下，会报错
并停止工作。添加此选项后，Cargo会尽可能尝试不使用网络来工作。

注意，在此情况下可能会产生与联网状态下不同的依赖解析(__Dependency Resolution__)结果。
Cargo只会使用本地已下载的crate，即便本地的索引副本中表明可能有新版本crate。在离线前下载
所需依赖的方法，参见 {{man "cargo-fetch" 1}} 。

也可以通过 `net.offline` [config value](../reference/config.html)指定。
{{/option}}
