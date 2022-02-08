# cargo(1)

## NAME

cargo - The Rust package manager

## SYNOPSIS

`cargo` [_options_] _command_ [_args_]\
`cargo` [_options_] `--version`\
`cargo` [_options_] `--list`\
`cargo` [_options_] `--help`\
`cargo` [_options_] `--explain` _code_

## DESCRIPTION

This program is a package manager and build tool for the Rust language,
available at <https://rust-lang.org>.

## COMMANDS

### Build Commands

[cargo-bench(1)](cargo-bench.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Execute benchmarks of a package.

[cargo-build(1)](cargo-build.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Compile a package.

[cargo-check(1)](cargo-check.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Check a local package and all of its dependencies for errors.

[cargo-clean(1)](cargo-clean.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Remove artifacts that Cargo has generated in the past.

[cargo-doc(1)](cargo-doc.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Build a package's documentation.

[cargo-fetch(1)](cargo-fetch.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Fetch dependencies of a package from the network.

[cargo-fix(1)](cargo-fix.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Automatically fix lint warnings reported by rustc.

[cargo-run(1)](cargo-run.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Run a binary or example of the local package.

[cargo-rustc(1)](cargo-rustc.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Compile a package, and pass extra options to the compiler.

[cargo-rustdoc(1)](cargo-rustdoc.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Build a package's documentation, using specified custom flags.

[cargo-test(1)](cargo-test.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Execute unit and integration tests of a package.

### Manifest Commands

[cargo-generate-lockfile(1)](cargo-generate-lockfile.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Generate `Cargo.lock` for a project.

[cargo-locate-project(1)](cargo-locate-project.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Print a JSON representation of a `Cargo.toml` file's location.

[cargo-metadata(1)](cargo-metadata.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Output the resolved dependencies of a package in machine-readable format.

[cargo-pkgid(1)](cargo-pkgid.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Print a fully qualified package specification.

[cargo-tree(1)](cargo-tree.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Display a tree visualization of a dependency graph.

[cargo-update(1)](cargo-update.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Update dependencies as recorded in the local lock file.

[cargo-vendor(1)](cargo-vendor.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Vendor all dependencies locally.

[cargo-verify-project(1)](cargo-verify-project.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Check correctness of crate manifest.

### Package Commands

[cargo-init(1)](cargo-init.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Create a new Cargo package in an existing directory.

[cargo-install(1)](cargo-install.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Build and install a Rust binary.

[cargo-new(1)](cargo-new.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Create a new Cargo package.

[cargo-search(1)](cargo-search.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Search packages in crates.io.

[cargo-uninstall(1)](cargo-uninstall.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Remove a Rust binary.

### Publishing Commands

[cargo-login(1)](cargo-login.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Save an API token from the registry locally.

[cargo-owner(1)](cargo-owner.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Manage the owners of a crate on the registry.

[cargo-package(1)](cargo-package.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Assemble the local package into a distributable tarball.

[cargo-publish(1)](cargo-publish.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Upload a package to the registry.

[cargo-yank(1)](cargo-yank.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Remove a pushed crate from the index.

### General Commands

[cargo-help(1)](cargo-help.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Display help information about Cargo.

[cargo-version(1)](cargo-version.html)\
&nbsp;&nbsp;&nbsp;&nbsp;Show version information.

## OPTIONS

### Special Options

<dl>

<dt class="option-term" id="option-cargo--V"><a class="option-anchor" href="#option-cargo--V"></a><code>-V</code></dt>
<dt class="option-term" id="option-cargo---version"><a class="option-anchor" href="#option-cargo---version"></a><code>--version</code></dt>
<dd class="option-desc">Print version info and exit. If used with <code>--verbose</code>, prints extra
information.</dd>


<dt class="option-term" id="option-cargo---list"><a class="option-anchor" href="#option-cargo---list"></a><code>--list</code></dt>
<dd class="option-desc">List all installed Cargo subcommands. If used with <code>--verbose</code>, prints extra
information.</dd>


<dt class="option-term" id="option-cargo---explain"><a class="option-anchor" href="#option-cargo---explain"></a><code>--explain</code> <em>code</em></dt>
<dd class="option-desc">Run <code>rustc --explain CODE</code> which will print out a detailed explanation of an
error message (for example, <code>E0004</code>).</dd>


</dl>

### Display Options

<dl>

<dt class="option-term" id="option-cargo--v"><a class="option-anchor" href="#option-cargo--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo---verbose"><a class="option-anchor" href="#option-cargo---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">启用更加详细的输出。可两次使用来显示&quot;非常详细&quot;的输出，其中包含了诸如 依赖警告 以及 构建脚本输出 等额外的输出内容。
也可通过<code>term.verbose</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo--q"><a class="option-anchor" href="#option-cargo--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo---quiet"><a class="option-anchor" href="#option-cargo---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">不输出Cargo的日志信息。也可通过<code>term.quiet</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo---color"><a class="option-anchor" href="#option-cargo---color"></a><code>--color</code> <em>when</em></dt>
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
<dt class="option-term" id="option-cargo---frozen"><a class="option-anchor" href="#option-cargo---frozen"></a><code>--frozen</code></dt>
<dt class="option-term" id="option-cargo---locked"><a class="option-anchor" href="#option-cargo---locked"></a><code>--locked</code></dt>
<dd class="option-desc">这两个选项用于保证<code>Cargo.lock</code>文件是最新的。如果该锁文件不存在，或者不是最新的，Cargo
会报错退出。其中<code>--frozen</code>选项会阻止Cargo访问网络以检查锁文件是否是最新的。</p>
<p>这些选项，可用于保证<code>Cargo.lock</code>文件是最新的(比如持续集成的构建过程)，
或用于避免联网。</dd>


<dt class="option-term" id="option-cargo---offline"><a class="option-anchor" href="#option-cargo---offline"></a><code>--offline</code></dt>
<dd class="option-desc">禁止Cargo访问网络。如果不添加此选项，Cargo在需要访问网络但网络不可用的情况下，会报错
并停止工作。添加此选项后，Cargo会尽可能尝试不使用网络来工作。</p>
<p>注意，在此情况下可能会产生与联网状态下不同的依赖解析(<strong>Dependency Resolution</strong>)结果。
Cargo只会使用本地已下载的crate，即便本地的索引副本中表明可能有新版本crate。在离线前下载
所需依赖的方法，参见 <a href="cargo-fetch.html">cargo-fetch(1)</a> 。</p>
<p>也可以通过 <code>net.offline</code> <a href="../reference/config.html">config value</a>指定。</dd>


</dl>

### Common Options

<dl>

<dt class="option-term" id="option-cargo-+toolchain"><a class="option-anchor" href="#option-cargo-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo--h"><a class="option-anchor" href="#option-cargo--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo---help"><a class="option-anchor" href="#option-cargo---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo--Z"><a class="option-anchor" href="#option-cargo--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


## 环境

关于Cargo所读取的环境变量，可参见[the reference](../reference/environment-variables.html)


## 退出状态

* `0`: Cargo命令执行成功
* `101`: Cargo命令未能完成.


## FILES

`~/.cargo/`\
&nbsp;&nbsp;&nbsp;&nbsp;Default location for Cargo's "home" directory where it
stores various files. The location can be changed with the `CARGO_HOME`
environment variable.

`$CARGO_HOME/bin/`\
&nbsp;&nbsp;&nbsp;&nbsp;Binaries installed by [cargo-install(1)](cargo-install.html) will be located here. If using
[rustup], executables distributed with Rust are also located here.

`$CARGO_HOME/config.toml`\
&nbsp;&nbsp;&nbsp;&nbsp;The global configuration file. See [the reference](../reference/config.html)
for more information about configuration files.

`.cargo/config.toml`\
&nbsp;&nbsp;&nbsp;&nbsp;Cargo automatically searches for a file named `.cargo/config.toml` in the
current directory, and all parent directories. These configuration files
will be merged with the global configuration file.

`$CARGO_HOME/credentials.toml`\
&nbsp;&nbsp;&nbsp;&nbsp;Private authentication information for logging in to a registry.

`$CARGO_HOME/registry/`\
&nbsp;&nbsp;&nbsp;&nbsp;This directory contains cached downloads of the registry index and any
downloaded dependencies.

`$CARGO_HOME/git/`\
&nbsp;&nbsp;&nbsp;&nbsp;This directory contains cached downloads of git dependencies.

Please note that the internal structure of the `$CARGO_HOME` directory is not
stable yet and may be subject to change.

[rustup]: https://rust-lang.github.io/rustup/

## EXAMPLES

1. Build a local package and all of its dependencies:

       cargo build

2. Build a package with optimizations:

       cargo build --release

3. Run tests for a cross-compiled target:

       cargo test --target i686-unknown-linux-gnu

4. Create a new package that builds an executable:

       cargo new foobar

5. Create a package in the current directory:

       mkdir foo && cd foo
       cargo init .

6. Learn about a command's options and usage:

       cargo help clean

## BUGS

See <https://github.com/rust-lang/cargo/issues> for issues.

## SEE ALSO
[rustc(1)](https://doc.rust-lang.org/rustc/index.html), [rustdoc(1)](https://doc.rust-lang.org/rustdoc/index.html)
