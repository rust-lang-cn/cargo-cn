## Package ID Specifications

### Package ID specifications

包 ID 规范

Cargo 的子命令经常需要引用依赖关系图中的特定包来进行各种操作，例如更新，清理，构建等。为了解决这个问题，Cargo 支持包 ID 规范。规范是一个字符串，用于唯一地引用依赖关系图中的一个包.

#### Specification grammar

规范语法

包 ID 规范的形式语法是:

```notrust
pkgid := pkgname
       | [ proto "://" ] hostname-and-path [ "#" ( pkgname | semver ) ]
pkgname := name [ ":" semver ]

proto := "http" | "git" | ...
```

这里，括号表示内容是可选的.

#### Example specifications

规范示例

这些都可以是对`foo`包的引用，版本`1.2.3`，来自注册表`crates.io`

| pkgid                        | 名称  |   版本    |          网址          |
| :--------------------------- | :---: | :-----: | :--------------------: |
| `foo`                        | `foo` |   `*`   |          `*`           |
| `foo:1.2.3`                  | `foo` | `1.2.3` |          `*`           |
| `crates.io/foo`              | `foo` |   `*`   |  `*://crates.io/foo`   |
| `crates.io/foo#1.2.3`        | `foo` | `1.2.3` |  `*://crates.io/foo`   |
| `crates.io/bar#foo:1.2.3`    | `foo` | `1.2.3` |  `*://crates.io/bar`   |
| `http://crates.io/foo#1.2.3` | `foo` | `1.2.3` | `http://crates.io/foo` |

#### Brevity of specifications

规范的简洁

这样做的目的是用简洁和详尽的语法来引用依赖图中的包。而不明确的引用可以指代一个或多个包。若使用相同的规范会引用多个包，那大多数命令都会生成错误。
