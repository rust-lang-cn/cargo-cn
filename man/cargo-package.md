# cargo-package(1)
{{*set actionverb="Package"}}
{{*set noall=true}}

## 名称

cargo-package - 将本地包打包为可分发的压缩文件

## 用法

`cargo package` [_options_]

## 描述

此命令会在当前目录创建一个可分发的，压缩过的`.crate`文件并附带其源代码。文件会存储于
`target/package`目录中。命令的执行可分为以下步骤：

1. 加载当前的工作空间以进行基本的检查。
    - 如果不指定依赖的版本，就不能使用路径依赖。Cargo会在发布的包中忽略路径值。但
      `dev-dependencies` 没有这一限制。
2. 创建压缩的`.crate`文件
    - 原始的 `Cargo.toml`文件会被重写并规范化。
    - 清单中的 `[patch]`、`[replace]`、`[workspace]`段会被移除。
    - 如果包中有可执行的二进制或示例编译目标，则还会包含`Cargo.lock`文件。如果指定了
      `--locked`标志参数，{{man "cargo-install" 1}}会使用锁文件。
    - 如果可用，会通过`.cargo_vcs_info.json`文件保存当前版本控制系统的签出哈希值(__Checkout Hash__)。
      可通过`--allow-dirty`标志参数指定不生成该文件。
3. 解压`.crate`文件并对其进行构建，以验证其确实可以成功构建。
    - 这一步会从头开始构建，以确保可以从原始状态构建。可通过指定`--no-verify`标志参数来跳过这一步。
4. 检查构建脚本是否会修改源代码的文件。

包含文件的范围可通过清单中的`include`和`exclude`字段来控制。

更多关于打包和发布的细节可参见[the reference](../reference/publishing.html)

### .cargo_vcs_info.json 的格式

会以下面的格式生成`.cargo_vcs_info.json`文件。

```javascript
{
 "git": {
   "sha1": "aac20b6e7e543e6dd4118b246c77225e3a3a1302"
 },
 "path_in_vcs": ""
}
```

`path_in_vcs`会被设置为该软件包关于版本控制仓库的相对路径

## 可选参数

### 打包选项

{{#options}}

{{#option "`-l`" "`--list`" }}
输出包中包含的文件(不实际进行打包)。
{{/option}}

{{#option "`--no-verify`" }}
构建包时不进行校验。
{{/option}}

{{#option "`--no-metadata`" }}
忽略 缺少可读的元信息(如描述信息或采用的授权协议) 时产生的警告。
{{/option}}

{{#option "`--allow-dirty`" }}
允许打包 在版本控制系统中仍有未提交内容 的包。
{{/option}}

{{/options}}

{{> section-package-selection }}

### 编译选项

{{#options}}

{{> options-target-triple }}

{{> options-target-dir }}

{{/options}}

{{> section-features }}

### 清单选项

{{#options}}

{{> options-manifest-path }}

{{> options-locked }}

{{/options}}

### 混杂选项

{{#options}}
{{> options-jobs }}
{{/options}}

### 显示选项

{{#options}}
{{> options-display }}
{{/options}}

{{> section-options-common }}

{{> section-environment }}

{{> section-exit-status }}

## 示例

1. 为当前包创建一个压缩好的`.crate`文件:

        cargo package

## 相关
{{man "cargo" 1}}, {{man "cargo-publish" 1}}
