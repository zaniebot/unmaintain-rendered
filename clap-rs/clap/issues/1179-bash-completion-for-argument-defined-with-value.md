```yaml
number: 1179
title: "bash completion for argument defined with `value_name` should complete with file names"
type: issue
state: closed
author: jzinn
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2018-02-13T00:12:29Z
updated_at: 2018-08-02T03:30:18Z
url: https://github.com/clap-rs/clap/issues/1179
synced_at: 2026-01-12T16:14:10Z
```

# bash completion for argument defined with `value_name` should complete with file names

---

_@jzinn_

Ripgrep has a `-g` option that could use some improved Bash completion.  Please see
*   https://github.com/BurntSushi/ripgrep/issues/792

### Rust Version

```
$ rustc -V
rustc 1.25.0-nightly (b8398d947 2018-02-11)
```

### Affected Version of clap

```
$ grep clap ripgrep/Cargo.lock
name = "clap"
 "clap 2.29.4 (registry+https://github.com/rust-lang/crates.io-index)",
"checksum clap 2.29.4 (registry+https://github.com/rust-lang/crates.io-index)" = "7b8f59bcebcfe4269b09f71dab0da15b355c75916a8f975d3876ce81561893ee"
```

### Expected Behavior Summary

Either do nothing:
```
touch Foo.txt
rg -g Fo<tab>  # hit <tab> to initiate completion
rg -g Fo       # after completion
```

or complete with file names:
```
touch Foo.txt
rg -g Fo<tab>  # hit <tab> to initiate completion
rg -g Foo.txt  # after completion
```

Additionally, the Ripgrep `-g` option allows a leading `!` to indicate negation.  If Clap can support this, then an example would be
```
mkdir vendor
rg -g "!vend<tab>  # hit <tab> to initiate completion
rg -g "!vendor"/   # after completion
```

### Actual Behavior Summary

Currently the typed in value is erased and replaced with `<GLOB>...`:
```
touch Foo.txt
rg -g Fo<tab>    # hit <tab> to initiate completion
rg -g <GLOB>...  # after completion
```

### Steps to Reproduce the issue

See above.


### Sample Code or Link to Sample Code

I could easily be wrong, but Ripgrep seems to instantiate a `clap-rs` `Arg` using something similar to:

```rust
Arg::with_name("glob")
    .long("glob")
    .value_name("GLOB")
    .takes_value(true)
    .number_of_values(1)
    .short("g")
    .help("Include or exclude files.")
    .long_help("Include or exclude files and ...")
    .multiple(true)
    .allow_hyphen_values(true)
    ;
```

It seems that `value_name` is used in the completion (`<GLOB>...`) instead of file names.


### Debug output


---

_Label `T: bug` added by @kbknapp on 2018-02-13 00:35_

---

_Label `P2: need to have` added by @kbknapp on 2018-02-13 00:35_

---

_Label `D: intermediate` added by @kbknapp on 2018-02-13 00:35_

---

_Label `C: completion gen` added by @kbknapp on 2018-02-13 00:35_

---

_Comment by @kbknapp on 2018-02-13 00:36_

Thanks! I'll take a look at this tomorrow and see if I can get a patch in to just fall back to file completion in all cases.

---

_Comment by @jzinn on 2018-02-13 00:41_

As an aside, with the current behavior it seems that these options

```rust
.takes_value(true)
.number_of_values(1)
.multiple(true)
```

should not result in `<GLOB>...`, because the `...` indicates that repetition is possible.  However, repetition is not allowed in this case because of the call `number_of_values(1)`.


---

_Comment by @BurntSushi on 2018-02-13 01:16_

@jzinn That refers to the number of values per use of the flag, and not the number of times the flag can be used. That is, they are orthogonal options. :)

---

_Referenced in [clap-rs/clap#1037](../../clap-rs/clap/issues/1037.md) on 2018-02-13 02:05_

---

_Referenced in [clap-rs/clap#1180](../../clap-rs/clap/pulls/1180.md) on 2018-02-13 16:42_

---

_Closed by @kbknapp on 2018-02-13 18:06_

---

_Comment by @kbknapp on 2018-02-13 18:09_

v2.30.0 is published

---

_Referenced in [clap-rs/clap#1182](../../clap-rs/clap/pulls/1182.md) on 2018-02-13 21:49_

---
