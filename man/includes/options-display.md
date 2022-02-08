{{#option "`-v`" "`--verbose`"}}
启用更加详细的输出。可两次使用来显示"非常详细"的输出，其中包含了诸如 依赖警告 以及 构建脚本输出 等额外的输出内容。
也可通过`term.verbose`指定。
[config value](../reference/config.html).
{{/option}}

{{#option "`-q`" "`--quiet`"}}
不输出Cargo的日志信息。也可通过`term.quiet`指定。
[config value](../reference/config.html).
{{/option}}

{{#option "`--color` _when_"}}
控制输出内容的颜色。有效取值如下：

- `auto` (默认)：自动检测终端是否支持带颜色的输出。
- `always`：总显示带颜色的输出。
- `never`：从不显示带颜色的输出。

也可通过`term.color`指定。
[config value](../reference/config.html).
{{/option}}
