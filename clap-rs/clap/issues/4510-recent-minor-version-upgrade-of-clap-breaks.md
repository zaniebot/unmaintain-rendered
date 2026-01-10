---
number: 4510
title: Recent minor version upgrade of clap breaks building with a wasm target.
type: issue
state: closed
author: 0xForerunner
labels:
  - C-bug
assignees: []
created_at: 2022-11-25T22:10:51Z
updated_at: 2022-11-29T03:45:11Z
url: https://github.com/clap-rs/clap/issues/4510
synced_at: 2026-01-10T01:27:57Z
---

# Recent minor version upgrade of clap breaks building with a wasm target.

---

_Issue opened by @0xForerunner on 2022-11-25 22:10_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

rustc 1.67.0-nightly (b3bc6bf31 2022-11-24)

### Clap Version

between 4.0.22 and 4.0.27

### Minimal reproducible code

```rust
fn main() {}
```


### Steps to reproduce the bug with the above code

just compile to wasm32-unknown-unknown and it breaks. 4.0.22 works as expected, 4.0.27 does not compile.

### Actual Behaviour

```
~/lending-contracts (develop) » cargo update -p clap                                                                                                                                                                                                       ewoolsey@Erics-MacBook-Pro
    Updating crates.io index
    Updating clap v4.0.22 -> v4.0.27
      Adding errno v0.2.8
      Adding errno-dragonfly v0.1.2
      Adding hermit-abi v0.2.6
      Adding io-lifetimes v1.0.2
      Adding is-terminal v0.4.0
      Adding linux-raw-sys v0.1.3
      Adding rustix v0.36.3
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
~/lending-contracts (develop*) » d b                                                                                                                                                                                              ewoolsey@Erics-MacBook-Pro
   Compiling libc v0.2.137
   Compiling io-lifetimes v1.0.2
   Compiling rustix v0.36.3
   Compiling errno v0.2.8
   Compiling neptune-authorization v0.1.0 (/Users/ewoolsey/lending-contracts/packages/neptune-auth)
error[E0583]: file not found for module `sys`
  --> /Users/ewoolsey/.cargo/registry/src/github.com-1ecc6299db9ec823/errno-0.2.8/src/lib.rs:33:1
   |
33 | mod sys;
   | ^^^^^^^^
   |
   = help: to create the module `sys`, create file "/Users/ewoolsey/.cargo/registry/src/github.com-1ecc6299db9ec823/errno-0.2.8/src/sys.rs" or "/Users/ewoolsey/.cargo/registry/src/github.com-1ecc6299db9ec823/errno-0.2.8/src/sys/mod.rs"

error[E0425]: cannot find function `errno` in module `sys`
   --> /Users/ewoolsey/.cargo/registry/src/github.com-1ecc6299db9ec823/errno-0.2.8/src/lib.rs:101:10
    |
101 |     sys::errno()
    |          ^^^^^ not found in `sys`

error[E0425]: cannot find function `set_errno` in module `sys`
   --> /Users/ewoolsey/.cargo/registry/src/github.com-1ecc6299db9ec823/errno-0.2.8/src/lib.rs:106:10
    |
106 |     sys::set_errno(err)
    |          ^^^^^^^^^ not found in `sys`

Some errors have detailed explanations: E0425, E0583.
For more information about an error, try `rustc --explain E0425`.
error: could not compile `errno` due to 3 previous errors
warning: build failed, waiting for other jobs to finish...
process exited unsuccessfully: exit status: 101
```

### Expected Behaviour

Minor version increments should not introduce breaking changes. I'm not sure exactly which version increment causes the issue but it appears to be from the inclusion of errno.

### Additional Context

_No response_

### Debug Output

_No response_

---

_Label `C-bug` added by @0xForerunner on 2022-11-25 22:10_

---

_Comment by @0xForerunner on 2022-11-25 22:20_

After fiddling a little bit I can confirm that the offender in 4.0.27 specifically. Everything works as expected on 4.0.26.

---

_Comment by @0xForerunner on 2022-11-25 22:26_

Might be useful to add a wasm build to you CI workflow to avoid accidental bugs like that.

---

_Comment by @epage on 2022-11-26 01:49_

4.0.27 saw us switch from atty to is-terminal, so that is the root cause.

We also do check `wasm32-unknown-unknown`  and `wasm32-wasi` in CI
- https://github.com/clap-rs/clap/blob/master/.github/workflows/ci.yml#L78
- https://github.com/clap-rs/clap/blob/master/Makefile#L18

This was all done in #4249 and the wams jobs definitely built is-terminal and it worked
- https://github.com/clap-rs/clap/actions/runs/3541494860/jobs/5945830558
- https://github.com/clap-rs/clap/actions/runs/3541494860/jobs/5945830669

So why did this work in CI but not for you?

---

_Comment by @0xForerunner on 2022-11-26 01:56_

> So why did this work in CI but not for you?

That's a fantastic question. Is there any other information I can give you that might be useful? Unfortunately I'm working on a large not-yet-open-source project otherwise I'd zip my build for you. I'm working on a Mac, although I doubt that's related to the problem.

---

_Comment by @johanhelsing on 2022-11-26 07:23_

Seeing the same thing in our project as well.

To reproduce:

```
git clone https://github.com/johanhelsing/matchbox
cd matchbox/matchbox_demo
cargo check --target wasm32-unknown-unknown
# no errors
cargo update -p clap
cargo check --target wasm32-unknown-unknown
# same error as ewoolsey
```

---

_Comment by @pinkforest on 2022-11-26 08:12_

> So why did this work in CI but not for you?

it seems to have picked atty looking at the logs .. ?

`Checking atty v0.2.14`

EDIT: wasm CI doesn't include color feature which is the feature that brings is-terminal ?

`cargo check --features "deprecated derive cargo env unicode string unstable-replace unstable-grouped" --all-targets --workspace`

---

_Comment by @pinkforest on 2022-11-26 08:18_

Working on rustup default stable -> 1.65.0 w/ associated w32-u-u target installed

So granular dep changes here were:

root@5d44a4311b14:/matchbox/matchbox_demo# cargo update -p clap
```
    Updating crates.io index
    Updating clap v4.0.18 -> v4.0.27
    Updating clap_derive v4.0.18 -> v4.0.21
      Adding errno v0.2.8
      Adding errno-dragonfly v0.1.2
      Adding hermit-abi v0.2.6
      Adding io-lifetimes v1.0.2
      Adding is-terminal v0.4.0
      Adding linux-raw-sys v0.1.3
      Adding rustix v0.36.3
```

I can replicate it inside rust default docker w/ add rustup target add wasm32-u-u from scratch

Immediate work/around seems if one disables the `color` feature by explicitly setting the defaults:

`clap = { version = "4.0", default-features = false, features = ["derive", "std", "help", "usage", "error-context", "suggestions"] }`

This prevents feature color (default) being included which brings is-terminal dependency that doesn't work with wasm32-u-u

EDIT: Intiially I had a brainfreeze and I had written no-default-features = true which obviously doesn't work ;)

This would need some cfg magic I think in order for the cargo not include these crates that don't work in wasm32-u-u

It's probably quite involved to cfg default features with: `target_family = "wasm", target_os = "unknown"`

I've also asked @sunfishcode if is-terminal could stub wasm32-u-u: https://github.com/sunfishcode/is-terminal/issues/9

---

_Referenced in [clap-rs/clap#4513](../../clap-rs/clap/pulls/4513.md) on 2022-11-26 08:26_

---

_Referenced in [sunfishcode/is-terminal#9](../../sunfishcode/is-terminal/issues/9.md) on 2022-11-26 08:34_

---

_Comment by @epage on 2022-11-26 22:01_

> EDIT: wasm CI doesn't include color feature which is the feature that brings is-terminal ?

`color` is a default feature.

If you look in the linked [logs](https://github.com/clap-rs/clap/actions/runs/3541494860/jobs/5945830558), you see that `termcolor` and `is-terminal` are being built.  I think the question is if we are using the default target or what I thought was an overriding target.

---

_Comment by @epage on 2022-11-28 15:32_

Got a [reproduction in CI](https://github.com/clap-rs/clap/pull/4515)

There are two failures
- `io-lifetimes` fails to compile with `wasm32-unknown-unknown`
- The errno issue above for wasi

---

_Referenced in [clap-rs/clap#4515](../../clap-rs/clap/pulls/4515.md) on 2022-11-28 15:35_

---

_Referenced in [clap-rs/clap#4518](../../clap-rs/clap/pulls/4518.md) on 2022-11-29 03:09_

---

_Comment by @pinkforest on 2022-11-29 03:16_

@sunfishcode bumped up `io-lifetimes` and `is-terminal` that fixed builds for `wasm32`  - `unknown` and `wasi` Thanks!

fix bump applied at https://github.com/clap-rs/clap/pull/4518 with the CI chore fix from #4515 

```
$ cargo build --target wasm32-unknown-unknown
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
$ cargo build --target wasm32-wasi
    Finished dev [unoptimized + debuginfo] target(s) in 0.03s
```

However testing is borked for `wasm32` -

Now it complains about `os_pipe` dev dependency via `trycmd` that doesn't seem to understand `wasm32`

$ cargo tree -i os_pipe
```
os_pipe v1.1.1
└── snapbox v0.4.1
    └── trycmd v0.14.3
        [dev-dependencies]
        └── clap v4.0.27 (/home/foobar/cc/clap)
    [dev-dependencies]
    └── clap v4.0.27 (/home/foobar/cc/clap)
```

https://github.com/clap-rs/clap/actions/runs/3570677150/jobs/6001871085

So progess I guess :)

It builds fine though so that is what is essential but testing needs adjustment for wasm32 targets me thinks ?

---

_Closed by @epage on 2022-11-29 03:45_

---

_Referenced in [johanhelsing/matchbox#51](../../johanhelsing/matchbox/pulls/51.md) on 2022-11-29 04:05_

---
