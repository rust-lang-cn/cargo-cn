# cargo-vendor(1)

## NAME

cargo-vendor - Vendor all dependencies locally

## SYNOPSIS

`cargo vendor` [_options_] [_path_]

## DESCRIPTION

This cargo subcommand will vendor all crates.io and git dependencies for a
project into the specified directory at `<path>`. After this command completes
the vendor directory specified by `<path>` will contain all remote sources from
dependencies specified. Additional manifests beyond the default one can be
specified with the `-s` option.

The `cargo vendor` command will also print out the configuration necessary
to use the vendored sources, which you will need to add to `.cargo/config.toml`.

## OPTIONS

### Vendor Options

<dl>

<dt class="option-term" id="option-cargo-vendor--s"><a class="option-anchor" href="#option-cargo-vendor--s"></a><code>-s</code> <em>manifest</em></dt>
<dt class="option-term" id="option-cargo-vendor---sync"><a class="option-anchor" href="#option-cargo-vendor---sync"></a><code>--sync</code> <em>manifest</em></dt>
<dd class="option-desc">Specify extra <code>Cargo.toml</code> manifests to workspaces which should also be
vendored and synced to the output.</dd>


<dt class="option-term" id="option-cargo-vendor---no-delete"><a class="option-anchor" href="#option-cargo-vendor---no-delete"></a><code>--no-delete</code></dt>
<dd class="option-desc">Don't delete the &quot;vendor&quot; directory when vendoring, but rather keep all
existing contents of the vendor directory</dd>


<dt class="option-term" id="option-cargo-vendor---respect-source-config"><a class="option-anchor" href="#option-cargo-vendor---respect-source-config"></a><code>--respect-source-config</code></dt>
<dd class="option-desc">Instead of ignoring <code>[source]</code> configuration by default in <code>.cargo/config.toml</code>
read it and use it when downloading crates from crates.io, for example</dd>


<dt class="option-term" id="option-cargo-vendor---versioned-dirs"><a class="option-anchor" href="#option-cargo-vendor---versioned-dirs"></a><code>--versioned-dirs</code></dt>
<dd class="option-desc">Normally versions are only added to disambiguate multiple versions of the
same package. This option causes all directories in the &quot;vendor&quot; directory
to be versioned, which makes it easier to track the history of vendored
packages over time, and can help with the performance of re-vendoring when
only a subset of the packages have changed.</dd>


</dl>

### Manifest Options

<dl>

<dt class="option-term" id="option-cargo-vendor---manifest-path"><a class="option-anchor" href="#option-cargo-vendor---manifest-path"></a><code>--manifest-path</code> <em>path</em></dt>
<dd class="option-desc">用于指定<code>Cargo.toml</code>文件的路径。默认情况下，Cargo会在当前目录或上级目录中寻找<code>Cargo.toml</code>文件。</dd>



<dt class="option-term" id="option-cargo-vendor---frozen"><a class="option-anchor" href="#option-cargo-vendor---frozen"></a><code>--frozen</code></dt>
<dt class="option-term" id="option-cargo-vendor---locked"><a class="option-anchor" href="#option-cargo-vendor---locked"></a><code>--locked</code></dt>
<dd class="option-desc">这两个选项用于保证<code>Cargo.lock</code>文件是最新的。如果该锁文件不存在，或者不是最新的，Cargo
会报错退出。其中<code>--frozen</code>选项会阻止Cargo访问网络以检查锁文件是否是最新的。</p>
<p>这些选项，可用于保证<code>Cargo.lock</code>文件是最新的(比如持续集成的构建过程)，
或用于避免联网。</dd>


<dt class="option-term" id="option-cargo-vendor---offline"><a class="option-anchor" href="#option-cargo-vendor---offline"></a><code>--offline</code></dt>
<dd class="option-desc">禁止Cargo访问网络。如果不添加此选项，Cargo在需要访问网络但网络不可用的情况下，会报错
并停止工作。添加此选项后，Cargo会尽可能尝试不使用网络来工作。</p>
<p>注意，在此情况下可能会产生与联网状态下不同的依赖解析(<strong>Dependency Resolution</strong>)结果。
Cargo只会使用本地已下载的crate，即便本地的索引副本中表明可能有新版本crate。在离线前下载
所需依赖的方法，参见 <a href="cargo-fetch.html">cargo-fetch(1)</a> 。</p>
<p>也可以通过 <code>net.offline</code> <a href="../reference/config.html">config value</a>指定。</dd>



</dl>

### Display Options

<dl>

<dt class="option-term" id="option-cargo-vendor--v"><a class="option-anchor" href="#option-cargo-vendor--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-vendor---verbose"><a class="option-anchor" href="#option-cargo-vendor---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">启用更加详细的输出。可两次使用来显示&quot;非常详细&quot;的输出，其中包含了诸如 依赖警告 以及 构建脚本输出 等额外的输出内容。
也可通过<code>term.verbose</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-vendor--q"><a class="option-anchor" href="#option-cargo-vendor--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-vendor---quiet"><a class="option-anchor" href="#option-cargo-vendor---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">不输出Cargo的日志信息。也可通过<code>term.quiet</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-vendor---color"><a class="option-anchor" href="#option-cargo-vendor---color"></a><code>--color</code> <em>when</em></dt>
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

<dt class="option-term" id="option-cargo-vendor-+toolchain"><a class="option-anchor" href="#option-cargo-vendor-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo-vendor--h"><a class="option-anchor" href="#option-cargo-vendor--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-vendor---help"><a class="option-anchor" href="#option-cargo-vendor---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo-vendor--Z"><a class="option-anchor" href="#option-cargo-vendor--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


## 环境

关于Cargo所读取的环境变量，可参见[the reference](../reference/environment-variables.html)


## 退出状态

* `0`: Cargo命令执行成功
* `101`: Cargo命令未能完成.


## EXAMPLES

1. Vendor all dependencies into a local "vendor" folder

       cargo vendor

2. Vendor all dependencies into a local "third-party/vendor" folder

       cargo vendor third-party/vendor

3. Vendor the current workspace as well as another to "vendor"

       cargo vendor -s ../path/to/Cargo.toml

## SEE ALSO
[cargo(1)](cargo.html)

