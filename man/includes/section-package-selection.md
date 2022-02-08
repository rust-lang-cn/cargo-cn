### 选择包

默认情况下，如果没有指定包，则根据清单文件来选择包(如果没有通过`--manifest-path`给出清单文件路径，
则基于当前工作目录进行寻找)。如果是某个工作区的根清单，则选中该工作区的默认成员；否则仅选中清
单所定义的那个包。

工作区的默认成员可通过清单中`workspace.default-members`项来显式指定。如果未指定，则其虚拟
工作区会包含全体工作区成员(等同于传递`--workspace`标志参数时)，而非虚拟工作区则仅包含根部箱自身。

{{#options}}

{{#option "`-p` _spec_..." "`--package` _spec_..."}}
{{actionverb}}指定包. SPEC的格式参见 {{man "cargo-pkgid" 1}} 。
此标志参数可多次使用且支持Unix通配符(`*`, `?` 和 `[]`)。不过，为避免shell可能错误地在Cargo获取到
之前就将通配符展开，应在各个模式串两侧使用单引号或双引号。
{{/option}}

{{#option "`--workspace`" }}
{{actionverb}} 工作区中的全体成员.
{{/option}}

{{#unless noall}}
{{#option "`--all`" }}
`--workspace`的已废弃的别名。
{{/option}}
{{/unless}}

{{#option "`--exclude` _SPEC_..." }}
排除指定包。必须与`--workspace`标志参数共同使用。
此标志参数可多次使用且支持Unix通配符(`*`, `?` 和 `[]`)。不过，为避免shell可能错误地在Cargo获取到
之前就将通配符展开，应在各个模式串两侧使用单引号或双引号。
{{/option}}

{{/options}}
