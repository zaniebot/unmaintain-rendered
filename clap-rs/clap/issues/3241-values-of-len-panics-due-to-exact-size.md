```yaml
number: 3241
title: values_of ... len() panics due to exact_size
type: issue
state: closed
author: FauxFaux
labels:
  - C-bug
  - A-builder
  - E-easy
assignees: []
created_at: 2022-01-02T12:10:26Z
updated_at: 2022-01-03T18:13:17Z
url: https://github.com/clap-rs/clap/issues/3241
synced_at: 2026-01-12T16:14:14Z
```

# values_of ... len() panics due to exact_size

---

_@FauxFaux_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

1.58.0-nightly

### Clap Version

3.0.0

### Minimal reproducible code

```rust
fn main() {
    clap::App::new("boog").arg(
            clap::Arg::new("POTATO")
            .takes_value(true)
            .multiple_values(true)
            .required(true)
        )
        .get_matches()
        .values_of("POTATO")
        .expect("present")
        .len();
}
```


### Steps to reproduce the bug with the above code

RUST_BACKTRACE=1 cargo run foo bars

### Actual Behaviour

```
thread 'main' panicked at 'assertion failed: `(left == right)`
  left: `None`,
 right: `Some(0)`', /rustc/dd549dcab404ec4c7d07b5a83aca5bdd7171138f/library/core/src/iter/traits/exact_size.rs:108:9
stack backtrace:
   0: rust_begin_unwind
             at /rustc/dd549dcab404ec4c7d07b5a83aca5bdd7171138f/library/std/src/panicking.rs:498:5
   1: core::panicking::panic_fmt
             at /rustc/dd549dcab404ec4c7d07b5a83aca5bdd7171138f/library/core/src/panicking.rs:107:14
   2: core::panicking::assert_failed_inner
   3: core::panicking::assert_failed
             at /rustc/dd549dcab404ec4c7d07b5a83aca5bdd7171138f/library/core/src/panicking.rs:145:5
   4: core::iter::traits::exact_size::ExactSizeIterator::len
             at /rustc/dd549dcab404ec4c7d07b5a83aca5bdd7171138f/library/core/src/iter/traits/exact_size.rs:108:9
   5: toke::main
             at ./src/main.rs:2:5
```

### Expected Behaviour

`len()` == 2

### Additional Context

Maybe something to do with ExactSizeIterator's rules about `size_hint`?

### Debug Output

```
[      clap::parse::validator] 	Validator::validate_matched_args:iter:POTATO: vals=Flatten {
    inner: FlattenCompat {
        iter: Fuse {
            iter: Some(
                Iter(
                    [
                        [
                            "foo",
                            "bars",
                        ],
                    ],
                ),
            ),
        },
        frontiter: None,
        backiter: None,
    },
}
[      clap::parse::validator] 	Validator::validate_arg_num_vals
[      clap::parse::validator] 	Validator::validate_arg_values: arg="POTATO"
[      clap::parse::validator] 	Validator::validate_arg_num_occurs: "POTATO"=2
[    clap::parse::arg_matcher] 	ArgMatcher::get_global_values: global_arg_vec=[help]
thread 'main' panicked at 'assertion failed: `(left == right)`
```

---

_Label `C-bug` added by @FauxFaux on 2022-01-02 12:10_

---

_Referenced in [clap-rs/clap#3247](../../clap-rs/clap/pulls/3247.md) on 2022-01-03 17:55_

---

_Label `A-builder` added by @epage on 2022-01-03 18:08_

---

_Label `E-easy` added by @epage on 2022-01-03 18:08_

---

_Closed by @epage on 2022-01-03 18:08_

---

_Comment by @epage on 2022-01-03 18:13_

v3.0.1 is now released with the fix

---
