---
number: 2740
title: Run debug_asserts during compile time for derives
type: issue
state: closed
author: glittershark
labels:
  - C-enhancement
  - A-derive
  - S-wont-fix
assignees: []
created_at: 2021-08-27T14:00:25Z
updated_at: 2022-01-11T18:27:12Z
url: https://github.com/clap-rs/clap/issues/2740
synced_at: 2026-01-07T13:12:19-06:00
---

# Run debug_asserts during compile time for derives

---

_Issue opened by @glittershark on 2021-08-27 14:00_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.4

### Describe your use case

Consider the following app:

```rust
use clap::Clap;

#[derive(Clap)]
struct Opts {
    #[clap(long, requires("foo"))]
    bar: i32
}
```

That app has a bug - the `foo` argument doesn't exist - but you don't have any way of finding out about it unless you actually *run* the application! In the real world, outside of the realm of contrived example code, this happened at $work where one parallel branch renamed an argument, while another added a new argument that required the old name - and since we didn't (previously, we do now) have a test covering option parsing, this went uncaught until we actually tried to run the binary.

### Describe the solution you'd like

I was thinking it'd be awesome if `#[derive(Clap)]` could detect usages of `requires` (and other things that reference the name of another argument) and emit a compilation error if they reference arguments that don't exist. This might get a little hairy in the presence of `#[clap(flatten)]` et al, though, and I'm not super familiar with the internals of derive macros so the information may well just not be available at this point

### Alternatives, if applicable

Maybe if it's not feasible to do this inside the macro itself (or not desired, because of the weird interaction with `#[clap(flatten)]` we could instead have some sort of custom clippy lint that detects this?

### Additional Context

_No response_

---

_Label `T: new feature` added by @glittershark on 2021-08-27 14:00_

---

_Comment by @epage on 2021-08-27 14:05_

Yes, this would not be possible with `flatten`

I was thinking maybe we could if `flatten` isn't present but that wouldn't work if we were deriving a mix-in.

It looks like we do have a [debug_assert](https://github.com/clap-rs/clap/blob/master/src/build/app/debug_asserts.rs#L128) for this case, unless there is a corner we are missing.

---

_Comment by @glittershark on 2021-08-27 14:08_

I wonder how frequently people use `requires` on a field that isn't actually defined in the struct they're defining - I'd bet it's a minority of cases. I wonder if we could get away with something like

```rust
use clap::Clap;

#[derive(Clap)]
struct Opts {
    #[clap(long, requires("foo", allow_missing))]
    bar: i32
}
```

to opt-out of the compile-time check for the field existing.

---

_Comment by @glittershark on 2021-08-27 14:10_

> It looks like we do have a [debug_assert](https://github.com/clap-rs/clap/blob/master/src/build/app/debug_asserts.rs?rgh-link-date=2021-08-27T14%3A05%3A16Z#L128) for this case, unless there is a corner we are missing.

I'm not familiar with the structure of clap - is `assert_app` run as part of the derive macro (so at compile-time) or at runtime?

---

_Comment by @epage on 2021-08-27 14:20_

> I'm not familiar with the structure of clap - is assert_app run as part of the derive macro (so at compile-time) or at runtime?

It happens at runtime with debug builds.  You could have your CI run `--help` to ensure all debug asserts pass which gives you at least some level of enforcement.

---

_Comment by @glittershark on 2021-08-27 18:33_

> You could have your CI run `--help` to ensure all debug asserts pass which gives you at least some level of enforcement.

Yeah, I just wrote a test that calls `Opts::parse_from(...)`, but it'd be nice to have it as a compile-time check without having to remember to write a test.

---

_Comment by @epage on 2021-08-27 18:34_

Thats understandable

---

_Renamed from "Could `requires` and friends validate at compile-time in the new derive macro?" to "Run debug_asserts during compile time for derives" by @pksunkara on 2021-08-31 11:02_

---

_Label `C: derive macros` added by @pksunkara on 2021-08-31 11:02_

---

_Label `P4: nice to have` added by @pksunkara on 2021-08-31 11:03_

---

_Referenced in [epage/clapng#204](../../epage/clapng/issues/204.md) on 2021-12-06 21:21_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:09_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:09_

---

_Comment by @epage on 2021-12-13 17:49_

Note: #3133 is a subset of this issue.

---

_Comment by @epage on 2021-12-13 17:49_

Due to the challenge of `flatten`, I'm inclined to close this.

Our options
- Only do the checks when `flatten` isn't present
  - This gives people a false sense of security as their app scales
  - This can cause challenges with the struct you flatten into your top-level.  You could condition the assert on deriving `Parser` instead of `Args` but we let people derive `Parser` on those
- Require cross-struct references to opt-out
  - This is adding API and implementation complexity

On top of this, this requires us to duplicate our debug asserts.

We have added a `App::debug_assert()` for use in unit tests which is more extensive than the `Opt::parse_from` mentioned before (helps with subcommands).

If there are any concerns  with us closing this, let us know!

---

_Closed by @epage on 2021-12-13 17:49_

---

_Referenced in [clap-rs/clap#3133](../../clap-rs/clap/issues/3133.md) on 2021-12-13 17:50_

---

_Referenced in [clap-rs/clap#3202](../../clap-rs/clap/issues/3202.md) on 2021-12-22 19:19_

---

_Label `S-wont-fix` added by @epage on 2022-01-11 18:27_

---
