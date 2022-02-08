{{#option "`--target` _triple_"}}
为指定架构执行 {{actionverb}} 。默认情况下为本机的架构。三元组的格式为
`<arch><sub>-<vendor>-<sys>-<abi>`。执行 `rustc --print target-list`
可得到支持的构建目标列表。

也可通过`build.target`指定([config value](../reference/config.html))。

注意，指定该标志参数会使Cargo产生的构建工件放在与平常不同的目录下。
详情参见[build cache](../guide/build-cache.html)
{{/option}}
