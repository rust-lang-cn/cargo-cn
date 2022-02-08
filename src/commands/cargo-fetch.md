# cargo-fetch(1)


## NAME

cargo-fetch - Fetch dependencies of a package from the network

## SYNOPSIS

`cargo fetch` [_options_]

## DESCRIPTION

If a `Cargo.lock` file is available, this command will ensure that all of the
git dependencies and/or registry dependencies are downloaded and locally
available. Subsequent Cargo commands never touch the network after a `cargo
fetch` unless the lock file changes.

If the lock file is not available, then this command will generate the lock
file before fetching the dependencies.

If `--target` is not specified, then all target dependencies are fetched.

See also the [cargo-prefetch](https://crates.io/crates/cargo-prefetch)
plugin which adds a command to download popular crates. This may be useful if
you plan to use Cargo without a network with the `--offline` flag.

## OPTIONS

### Fetch options

<dl>
<dt class="option-term" id="option-cargo-fetch---target"><a class="option-anchor" href="#option-cargo-fetch---target"></a><code>--target</code> <em>triple</em></dt>
<dd class="option-desc">为指定架构执行 Fetch 。默认情况下为本机的架构。三元组的格式为
<code>&lt;arch&gt;&lt;sub&gt;-&lt;vendor&gt;-&lt;sys&gt;-&lt;abi&gt;</code>。执行 <code>rustc --print target-list</code>
可得到支持的构建目标列表。</p>
<p>也可通过<code>build.target</code>指定(<a href="../reference/config.html">config value</a>)。</p>
<p>注意，指定该标志参数会使Cargo产生的构建工件放在与平常不同的目录下。
详情参见<a href="../guide/build-cache.html">build cache</a></dd>


</dl>

### Display Options

<dl>
<dt class="option-term" id="option-cargo-fetch--v"><a class="option-anchor" href="#option-cargo-fetch--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-fetch---verbose"><a class="option-anchor" href="#option-cargo-fetch---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">启用更加详细的输出。可两次使用来显示&quot;非常详细&quot;的输出，其中包含了诸如 依赖警告 以及 构建脚本输出 等额外的输出内容。
也可通过<code>term.verbose</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-fetch--q"><a class="option-anchor" href="#option-cargo-fetch--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-fetch---quiet"><a class="option-anchor" href="#option-cargo-fetch---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">不输出Cargo的日志信息。也可通过<code>term.quiet</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-fetch---color"><a class="option-anchor" href="#option-cargo-fetch---color"></a><code>--color</code> <em>when</em></dt>
<dd class="option-desc">控制输出内容的颜色。有效取值如下：</p>
<ul>
<li><code>auto</code> (默认)：自动检测终端是否支持带颜色的输出。</li>
<li><code>always</code>：总显示带颜色的输出。</li>
<li><code>never</code>：从不显示带颜色的输出。</li>
</ul>
<p>也可通过<code>term.color</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


</dl>

### Manifest Options

<dl>
<dt class="option-term" id="option-cargo-fetch---manifest-path"><a class="option-anchor" href="#option-cargo-fetch---manifest-path"></a><code>--manifest-path</code> <em>path</em></dt>
<dd class="option-desc">用于指定<code>Cargo.toml</code>文件的路径。默认情况下，Cargo会在当前目录或上级目录中寻找<code>Cargo.toml</code>文件。</dd>



<dt class="option-term" id="option-cargo-fetch---frozen"><a class="option-anchor" href="#option-cargo-fetch---frozen"></a><code>--frozen</code></dt>
<dt class="option-term" id="option-cargo-fetch---locked"><a class="option-anchor" href="#option-cargo-fetch---locked"></a><code>--locked</code></dt>
<dd class="option-desc">这两个选项用于保证<code>Cargo.lock</code>文件是最新的。如果该锁文件不存在，或者不是最新的，Cargo
会报错退出。其中<code>--frozen</code>选项会阻止Cargo访问网络以检查锁文件是否是最新的。</p>
<p>这些选项，可用于保证<code>Cargo.lock</code>文件是最新的(比如持续集成的构建过程)，
或用于避免联网。</dd>


<dt class="option-term" id="option-cargo-fetch---offline"><a class="option-anchor" href="#option-cargo-fetch---offline"></a><code>--offline</code></dt>
<dd class="option-desc">禁止Cargo访问网络。如果不添加此选项，Cargo在需要访问网络但网络不可用的情况下，会报错
并停止工作。添加此选项后，Cargo会尽可能尝试不使用网络来工作。</p>
<p>注意，在此情况下可能会产生与联网状态下不同的依赖解析(<strong>Dependency Resolution</strong>)结果。
Cargo只会使用本地已下载的crate，即便本地的索引副本中表明可能有新版本crate。在离线前下载
所需依赖的方法，参见 <a href="cargo-fetch.html">cargo-fetch(1)</a> 。</p>
<p>也可以通过 <code>net.offline</code> <a href="../reference/config.html">config value</a>指定。</dd>


</dl>

### Common Options

<dl>

<dt class="option-term" id="option-cargo-fetch-+toolchain"><a class="option-anchor" href="#option-cargo-fetch-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo-fetch--h"><a class="option-anchor" href="#option-cargo-fetch--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-fetch---help"><a class="option-anchor" href="#option-cargo-fetch---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo-fetch--Z"><a class="option-anchor" href="#option-cargo-fetch--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


## 环境

关于Cargo所读取的环境变量，可参见[the reference](../reference/environment-variables.html)


## 退出状态

* `0`: Cargo命令执行成功
* `101`: Cargo命令未能完成.


## EXAMPLES

1. Fetch all dependencies:

       cargo fetch

## SEE ALSO
[cargo(1)](cargo.html), [cargo-update(1)](cargo-update.html), [cargo-generate-lockfile(1)](cargo-generate-lockfile.html)
