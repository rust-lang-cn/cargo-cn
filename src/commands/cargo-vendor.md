# cargo-vendor(1)

## 名称

cargo-vendor --- 将所有依赖项下载到本地

## 用法概要

`cargo vendor` [_选项_] [_路径_]

## 功能介绍

该cargo子命令将下载所有crates.io和git源指定的依赖项到指定的目录`<path>`中。
待命令运行完毕之时，`<path>`指定的目录中将包含所有依赖项的远程源代码内容。
可以使用`-s`选项指定额外的`Cargo.toml`清单文件。

命令运行完毕时，stdout将输出配置信息，指导如何令分发（vendor）操作真正生效。
一般来说，您需要将输出内容添加到`.cargo/config.toml`文件中。

Cargo将经分发的源代码（vendored sources）视为只读的，就像它对registry源代码和git源代码那样。
如果您意图对远程源代码中的crate进行修改，请在`Cargo.toml`清单文件中使用`[patch]`或指向本地副本的`path`依赖项。
Cargo将正确处理该crate的增量构建过程，因为它知道它已经不是只读依赖项了。

## 选项

### 分发选项

<dl>

<dt class="option-term" id="option-cargo-vendor--s"><a class="option-anchor" href="#option-cargo-vendor--s"></a><code>-s</code> <em>manifest</em></dt>
<dt class="option-term" id="option-cargo-vendor---sync"><a class="option-anchor" href="#option-cargo-vendor---sync"></a><code>--sync</code> <em>manifest</em></dt>
<dd class="option-desc">指定一个额外的<code>Cargo.toml</code>清单文件到当前工作空间中，该文件记录的依赖项也会被分发并同步到输出目录。可以多次指定这个选项</dd>


<dt class="option-term" id="option-cargo-vendor---no-delete"><a class="option-anchor" href="#option-cargo-vendor---no-delete"></a><code>--no-delete</code></dt>
<dd class="option-desc">在分发时，不再删除"vendor"目录，即：保留那个目录中的所有已有内容</dd>


<dt class="option-term" id="option-cargo-vendor---respect-source-config"><a class="option-anchor" href="#option-cargo-vendor---respect-source-config"></a><code>--respect-source-config</code></dt>
<dd class="option-desc">默认情况下被忽略的<code>.cargo/config.toml</code>中的<code>[source]</code>此时不再被忽略。它将被用于诸如下载crates等用途</dd>


<dt class="option-term" id="option-cargo-vendor---versioned-dirs"><a class="option-anchor" href="#option-cargo-vendor---versioned-dirs"></a><code>--versioned-dirs</code></dt>
<dd class="option-desc">一般来说，版本号仅在去歧义时才会被添加到依赖项的目录名中。使用此选项将导致"vendor"目录的所有依赖项目录名中包含完整的版本号。这将有利于追踪开发时间线上被分发crate的改动。当一部分被分发的crate发生改动时，重分发的效率也会有所提高</dd>


</dl>

### 清单选项

<dl>

<dt class="option-term" id="option-cargo-vendor---manifest-path"><a class="option-anchor" href="#option-cargo-vendor---manifest-path"></a><code>--manifest-path</code> <em>path</em></dt>
<dd class="option-desc"><code>Cargo.toml</code>文件的路径。默认情况下，Cargo会在当前目录及父目录中搜索该文件</dd>


<dt class="option-term" id="option-cargo-vendor---locked"><a class="option-anchor" href="#option-cargo-vendor---locked"></a><code>--locked</code></dt>
<dd class="option-desc">确保分发时使用的依赖项及其版本均与<code>Cargo.lock</code>文件中记载的一致。当下列情景发生时，Cargo将会报错并退出：
<ul>
<li><code>Cargo.lock</code>文件丢失。</li>
<li>由于依赖项解析结果发生变化，Cargo尝试修改<code>Cargo.lock</code>文件。（原文：Cargo attempted to change the lock file due to a different dependency resolution.）</li>
</ul>
<p>该选项在需要确定性构建的场景中十分有用，例如CI管道（CI pipelines）。</dd>


<dt class="option-term" id="option-cargo-vendor---offline"><a class="option-anchor" href="#option-cargo-vendor---offline"></a><code>--offline</code></dt>
<dd class="option-desc">阻止Cargo联网。若无设定此选项，Cargo将在需要联网但网络不可用时报错退出。设定此选项后，Cargo将尝试在无网络的情况下继续运行。</p>
<p>注意：指定该选项之后，解析的依赖项可能与在线模式的不同。Cargo将限制自己仅使用本地已下载的crate，即使在线模式下可能有更新的版本也不会使用。请另行参阅<a href="cargo-fetch.html">cargo-fetch(1)</a>命令，以在离线前提前下载依赖项。</p>
<p>也可以通过指定<code>net.offline</code> <a href="../reference/config.html">配置项（config value）</a>实现同样的效果。</dd>


<dt class="option-term" id="option-cargo-vendor---frozen"><a class="option-anchor" href="#option-cargo-vendor---frozen"></a><code>--frozen</code></dt>
<dd class="option-desc">效果等同于同时设置<code>--locked</code>和<code>--offline</code>。</dd>


<dt class="option-term" id="option-cargo-vendor---lockfile-path"><a class="option-anchor" href="#option-cargo-vendor---lockfile-path"></a><code>--lockfile-path</code> <em>PATH</em></dt>
<dd class="option-desc">修改Cargo.lock文件的路径为<em>PATH</em>，不再使用默认路径(<code>&lt;workspace_root&gt;/Cargo.lock</code>)。<em>PATH</em>必须以<code>Cargo.lock</code>结尾(例如：<code>--lockfile-path /tmp/temporary-lockfile/Cargo.lock</code>)。注意，若指定了<code>--lockfile-path</code>选项，则默认路径的Cargo.lock将被忽略，转而：要么用<em>PATH</em>指定的Cargo.lock，要么根据<em>PATH</em>指定的路径生成新的Cargo.lock文件。该选项可在只读的文件中运行，并在指定的<em>PATH</em>中写入Cargo.lock。</p>
<p>该选项仅在<a href="https://doc.rust-lang.org/book/appendix-07-nightly-rust.html">nightly channel</a>中可用，且需要启用<code>-Z unstable-options</code>选项(请参阅
<a href="https://github.com/rust-lang/cargo/issues/14421">#14421</a>).</dd>


</dl>

### 显示选项

<dl>

<dt class="option-term" id="option-cargo-vendor--v"><a class="option-anchor" href="#option-cargo-vendor--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-vendor---verbose"><a class="option-anchor" href="#option-cargo-vendor---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">输出详细信息。可以指定两次，以输出超详细信息，其中包含依赖项警告和构建脚本（build script）的输出。也可以通过指定<code>term.verbose</code>
<a href="../reference/config.html">配置选项</a>达成同样效果。</dd>


<dt class="option-term" id="option-cargo-vendor--q"><a class="option-anchor" href="#option-cargo-vendor--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-vendor---quiet"><a class="option-anchor" href="#option-cargo-vendor---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">不输出Cargo日志信息。也可以通过指定<code>term.quiet</code><a href="../reference/config.html">配置选项</a>来达成同样效果。</dd>


<dt class="option-term" id="option-cargo-vendor---color"><a class="option-anchor" href="#option-cargo-vendor---color"></a><code>--color</code> <em>when</em></dt>
<dd class="option-desc">控制是否输出彩色内容。可用的选项包括：</p>
<ul>
<li><code>auto</code> (默认): 自动检测终端是否支持彩色输出。</li>
<li><code>always</code>: 总是使用彩色输出。</li>
<li><code>never</code>: 不使用彩色输出。</li>
</ul>
<p>也可以通过指定<code>term.color</code>
<a href="../reference/config.html">配置选项</a>达成同样效果。</dd>


</dl>

### 通用选项

<dl>

<dt class="option-term" id="option-cargo-vendor-+toolchain"><a class="option-anchor" href="#option-cargo-vendor-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">若Cargo是由rustup安装的，且cargo的第一个参数以+开头，Cargo将会把此参数视为rustup工具链名称（例如+stable、+nightly等）请参阅<a href="https://rust-lang.github.io/rustup/overrides.html">rustup文档</a>以理解更多关于工具链覆盖（override）的信息。</dd>


<dt class="option-term" id="option-cargo-vendor---config"><a class="option-anchor" href="#option-cargo-vendor---config"></a><code>--config</code> <em>KEY=VALUE</em> or <em>PATH</em></dt>
<dd class="option-desc">覆盖一项Cargo配置选项。该参数应为TOML的键值对<code>KEY=VALUE</code>格式，或是一个指向Cargo配置文件的路径。该选项可多次设定。请参阅<a href="../reference/config.html#command-line-overrides">命令行覆盖</a>以了解更多信息。</dd>


<dt class="option-term" id="option-cargo-vendor--C"><a class="option-anchor" href="#option-cargo-vendor--C"></a><code>-C</code> <em>PATH</em></dt>
<dd class="option-desc">在执行任何Cargo操作前，先将工作目录替换为给定路径。这将影响诸如Cargo寻找默认的清单文件(<code>Cargo.toml</code>)以及Cargo配置文件<code>.cargo/config.toml</code>等的行为。该选项必须在任何子命令之前给出，例如<code>cargo -C path/to/my-project build</code>。</p>
<p>该选项尽在<a href="https://doc.rust-lang.org/book/appendix-07-nightly-rust.html">nightly channel</a>版本工具链中可用，且需要打开<code>-Z unstable-options</code>选项 (请参阅<a href="https://github.com/rust-lang/cargo/issues/10098">#10098</a>)。</dd>


<dt class="option-term" id="option-cargo-vendor--h"><a class="option-anchor" href="#option-cargo-vendor--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-vendor---help"><a class="option-anchor" href="#option-cargo-vendor---help"></a><code>--help</code></dt>
<dd class="option-desc">打印帮助信息。</dd>


<dt class="option-term" id="option-cargo-vendor--Z"><a class="option-anchor" href="#option-cargo-vendor--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">启用Cargo不稳定功能（nightly专有）。运行<code>cargo -Z help</code>以了解详细信息。</dd>


</dl>

## 环境变量

请参阅[该参考资料](../reference/environment-variables.html)，以详细了解Cargo所读取的环境变量。

## 退出状态码

* `0`: Cargo操作成功。
* `101`: Cargo操作未能成功。

## 使用样例

1. 将全部依赖分发到本地的`vendor`目录中

       cargo vendor

2. 将全部依赖分发到本地的`third-party/vendor`目录中

       cargo vendor third-party/vendor

3. 将当前工作空间和另一个工作空间（由Cargo.toml文件指定）分发到本地的`vendor`目录中

       cargo vendor -s ../path/to/Cargo.toml

4. Vendor and redirect the necessary vendor configs to a config file.

       cargo vendor > path/to/my/cargo/config.toml

## 另请参阅
[cargo(1)](cargo.html)
