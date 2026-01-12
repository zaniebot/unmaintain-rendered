```yaml
number: 4869
title: "4.0 regression: dashes are not accepted any more"
type: issue
state: closed
author: est31
labels:
  - C-bug
assignees: []
created_at: 2023-04-28T20:02:18Z
updated_at: 2023-04-29T01:14:59Z
url: https://github.com/clap-rs/clap/issues/4869
synced_at: 2026-01-12T16:14:16Z
```

# 4.0 regression: dashes are not accepted any more

---

_@est31_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Rust Version

-

### Clap Version

4.0

### Minimal reproducible code

```rust
use clap::Parser;
use clap::CommandFactory;

fn main() {
    let matches = OptFoo::command().try_get_matches_from(std::env::args()).unwrap();

    println!("matches foo-bar: {:?}", matches.try_get_one::<bool>("foo-bar"));
    println!();
    println!("matches someflag: {:?}", matches.try_get_one::<bool>("someflag"));
}

#[derive(Parser, Debug)]
#[allow(dead_code)]
struct OptFoo {
	#[clap(long, action)]
	foo_bar: bool,
	#[clap(long, action)]
	someflag: bool,
}
```


### Steps to reproduce the bug with the above code

Do `cargo run -- --foo-bar --someflag`

### Actual Behaviour

It prints:
```
matches foo-bar: Err(UnknownArgument)

matches someflag: Ok(Some(true))
```

### Expected Behaviour

It prints:

```
matches foo-bar: Ok(Some(true))

matches someflag: Ok(Some(true))
```

This is what Clap 3.0 did, you can confirm this by running the example with Clap 3.0, i've written it to compile fine there as well. This, this is a regression of 4.0.0.

### Additional Context

This caused https://github.com/est31/cargo-udeps/issues/151 where I list all the `cargo check` flags in a struct that derives `Parser`, so that they can be passed on to cargo, but where cargo internally does the equivalent of `arg_matches.try_get_one::<bool>("all-targets")` (it does it via an extension trait on ArgMatches, via a `fn flag(&self, name: &str) -> bool` function).

### Debug Output

_No response_

---

_Label `C-bug` added by @est31 on 2023-04-28 20:02_

---

_Renamed from "4.0 regression: underscores are not accepted any more" to "4.0 regression: dashes are not accepted any more" by @est31 on 2023-04-28 20:05_

---

_Comment by @est31 on 2023-04-28 20:05_

A workaround is to add `name = "thing-without-underscores-but-dashes"` as an attribute, [which I did in cargo-udeps](https://github.com/est31/cargo-udeps/commit/0d4889b18b9fa743c6618bf76d116fa39537606e). It's not really nice though that you have to go through the entire list and check for names with `_` inside.

---

_Referenced in [est31/cargo-udeps#151](../../est31/cargo-udeps/issues/151.md) on 2023-04-28 20:08_

---

_Comment by @epage on 2023-04-28 20:26_

We called this change out in the breaking changes for 4.0 under the "subtle" section, meaning these were the high priority ones to review when upgrading

> (derive) Leave Arg::id as verbatim casing, requiring updating of string references to other args like in conflicts_with or requires (#3282)

As for the workaroumd, it is better to use `id` rather than `name`.  Its an accident that `name`i is allowed and we'll be removing it in clap v5

---

_Comment by @est31 on 2023-04-28 20:42_

Note, that changelog line is very terse and unless you know clap's internals, or you have been pointed towards it, you won't really be able to derive actionable advice from it.

Also, even if it is pointed out, maybe it's not the best idea to have that change, given it makes such hybrid uses harder.

> it is better to use `id` rather than `name`.

Thanks, will do!

Question: if #3709 is implemented, will I be able to just specify that?

---

_Comment by @epage on 2023-04-28 21:03_

> Also, even if it is pointed out, maybe it's not the best idea to have that change, given it makes such hybrid uses harder.

The change was made to improve the pure-derive workflow.  If you want to declare an arg to conflict with another arg, you now refer to it by the field's name, not what the long name or the value name would be, if it exists.  This improves discoverability of the behavior.

> Question: if https://github.com/clap-rs/clap/issues/3709 is implemented, will I be able to just specify that?

We have no plans for controlling naming of `id`, for the reason given above.

---

_Comment by @est31 on 2023-04-29 00:57_

Thanks for informing me, and thanks for the advice. I guess it's a tradeoff decision in the end, although of course I'm not happy that the only way to use arguments both in cargo and cargo-udeps is to either make cargo-udeps not use derives, or to put an explicit `id` attribute everywhere. The bug in cargo-udeps was especially hard to debug because i didn't know if the bug was in cargo, cargo-udeps or clap. I guess most projects have it easier, given that most projects probably only expose a CLI API surface in a fraction of the size that cargo uses.

---

_Closed by @est31 on 2023-04-29 00:57_

---

_Comment by @epage on 2023-04-29 01:14_

For my own projects, I recreate the cargo CLI, rather than reuse it (see `clap-cargo`).  Granted, if you are using cargo-as-a-library for other things then that might make sense.  I tend to avoid that as its not a well supported workflow (intentionally at the moment)

---

_Referenced in [bbusse/waystream#2](../../bbusse/waystream/issues/2.md) on 2023-07-11 06:01_

---
