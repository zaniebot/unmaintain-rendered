---
number: 1965
title: Example code of README.md using builder pattern results in panic
type: issue
state: closed
author: asteding
labels:
  - C-bug
  - E-easy
assignees: []
created_at: 2020-06-08T05:22:27Z
updated_at: 2020-06-09T05:50:37Z
url: https://github.com/clap-rs/clap/issues/1965
synced_at: 2026-01-10T01:27:10Z
---

# Example code of README.md using builder pattern results in panic

---

_Issue opened by @asteding on 2020-06-08 05:22_

### Make sure you completed the following tasks

- [x] Searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] Searched the closes issues

### Code

```rust
// (Full example with detailed comments in examples/01a_quick_example.rs)
//
// This example demonstrates clap's "builder pattern" method of creating arguments
// which the most flexible, but also most verbose.
use clap::{Arg, App};

fn main() {
    let matches = App::new("My Super Program")
        .version("1.0")
        .author("Kevin K. <kbknapp@gmail.com>")
        .about("Does awesome things")
        .arg(Arg::new("config")
            .short('c')
            .long("config")
            .value_name("FILE")
            .about("Sets a custom config file")
            .takes_value(true))
        .arg(Arg::new("INPUT")
            .about("Sets the input file to use")
            .required(true)
            .index(1))
        .arg(Arg::new("v")
            .short('v')
            .multiple(true)
            .about("Sets the level of verbosity"))
        .subcommand(App::new("test")
            .about("controls testing features")
            .version("1.3")
            .author("Someone E. <someone_else@other.com>")
            .arg(Arg::new("debug")
                .short('d')
                .about("print debug information verbosely")))
        .get_matches();

    // Same as above examples...
}
```

### Steps to reproduce the issue

1. Run `cargo run`

### Version

* **Rust**: rustc 1.45.0-nightly (47c3158c3 2020-06-04)
* **Clap**: 3.0.0-beta.1

### Actual Behavior Summary

If i use the above example code from the clap-rs [README.md](https://github.com/clap-rs/clap/blob/master/README.md#using-builder-pattern) i get a
thread 'main' panicked at 'Argument names must be unique, but '' is in use by more than one argument or group'

### Expected Behavior Summary

I think *this* should happen instead.
error: The following required arguments were not provided:
    <INPUT>

USAGE:
    clap_bug [FLAGS] [OPTIONS] <INPUT> [SUBCOMMAND]

### Additional context



### Debug output

Compile clap with `debug` feature:

```toml
[dependencies]
clap = { version = "*", features = ["debug"] }
```

The output may be very long, so feel free to link to a gist or attach a text file

<details>
<summary> Debug Output </summary>
<pre>
<code>

Paste Debug Output Here
[            clap::build::app]  App::_do_parse
[            clap::build::app]  App::_build
[            clap::build::app]  App::_derive_display_order:My Super Program
[            clap::build::app]  App::_derive_display_order:test
[            clap::build::app]  App::_create_help_and_version
[            clap::build::app]  App::_create_help_and_version: Building --help
[            clap::build::app]  App::_create_help_and_version: Building --version
[            clap::build::app]  App::_create_help_and_version: Building help
[            clap::build::app]  App::_debug_asserts
[            clap::build::arg]  Arg::_debug_asserts:
thread 'main' panicked at 'Argument names must be unique, but '' is in use by more than one argument or group', /home/andi/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-beta.1/src/build/app/mod.rs:1613:13
note: run with `RUST_BACKTRACE=1` environment variable to display a backtrace
</code>
</pre>
</details>


---

_Label `T: bug` added by @asteding on 2020-06-08 05:22_

---

_Added to milestone `3.0` by @pksunkara on 2020-06-08 08:48_

---

_Label `D: easy` added by @pksunkara on 2020-06-08 08:48_

---

_Label `Z: good first issue` added by @pksunkara on 2020-06-08 08:48_

---

_Comment by @asteding on 2020-06-08 10:18_

After some research it looks like the field [arg.name](https://github.com/clap-rs/clap/blob/master/src/build/arg/mod.rs#L60) is always `""`

So 
```rust
assert!(
    two_args_of(|x| x.name == arg.name).is_none(),
    "Argument names must be unique, but '{}' is in use by more than one argument or group",
    arg.name,
    );
```
returns always a `false`.

Due to my still little knowledge of Rust i'd be happy to help and learn.
Would someone provide some information where i should look? Probably I need to investigate

```rust
impl<'help> Arg<'help> {
    /// @TODO @p2 @docs @v3-beta1: Write Docs
    pub fn new<T: Key + ToString>(t: T) -> Self {
        Arg {
            id: t.into(),
            disp_ord: 999,
            unified_ord: 999,
            ..Default::default()
        }
    }
}
```
which is lacking a name field too.

---

_Comment by @CreepySkeleton on 2020-06-08 11:54_

In the first snippet, the condition must be `x.id == arg.id`.

Where did you get the second snippet from? I can't find it in the codebase.

---

_Comment by @pksunkara on 2020-06-08 11:56_

I don't get it. Our tests should be failing if the condition is wrong.

On Mon, Jun 8, 2020, 13:55 CreepySkeleton <notifications@github.com> wrote:

> In the first snippet, the condition must be x.id == arg.id.
>
> Where did you get the second snippet from? I can't find it in the codebase.
>
> —
> You are receiving this because you are subscribed to this thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/clap-rs/clap/issues/1965#issuecomment-640557191>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AABKU356EBAPR4DDAIDJSZ3RVTGSDANCNFSM4NX7WYWA>
> .
>


---

_Comment by @asteding on 2020-06-08 11:58_

Hello @CreepySkeleton ,
it's from [here](https://github.com/clap-rs/clap/blob/v3.0.0-beta.1/src/build/arg/mod.rs#L137) 
It also looks like it's already fixed on the master branch?

---

_Comment by @CreepySkeleton on 2020-06-08 12:04_

Ah, yes. There used to be `Arg::new` method different from `with_name`.  We eventually decided to ditch the old `new` and just rename `with_name` to `new`. it happened after  the `beta` release.

This is fixed in master. The condition must be fixed though - it's not _wrong_, but it would be "more correct" to check `id`. We would appreciate such PR!

---

_Comment by @asteding on 2020-06-08 12:10_

Alright.
Thanks for the feedback. 
I'll set up a PR if nobody minds? :-)

---

_Referenced in [clap-rs/clap#1966](../../clap-rs/clap/pulls/1966.md) on 2020-06-08 12:37_

---

_Closed by @asteding on 2020-06-09 05:50_

---
