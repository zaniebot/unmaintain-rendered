---
number: 3269
title: clap_derive Arg methods with flattened
type: issue
state: closed
author: pinkforest
labels:
  - C-enhancement
  - A-derive
  - S-waiting-on-author
assignees: []
created_at: 2022-01-08T03:15:07Z
updated_at: 2022-05-04T18:21:05Z
url: https://github.com/clap-rs/clap/issues/3269
synced_at: 2026-01-10T01:27:37Z
---

# clap_derive Arg methods with flattened

---

_Issue opened by @pinkforest on 2022-01-08 03:15_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.5

### Describe your use case

I got two example use-cases

I came across this initially at [cargo-clap/issues/27](https://github.com/crate-ci/clap-cargo/issues/27) and since it's use requires the use of flattened this should have some consideration what might be supported and document them for the future reference.

### global on Enum Subcommand

Allow using [global](https://docs.rs/clap/latest/clap/struct.Arg.html#method.global) to reduce amount of code e.g. in enum Subcommand
```
#[derive(Subcommand)]
#[clap(setting(AppSettings::DeriveDisplayOrder))]
pub enum GeigerCommands {
    /// Geiger with build tree
    Build {
        #[clap(flatten)]
        workspace: clap_cargo::Workspace,
    },
    /// Geiger with test tree
    Test {
        #[clap(flatten)]
        workspace: clap_cargo::Workspace,
    }
}
```
### Override members of pre-flattened arguments

Also f.ex. [default_value](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_value) & co - it would require getting rid of flattened and a way to pass "overrides" against members

e.g. cargo build to set the [help](https://docs.rs/clap/latest/clap/struct.Arg.html#method.help) that differs between build:

```
        --test <NAME>...             Build only the specified test target
        --tests                      Build all tests
```

and vs cargo test:

```
        --test <NAME>...             Test only the specified test target
        --tests                      Test all tests
```

Also [env](https://docs.rs/clap/latest/clap/struct.Arg.html#method.env) variant for this would be cool but I would think these should be hardcoded there - not sure.

Plus it would be nice to group workspace / manifest under their own heading with [help_heading](https://docs.rs/clap/latest/clap/struct.Arg.html#method.help_heading)

### Describe the solution you'd like

First it would be great to get a support statement and document them in the [derive reference](https://github.com/clap-rs/clap/blob/master/examples/derive_ref/README.md) perhaps

Relevant with flatten might be:

- [ ] - [index](https://docs.rs/clap/latest/clap/struct.Arg.html#method.index) 
- [ ] - [last](https://docs.rs/clap/latest/clap/struct.Arg.html#method.last)
- [ ] - [required](https://docs.rs/clap/latest/clap/struct.Arg.html#method.required)
- [ ] - [requires](https://docs.rs/clap/latest/clap/struct.Arg.html#method.requires)
- [ ] - [is_set](https://docs.rs/clap/latest/clap/struct.Arg.html#method.is_set)
- [ ] - [setting](https://docs.rs/clap/latest/clap/struct.Arg.html#method.is_set)
- [ ] - [unset_setting](https://docs.rs/clap/latest/clap/struct.Arg.html#method.is_set)
- [ ] - [env](https://docs.rs/clap/latest/clap/struct.Arg.html#method.env)
- [ ] - [help](https://docs.rs/clap/latest/clap/struct.Arg.html#method.help)
- [ ] - [help_heading](https://docs.rs/clap/latest/clap/struct.Arg.html#method.help_heading)
- [ ] - [hide](https://docs.rs/clap/latest/clap/struct.Arg.html#method.hide)
- [ ] - [hide_possible_values](https://docs.rs/clap/latest/clap/struct.Arg.html#method.hide_possible_values)
- [ ] - [hide_default_value](https://docs.rs/clap/latest/clap/struct.Arg.html#method.hide_default_value)
- [ ] - [hide_env](https://docs.rs/clap/latest/clap/struct.Arg.html#method.hide_env)
- [ ] - [hide_env_values](https://docs.rs/clap/latest/clap/struct.Arg.html#method.hide_env_values)
- [ ] - [hide_short_help](https://docs.rs/clap/latest/clap/struct.Arg.html#method.hide_short_help)
- [ ] - [hide_long_help](https://docs.rs/clap/latest/clap/struct.Arg.html#method.hide_long_help)
- [ ] - [group](https://docs.rs/clap/latest/clap/struct.Arg.html#method.group)
- [ ] - [groups](https://docs.rs/clap/latest/clap/struct.Arg.html#method.groups)
- [ ] - [default_value_if](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_value_if)
- [ ] - [default_value_if_os](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_value_if_os)
- [ ] - [default_value_ifs](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_value_ifs)
- [ ] - [default_value_ifs_os](https://docs.rs/clap/latest/clap/struct.Arg.html#method.default_value_ifs_os)
- [ ] - [required_unless_present](https://docs.rs/clap/latest/clap/struct.Arg.html#method.required_unless_present)
- [ ] - [required_unless_present_all](https://docs.rs/clap/latest/clap/struct.Arg.html#method.required_unless_present_all)
- [ ] - [required_unless_present_any](https://docs.rs/clap/latest/clap/struct.Arg.html#method.required_unless_present_any)
- [ ] - [required_if_eq](https://docs.rs/clap/latest/clap/struct.Arg.html#method.required_if_eq)
- [ ] - [required_if_eq_any](https://docs.rs/clap/latest/clap/struct.Arg.html#method.required_if_eq_any)
- [ ] - [required_if_eq_all](https://docs.rs/clap/latest/clap/struct.Arg.html#method.required_if_eq_all)
- [ ] - [required_if](https://docs.rs/clap/latest/clap/struct.Arg.html#method.requires_if)
- [ ] - [required_ifs](https://docs.rs/clap/latest/clap/struct.Arg.html#method.requires_ifs)
- [ ] - [requires_all](https://docs.rs/clap/latest/clap/struct.Arg.html#method.requires_all)
- [ ] - [conflicts_with](https://docs.rs/clap/latest/clap/struct.Arg.html#method.conflicts_with)
- [ ] - [conflicts_with_all](https://docs.rs/clap/latest/clap/struct.Arg.html#method.conflicts_with_all)
- [ ] - [overrides_with](https://docs.rs/clap/latest/clap/struct.Arg.html#method.overrides_with)
- [ ] - [overrides_with_all](https://docs.rs/clap/latest/clap/struct.Arg.html#method.overrides_with_all)


### Alternatives, if applicable

It would be nice if the functions get implemented for at least some in the future :)

### Additional Context

I am migrating cargo-geiger to clap, clap-cargo and clap-cargo items require using flatten.

---

_Label `C-enhancement` added by @pinkforest on 2022-01-08 03:15_

---

_Referenced in [crate-ci/clap-cargo#27](../../crate-ci/clap-cargo/issues/27.md) on 2022-01-08 03:16_

---

_Renamed from "Arg Methods with flattened" to "clap_derive Arg methods with flattened" by @pinkforest on 2022-01-08 03:16_

---

_Referenced in [crate-ci/clap-cargo#28](../../crate-ci/clap-cargo/pulls/28.md) on 2022-01-08 03:32_

---

_Referenced in [clap-rs/clap#3189](../../clap-rs/clap/issues/3189.md) on 2022-01-08 04:07_

---

_Comment by @epage on 2022-01-09 01:59_

> Allow using global to reduce amount of code e.g. in enum Subcommand
> ...
> Relevant with flatten might be:

*(focusing on the "apply this method to everything flattened)*

The problem with this is we can't really communicate in the derive code between these different derives (on top of people hand-implementing).  This means we have to do all of the communication through the trait at runtime.  This puts a large burden on the trait implementation (which could be quite brittle from an API stability perspective) and there s a lot of complexity to get these interactions working correctly. 

So far we have been rejecting features dependent on this.

>  Also f.ex. default_value & co - it would require getting rid of flattened and a way to pass "overrides" against members

*(focusing on overriding a specific argument from a flatten)*

Building this into the flatten construct has an extra level of complexity because of having to specify which argument an override applies to.

We do have the `App::mut_arg` method that can be used for modifying an argument.  struct-level attributes are almost all evaluated after arguments, so the ordering would work.  So you can do some level of overrides like
```rust
#[derive(clap::Parser)]
#[clap(mut_arg("test", |arg| arg.help("Test only the specified test target"))]
struct Cli {
...
}
```

One option might be to allow `mut_arg` on the flatten call so it is "close" to where it is needed.  We might allow other app methods in this context to help with issues like https://github.com/clap-rs/clap/issues/1807 though those might be **before** adding the flattened arguments though if we add a `#[clap::app()]` attribute, that could allow for controlling ordering.

---

_Comment by @epage on 2022-01-09 02:02_

btw customizing `clap-cargo` is something I have been thinking about.  One other thought I had is to use macros to create distinct versions of the struct but that will lead to code bloat and make it hard to interact with them the same way.

---

_Label `A-derive` added by @epage on 2022-01-11 17:08_

---

_Label `S-waiting-on-author` added by @epage on 2022-01-11 17:08_

---

_Comment by @taladar on 2022-01-13 14:07_

It would be nice if this could at least be implemented for things like help_heading so we could easily group everything flattened in one spot under one help heading.

---

_Comment by @epage on 2022-01-13 15:00_

`help_heading` is one of the few exceptions of what works because of how its implemented (`App::help_heading`).  See [this test](https://github.com/clap-rs/clap/blob/master/tests/derive/help.rs#L167).

The [derive reference](https://github.com/clap-rs/clap/blob/v3.0.7/examples/derive_ref/README.md#arg-attributes) was previously updated to mention this.

---

_Comment by @epage on 2022-02-09 00:07_

It sounds like this issue boils down to
- "apply this method to everything flattened": this is not being resolved due to derive limitations
- "focusing on overriding a specific argument from a flatten": this is possible with `App::mut_arg`

Did I miss anything?  If not, I'm inclined to close this issue

---

_Comment by @epage on 2022-05-04 18:21_

Without any further updates, I'm moving forward with closing this.  If more details become available, we can re-evaluate.

---

_Closed by @epage on 2022-05-04 18:21_

---
