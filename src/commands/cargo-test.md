# cargo-test(1)



## NAME

cargo-test - Execute unit and integration tests of a package

## SYNOPSIS

`cargo test` [_options_] [_testname_] [`--` _test-options_]

## DESCRIPTION

Compile and execute unit and integration tests.

The test filtering argument `TESTNAME` and all the arguments following the two
dashes (`--`) are passed to the test binaries and thus to _libtest_ (rustc's
built in unit-test and micro-benchmarking framework).  If you're passing
arguments to both Cargo and the binary, the ones after `--` go to the binary,
the ones before go to Cargo.  For details about libtest's arguments see the
output of `cargo test -- --help` and check out the rustc book's chapter on
how tests work at <https://doc.rust-lang.org/rustc/tests/index.html>.

As an example, this will filter for tests with `foo` in their name and run them
on 3 threads in parallel:

    cargo test foo -- --test-threads 3

Tests are built with the `--test` option to `rustc` which creates an
executable with a `main` function that automatically runs all functions
annotated with the `#[test]` attribute in multiple threads. `#[bench]`
annotated functions will also be run with one iteration to verify that they
are functional.

The libtest harness may be disabled by setting `harness = false` in the target
manifest settings, in which case your code will need to provide its own `main`
function to handle running tests.

Documentation tests are also run by default, which is handled by `rustdoc`. It
extracts code samples from documentation comments and executes them. See the
[rustdoc book](https://doc.rust-lang.org/rustdoc/) for more information on
writing doc tests.

## OPTIONS

### Test Options

<dl>

<dt class="option-term" id="option-cargo-test---no-run"><a class="option-anchor" href="#option-cargo-test---no-run"></a><code>--no-run</code></dt>
<dd class="option-desc">Compile, but don't run tests.</dd>


<dt class="option-term" id="option-cargo-test---no-fail-fast"><a class="option-anchor" href="#option-cargo-test---no-fail-fast"></a><code>--no-fail-fast</code></dt>
<dd class="option-desc">Run all tests regardless of failure. Without this flag, Cargo will exit
after the first executable fails. The Rust test harness will run all tests
within the executable to completion, this flag only applies to the executable
as a whole.</dd>


</dl>


### 选择包

默认情况下，如果没有指定包，则根据清单文件来选择包(如果没有通过`--manifest-path`给出清单文件路径，
则基于当前工作目录进行寻找)。如果是某个工作区的根清单，则选中该工作区的默认成员；否则仅选中清
单所定义的那个包。

工作区的默认成员可通过清单中`workspace.default-members`项来显式指定。如果未指定，则其虚拟
工作区会包含全体工作区成员(等同于传递`--workspace`标志参数时)，而非虚拟工作区则仅包含根部箱自身。

<dl>

<dt class="option-term" id="option-cargo-test--p"><a class="option-anchor" href="#option-cargo-test--p"></a><code>-p</code> <em>spec</em>...</dt>
<dt class="option-term" id="option-cargo-test---package"><a class="option-anchor" href="#option-cargo-test---package"></a><code>--package</code> <em>spec</em>...</dt>
<dd class="option-desc">Test指定包. SPEC的格式参见 <a href="cargo-pkgid.html">cargo-pkgid(1)</a> 。
此标志参数可多次使用且支持Unix通配符(<code>*</code>, <code>?</code> 和 <code>[]</code>)。不过，为避免shell可能错误地在Cargo获取到
之前就将通配符展开，应在各个模式串两侧使用单引号或双引号。</dd>


<dt class="option-term" id="option-cargo-test---workspace"><a class="option-anchor" href="#option-cargo-test---workspace"></a><code>--workspace</code></dt>
<dd class="option-desc">Test 工作区中的全体成员.</dd>



<dt class="option-term" id="option-cargo-test---all"><a class="option-anchor" href="#option-cargo-test---all"></a><code>--all</code></dt>
<dd class="option-desc"><code>--workspace</code>的已废弃的别名。</dd>



<dt class="option-term" id="option-cargo-test---exclude"><a class="option-anchor" href="#option-cargo-test---exclude"></a><code>--exclude</code> <em>SPEC</em>...</dt>
<dd class="option-desc">排除指定包。必须与<code>--workspace</code>标志参数共同使用。
此标志参数可多次使用且支持Unix通配符(<code>*</code>, <code>?</code> 和 <code>[]</code>)。不过，为避免shell可能错误地在Cargo获取到
之前就将通配符展开，应在各个模式串两侧使用单引号或双引号。</dd>


</dl>


### Target Selection

When no target selection options are given, `cargo test` will build the
following targets of the selected packages:

- lib — used to link with binaries, examples, integration tests, and doc tests
- bins (only if integration tests are built and required features are
  available)
- examples — to ensure they compile
- lib as a unit test
- bins as unit tests
- integration tests
- doc tests for the lib target

The default behavior can be changed by setting the `test` flag for the target
in the manifest settings. Setting examples to `test = true` will build and run
the example as a test. Setting targets to `test = false` will stop them from
being tested by default. Target selection options that take a target by name
ignore the `test` flag and will always test the given target.

Doc tests for libraries may be disabled by setting `doctest = false` for the
library in the manifest.

Binary targets are automatically built if there is an integration test or
benchmark. This allows an integration test to execute the binary to exercise
and test its behavior. The `CARGO_BIN_EXE_<name>`
[environment variable](../reference/environment-variables.html#environment-variables-cargo-sets-for-crates)
is set when the integration test is built so that it can use the
[`env` macro](https://doc.rust-lang.org/std/macro.env.html) to locate the
executable.

Passing target selection flags will test only the specified
targets. 

Note that `--bin`, `--example`, `--test` and `--bench` flags also 
support common Unix glob patterns like `*`, `?` and `[]`. However, to avoid your 
shell accidentally expanding glob patterns before Cargo handles them, you must 
use single quotes or double quotes around each glob pattern.

<dl>

<dt class="option-term" id="option-cargo-test---lib"><a class="option-anchor" href="#option-cargo-test---lib"></a><code>--lib</code></dt>
<dd class="option-desc">Test the package's library.</dd>


<dt class="option-term" id="option-cargo-test---bin"><a class="option-anchor" href="#option-cargo-test---bin"></a><code>--bin</code> <em>name</em>...</dt>
<dd class="option-desc">Test the specified binary. This flag may be specified multiple times
and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-test---bins"><a class="option-anchor" href="#option-cargo-test---bins"></a><code>--bins</code></dt>
<dd class="option-desc">Test all binary targets.</dd>



<dt class="option-term" id="option-cargo-test---example"><a class="option-anchor" href="#option-cargo-test---example"></a><code>--example</code> <em>name</em>...</dt>
<dd class="option-desc">Test the specified example. This flag may be specified multiple times
and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-test---examples"><a class="option-anchor" href="#option-cargo-test---examples"></a><code>--examples</code></dt>
<dd class="option-desc">Test all example targets.</dd>


<dt class="option-term" id="option-cargo-test---test"><a class="option-anchor" href="#option-cargo-test---test"></a><code>--test</code> <em>name</em>...</dt>
<dd class="option-desc">Test the specified integration test. This flag may be specified
multiple times and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-test---tests"><a class="option-anchor" href="#option-cargo-test---tests"></a><code>--tests</code></dt>
<dd class="option-desc">Test all targets in test mode that have the <code>test = true</code> manifest
flag set. By default this includes the library and binaries built as
unittests, and integration tests. Be aware that this will also build any
required dependencies, so the lib target may be built twice (once as a
unittest, and once as a dependency for binaries, integration tests, etc.).
Targets may be enabled or disabled by setting the <code>test</code> flag in the
manifest settings for the target.</dd>


<dt class="option-term" id="option-cargo-test---bench"><a class="option-anchor" href="#option-cargo-test---bench"></a><code>--bench</code> <em>name</em>...</dt>
<dd class="option-desc">Test the specified benchmark. This flag may be specified multiple
times and supports common Unix glob patterns.</dd>


<dt class="option-term" id="option-cargo-test---benches"><a class="option-anchor" href="#option-cargo-test---benches"></a><code>--benches</code></dt>
<dd class="option-desc">Test all targets in benchmark mode that have the <code>bench = true</code>
manifest flag set. By default this includes the library and binaries built
as benchmarks, and bench targets. Be aware that this will also build any
required dependencies, so the lib target may be built twice (once as a
benchmark, and once as a dependency for binaries, benchmarks, etc.).
Targets may be enabled or disabled by setting the <code>bench</code> flag in the
manifest settings for the target.</dd>


<dt class="option-term" id="option-cargo-test---all-targets"><a class="option-anchor" href="#option-cargo-test---all-targets"></a><code>--all-targets</code></dt>
<dd class="option-desc">Test all targets. This is equivalent to specifying <code>--lib --bins --tests --benches --examples</code>.</dd>


</dl>


<dl>

<dt class="option-term" id="option-cargo-test---doc"><a class="option-anchor" href="#option-cargo-test---doc"></a><code>--doc</code></dt>
<dd class="option-desc">Test only the library's documentation. This cannot be mixed with other
target options.</dd>


</dl>

### 特性选择

可通过传递特性参数来控制启用哪些特性。如果没有给定要使用的特性，
则每个已选择的包都会自动使用`default`特性。

详见[the features documentation](../reference/features.html#command-line-feature-options)。

<dl>

<dt class="option-term" id="option-cargo-test---features"><a class="option-anchor" href="#option-cargo-test---features"></a><code>--features</code> <em>features</em></dt>
<dd class="option-desc">传递以空格或者逗号分隔的列表，其中给出要启用的特性。工作区成员的特性可通过<code>包名/特性名</code>的语法启用。
此参数可多次给定，以分别启用给定的特性。</dd>


<dt class="option-term" id="option-cargo-test---all-features"><a class="option-anchor" href="#option-cargo-test---all-features"></a><code>--all-features</code></dt>
<dd class="option-desc">为给定的包启用全部可用特性</dd>


<dt class="option-term" id="option-cargo-test---no-default-features"><a class="option-anchor" href="#option-cargo-test---no-default-features"></a><code>--no-default-features</code></dt>
<dd class="option-desc">不启用给定包的<code>default</code>特性</dd>


</dl>


### Compilation Options

<dl>

<dt class="option-term" id="option-cargo-test---target"><a class="option-anchor" href="#option-cargo-test---target"></a><code>--target</code> <em>triple</em></dt>
<dd class="option-desc">为指定架构执行 Test 。默认情况下为本机的架构。三元组的格式为
<code>&lt;arch&gt;&lt;sub&gt;-&lt;vendor&gt;-&lt;sys&gt;-&lt;abi&gt;</code>。执行 <code>rustc --print target-list</code>
可得到支持的构建目标列表。</p>
<p>也可通过<code>build.target</code>指定(<a href="../reference/config.html">config value</a>)。</p>
<p>注意，指定该标志参数会使Cargo产生的构建工件放在与平常不同的目录下。
详情参见<a href="../guide/build-cache.html">build cache</a></dd>



<dt class="option-term" id="option-cargo-test--r"><a class="option-anchor" href="#option-cargo-test--r"></a><code>-r</code></dt>
<dt class="option-term" id="option-cargo-test---release"><a class="option-anchor" href="#option-cargo-test---release"></a><code>--release</code></dt>
<dd class="option-desc">Test optimized artifacts with the <code>release</code> profile.
See also the <code>--profile</code> option for choosing a specific profile by name.</dd>



<dt class="option-term" id="option-cargo-test---profile"><a class="option-anchor" href="#option-cargo-test---profile"></a><code>--profile</code> <em>name</em></dt>
<dd class="option-desc">Test with the given profile.
See the <a href="../reference/profiles.html">the reference</a> for more details on profiles.</dd>



<dt class="option-term" id="option-cargo-test---ignore-rust-version"><a class="option-anchor" href="#option-cargo-test---ignore-rust-version"></a><code>--ignore-rust-version</code></dt>
<dd class="option-desc">Test the target even if the selected Rust compiler is older than the
required Rust version as configured in the project's <code>rust-version</code> field.</dd>



</dl>

### Output Options

<dl>
<dt class="option-term" id="option-cargo-test---target-dir"><a class="option-anchor" href="#option-cargo-test---target-dir"></a><code>--target-dir</code> <em>directory</em></dt>
<dd class="option-desc">用于存放生成的工件以及中间文件的目录。也可通过环境变量<code>CARGO_TARGET_DIR</code> 或 
<code>build.target-dir</code> <a href="../reference/config.html">config value</a>指定。</p>
<p>默认情况下为根工作区中的<code>target</code>目录。</dd>


</dl>

### Display Options

By default the Rust test harness hides output from test execution to keep
results readable. Test output can be recovered (e.g., for debugging) by passing
`--nocapture` to the test binaries:

    cargo test -- --nocapture

<dl>

<dt class="option-term" id="option-cargo-test--v"><a class="option-anchor" href="#option-cargo-test--v"></a><code>-v</code></dt>
<dt class="option-term" id="option-cargo-test---verbose"><a class="option-anchor" href="#option-cargo-test---verbose"></a><code>--verbose</code></dt>
<dd class="option-desc">启用更加详细的输出。可两次使用来显示&quot;非常详细&quot;的输出，其中包含了诸如 依赖警告 以及 构建脚本输出 等额外的输出内容。
也可通过<code>term.verbose</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-test--q"><a class="option-anchor" href="#option-cargo-test--q"></a><code>-q</code></dt>
<dt class="option-term" id="option-cargo-test---quiet"><a class="option-anchor" href="#option-cargo-test---quiet"></a><code>--quiet</code></dt>
<dd class="option-desc">不输出Cargo的日志信息。也可通过<code>term.quiet</code>指定。
<a href="../reference/config.html">config value</a>.</dd>


<dt class="option-term" id="option-cargo-test---color"><a class="option-anchor" href="#option-cargo-test---color"></a><code>--color</code> <em>when</em></dt>
<dd class="option-desc">控制输出内容的颜色。有效取值如下：</p>
<ul>
<li><code>auto</code> (默认)：自动检测终端是否支持带颜色的输出。</li>
<li><code>always</code>：总显示带颜色的输出。</li>
<li><code>never</code>：从不显示带颜色的输出。</li>
</ul>
<p>也可通过<code>term.color</code>指定。
<a href="../reference/config.html">config value</a>.</dd>



<dt class="option-term" id="option-cargo-test---message-format"><a class="option-anchor" href="#option-cargo-test---message-format"></a><code>--message-format</code> <em>fmt</em></dt>
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

<dt class="option-term" id="option-cargo-test---manifest-path"><a class="option-anchor" href="#option-cargo-test---manifest-path"></a><code>--manifest-path</code> <em>path</em></dt>
<dd class="option-desc">用于指定<code>Cargo.toml</code>文件的路径。默认情况下，Cargo会在当前目录或上级目录中寻找<code>Cargo.toml</code>文件。</dd>



<dt class="option-term" id="option-cargo-test---frozen"><a class="option-anchor" href="#option-cargo-test---frozen"></a><code>--frozen</code></dt>
<dt class="option-term" id="option-cargo-test---locked"><a class="option-anchor" href="#option-cargo-test---locked"></a><code>--locked</code></dt>
<dd class="option-desc">这两个选项用于保证<code>Cargo.lock</code>文件是最新的。如果该锁文件不存在，或者不是最新的，Cargo
会报错退出。其中<code>--frozen</code>选项会阻止Cargo访问网络以检查锁文件是否是最新的。</p>
<p>这些选项，可用于保证<code>Cargo.lock</code>文件是最新的(比如持续集成的构建过程)，
或用于避免联网。</dd>


<dt class="option-term" id="option-cargo-test---offline"><a class="option-anchor" href="#option-cargo-test---offline"></a><code>--offline</code></dt>
<dd class="option-desc">禁止Cargo访问网络。如果不添加此选项，Cargo在需要访问网络但网络不可用的情况下，会报错
并停止工作。添加此选项后，Cargo会尽可能尝试不使用网络来工作。</p>
<p>注意，在此情况下可能会产生与联网状态下不同的依赖解析(<strong>Dependency Resolution</strong>)结果。
Cargo只会使用本地已下载的crate，即便本地的索引副本中表明可能有新版本crate。在离线前下载
所需依赖的方法，参见 <a href="cargo-fetch.html">cargo-fetch(1)</a> 。</p>
<p>也可以通过 <code>net.offline</code> <a href="../reference/config.html">config value</a>指定。</dd>



</dl>

### Common Options

<dl>

<dt class="option-term" id="option-cargo-test-+toolchain"><a class="option-anchor" href="#option-cargo-test-+toolchain"></a><code>+</code><em>toolchain</em></dt>
<dd class="option-desc">If Cargo has been installed with rustup, and the first argument to <code>cargo</code>
begins with <code>+</code>, it will be interpreted as a rustup toolchain name (such
as <code>+stable</code> or <code>+nightly</code>).
See the <a href="https://rust-lang.github.io/rustup/overrides.html">rustup documentation</a>
for more information about how toolchain overrides work.</dd>


<dt class="option-term" id="option-cargo-test--h"><a class="option-anchor" href="#option-cargo-test--h"></a><code>-h</code></dt>
<dt class="option-term" id="option-cargo-test---help"><a class="option-anchor" href="#option-cargo-test---help"></a><code>--help</code></dt>
<dd class="option-desc">Prints help information.</dd>


<dt class="option-term" id="option-cargo-test--Z"><a class="option-anchor" href="#option-cargo-test--Z"></a><code>-Z</code> <em>flag</em></dt>
<dd class="option-desc">Unstable (nightly-only) flags to Cargo. Run <code>cargo -Z help</code> for details.</dd>


</dl>


### Miscellaneous Options

The `--jobs` argument affects the building of the test executable but does not
affect how many threads are used when running the tests. The Rust test harness
includes an option to control the number of threads used:

    cargo test -j 2 -- --test-threads=2

<dl>

<dt class="option-term" id="option-cargo-test--j"><a class="option-anchor" href="#option-cargo-test--j"></a><code>-j</code> <em>N</em></dt>
<dt class="option-term" id="option-cargo-test---jobs"><a class="option-anchor" href="#option-cargo-test---jobs"></a><code>--jobs</code> <em>N</em></dt>
<dd class="option-desc">要并行运行的作业数量。也可通过<code>build.jobs</code> <a href="../reference/config.html">config value</a>指定。
默认为CPU数量。</dd>


<dt class="option-term" id="option-cargo-test---future-incompat-report"><a class="option-anchor" href="#option-cargo-test---future-incompat-report"></a><code>--future-incompat-report</code></dt>
<dd class="option-desc">Displays a future-incompat report for any future-incompatible warnings
produced during execution of this command</p>
<p>See <a href="cargo-report.html">cargo-report(1)</a></dd>



</dl>

## 环境

关于Cargo所读取的环境变量，可参见[the reference](../reference/environment-variables.html)


## 退出状态

* `0`: Cargo命令执行成功
* `101`: Cargo命令未能完成.


## EXAMPLES

1. Execute all the unit and integration tests of the current package:

       cargo test

2. Run only tests whose names match against a filter string:

       cargo test name_filter

3. Run only a specific test within a specific integration test:

       cargo test --test int_test_name -- modname::test_name

## SEE ALSO
[cargo(1)](cargo.html), [cargo-bench(1)](cargo-bench.html)
