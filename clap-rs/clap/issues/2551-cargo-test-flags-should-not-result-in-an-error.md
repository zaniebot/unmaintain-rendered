```yaml
number: 2551
title: "`cargo test` flags should not result in an error for integration tests"
type: issue
state: closed
author: ErwanDL
labels:
  - C-bug
assignees: []
created_at: 2021-06-18T15:41:05Z
updated_at: 2021-06-18T17:20:51Z
url: https://github.com/clap-rs/clap/issues/2551
synced_at: 2026-01-12T16:14:13Z
```

# `cargo test` flags should not result in an error for integration tests

---

_@ErwanDL_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.52.1

### Clap Version

2.33.3

### Minimal reproducible code

Have any integration test (those in the `tests/` folder, as opposed to regular tests in `src/`) that uses `clap`, for instance by accessing options parsed by _structopt_.

### Steps to reproduce the bug with the above code

Run the test suite with additional flags, for example `--nocapture` so that all tests output their logs : 
```
cargo test -- --nocapture
```

### Actual Behaviour

Unit tests will run as expected, but integration tests fail to run, as clap (called by _structopt_) errors out because it tries to parse the `--nocapture` flag:
```
error: Found argument '--nocapture' which wasn't expected, or isn't valid in this context
```

### Expected Behaviour

`clap` should try and treat integration tests as regular unit tests, which do not exhibit the same behaviour, and continue executing normally when passed `--nocapture`.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `T: bug` added by @ErwanDL on 2021-06-18 15:41_

---

_Closed by @pksunkara on 2021-06-18 17:20_

---

_Locked by @clap-rs on 2021-06-18 17:20_

---
