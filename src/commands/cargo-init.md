# cargo-init(1)

## NAME

cargo-init - Create a new Cargo package in an existing directory

## SYNOPSIS

`cargo init` [_options_] [_path_]

## DESCRIPTION

This command will create a new Cargo manifest in the current directory. Give a
path as an argument to create in the given directory.

If there are typically-named Rust source files already in the directory, those
will be used. If not, then a sample `src/main.rs` file will be created, or
`src/lib.rs` if `--lib` is passed.

If the directory is not already in a VCS repository, then a new repository
is created (see `--vcs` below).

See [cargo-new(1)](cargo-new.html) for a similar command which will create a new package in
a new directory.

## OPTIONS

### Init Options

<dl>

<dt class="option-term" id="option-cargo-init---bin"><a class="option-anchor" href="#option-cargo-init---bin"></a><code>--bin</code></dt>
<dd class="option-desc">Create a package with a binary target (<code>src/main.rs</code>).
This is the default behavior.</dd>


<dt class="option-term" id="option-cargo-init---lib"><a class="option-anchor" href="#option-cargo-init---lib"></a><code>--lib</code></dt>
<dd class="option-desc">Create a package with a library target (<code>src/lib.rs</code>).</dd>


<dt class="option-term" id="option-cargo-init---edition"><a class="option-anchor" href="#option-cargo-init---edition"></a><code>--edition</code> <em>edition</em></dt>
<dd class="option-desc">Specify the Rust edition to use. Default is 2021.
Possible values: 2015, 2018, 2021</dd>


<dt class="option-term" id="option-cargo-init---name"><a class="option-anchor" href="#option-cargo-init---name"></a><code>--name</code> <em>name</em></dt>
<dd class="option-desc">Set the package name. Defaults to the directory name.</dd>


<dt class="option-term" id="option-cargo-init---vcs"><a class="option-anchor" href="#option-cargo-init---vcs"></a><code>--vcs</code> <em>vcs</em></dt>
<dd class="option-desc">Initialize a new VCS repository for the given version control system (git,
hg, pijul, or fossil) or do not initialize any version control at all
(none). If not specified, defaults to <code>git</code> or the configuration value
<code>cargo-new.vcs</code>, or <code>none</code> if already inside a VCS repository.</dd>


<dt class="option-term" id="option-cargo-init---registry"><a class="option-anchor" href="#option-cargo-init---registry"></a><code>--registry</code> <em>registry</em></dt>
<dd class="option-desc">This sets the <code>publish</code> field in <code>Cargo.toml</code> to the given registry name
which will restrict publishing only to that registry.</p>
<p>Registry names are defined in <a href="../reference/config.html">Cargo config files</a>.
If not specified, the default registry defined by the <code>registry.default</code>
config key is used. If the default registry is not set and <code>--registry</code> is not
used, the <code>publish</code> field will not be set which means that publishing will not
be restricted.</dd>


</dl>


### Display Options

<dl>
<dt class="option-term" id="option-cargo-init--v"><a class="option-anchor" href="#option-cargo-init--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-init---verbose"><a class="option-anchor" href="#option-cargo-init---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">??????????????????????????????????????????????????????&quot;????????????&quot;????????????????????????????????? ???????????? ?????? ?????????????????? ???????????????????????????
????????????<code>term.verbose</code>?????????
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-init--q"><a class="option-anchor" href="#option-cargo-init--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-init---quiet"><a class="option-anchor" href="#option-cargo-init---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">?????????Cargo??????????????????????????????<code>term.quiet</code>?????????
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-init---color"><a class="option-anchor" href="#option-cargo-init---color"></a><code>--color</code> <em>when</em></dt>
<dd class="option-desc">???????????????????????????????????????????????????</p>
<ul>
<li><code>auto</code> (??????)??????????????????????????????????????????????????????</li>
<li><code>always</code>?????????????????????????????????</li>
<li><code>never</code>????????????????????????????????????</li>
</ul>
<p>????????????<code>term.color</code>?????????
<a href="../reference/config.html">config value</a>.</dd>


</dl>

### Common Options

<dl>

<dt class="option-term" id="option-cargo-init-+toolchain"><a class="option-anchor" href="#option-cargo-init-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo-init--h"><a class="option-anchor" href="#option-cargo-init--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-init---help"><a class="option-anchor" href="#option-cargo-init---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo-init--Z"><a class="option-anchor" href="#option-cargo-init--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


## ??????

??????Cargo????????????????????????????????????[the reference](../reference/environment-variables.html)


## ????????????

* `0`: Cargo??????????????????
* `101`: Cargo??????????????????.


## EXAMPLES

1. Create a binary Cargo package in the current directory:

       cargo init

## SEE ALSO
[cargo(1)](cargo.html), [cargo-new(1)](cargo-new.html)
