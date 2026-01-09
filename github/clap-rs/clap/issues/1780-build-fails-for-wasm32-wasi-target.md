---
number: 1780
title: build fails for wasm32-wasi target
type: issue
state: closed
author: wucke13
labels:
  - C-bug
assignees: []
created_at: 2020-04-04T00:09:38Z
updated_at: 2020-04-27T18:13:34Z
url: https://github.com/clap-rs/clap/issues/1780
synced_at: 2026-01-07T13:12:19-06:00
---

# build fails for wasm32-wasi target

---

_Issue opened by @wucke13 on 2020-04-04 00:09_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.42.0 (b8cedc004 2020-03-09)

### Code

```rust
fn main()->{}
```

### Steps to reproduce the issue

1. Run `cargo run --target wasm32-wasi`

### Affected Version of `clap*`

master

### Actual Behavior Summary

The compilation does not finish due to 31 errors, mostly:

+ error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
+ error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope

### Expected Behavior Summary

The compilation does finish with 0 errors

### Additional context

I really like the idea of having __universal bytecode__, and I really like powerful CLI tools. I would love to use clap for the wasm32-wasi target!


---

_Label `T: bug` added by @wucke13 on 2020-04-04 00:09_

---

_Comment by @Dylan-DPC-zz on 2020-04-04 01:50_

Yeah i don't think we support or have focused supporting this target. Will need to investigate a way out. If possible can you share all the errors? Thanks

---

_Comment by @wucke13 on 2020-04-04 10:11_

Sure, it is really just those too errors.

```
error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
   --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/parse/parser.rs:440:49
    |
440 |                 let key_bytes = OsStr::new(key).as_bytes();
    |                                                 ^^^^^^^^ method not found in `&std::ffi::OsStr`
    |
    = help: items from traits can only be used if the trait is in scope
    = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
            `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
   --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/parse/parser.rs:442:40
    |
442 |                 if key_bytes == arg_os.as_bytes() {
    |                                        ^^^^^^^^ method not found in `&std::ffi::OsStr`
    |
    = help: items from traits can only be used if the trait is in scope
    = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
            `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
    --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/build/arg/mod.rs:2305:41
     |
2305 |         self.default_values_os(&[OsStr::from_bytes(val.as_bytes())])
     |                                         ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
     |
     = help: items from traits can only be used if the trait is in scope
     = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
             `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
    --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/build/arg/mod.rs:2321:31
     |
2321 |             .map(|val| OsStr::from_bytes(val.as_bytes()))
     |                               ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
     |
     = help: items from traits can only be used if the trait is in scope
     = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
             `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
    --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/build/arg/mod.rs:2440:47
     |
2440 |             val.map(str::as_bytes).map(OsStr::from_bytes),
     |                                               ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
     |
     = help: items from traits can only be used if the trait is in scope
     = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
             `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
    --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/build/arg/mod.rs:2441:20
     |
2441 |             OsStr::from_bytes(default.as_bytes()),
     |                    ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
     |
     = help: items from traits can only be used if the trait is in scope
     = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
             `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
    --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/build/arg/mod.rs:2559:51
     |
2559 |                 val.map(str::as_bytes).map(OsStr::from_bytes),
     |                                                   ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
     |
     = help: items from traits can only be used if the trait is in scope
     = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
             `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
    --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/build/arg/mod.rs:2560:24
     |
2560 |                 OsStr::from_bytes(default.as_bytes()),
     |                        ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
     |
     = help: items from traits can only be used if the trait is in scope
     = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
             `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
   --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/parse/parser.rs:796:29
    |
796 |             let n_bytes = n.as_bytes();
    |                             ^^^^^^^^ method not found in `&std::ffi::OsStr`
    |
    = help: items from traits can only be used if the trait is in scope
    = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
            `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
   --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/parse/parser.rs:797:41
    |
797 |             let h_bytes = OsStr::new(h).as_bytes();
    |                                         ^^^^^^^^ method not found in `&std::ffi::OsStr`
    |
    = help: items from traits can only be used if the trait is in scope
    = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
            `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:37:14
   |
37 |         self.as_bytes().starts_with(s)
   |              ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:41:23
   |
41 |         for b in self.as_bytes() {
   |                       ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:50:28
   |
50 |         for (i, b) in self.as_bytes().iter().enumerate() {
   |                            ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:53:28
   |
53 |                     OsStr::from_bytes(&self.as_bytes()[..i]),
   |                            ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:53:45
   |
53 |                     OsStr::from_bytes(&self.as_bytes()[..i]),
   |                                             ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:54:28
   |
54 |                     OsStr::from_bytes(&self.as_bytes()[i + 1..]),
   |                            ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:54:45
   |
54 |                     OsStr::from_bytes(&self.as_bytes()[i + 1..]),
   |                                             ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:60:20
   |
60 |             OsStr::from_bytes(&self.as_bytes()[self.len()..self.len()]),
   |                    ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:60:37
   |
60 |             OsStr::from_bytes(&self.as_bytes()[self.len()..self.len()]),
   |                                     ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:66:28
   |
66 |         for (i, b) in self.as_bytes().iter().enumerate() {
   |                            ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:68:31
   |
68 |                 return OsStr::from_bytes(&self.as_bytes()[i..]);
   |                               ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:68:48
   |
68 |                 return OsStr::from_bytes(&self.as_bytes()[i..]);
   |                                                ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:74:27
   |
74 |             return OsStr::from_bytes(&self.as_bytes()[self.len()..]);
   |                           ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:74:44
   |
74 |             return OsStr::from_bytes(&self.as_bytes()[self.len()..]);
   |                                            ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:81:20
   |
81 |             OsStr::from_bytes(&self.as_bytes()[..i]),
   |                    ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:81:37
   |
81 |             OsStr::from_bytes(&self.as_bytes()[..i]),
   |                                     ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:82:20
   |
82 |             OsStr::from_bytes(&self.as_bytes()[i..]),
   |                    ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:82:37
   |
82 |             OsStr::from_bytes(&self.as_bytes()[i..]),
   |                                     ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no method named `as_bytes` found for reference `&std::ffi::OsStr` in the current scope
  --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:89:23
   |
89 |             val: self.as_bytes(),
   |                       ^^^^^^^^ method not found in `&std::ffi::OsStr`
   |
   = help: items from traits can only be used if the trait is in scope
   = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
           `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
   --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:115:36
    |
115 |                 return Some(OsStr::from_bytes(&self.val[start..self.pos - 1]));
    |                                    ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
    |
    = help: items from traits can only be used if the trait is in scope
    = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
            `use std::os::wasi::ffi::OsStrExt;`

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
   --> /home/wucke13/.cargo/git/checkouts/clap-78dbe9b58f9073fe/4b5d772/src/util/osstringext.rs:118:21
    |
118 |         Some(OsStr::from_bytes(&self.val[start..]))
    |                     ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
    |
    = help: items from traits can only be used if the trait is in scope
    = note: the following trait is implemented but not in scope; perhaps add a `use` for it:
            `use std::os::wasi::ffi::OsStrExt;`

error: aborting due to 31 previous errors

For more information about this error, try `rustc --explain E0599`.
error: could not compile `clap`.

To learn more, run the command again with --verbose.
```

---

_Comment by @CreepySkeleton on 2020-04-07 14:00_

It's really weird. The only platform-dependent code in clap is located [here](https://github.com/clap-rs/clap/blob/master/src/util/osstringext.rs), and wasm32 is covered. I'll investigate it further later. 

---

_Comment by @CreepySkeleton on 2020-04-24 14:05_

I suspect this is fixed by https://github.com/clap-rs/clap/pull/1852 . @wucke13 Could you please help confirm that? Or perhaps provide precise instructions on how to reproduce this? My cargo build fails with
```
error[E0463]: can't find crate for `core`
  |
  = note: the `wasm32-wasi` target may not be installed

error[E0463]: can't find crate for `std`
  |
  = note: the `wasm32-wasi` target may not be installed

error: aborting due to previous error
``` 

---

_Comment by @wucke13 on 2020-04-25 11:07_

@CreepySkeleton 
Regarding 

```
error[E0463]: can't find crate for `core`
  |
  = note: the `wasm32-wasi` target may not be installed
```

You need to install the target, to be able to compile to it:

```
rustup target add wasm32-wasi
```

Other than that: Yes, the original error is fixed, though in exchange for a new one: the `os_str_bytes` crate fails to compile on `wasm32-wasi`. I guess, this can be circumvented at least partial, as the compiler points out that there is a use which might work:

```
> cargo build --target wasm32-wasi
   Compiling proc-macro2 v1.0.10
   Compiling unicode-xid v0.2.0
   Compiling version_check v0.9.1
   Compiling syn v1.0.18
   Compiling autocfg v1.0.0
   Compiling unicode-segmentation v1.6.0
   Compiling bitflags v1.2.1
   Compiling unicode-width v0.1.7
   Compiling strsim v0.10.0
   Compiling atty v0.2.14
   Compiling vec_map v0.8.1
   Compiling termcolor v1.1.0
   Compiling os_str_bytes v2.1.0
error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsStr` in the current scope
   --> /home/wucke13/.cargo/registry/src/github.com-1ecc6299db9ec823/os_str_bytes-2.1.0/src/lib.rs:243:16
    |
243 |         OsStr::from_bytes(string).map(|os_string| match os_string {
    |                ^^^^^^^^^^ function or associated item not found in `std::ffi::OsStr`
    |
    = help: items from traits can only be used if the trait is in scope
help: the following trait is implemented but not in scope; perhaps add a `use` for it:
    |
127 | use std::os::wasi::ffi::OsStrExt;
    |

error[E0599]: no method named `to_bytes` found for reference `&std::ffi::OsStr` in the current scope
   --> /home/wucke13/.cargo/registry/src/github.com-1ecc6299db9ec823/os_str_bytes-2.1.0/src/lib.rs:251:26
    |
251 |         self.as_os_str().to_bytes()
    |                          ^^^^^^^^ method not found in `&std::ffi::OsStr`
    |
    = help: items from traits can only be used if the trait is implemented and in scope
note: `OsStrBytes` defines an item `to_bytes`, perhaps you need to implement it
   --> /home/wucke13/.cargo/registry/src/github.com-1ecc6299db9ec823/os_str_bytes-2.1.0/src/lib.rs:185:1
    |
185 | pub trait OsStrBytes: private::Sealed + ToOwned {
    | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^

error[E0599]: no function or associated item named `from_bytes` found for struct `std::ffi::OsString` in the current scope
   --> /home/wucke13/.cargo/registry/src/github.com-1ecc6299db9ec823/os_str_bytes-2.1.0/src/lib.rs:379:19
    |
379 |         OsString::from_bytes(string).map(Into::into)
    |                   ^^^^^^^^^^ function or associated item not found in `std::ffi::OsString`
    |
    = help: items from traits can only be used if the trait is implemented and in scope
    = note: the following traits define an item `from_bytes`, perhaps you need to implement one of them:
            candidate #1: `OsStrBytes`
            candidate #2: `OsStringBytes`

error[E0599]: no function or associated item named `from_vec` found for struct `std::ffi::OsString` in the current scope
   --> /home/wucke13/.cargo/registry/src/github.com-1ecc6299db9ec823/os_str_bytes-2.1.0/src/lib.rs:384:19
    |
384 |         OsString::from_vec(string).map(Into::into)
    |                   ^^^^^^^^ function or associated item not found in `std::ffi::OsString`
    |
    = help: items from traits can only be used if the trait is in scope
help: the following trait is implemented but not in scope; perhaps add a `use` for it:
    |
127 | use std::os::wasi::ffi::OsStringExt;
    |

error[E0599]: no method named `into_vec` found for struct `std::ffi::OsString` in the current scope
   --> /home/wucke13/.cargo/registry/src/github.com-1ecc6299db9ec823/os_str_bytes-2.1.0/src/lib.rs:389:31
    |
389 |         self.into_os_string().into_vec()
    |                               ^^^^^^^^ method not found in `std::ffi::OsString`
    |
    = help: items from traits can only be used if the trait is in scope
help: the following trait is implemented but not in scope; perhaps add a `use` for it:
    |
127 | use std::os::wasi::ffi::OsStringExt;
    |

   Compiling lazy_static v1.4.0
error: aborting due to 5 previous errors

For more information about this error, try `rustc --explain E0599`.
error: could not compile `os_str_bytes`.

To learn more, run the command again with --verbose.
warning: build failed, waiting for other jobs to finish...
error: build failed
```

Edit:

> Could you please help confirm that?

I can confirm that the errors decreased and changed, but there are some.

> Or perhaps provide precise instructions on how to reproduce this?

I thought this was a no-brainer, please pardon me for that.
```
rustup target add wasm32-wasi
cargo init --bin wasm-test
cd wasm-test
echo 'clap = { git = "https://github.com/clap-rs/clap" }' >> Cargo.toml
cargo build --target wasm32-wasi
```

---

_Comment by @pksunkara on 2020-04-25 11:14_

@dylni Could you look into this issue and fix it in your crate?

---

_Added to milestone `3.0` by @pksunkara on 2020-04-25 11:14_

---

_Comment by @CreepySkeleton on 2020-04-25 12:24_

@wucke13 

> I thought this was a no-brainer, please pardon me for that.

Brains may be different 😉. No problemo.

Basically, the `os_str_bytes` crate we rely on needs to make some assumptions on how the underlying `std::ffi::OsString` is represented on the target platform. It does so in a platform-specific manner, basically distinguishing between Windows and Unix.

I _suspect_ OsString on `wasm` is the same as on Unix, right? If so, A small upsteam patch should do the job. Could you please provide a link to the relevant part of wasm documentation? I just have no experience with it, sorry.

---

_Comment by @dylni on 2020-04-25 12:58_

@pksunkara This is a language limitation. There's no safe way to get anything lossless from `OsStr` on wasm32 targets. However, it's also [not possible to receive arguments](https://github.com/rust-lang/rust/blob/master/src/libstd/sys/wasm/args.rs#L11-L13) for that target, so I'm not sure how Clap could be used.

I haven't found anywhere else that `OsStr` is created for that target, so it might be enough to attempt a conversion to `str` and unwrap it as a fix. There should be no way for it to contain non-UTF-8 data if the language doesn't construct it.

---

_Comment by @dylni on 2020-04-26 01:19_

Ignore my previous comment. I was looking at code for `wasm32-unknown-unknown`.

The API looks compatible with Unix, so I'll work on an update to add support for the target this week.

---

_Comment by @dylni on 2020-04-27 17:33_

[Version 2.2.0](https://github.com/dylni/os_str_bytes/releases/tag/2.2.0) supports this target.

---

_Referenced in [clap-rs/clap#1872](../../clap-rs/clap/pulls/1872.md) on 2020-04-27 17:37_

---

_Comment by @CreepySkeleton on 2020-04-27 17:38_

@dylni We are very grateful for the quick fix. Thank you a lot.

@wucke13 After the PR is merged, could you please confirm that the build is actually fixed?

---

_Comment by @dylni on 2020-04-27 17:52_

@CreepySkeleton No problem. I would prefer that it supports as many targets as possible.

I think all buildable targets that take arguments are covered now ([except sgx](https://github.com/rust-lang/rust/issues/56975)), but let me know if you find any others.

---

_Comment by @wucke13 on 2020-04-27 18:13_

@CreepySkeleton Indeed, compilation succeeds now!
Thank you all for this fast and straightforward process. Let's hope the best for wasm-wasi!

---

_Closed by @wucke13 on 2020-04-27 18:13_

---
