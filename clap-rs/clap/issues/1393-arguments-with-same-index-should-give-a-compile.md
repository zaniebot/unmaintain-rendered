```yaml
number: 1393
title: Arguments with same index should give a compile time error
type: issue
state: closed
author: JadedBlueEyes
labels: []
assignees: []
created_at: 2018-12-06T08:54:11Z
updated_at: 2020-02-01T19:13:05Z
url: https://github.com/clap-rs/clap/issues/1393
synced_at: 2026-01-12T16:14:10Z
```

# Arguments with same index should give a compile time error

---

_@JadedBlueEyes_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`


### Rust Version

* Use the output of `rustc -V`

### Affected Version of clap

* Can be found in Cargo.lock of your project (i.e. `grep clap Cargo.lock`)
-->

### Bug or Feature Request Summary

When you specify 2 arguments with the same index, you should get a compile time descriptive error, rather than just panicking with the error `Fatal internal error. Please consider filing a bug report at https://github.com/kbknapp/clap-rs/issues`

### Expected Behavior Summary

Compile time error.

### Actual Behavior Summary

non-descriptive runtime panic

### Steps to Reproduce the issue

run the code in sample code in release mode.

### Sample Code or Link to Sample Code


```rust
extern crate clap;
use clap::App;

fn main() {
    App::new("this wil crash")
       .version("0.1.0")
       .author("DerpMarine")
       .arg(clap::Arg::with_name("ONE")
            .help("this is the fires argument")
            .required(true)
            .index(1))
       .arg(clap::Arg::with_name("TWO")
            .help("this is the secong argument with the same index.")
            .required(true)
            .index(1))
       .get_matches();
}
```
### Debug output

Compile clap with cargo features `"debug"` such as:

```toml
[dependencies]
clap = { version = "2", features = ["debug"] }
```

<details>
<summary> Debug Output </summary>
<pre>
<code>
DEBUG:clap:Parser::propagate_settings: self=PopUp CLI, g_settings=AppFlags(
    (empty)
)
DEBUG:clap:Parser::get_matches_with;
DEBUG:clap:Parser::create_help_and_version;
DEBUG:clap:Parser::create_help_and_version: Building --help
DEBUG:clap:Parser::create_help_and_version: Building --version
DEBUG:clap:ArgMatcher::process_arg_overrides:None;
DEBUG:clap:Parser::remove_overrides:[];
DEBUG:clap:Validator::validate;
DEBUG:clap:Parser::add_defaults;
DEBUG:clap:Parser::add_defaults:iter:MESSAGE: doesn't have conditional defaults
DEBUG:clap:Parser::add_defaults:iter:MESSAGE: doesn't have default vals
DEBUG:clap:Validator::validate_blacklist;
DEBUG:clap:Validator::validate_required: required=["TITLE", "MESSAGE"];
DEBUG:clap:Validator::validate_required:iter:TITLE:
DEBUG:clap:Validator::missing_required_error: extra=None
DEBUG:clap:Parser::color;
DEBUG:clap:Parser::color: Color setting...Auto
DEBUG:clap:is_a_tty: stderr=true
DEBUG:clap:Validator::missing_required_error: reqs=[
    "TITLE",
    "MESSAGE"
]
DEBUG:clap:usage::get_required_usage_from: reqs=["TITLE", "MESSAGE"], extra=None
DEBUG:clap:usage::get_required_usage_from: after init desc_reqs=[]
DEBUG:clap:usage::get_required_usage_from: no more children
DEBUG:clap:usage::get_required_usage_from: final desc_reqs=["MESSAGE", "TITLE"]
DEBUG:clap:usage::get_required_usage_from: args_in_groups=[]
DEBUG:clap:usage::get_required_usage_from:iter:TITLE:
thread 'main' panicked at 'Fatal internal error. Please consider filing a bug report at https://github.com/kbknapp/clap-rs/issues', libcore\option.rs:1000:5
stack backtrace:
   0: std::sys::windows::backtrace::set_frames
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\sys\windows\backtrace\mod.rs:104
   1: std::sys::windows::backtrace::set_frames
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\sys\windows\backtrace\mod.rs:104
   2: std::sys_common::backtrace::_print
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\sys_common\backtrace.rs:71
   3: std::sys_common::backtrace::_print
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\sys_common\backtrace.rs:71
   4: std::panicking::default_hook::{{closure}}
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\panicking.rs:211
   5: std::panicking::default_hook
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\panicking.rs:227
   6: std::panicking::rust_panic_with_hook
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\panicking.rs:477
   7: std::panicking::continue_panic_fmt
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\panicking.rs:391
   8: std::panicking::rust_begin_panic
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\panicking.rs:326
   9: core::panicking::panic_fmt
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libcore\panicking.rs:77
  10: core::option::expect_failed
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libcore\option.rs:1000
  11: clap::app::usage::get_required_usage_from
  12: clap::app::validator::Validator::validate
  13: clap::app::validator::Validator::validate
  14: clap::app::validator::Validator::validate
  15: clap::app::parser::Parser::get_matches_with
  16: clap::app::App::get_matches
  17: clap::app::App::get_matches
  18: alloc::alloc::box_free
  19: <unknown>
  20: std::rt::lang_start_internal::{{closure}}
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\rt.rs:59
  21: std::rt::lang_start_internal::{{closure}}
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\rt.rs:59
  22: panic_unwind::__rust_maybe_catch_panic
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libpanic_unwind\lib.rs:103
  23: std::panicking::try
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\panicking.rs:289
  24: std::panicking::try
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\panicking.rs:289
  25: std::panicking::try
             at /rustc/1433507eba7d1a114e4c6f27ae0e1a74f60f20de\src/libstd\panicking.rs:289
  26: main
  27: invoke_main
             at f:\dd\vctools\crt\vcstartup\src\startup\exe_common.inl:78
  28: invoke_main
             at f:\dd\vctools\crt\vcstartup\src\startup\exe_common.inl:78
  29: BaseThreadInitThunk
  30: RtlUserThreadStart

</code>
</pre>
</details>

---

_Comment by @ErichDonGubler on 2018-12-18 22:41_

Making this compile-time seems totally infeasible without either const generics landing or a whole lot of type trickery. Do you have a proposed implementation?

---

_Comment by @JadedBlueEyes on 2018-12-19 13:50_

no, but perhaps there could be something with macros or other compile time features? Finding a way to do this would be useful for many frameworks and libraries.
If there’s no way to do this, there should be, without hackery and trickery..
It’s not the highest priority, but it goes along with rust’s compile time garuntees (etc) and will save some pain.

---

_Comment by @ErichDonGubler on 2018-12-19 17:46_

@derpmarine168: After some thought, I have a question: why do you need to explicitly set the index? If you compile something like this with `cargo-script`:


```rust
//! ```cargo
//! [package]
//! edition = "2018"
//!
//! [dependencies]
//! clap = "2.32.0"
//! ```

use clap::App;

fn main() {
    let app = App::new("this wil crash")
       .version("0.1.0")
       .author("DerpMarine")
       .arg(clap::Arg::with_name("ONE")
            .help("this is the fires argument")
            .required(true))
       .arg(clap::Arg::with_name("TWO")
            .help("this is the secong argument with the same index.")
            .required(true))
       .get_matches();
    println!("app: {:#?}", app);
}
```

...then it's impossible for ONE and TWO to be mixed up in their order. The `--help` flag prints this out:

```
this wil crash 0.1.0
DerpMarine

USAGE:
    test_indices.exe <ONE> <TWO>

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    <ONE>    this is the fires argument
    <TWO>    this is the secong argument with the same index.
```

So, in this trivial example, I don't see the necessity of `index`. Is there a reason you're specifying it?

---

_Comment by @CreepySkeleton on 2020-02-01 15:24_

Unfortunately, this is not how it works. We can't emit compile time errors in cases like this, so I'm closing it. Feel free to reopen if you have a concrete implementation proposal (and not SFINAE-like one!)

---

_Closed by @CreepySkeleton on 2020-02-01 15:24_

---

_Label `C: positional args` added by @pksunkara on 2020-02-01 19:13_

---

_Label `T: RFC / question` added by @pksunkara on 2020-02-01 19:13_

---

_Label `W: wont do` added by @pksunkara on 2020-02-01 19:13_

---
