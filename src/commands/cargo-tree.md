# cargo-tree(1)



## NAME

cargo-tree - Display a tree visualization of a dependency graph

## SYNOPSIS

`cargo tree` [_options_]

## DESCRIPTION

This command will display a tree of dependencies to the terminal. An example
of a simple project that depends on the "rand" package:

```
myproject v0.1.0 (/myproject)
└── rand v0.7.3
    ├── getrandom v0.1.14
    │   ├── cfg-if v0.1.10
    │   └── libc v0.2.68
    ├── libc v0.2.68 (*)
    ├── rand_chacha v0.2.2
    │   ├── ppv-lite86 v0.2.6
    │   └── rand_core v0.5.1
    │       └── getrandom v0.1.14 (*)
    └── rand_core v0.5.1 (*)
[build-dependencies]
└── cc v1.0.50
```

Packages marked with `(*)` have been "de-duplicated". The dependencies for the
package have already been shown elsewhere in the graph, and so are not
repeated. Use the `--no-dedupe` option to repeat the duplicates.

The `-e` flag can be used to select the dependency kinds to display. The
"features" kind changes the output to display the features enabled by
each dependency. For example, `cargo tree -e features`:

```
myproject v0.1.0 (/myproject)
└── log feature "serde"
    └── log v0.4.8
        ├── serde v1.0.106
        └── cfg-if feature "default"
            └── cfg-if v0.1.10
```

In this tree, `myproject` depends on `log` with the `serde` feature. `log` in
turn depends on `cfg-if` with "default" features. When using `-e features` it
can be helpful to use `-i` flag to show how the features flow into a package.
See the examples below for more detail.

## OPTIONS

### Tree Options

<dl>

<dt class="option-term" id="option-cargo-tree--i"><a class="option-anchor" href="#option-cargo-tree--i"></a><code>-i</code> <em>spec</em></dt>
<dt class="option-term" id="option-cargo-tree---invert"><a class="option-anchor" href="#option-cargo-tree---invert"></a><code>--invert</code> <em>spec</em></dt>
<dd class="option-desc">Show the reverse dependencies for the given package. This flag will invert
the tree and display the packages that depend on the given package.</p>
<p>Note that in a workspace, by default it will only display the package's
reverse dependencies inside the tree of the workspace member in the current
directory. The <code>--workspace</code> flag can be used to extend it so that it will
show the package's reverse dependencies across the entire workspace. The <code>-p</code>
flag can be used to display the package's reverse dependencies only with the
subtree of the package given to <code>-p</code>.</dd>


<dt class="option-term" id="option-cargo-tree---prune"><a class="option-anchor" href="#option-cargo-tree---prune"></a><code>--prune</code> <em>spec</em></dt>
<dd class="option-desc">Prune the given package from the display of the dependency tree.</dd>


<dt class="option-term" id="option-cargo-tree---depth"><a class="option-anchor" href="#option-cargo-tree---depth"></a><code>--depth</code> <em>depth</em></dt>
<dd class="option-desc">Maximum display depth of the dependency tree. A depth of 1 displays the direct
dependencies, for example.</dd>


<dt class="option-term" id="option-cargo-tree---no-dedupe"><a class="option-anchor" href="#option-cargo-tree---no-dedupe"></a><code>--no-dedupe</code></dt>
<dd class="option-desc">Do not de-duplicate repeated dependencies. Usually, when a package has already
displayed its dependencies, further occurrences will not re-display its
dependencies, and will include a <code>(*)</code> to indicate it has already been shown.
This flag will cause those duplicates to be repeated.</dd>


<dt class="option-term" id="option-cargo-tree--d"><a class="option-anchor" href="#option-cargo-tree--d"></a><code>-d</code></dt>
<dt class="option-term" id="option-cargo-tree---duplicates"><a class="option-anchor" href="#option-cargo-tree---duplicates"></a><code>--duplicates</code></dt>
<dd class="option-desc">Show only dependencies which come in multiple versions (implies <code>--invert</code>).
When used with the <code>-p</code> flag, only shows duplicates within the subtree of the
given package.</p>
<p>It can be beneficial for build times and executable sizes to avoid building
that same package multiple times. This flag can help identify the offending
packages. You can then investigate if the package that depends on the
duplicate with the older version can be updated to the newer version so that
only one instance is built.</dd>


<dt class="option-term" id="option-cargo-tree--e"><a class="option-anchor" href="#option-cargo-tree--e"></a><code>-e</code> <em>kinds</em></dt>
<dt class="option-term" id="option-cargo-tree---edges"><a class="option-anchor" href="#option-cargo-tree---edges"></a><code>--edges</code> <em>kinds</em></dt>
<dd class="option-desc">The dependency kinds to display. Takes a comma separated list of values:</p>
<ul>
<li><code>all</code> — Show all edge kinds.</li>
<li><code>normal</code> — Show normal dependencies.</li>
<li><code>build</code> — Show build dependencies.</li>
<li><code>dev</code> — Show development dependencies.</li>
<li><code>features</code> — Show features enabled by each dependency. If this is the only
kind given, then it will automatically include the other dependency kinds.</li>
<li><code>no-normal</code> — Do not include normal dependencies.</li>
<li><code>no-build</code> — Do not include build dependencies.</li>
<li><code>no-dev</code> — Do not include development dependencies.</li>
<li><code>no-proc-macro</code> — Do not include procedural macro dependencies.</li>
</ul>
<p>The <code>normal</code>, <code>build</code>, <code>dev</code>, and <code>all</code> dependency kinds cannot be mixed with
<code>no-normal</code>, <code>no-build</code>, or <code>no-dev</code> dependency kinds.</p>
<p>The default is <code>normal,build,dev</code>.</dd>


<dt class="option-term" id="option-cargo-tree---target"><a class="option-anchor" href="#option-cargo-tree---target"></a><code>--target</code> <em>triple</em></dt>
<dd class="option-desc">Filter dependencies matching the given target-triple. The default is the host
platform. Use the value <code>all</code> to include <em>all</em> targets.</dd>


</dl>

### Tree Formatting Options

<dl>

<dt class="option-term" id="option-cargo-tree---charset"><a class="option-anchor" href="#option-cargo-tree---charset"></a><code>--charset</code> <em>charset</em></dt>
<dd class="option-desc">Chooses the character set to use for the tree. Valid values are &quot;utf8&quot; or
&quot;ascii&quot;. Default is &quot;utf8&quot;.</dd>


<dt class="option-term" id="option-cargo-tree--f"><a class="option-anchor" href="#option-cargo-tree--f"></a><code>-f</code> <em>format</em></dt>
<dt class="option-term" id="option-cargo-tree---format"><a class="option-anchor" href="#option-cargo-tree---format"></a><code>--format</code> <em>format</em></dt>
<dd class="option-desc">Set the format string for each package. The default is &quot;{p}&quot;.</p>
<p>This is an arbitrary string which will be used to display each package. The following
strings will be replaced with the corresponding value:</p>
<ul>
<li><code>{p}</code> — The package name.</li>
<li><code>{l}</code> — The package license.</li>
<li><code>{r}</code> — The package repository URL.</li>
<li><code>{f}</code> — Comma-separated list of package features that are enabled.</li>
<li><code>{lib}</code> — The name, as used in a <code>use</code> statement, of the package's library.</li>
</ul></dd>


<dt class="option-term" id="option-cargo-tree---prefix"><a class="option-anchor" href="#option-cargo-tree---prefix"></a><code>--prefix</code> <em>prefix</em></dt>
<dd class="option-desc">Sets how each line is displayed. The <em>prefix</em> value can be one of:</p>
<ul>
<li><code>indent</code> (default) — Shows each line indented as a tree.</li>
<li><code>depth</code> — Show as a list, with the numeric depth printed before each entry.</li>
<li><code>none</code> — Show as a flat list.</li>
</ul></dd>


</dl>

### 选择包

默认情况下，如果没有指定包，则根据清单文件来选择包(如果没有通过`--manifest-path`给出清单文件路径，
则基于当前工作目录进行寻找)。如果是某个工作区的根清单，则选中该工作区的默认成员；否则仅选中清
单所定义的那个包。

工作区的默认成员可通过清单中`workspace.default-members`项来显式指定。如果未指定，则其虚拟
工作区会包含全体工作区成员(等同于传递`--workspace`标志参数时)，而非虚拟工作区则仅包含根部箱自身。

<dl>

<dt class="option-term" id="option-cargo-tree--p"><a class="option-anchor" href="#option-cargo-tree--p"></a><code>-p</code> <em>spec</em>...</dt>
<dt class="option-term" id="option-cargo-tree---package"><a class="option-anchor" href="#option-cargo-tree---package"></a><code>--package</code> <em>spec</em>...</dt>
<dd class="option-desc">Display指定包. SPEC的格式参见 <a href="cargo-pkgid.html">cargo-pkgid(1)</a> 。
此标志参数可多次使用且支持Unix通配符(<code>*</code>, <code>?</code> 和 <code>[]</code>)。不过，为避免shell可能错误地在Cargo获取到
之前就将通配符展开，应在各个模式串两侧使用单引号或双引号。</dd>


<dt class="option-term" id="option-cargo-tree---workspace"><a class="option-anchor" href="#option-cargo-tree---workspace"></a><code>--workspace</code></dt>
<dd class="option-desc">Display 工作区中的全体成员.</dd>




<dt class="option-term" id="option-cargo-tree---exclude"><a class="option-anchor" href="#option-cargo-tree---exclude"></a><code>--exclude</code> <em>SPEC</em>...</dt>
<dd class="option-desc">排除指定包。必须与<code>--workspace</code>标志参数共同使用。
此标志参数可多次使用且支持Unix通配符(<code>*</code>, <code>?</code> 和 <code>[]</code>)。不过，为避免shell可能错误地在Cargo获取到
之前就将通配符展开，应在各个模式串两侧使用单引号或双引号。</dd>


</dl>


### Manifest Options

<dl>

<dt class="option-term" id="option-cargo-tree---manifest-path"><a class="option-anchor" href="#option-cargo-tree---manifest-path"></a><code>--manifest-path</code> <em>path</em></dt>
<dd class="option-desc">用于指定<code>Cargo.toml</code>文件的路径。默认情况下，Cargo会在当前目录或上级目录中寻找<code>Cargo.toml</code>文件。</dd>



<dt class="option-term" id="option-cargo-tree---frozen"><a class="option-anchor" href="#option-cargo-tree---frozen"></a><code>--frozen</code></dt>
<dt class="option-term" id="option-cargo-tree---locked"><a class="option-anchor" href="#option-cargo-tree---locked"></a><code>--locked</code></dt>
<dd class="option-desc">这两个选项用于保证<code>Cargo.lock</code>文件是最新的。如果该锁文件不存在，或者不是最新的，Cargo
会报错退出。其中<code>--frozen</code>选项会阻止Cargo访问网络以检查锁文件是否是最新的。</p>
<p>这些选项，可用于保证<code>Cargo.lock</code>文件是最新的(比如持续集成的构建过程)，
或用于避免联网。</dd>


<dt class="option-term" id="option-cargo-tree---offline"><a class="option-anchor" href="#option-cargo-tree---offline"></a><code>--offline</code></dt>
<dd class="option-desc">禁止Cargo访问网络。如果不添加此选项，Cargo在需要访问网络但网络不可用的情况下，会报错
并停止工作。添加此选项后，Cargo会尽可能尝试不使用网络来工作。</p>
<p>注意，在此情况下可能会产生与联网状态下不同的依赖解析(<strong>Dependency Resolution</strong>)结果。
Cargo只会使用本地已下载的crate，即便本地的索引副本中表明可能有新版本crate。在离线前下载
所需依赖的方法，参见 <a href="cargo-fetch.html">cargo-fetch(1)</a> 。</p>
<p>也可以通过 <code>net.offline</code> <a href="../reference/config.html">config value</a>指定。</dd>



</dl>

### 特性选择

可通过传递特性参数来控制启用哪些特性。如果没有给定要使用的特性，
则每个已选择的包都会自动使用`default`特性。

详见[the features documentation](../reference/features.html#command-line-feature-options)。

<dl>

<dt class="option-term" id="option-cargo-tree---features"><a class="option-anchor" href="#option-cargo-tree---features"></a><code>--features</code> <em>features</em></dt>
<dd class="option-desc">传递以空格或者逗号分隔的列表，其中给出要启用的特性。工作区成员的特性可通过<code>包名/特性名</code>的语法启用。
此参数可多次给定，以分别启用给定的特性。</dd>


<dt class="option-term" id="option-cargo-tree---all-features"><a class="option-anchor" href="#option-cargo-tree---all-features"></a><code>--all-features</code></dt>
<dd class="option-desc">为给定的包启用全部可用特性</dd>


<dt class="option-term" id="option-cargo-tree---no-default-features"><a class="option-anchor" href="#option-cargo-tree---no-default-features"></a><code>--no-default-features</code></dt>
<dd class="option-desc">不启用给定包的<code>default</code>特性</dd>


</dl>


### Display Options

<dl>

<dt class="option-term" id="option-cargo-tree--v"><a class="option-anchor" href="#option-cargo-tree--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-tree---verbose"><a class="option-anchor" href="#option-cargo-tree---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">启用更加详细的输出。可两次使用来显示&quot;非常详细&quot;的输出，其中包含了诸如 依赖警告 以及 构建脚本输出 等额外的输出内容。
也可通过<code>term.verbose</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-tree--q"><a class="option-anchor" href="#option-cargo-tree--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-tree---quiet"><a class="option-anchor" href="#option-cargo-tree---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">不输出Cargo的日志信息。也可通过<code>term.quiet</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-tree---color"><a class="option-anchor" href="#option-cargo-tree---color"></a><code>--color</code> <em>when</em></dt>
<dd class="option-desc">控制输出内容的颜色。有效取值如下：</p>
<ul>
<li><code>auto</code> (默认)：自动检测终端是否支持带颜色的输出。</li>
<li><code>always</code>：总显示带颜色的输出。</li>
<li><code>never</code>：从不显示带颜色的输出。</li>
</ul>
<p>也可通过<code>term.color</code>指定。
<a href="../reference/config.html">config value</a>.</dd>



</dl>

### Common Options

<dl>

<dt class="option-term" id="option-cargo-tree-+toolchain"><a class="option-anchor" href="#option-cargo-tree-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo-tree--h"><a class="option-anchor" href="#option-cargo-tree--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-tree---help"><a class="option-anchor" href="#option-cargo-tree---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo-tree--Z"><a class="option-anchor" href="#option-cargo-tree--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


## 环境

关于Cargo所读取的环境变量，可参见[the reference](../reference/environment-variables.html)


## 退出状态

* `0`: Cargo命令执行成功
* `101`: Cargo命令未能完成.


## EXAMPLES

1. Display the tree for the package in the current directory:

       cargo tree

2. Display all the packages that depend on the `syn` package:

       cargo tree -i syn

3. Show the features enabled on each package:

       cargo tree --format "{p} {f}"

4. Show all packages that are built multiple times. This can happen if multiple
   semver-incompatible versions appear in the tree (like 1.0.0 and 2.0.0).

       cargo tree -d

5. Explain why features are enabled for the `syn` package:

       cargo tree -e features -i syn

   The `-e features` flag is used to show features. The `-i` flag is used to
   invert the graph so that it displays the packages that depend on `syn`. An
   example of what this would display:

   ```
   syn v1.0.17
   ├── syn feature "clone-impls"
   │   └── syn feature "default"
   │       └── rustversion v1.0.2
   │           └── rustversion feature "default"
   │               └── myproject v0.1.0 (/myproject)
   │                   └── myproject feature "default" (command-line)
   ├── syn feature "default" (*)
   ├── syn feature "derive"
   │   └── syn feature "default" (*)
   ├── syn feature "full"
   │   └── rustversion v1.0.2 (*)
   ├── syn feature "parsing"
   │   └── syn feature "default" (*)
   ├── syn feature "printing"
   │   └── syn feature "default" (*)
   ├── syn feature "proc-macro"
   │   └── syn feature "default" (*)
   └── syn feature "quote"
       ├── syn feature "printing" (*)
       └── syn feature "proc-macro" (*)
   ```

   To read this graph, you can follow the chain for each feature from the root
   to see why it is included. For example, the "full" feature is added by the
   `rustversion` crate which is included from `myproject` (with the default
   features), and `myproject` is the package selected on the command-line. All
   of the other `syn` features are added by the "default" feature ("quote" is
   added by "printing" and "proc-macro", both of which are default features).

   If you're having difficulty cross-referencing the de-duplicated `(*)`
   entries, try with the `--no-dedupe` flag to get the full output.

## SEE ALSO
[cargo(1)](cargo.html), [cargo-metadata(1)](cargo-metadata.html)
