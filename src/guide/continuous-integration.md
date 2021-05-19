## 持续集成

### Travis CI

要在 Travis CI 上测试您的项目，这里有一个`.travis.yml`文件示例:

```yaml
language: rust
rust:
  - stable
  - beta
  - nightly
matrix:
  allow_failures:
    - rust: nightly
```

这将在所有三个 rust 版本下，进行测试，但 nightly 的任何破坏，都不会使整体构建失败。请看看[Travis CI Rust 文档](https://docs.travis-ci.com/user/languages/rust/)了解更多信息.

### GitLab CI

要在 GitLab CI 上测试您的包，这里有一个`.gitlab-ci.yml`文件示例:

```yaml
stages:
  - build

rust-latest:
  stage: build
  image: rust:latest
  script:
    - cargo build --verbose
    - cargo test --verbose

rust-nightly:
  stage: build
  image: rustlang/rust:nightly
  script:
    - cargo build --verbose
    - cargo test --verbose
  allow_failure: true
```

这将测试 stable 版本和 nightly 版本，但 nightly 的任何破损，都不会使整体构建失败。欲获得更多信息，请看[GitLab CI](https://docs.gitlab.com/ce/ci/yaml/README.md).
