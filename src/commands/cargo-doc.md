# cargo-doc(1)


## NAME

cargo-doc - Build a package's documentation

## SYNOPSIS

`cargo doc` [_options_]

## DESCRIPTION

Build the documentation for the local package and all dependencies. The output
is placed in `target/doc` in rustdoc's usual format.

## OPTIONS

### Documentation Options

<dl>

<dt class="option-term" id="option-cargo-doc---open"><a class="option-anchor" href="#option-cargo-doc---open"></a><code>--open</code></dt>
<dd class="option-desc">Open the docs in a browser after building them. This will use your default
browser unless you define another one in the <code>BROWSER</code> environment variable
or use the <a href="../reference/config.html#docbrowser"><code>doc.browser</code></a> configuration
option.</dd>


<dt class="option-term" id="option-cargo-doc---no-deps"><a class="option-anchor" href="#option-cargo-doc---no-deps"></a><code>--no-deps</code></dt>
<dd class="option-desc">Do not build documentation for dependencies.</dd>


<dt class="option-term" id="option-cargo-doc---document-private-items"><a class="option-anchor" href="#option-cargo-doc---document-private-items"></a><code>--document-private-items</code></dt>
<dd class="option-desc">Include non-public items in the documentation. This will be enabled by default if documenting a binary target.</dd>


</dl>

### 选择包

默认情况下，如果没有指定包，则根据清单文件来选择包(如果没有通过`--manifest-path`给出清单文件路径，
则基于当前工作目录进行寻找)。如果是某个工作区的根清单，则选中该工作区的默认成员；否则仅选中清
单所定义的那个包。

工作区的默认成员可通过清单中`workspace.default-members`项来显式指定。如果未指定，则其虚拟
工作区会包含全体工作区成员(等同于传递`--workspace`标志参数时)，而非虚拟工作区则仅包含根部箱自身。

<dl>

<dt class="option-term" id="option-cargo-doc--p"><a class="option-anchor" href="#option-cargo-doc--p"></a><code>-p</code> <em>spec</em>...</dt>
<dt class="option-term" id="option-cargo-doc---package"><a class="option-anchor" href="#option-cargo-doc---package"></a><code>--package</code> <em>spec</em>...</dt>
<dd class="option-desc">Document指定包. SPEC的格式参见 <a href="cargo-pkgid.html">cargo-pkgid(1)</a> 。
此标志参数可多次使用且支持Unix通配符(<code>*</code>, <code>?</code> 和 <code>[]</code>)。不过，为避免shell可能错误地在Cargo获取到
之前就将通配符展开，应在各个模式串两侧使用单引号或双引号。</dd>


<dt class="option-term" id="option-cargo-doc---workspace"><a class="option-anchor" href="#option-cargo-doc---workspace"></a><code>--workspace</code></dt>
<dd class="option-desc">Document 工作区中的全体成员.</dd>



<dt class="option-term" id="option-cargo-doc---all"><a class="option-anchor" href="#option-cargo-doc---all"></a><code>--all</code></dt>
<dd class="option-desc"><code>--workspace</code>的已废弃的别名。</dd>



<dt class="option-term" id="option-cargo-doc---exclude"><a class="option-anchor" href="#option-cargo-doc---exclude"></a><code>--exclude</code> <em>SPEC</em>...</dt>
<dd class="option-desc">排除指定包。必须与<code>--workspace</code>标志参数共同使用。
此标志参数可多次使用且支持Unix通配符(<code>*</code>, <code>?</code> 和 <code>[]</code>)。不过，为避免shell可能错误地在Cargo获取到
之前就将通配符展开，应在各个模式串两侧使用单引号或双引号。</dd>


</dl>


### Target Selection

When no target selection options are given, `cargo doc` will document all
binary and library targets of the selected package. The binary will be skipped
if its name is the same as the lib target. Binaries are skipped if they have
`required-features` that are missing.

The default behavior can be changed by setting `doc = false` for the target in
the manifest settings. Using target selection options will ignore the `doc`
flag and will always document the given target.

<dl>
<dt class="option-term" id="option-cargo-doc---lib"><a class="option-anchor" href="#option-cargo-doc---lib"></a><code>--lib</code></dt>
<dd class="option-desc">Document the package's library.</dd>


<dt class="option-term" id="option-cargo-doc---bin"><a class="option-anchor" href="#option-cargo-doc---bin"></a><code>--bin</code> <em>name</em>...</dt>
<dd class="option-desc">Document the specified binary. This flag may be specified multiple times
and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-doc---bins"><a class="option-anchor" href="#option-cargo-doc---bins"></a><code>--bins</code></dt>
<dd class="option-desc">Document all binary targets.</dd>



<dt class="option-term" id="option-cargo-doc---example"><a class="option-anchor" href="#option-cargo-doc---example"></a><code>--example</code> <em>name</em>...</dt>
<dd class="option-desc">Document the specified example. This flag may be specified multiple times
and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-doc---examples"><a class="option-anchor" href="#option-cargo-doc---examples"></a><code>--examples</code></dt>
<dd class="option-desc">Document all example targets.</dd>


</dl>

### 特性选择

可通过传递特性参数来控制启用哪些特性。如果没有给定要使用的特性，
则每个已选择的包都会自动使用`default`特性。

详见[the features documentation](../reference/features.html#command-line-feature-options)。

<dl>

<dt class="option-term" id="option-cargo-doc---features"><a class="option-anchor" href="#option-cargo-doc---features"></a><code>--features</code> <em>features</em></dt>
<dd class="option-desc">传递以空格或者逗号分隔的列表，其中给出要启用的特性。工作区成员的特性可通过<code>包名/特性名</code>的语法启用。
此参数可多次给定，以分别启用给定的特性。</dd>


<dt class="option-term" id="option-cargo-doc---all-features"><a class="option-anchor" href="#option-cargo-doc---all-features"></a><code>--all-features</code></dt>
<dd class="option-desc">为给定的包启用全部可用特性</dd>


<dt class="option-term" id="option-cargo-doc---no-default-features"><a class="option-anchor" href="#option-cargo-doc---no-default-features"></a><code>--no-default-features</code></dt>
<dd class="option-desc">不启用给定包的<code>default</code>特性</dd>


</dl>


### Compilation Options

<dl>

<dt class="option-term" id="option-cargo-doc---target"><a class="option-anchor" href="#option-cargo-doc---target"></a><code>--target</code> <em>triple</em></dt>
<dd class="option-desc">为指定架构执行 Document 。默认情况下为本机的架构。三元组的格式为
<code>&lt;arch&gt;&lt;sub&gt;-&lt;vendor&gt;-&lt;sys&gt;-&lt;abi&gt;</code>。执行 <code>rustc --print target-list</code>
可得到支持的构建目标列表。</p>
<p>也可通过<code>build.target</code>指定(<a href="../reference/config.html">config value</a>)。</p>
<p>注意，指定该标志参数会使Cargo产生的构建工件放在与平常不同的目录下。
详情参见<a href="../guide/build-cache.html">build cache</a></dd>



<dt class="option-term" id="option-cargo-doc--r"><a class="option-anchor" href="#option-cargo-doc--r"></a><code>-r</code></dt>
<dt class="option-term" id="option-cargo-doc---release"><a class="option-anchor" href="#option-cargo-doc---release"></a><code>--release</code></dt>
<dd class="option-desc">Document optimized artifacts with the <code>release</code> profile.
See also the <code>--profile</code> option for choosing a specific profile by name.</dd>



<dt class="option-term" id="option-cargo-doc---profile"><a class="option-anchor" href="#option-cargo-doc---profile"></a><code>--profile</code> <em>name</em></dt>
<dd class="option-desc">Document with the given profile.
See the <a href="../reference/profiles.html">the reference</a> for more details on profiles.</dd>



<dt class="option-term" id="option-cargo-doc---ignore-rust-version"><a class="option-anchor" href="#option-cargo-doc---ignore-rust-version"></a><code>--ignore-rust-version</code></dt>
<dd class="option-desc">Document the target even if the selected Rust compiler is older than the
required Rust version as configured in the project's <code>rust-version</code> field.</dd>



</dl>

### Output Options

<dl>
<dt class="option-term" id="option-cargo-doc---target-dir"><a class="option-anchor" href="#option-cargo-doc---target-dir"></a><code>--target-dir</code> <em>directory</em></dt>
<dd class="option-desc">用于存放生成的工件以及中间文件的目录。也可通过环境变量<code>CARGO_TARGET_DIR</code> 或 
<code>build.target-dir</code> <a href="../reference/config.html">config value</a>指定。</p>
<p>默认情况下为根工作区中的<code>target</code>目录。</dd>


</dl>

### Display Options

<dl>
<dt class="option-term" id="option-cargo-doc--v"><a class="option-anchor" href="#option-cargo-doc--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-doc---verbose"><a class="option-anchor" href="#option-cargo-doc---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">启用更加详细的输出。可两次使用来显示&quot;非常详细&quot;的输出，其中包含了诸如 依赖警告 以及 构建脚本输出 等额外的输出内容。
也可通过<code>term.verbose</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-doc--q"><a class="option-anchor" href="#option-cargo-doc--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-doc---quiet"><a class="option-anchor" href="#option-cargo-doc---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">不输出Cargo的日志信息。也可通过<code>term.quiet</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-doc---color"><a class="option-anchor" href="#option-cargo-doc---color"></a><code>--color</code> <em>when</em></dt>
<dd class="option-desc">控制输出内容的颜色。有效取值如下：</p>
<ul>
<li><code>auto</code> (默认)：自动检测终端是否支持带颜色的输出。</li>
<li><code>always</code>：总显示带颜色的输出。</li>
<li><code>never</code>：从不显示带颜色的输出。</li>
</ul>
<p>也可通过<code>term.color</code>指定。
<a href="../reference/config.html">config value</a>.</dd>



<dt class="option-term" id="option-cargo-doc---message-format"><a class="option-anchor" href="#option-cargo-doc---message-format"></a><code>--message-format</code> <em>fmt</em></dt>
<dd class="option-desc">The output format for diagnostic messages. Can be specified multiple times
and consists of comma-separated values. Valid values:</p>
<ul>
<li><code>human</code> (default): Display in a human-readable text format. Conflicts with
<code>short</code> and <code>json</code>.</li>
<li><code>short</code>: Emit shorter, human-readable text messages. Conflicts with <code>human</code>
and <code>json</code>.</li>
<li><code>json</code>: Emit JSON messages to stdout. See
<a href="../reference/external-tools.html#json-messages">the reference</a>
for more details. Conflicts with <code>human</code> and <code>short</code>.</li>
<li><code>json-diagnostic-short</code>: Ensure the <code>rendered</code> field of JSON messages contains
the &quot;short&quot; rendering from rustc. Cannot be used with <code>human</code> or <code>short</code>.</li>
<li><code>json-diagnostic-rendered-ansi</code>: Ensure the <code>rendered</code> field of JSON messages
contains embedded ANSI color codes for respecting rustc's default color
scheme. Cannot be used with <code>human</code> or <code>short</code>.</li>
<li><code>json-render-diagnostics</code>: Instruct Cargo to not include rustc diagnostics in
in JSON messages printed, but instead Cargo itself should render the
JSON diagnostics coming from rustc. Cargo's own JSON diagnostics and others
coming from rustc are still emitted. Cannot be used with <code>human</code> or <code>short</code>.</li>
</ul></dd>


</dl>

### Manifest Options

<dl>
<dt class="option-term" id="option-cargo-doc---manifest-path"><a class="option-anchor" href="#option-cargo-doc---manifest-path"></a><code>--manifest-path</code> <em>path</em></dt>
<dd class="option-desc">用于指定<code>Cargo.toml</code>文件的路径。默认情况下，Cargo会在当前目录或上级目录中寻找<code>Cargo.toml</code>文件。</dd>



<dt class="option-term" id="option-cargo-doc---frozen"><a class="option-anchor" href="#option-cargo-doc---frozen"></a><code>--frozen</code></dt>
<dt class="option-term" id="option-cargo-doc---locked"><a class="option-anchor" href="#option-cargo-doc---locked"></a><code>--locked</code></dt>
<dd class="option-desc">这两个选项用于保证<code>Cargo.lock</code>文件是最新的。如果该锁文件不存在，或者不是最新的，Cargo
会报错退出。其中<code>--frozen</code>选项会阻止Cargo访问网络以检查锁文件是否是最新的。</p>
<p>这些选项，可用于保证<code>Cargo.lock</code>文件是最新的(比如持续集成的构建过程)，
或用于避免联网。</dd>


<dt class="option-term" id="option-cargo-doc---offline"><a class="option-anchor" href="#option-cargo-doc---offline"></a><code>--offline</code></dt>
<dd class="option-desc">禁止Cargo访问网络。如果不添加此选项，Cargo在需要访问网络但网络不可用的情况下，会报错
并停止工作。添加此选项后，Cargo会尽可能尝试不使用网络来工作。</p>
<p>注意，在此情况下可能会产生与联网状态下不同的依赖解析(<strong>Dependency Resolution</strong>)结果。
Cargo只会使用本地已下载的crate，即便本地的索引副本中表明可能有新版本crate。在离线前下载
所需依赖的方法，参见 <a href="cargo-fetch.html">cargo-fetch(1)</a> 。</p>
<p>也可以通过 <code>net.offline</code> <a href="../reference/config.html">config value</a>指定。</dd>


</dl>

### Common Options

<dl>

<dt class="option-term" id="option-cargo-doc-+toolchain"><a class="option-anchor" href="#option-cargo-doc-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo-doc--h"><a class="option-anchor" href="#option-cargo-doc--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-doc---help"><a class="option-anchor" href="#option-cargo-doc---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo-doc--Z"><a class="option-anchor" href="#option-cargo-doc--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


### Miscellaneous Options

<dl>
<dt class="option-term" id="option-cargo-doc--j"><a class="option-anchor" href="#option-cargo-doc--j"></a><code>-j</code> <em>N</em></dt>
<dt class="option-term" id="option-cargo-doc---jobs"><a class="option-anchor" href="#option-cargo-doc---jobs"></a><code>--jobs</code> <em>N</em></dt>
<dd class="option-desc">要并行运行的作业数量。也可通过<code>build.jobs</code> <a href="../reference/config.html">config value</a>指定。
默认为CPU数量。</dd>


</dl>

## 环境

关于Cargo所读取的环境变量，可参见[the reference](../reference/environment-variables.html)


## 退出状态

* `0`: Cargo命令执行成功
* `101`: Cargo命令未能完成.


## EXAMPLES

1. Build the local package documentation and its dependencies and output to
   `target/doc`.

       cargo doc

## SEE ALSO
[cargo(1)](cargo.html), [cargo-rustdoc(1)](cargo-rustdoc.html), [rustdoc(1)](https://doc.rust-lang.org/rustdoc/index.html)
