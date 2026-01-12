```yaml
number: 5400
title: "build delta-inspect raise could not find `delta_datafusion` in the crate root"
type: issue
state: closed
author: dgouyette
labels:
  - C-bug
assignees: []
created_at: 2024-03-18T20:20:31Z
updated_at: 2024-03-19T06:38:50Z
url: https://github.com/clap-rs/clap/issues/5400
synced_at: 2026-01-12T16:14:17Z
```

# build delta-inspect raise could not find `delta_datafusion` in the crate root

---

_@dgouyette_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

  stable-aarch64-apple-darwin unchanged - rustc 1.76.0 (07dca489a 2024-02-04)

### Clap Version

3.2.23

### Minimal reproducible code

cd delta-inspect

```rust
cargo build
```

will raise : 

```bash
error[E0432]: unresolved import `crate::delta_datafusion`
 --> crates/core/src/operations/transaction/conflict_checker.rs:5:12
  |
5 | use crate::delta_datafusion::DataFusionMixins;
  |            ^^^^^^^^^^^^^^^^ could not find `delta_datafusion` in the crate root

error[E0700]: hidden type for `impl Iterator<Item = actions::Add>` captures lifetime that does not appear in bounds
   --> crates/core/src/operations/transaction/conflict_checker.rs:196:9
    |
109 | impl<'a> TransactionInfo<'a> {
    |      -- hidden type `impl Iterator<Item = actions::Add> + 'a` captures the lifetime `'a` as defined here
...
195 |     pub fn read_files(&self) -> Result<impl Iterator<Item = Add>, CommitConflictError> {
    |                                        ------------------------- opaque type defined here
196 |         Ok(self.read_snapshot.file_actions().unwrap().into_iter())
    |         ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

Some errors have detailed explanations: E0432, E0700.
For more information about an error, try `rustc --explain E0432`.

```


### Steps to reproduce the bug with the above code

`cd delta-inspect`

`cargo build`




### Actual Behaviour

build is failing

### Expected Behaviour

build successful 

### Additional Context

in 
```[dependencies.deltalake]
path = "../crates/deltalake"
version = "0"
features = ["azure", "gcs"]
```

it misses `datafusion`

I can provide a PR if needed

### Debug Output

_No response_

---

_Label `C-bug` added by @dgouyette on 2024-03-18 20:20_

---

_Comment by @epage on 2024-03-18 20:54_

Is there a reason you opened this issue on the clap repo?

---

_Comment by @dgouyette on 2024-03-19 06:38_

😞 

---

_Closed by @dgouyette on 2024-03-19 06:38_

---
