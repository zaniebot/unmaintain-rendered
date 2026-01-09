---
number: 1467
title: "Could not compile `clap`. process didn't exit successfully"
type: issue
state: closed
author: asmarcz
labels: []
assignees: []
created_at: 2019-05-05T14:56:17Z
updated_at: 2019-09-26T09:37:45Z
url: https://github.com/clap-rs/clap/issues/1467
synced_at: 2026-01-07T13:12:19-06:00
---

# Could not compile `clap`. process didn't exit successfully

---

_Issue opened by @asmarcz on 2019-05-05 14:56_

Hi, I am a bit unsure where to put this issue as it happens when using `tether` crate but only `bindgen` is dependent on `clap`.

### Rust Version
rustc 1.34.1 (fc50f328b 2019-04-24)

### Affected Version of clap
2.33.0

### Bug Summary
RSL won't start because it can't compile clap in VS Code extension.

### Actual Behavior Summary
An Error is shown:
```
{
	"resource": "/home/asmar/projects/rust/tether_test/Cargo.toml",
	"owner": "rust",
	"severity": 8,
	"message": "Could not compile `clap`.\nprocess didn't exit successfully: `/home/asmar/.rustup/toolchains/stable-x86_64-unknown-linux-gnu/bin/rls --crate-name clap /home/asmar/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-2.33.0/src/lib.rs --color never --crate-type lib --emit=dep-info,link -C debuginfo=2 --cfg 'feature=\"ansi_term\"' --cfg 'feature=\"atty\"' --cfg 'feature=\"color\"' --cfg 'feature=\"default\"' --cfg 'feature=\"strsim\"' --cfg 'feature=\"suggestions\"' --cfg 'feature=\"vec_map\"' -C metadata=92f4b88ca94d45f6 -C extra-filename=-92f4b88ca94d45f6 --out-dir /home/asmar/projects/rust/tether_test/target/rls/debug/deps -L dependency=/home/asmar/projects/rust/tether_test/target/rls/debug/deps --extern ansi_term=/home/asmar/projects/rust/tether_test/target/rls/debug/deps/libansi_term-5d0ea3e7965a4619.rlib --extern atty=/home/asmar/projects/rust/tether_test/target/rls/debug/deps/libatty-b238ee1612164c59.rlib --extern bitflags=/home/asmar/projects/rust/tether_test/target/rls/debug/deps/libbitflags-1eecf389306a3c09.rlib --extern strsim=/home/asmar/projects/rust/tether_test/target/rls/debug/deps/libstrsim-9254da5d76dd790e.rlib --extern textwrap=/home/asmar/projects/rust/tether_test/target/rls/debug/deps/libtextwrap-bcb2e577ece43bfb.rlib --extern unicode_width=/home/asmar/projects/rust/tether_test/target/rls/debug/deps/libunicode_width-19d5b772cca570d6.rlib --extern vec_map=/home/asmar/projects/rust/tether_test/target/rls/debug/deps/libvec_map-fa11111d90438a74.rlib --cap-lints allow --error-format=json --sysroot /home/asmar/.rustup/toolchains/stable-x86_64-unknown-linux-gnu` (exit code: 101)",
	"startLineNumber": 1,
	"startColumn": 1,
	"endLineNumber": 10000,
	"endColumn": 1
}
```

### Steps to Reproduce the issue
Open Cargo project that has `tether="0.3.0"` dependency with VS Code and Rust (rls) extension.

---

_Comment by @mooreryan on 2019-05-06 04:10_

I ran into a similar issue.  Posted it as an issue to `rls` here: https://github.com/rust-lang/rls/issues/1454

---

_Comment by @mooreryan on 2019-05-06 04:12_

For some reason `rls` fails to compile `clap` or things that depend on it.  Interesting thing is that just compiling/running my program with `cargo run` will work, just not when VS Code tries to compile using `rls`.

---

_Comment by @mati865 on 2019-05-06 15:14_

It's not `clap` issue: https://github.com/rust-lang/rls/issues/1449#issuecomment-489026206

---

_Comment by @mooreryan on 2019-05-06 20:25_

@mati865 is right.  Try the fix in https://github.com/bitflags/bitflags/issues/177, it worked for me.

---

_Comment by @philipjkim on 2019-05-07 02:48_

How can I lock the version of bitflags, which is a dependency of clap? In my case, adding `bitflags = "1.0.4"` to `Cargo.toml` didn't work (`Cargo.lock` still contains bitflags 1.0.5, not 1.0.4). What worked for me is that I edited auto-generated `Cargo.lock` manually:

1. replacing all occurrences of bitflags version `1.0.5` to `1.0.4` and 
1. replacing checksum value of `bitflags 1.0.5`to the value of `1.0.4`. I found the checksum value [here](https://github.com/rust-lang/crates.io-index/blob/master/bi/tf/bitflags)

    ```
    "checksum bitflags 1.0.4 (registry+https://github.com/rust-lang/crates.io-index)" = "228047a76f468627ca71776ecdebd732a3423081fcf5125585bcd7c49886ce12"
    ```



---

_Comment by @mooreryan on 2019-05-07 04:36_

Add this to your `Cargo.toml`

```
[dependencies]
bitflags = "=1.0.4"
```

Looks like you are missing the second `=` sign (`"=1.0.4"`).

---

_Closed by @Dylan-DPC-zz on 2019-09-26 09:37_

---
