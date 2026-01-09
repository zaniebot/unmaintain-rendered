---
number: 2475
title: clap_derive casing change stringly type
type: issue
state: open
author: pickfire
labels:
  - C-enhancement
  - M-breaking-change
  - S-waiting-on-decision
  - A-derive
assignees: []
created_at: 2021-05-09T13:47:56Z
updated_at: 2024-06-07T06:53:19Z
url: https://github.com/clap-rs/clap/issues/2475
synced_at: 2026-01-07T13:12:19-06:00
---

# clap_derive casing change stringly type

---

_Issue opened by @pickfire on 2021-05-09 13:47_

Maintainer's notes:
- With #3335 resolved, the remaining work is
  - Apply renames to `value_name` and not `id`
---
### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Rust Version

rustc 1.52.0 (88f19c6da 2021-05-03)

### Clap Version

3.0.0-beta.2

### Minimal reproducible code

```rust
use clap::Clap;

#[derive(Debug, Clap)]
struct Opts {
    #[clap(long, conflicts_with = "hello-world")] // I thought it should be hello_world
    one: Option<String>,
    #[clap(long)]
    hello_world: Option<String>,
}

fn main() {
    Opts::parse();
}
```

It is a bit confusing that conflicts_with only work with the transformed string and the issue is that it only errors on runtime, during compile-time there is no error saying that that is incorrect. Can it be checked during compile-time? It is also confusing that I put `hello_world` in the struct but I need to use `hello-world` in the option.

### Steps to reproduce the bug with the above code

`cargo run`

### Actual Behaviour

```
thread 'main' panicked at 'Argument or group 'hello_world' specified in 'conflicts_with*' for 'one' does not exist', /home/ivan/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.0-beta.2/src/build/app/debug_asserts.rs:158:13
```

### Expected Behaviour

First, using `hello_world` should work instead of `hello-world`.

Second, it should error during compile-time.

### Additional Context

_No response_

### Debug Output

```
error[E0432]: unresolved import `clap::Clap`
 --> src/main.rs:1:5
  |
1 | use clap::Clap;
  |     ^^^^^^^^^^ no `Clap` in the root

error: cannot determine resolution for the derive macro `Clap`
 --> src/main.rs:3:17
  |
3 | #[derive(Debug, Clap)]
  |                 ^^^^
  |
  = note: import resolution is stuck, try simplifying macro imports

error: cannot find attribute `clap` in this scope
 --> src/main.rs:5:7
  |
5 |     #[clap(long, conflicts_with = "hello_world")] // I thought it should be helllo_world
  |       ^^^^

error: cannot find attribute `clap` in this scope
 --> src/main.rs:7:7
  |
7 |     #[clap(long)]
  |       ^^^^

error[E0599]: no function or associated item named `parse` found for struct `Opts` in the current scope
  --> src/main.rs:12:11
   |
4  | struct Opts {
   | ----------- function or associated item `parse` not found for this
...
12 |     Opts::parse();
   |           ^^^^^ function or associated item not found in `Opts`

error: aborting due to 5 previous errors

Some errors have detailed explanations: E0432, E0599.
For more information about an error, try `rustc --explain E0432`.
error: could not compile `hello`

To learn more, run the command again with --verbose.
```

---

_Label `T: bug` added by @pickfire on 2021-05-09 13:47_

---

_Label `T: bug` removed by @pksunkara on 2021-05-09 15:36_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-05-09 15:36_

---

_Label `C: asserts` added by @pksunkara on 2021-05-09 15:36_

---

_Label `T: enhancement` added by @pksunkara on 2021-05-09 15:36_

---

_Label `Z: good first issue` added by @pksunkara on 2021-05-09 15:36_

---

_Added to milestone `3.0` by @pksunkara on 2021-05-09 15:36_

---

_Removed from milestone `3.0` by @pksunkara on 2021-05-26 10:50_

---

_Comment by @epage on 2021-10-04 18:07_

Agreed on not being in v3

`Structopt` passes `cased_name` to `Arg::with_name`, so I think its "broken" there too.  So by leaving this, we don't introduce any more of a migration problem than if we change it now.  

---

_Comment by @pksunkara on 2021-10-09 00:04_

I am not exactly sure what the expected behaviour should be.

---

_Comment by @epage on 2021-10-09 10:27_

> I am not exactly sure what the expected behaviour should be.

As a user solely of `clap_derive`, I do not know of the existence of the "name" field, just what my variables are.  I have a variable named `hello_world`, I assume when working with `conflicts_with`, I would pass in that variable name.

---

_Added to milestone `4.0` by @epage on 2021-10-09 10:27_

---

_Label `E: breaking change` added by @epage on 2021-10-19 16:34_

---

_Referenced in [epage/clapng#186](../../epage/clapng/issues/186.md) on 2021-12-06 21:14_

---

_Label `C: asserts` removed by @epage on 2021-12-08 19:58_

---

_Label `A-derive` added by @epage on 2021-12-08 19:58_

---

_Referenced in [clap-rs/clap#3165](../../clap-rs/clap/issues/3165.md) on 2021-12-13 17:01_

---

_Referenced in [clap-rs/clap#3171](../../clap-rs/clap/issues/3171.md) on 2021-12-14 00:31_

---

_Referenced in [clap-rs/clap#3335](../../clap-rs/clap/issues/3335.md) on 2022-01-24 17:00_

---

_Referenced in [clap-rs/clap#3453](../../clap-rs/clap/pulls/3453.md) on 2022-02-11 20:13_

---

_Comment by @epage on 2022-02-15 17:22_

With #3453, I expect `rename_all` for args will change the value_name and not what is passed to `Arg::new`

---

_Referenced in [clap-rs/clap#3513](../../clap-rs/clap/issues/3513.md) on 2022-02-27 02:12_

---

_Comment by @epage on 2022-05-06 21:32_

Needing to re-think this

`rename_all`:
- Args:
  - Changes the `id`, `long`, and `short`
  - value_name is hard coded to screaming-case
  - Without `rename_all`, we already apply a default casing for `id`, `long`, and `short`
- Subcommands:
  - Changes the `name` which serves as both the user value and the id
- ArgEnums:
  - Changes the `name`

So what is
- The most commonly wanted behavior for `rename_all`
- The most obvious behavior for `rename_all`

For example, this issue talks about the `id` becoming morphed but the equivalent of the id for subcommands can't be passed up.  If we do this, then it will be inconsistent.

`rename_all` is generally meant to workaround the language's casing from mismatching with the problem domain so from that perspective, renaming the `id` can make sense since its happening *before* it gets to the problem domain.

short casing can be handled naiively (verbatim) but since long casing has to change anyways, we need a way to control that, right?

value name seems the most likely to need a `rename_all` because its pretty split between SCREAMING_CASE and kebab-case.

With this level of uncertainty, I'm going to hold off for now.

---

_Removed from milestone `4.0` by @epage on 2022-05-06 21:32_

---

_Label `E-easy` removed by @epage on 2022-05-06 21:32_

---

_Label `S-waiting-on-decision` added by @epage on 2022-05-06 21:32_

---

_Comment by @epage on 2022-06-08 16:06_

#3709 proposes more specific rename attributes

---

_Added to milestone `5.0` by @epage on 2022-06-08 16:06_

---

_Label `:money_with_wings: $5` removed by @pksunkara on 2024-06-07 06:53_

---

_Referenced in [doy/rbw#198](../../doy/rbw/issues/198.md) on 2024-07-28 20:09_

---
