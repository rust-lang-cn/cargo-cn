# cargo-owner(1)

## NAME

cargo-owner - Manage the owners of a crate on the registry

## SYNOPSIS

`cargo owner` [_options_] `--add` _login_ [_crate_]\
`cargo owner` [_options_] `--remove` _login_ [_crate_]\
`cargo owner` [_options_] `--list` [_crate_]

## DESCRIPTION

This command will modify the owners for a crate on the registry. Owners of a
crate can upload new versions and yank old versions. Non-team owners can also
modify the set of owners, so take care!

This command requires you to be authenticated with either the `--token` option
or using [cargo-login(1)](cargo-login.html).

If the crate name is not specified, it will use the package name from the
current directory.

See [the reference](../reference/publishing.html#cargo-owner) for more
information about owners and publishing.

## OPTIONS

### Owner Options

<dl>

<dt class="option-term" id="option-cargo-owner--a"><a class="option-anchor" href="#option-cargo-owner--a"></a><code>-a</code></dt>
<dt class="option-term" id="option-cargo-owner---add"><a class="option-anchor" href="#option-cargo-owner---add"></a><code>--add</code> <em>login</em>...</dt>
<dd class="option-desc">Invite the given user or team as an owner.</dd>


<dt class="option-term" id="option-cargo-owner--r"><a class="option-anchor" href="#option-cargo-owner--r"></a><code>-r</code></dt>
<dt class="option-term" id="option-cargo-owner---remove"><a class="option-anchor" href="#option-cargo-owner---remove"></a><code>--remove</code> <em>login</em>...</dt>
<dd class="option-desc">Remove the given user or team as an owner.</dd>


<dt class="option-term" id="option-cargo-owner--l"><a class="option-anchor" href="#option-cargo-owner--l"></a><code>-l</code></dt>
<dt class="option-term" id="option-cargo-owner---list"><a class="option-anchor" href="#option-cargo-owner---list"></a><code>--list</code></dt>
<dd class="option-desc">List owners of a crate.</dd>


<dt class="option-term" id="option-cargo-owner---token"><a class="option-anchor" href="#option-cargo-owner---token"></a><code>--token</code> <em>token</em></dt>
<dd class="option-desc">API token to use when authenticating. This overrides the token stored in
the credentials file (which is created by <a href="cargo-login.html">cargo-login(1)</a>).</p>
<p><a href="../reference/config.html">Cargo config</a> environment variables can be
used to override the tokens stored in the credentials file. The token for
crates.io may be specified with the <code>CARGO_REGISTRY_TOKEN</code> environment
variable. Tokens for other registries may be specified with environment
variables of the form <code>CARGO_REGISTRIES_NAME_TOKEN</code> where <code>NAME</code> is the name
of the registry in all capital letters.</dd>



<dt class="option-term" id="option-cargo-owner---index"><a class="option-anchor" href="#option-cargo-owner---index"></a><code>--index</code> <em>index</em></dt>
<dd class="option-desc">The URL of the registry index to use.</dd>



<dt class="option-term" id="option-cargo-owner---registry"><a class="option-anchor" href="#option-cargo-owner---registry"></a><code>--registry</code> <em>registry</em></dt>
<dd class="option-desc">Name of the registry to use. Registry names are defined in <a href="../reference/config.html">Cargo config
files</a>. If not specified, the default registry is used,
which is defined by the <code>registry.default</code> config key which defaults to
<code>crates-io</code>.</dd>



</dl>

### Display Options

<dl>
<dt class="option-term" id="option-cargo-owner--v"><a class="option-anchor" href="#option-cargo-owner--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-owner---verbose"><a class="option-anchor" href="#option-cargo-owner---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">启用更加详细的输出。可两次使用来显示&quot;非常详细&quot;的输出，其中包含了诸如 依赖警告 以及 构建脚本输出 等额外的输出内容。
也可通过<code>term.verbose</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-owner--q"><a class="option-anchor" href="#option-cargo-owner--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-owner---quiet"><a class="option-anchor" href="#option-cargo-owner---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">不输出Cargo的日志信息。也可通过<code>term.quiet</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-owner---color"><a class="option-anchor" href="#option-cargo-owner---color"></a><code>--color</code> <em>when</em></dt>
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

<dt class="option-term" id="option-cargo-owner-+toolchain"><a class="option-anchor" href="#option-cargo-owner-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo-owner--h"><a class="option-anchor" href="#option-cargo-owner--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-owner---help"><a class="option-anchor" href="#option-cargo-owner---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo-owner--Z"><a class="option-anchor" href="#option-cargo-owner--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


## 环境

关于Cargo所读取的环境变量，可参见[the reference](../reference/environment-variables.html)


## 退出状态

* `0`: Cargo命令执行成功
* `101`: Cargo命令未能完成.


## EXAMPLES

1. List owners of a package:

       cargo owner --list foo

2. Invite an owner to a package:

       cargo owner --add username foo

3. Remove an owner from a package:

       cargo owner --remove username foo

## SEE ALSO
[cargo(1)](cargo.html), [cargo-login(1)](cargo-login.html), [cargo-publish(1)](cargo-publish.html)
