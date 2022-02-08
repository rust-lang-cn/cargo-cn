### 特性选择

可通过传递特性参数来控制启用哪些特性。如果没有给定要使用的特性，
则每个已选择的包都会自动使用`default`特性。

详见[the features documentation](../reference/features.html#command-line-feature-options)。

{{#options}}

{{#option "`--features` _features_" }}
传递以空格或者逗号分隔的列表，其中给出要启用的特性。工作区成员的特性可通过`包名/特性名`的语法启用。
此参数可多次给定，以分别启用给定的特性。
{{/option}}

{{#option "`--all-features`" }}
为给定的包启用全部可用特性
{{/option}}

{{#option "`--no-default-features`" }}
不启用给定包的`default`特性
{{/option}}

{{/options}}
