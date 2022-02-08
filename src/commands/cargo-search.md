# cargo-search(1)

## NAME

cargo-search - Search packages in crates.io

## SYNOPSIS

`cargo search` [_options_] [_query_...]

## DESCRIPTION

This performs a textual search for crates on <https://crates.io>. The matching
crates will be displayed along with their description in TOML format suitable
for copying into a `Cargo.toml` manifest.

## OPTIONS

### Search Options

<dl>

<dt class="option-term" id="option-cargo-search---limit"><a class="option-anchor" href="#option-cargo-search---limit"></a><code>--limit</code> <em>limit</em></dt>
<dd class="option-desc">Limit the number of results (default: 10, max: 100).</dd>


<dt class="option-term" id="option-cargo-search---index"><a class="option-anchor" href="#option-cargo-search---index"></a><code>--index</code> <em>index</em></dt>
<dd class="option-desc">The URL of the registry index to use.</dd>



<dt class="option-term" id="option-cargo-search---registry"><a class="option-anchor" href="#option-cargo-search---registry"></a><code>--registry</code> <em>registry</em></dt>
<dd class="option-desc">Name of the registry to use. Registry names are defined in <a href="../reference/config.html">Cargo config
files</a>. If not specified, the default registry is used,
which is defined by the <code>registry.default</code> config key which defaults to
<code>crates-io</code>.</dd>



</dl>

### Display Options

<dl>
<dt class="option-term" id="option-cargo-search--v"><a class="option-anchor" href="#option-cargo-search--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-search---verbose"><a class="option-anchor" href="#option-cargo-search---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">启用更加详细的输出。可两次使用来显示&quot;非常详细&quot;的输出，其中包含了诸如 依赖警告 以及 构建脚本输出 等额外的输出内容。
也可通过<code>term.verbose</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-search--q"><a class="option-anchor" href="#option-cargo-search--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-search---quiet"><a class="option-anchor" href="#option-cargo-search---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">不输出Cargo的日志信息。也可通过<code>term.quiet</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-search---color"><a class="option-anchor" href="#option-cargo-search---color"></a><code>--color</code> <em>when</em></dt>
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

<dt class="option-term" id="option-cargo-search-+toolchain"><a class="option-anchor" href="#option-cargo-search-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo-search--h"><a class="option-anchor" href="#option-cargo-search--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-search---help"><a class="option-anchor" href="#option-cargo-search---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo-search--Z"><a class="option-anchor" href="#option-cargo-search--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


## 环境

关于Cargo所读取的环境变量，可参见[the reference](../reference/environment-variables.html)


## 退出状态

* `0`: Cargo命令执行成功
* `101`: Cargo命令未能完成.


## EXAMPLES

1. Search for a package from crates.io:

       cargo search serde

## SEE ALSO
[cargo(1)](cargo.html), [cargo-install(1)](cargo-install.html), [cargo-publish(1)](cargo-publish.html)
