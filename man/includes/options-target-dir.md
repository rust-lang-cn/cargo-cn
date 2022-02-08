{{#option "`--target-dir` _directory_"}}
用于存放生成的工件以及中间文件的目录。也可通过环境变量`CARGO_TARGET_DIR` 或 
`build.target-dir` [config value](../reference/config.html)指定。

{{#if temp-target-dir}} 默认情况下位于该平台临时目录下的一个新的临时文件夹。

当指定 `--path`标志参数时，若未指定`--target-dir`，则默认会使用工作区中的`target`目录。
{{else}} 默认情况下为根工作区中的`target`目录。
{{/if}}
{{/option}}
