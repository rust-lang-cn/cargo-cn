# cargo-clean(1)
{{*set actionverb="Clean"}}

## 名称

cargo-clean - 移除已生成的工件

## 用法

`cargo clean` [_options_]

## 描述

将Cargo生成的工件从生成目标目录中移除。

若不添加参数，`cargo clean` 会删除整个target目录。

## 可选参数

### 选择包

若不指定包名，则清除工作目录中的所有的包和依赖

{{#options}}
{{#option "`-p` _spec_..." "`--package` _spec_..." }}
只清理参数指定的包中的文件。这个参数可以多次使用来指定多个包，详见 {{man "cargo-pkgid" 1}} 。
{{/option}}
{{/options}}

### 清理选项

{{#options}}

{{#option "`--doc`" }}
添加该参数后，`cargo clean`命令只会移除生成目标目录中的`doc`目录。
{{/option}}

{{#option "`--release`" }}
移除`release`目录中的工件。
{{/option}}

{{#option "`--profile` _name_" }}
移除指定编译配置(__Profile__)目录中的全部工件。
{{/option}}

{{> options-target-dir }}

{{> options-target-triple }}

{{/options}}

### 显示选项

{{#options}}
{{> options-display }}
{{/options}}

### 清单选项

{{#options}}
{{> options-manifest-path }}

{{> options-locked }}
{{/options}}

{{> section-options-common }}

{{> section-environment }}

{{> section-exit-status }}

## 示例

1. 移除整个生成目标目录:

       cargo clean

2. 仅移除release目录中的工件:

       cargo clean --release

## 相关
{{man "cargo" 1}}, {{man "cargo-build" 1}}
