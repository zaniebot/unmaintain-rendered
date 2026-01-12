```yaml
number: 3002
title: Ability to apply a setting to all args
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-builder
  - A-derive
assignees: []
created_at: 2021-11-08T14:14:05Z
updated_at: 2023-08-01T00:55:04Z
url: https://github.com/clap-rs/clap/issues/3002
synced_at: 2026-01-12T16:14:14Z
```

# Ability to apply a setting to all args

---

_@epage_

### Use Cases

- "in our application tests [we need] to set an argument setting on all arguments programmatically just in the test context, and we had a lot of arguments so safest, cleanest, and easiest, to be able to set it with this new function.  Essentially: `app.mut_args(|arg| Some(arg.setting(ArgSettings::HideEnvValues)))`".  See #2966
- [`AppSettings::AllArgsOverrideSelf`](https://docs.rs/clap/3.0.0-beta.5/clap/enum.AppSettings.html#variant.AllArgsOverrideSelf) is arg configuration that we want to apply everywhere.  Rather than creating specialized arguments, is there a different way?
- One option for sorting arguments in help after https://github.com/clap-rs/clap/issues/2808 is to set the same display order on all arguments
- [`App::help_heading`](https://docs.rs/clap/3.0.0-beta.5/clap/struct.App.html#method.help_heading) allows applying `Arg:help_heading` to all future arguments to take advantage of the natural grouping of arguments when adding them.

Note: in some cases, users will want this to apply globally (ie to all subcommand args)

### Solution Options

*Note: this isn't a "pick one" but we can use a mixture*

- Specialize `Arg` settings in `App` (ie **do nothing**), like
  - `AppSettings::AllArgsOverrideSelf`
  - `App::help_heading`
  - A `App::next_display_order` for #2808
  - **Con::** design / implementation is needed for each solution
  - **Con:** each solution ends up different than the others
- Direct users to use reflection + `mut_arg` to manually apply a setting (ie **do nothing**, see #2976)
  - This ended up being what we used instead of #2966 which led to it being reverted in #2994
  - **Con:** Not ergonomic for `clap_derive` users (instead of calling `Parser`, they have to call `IntoApp` and `FromArgMarches`)
- Provide a `App:args_mut(&mut self) -> impl Iterator<Item = &mut Arg>`.
  - **Con:** Not ergonomic for `clap_derive` users (instead of calling `Parser`, they have to call `IntoApp` and `FromArgMarches`)
- Provide a `App::arg_setting`
  - **Con:** Only works with `ArgSettings`, so doesn't help with `AppSettings::AllArgsOverrideSelf` or `display_order`
  - **Con:** We are deprecating settings (#2717)
- `mut_args`, modeled after `mut_arg` (see #2966, #2994)
  - **Con:** The return type is confusing (should it be just `bool`?)
  - **Con:** Policy of when to return what is unclear
- Like `mut_args` above but `Fn` doesn't have a return type
  - `mut_arg` assumes you are calling it on an arg you intend to keep, so an implicit `--version` becomes explicit, making it show up when you might have disabled it
  - #2966 carried this policy forward but provided a return value to avoid adding disabled args
  - This solution assumes that mass-setting an argument is argument-agnostic and it does not imply the intention of using that argument, so it keeps `--version` implicit
- `App::default_arg(arg: Arg)` or `App::mut_default_arg` is layered underneath each arg
  - We'd have to implement support for tracking what arguments are explicitly set or not to know what is set in the "default arg" and what not to override in each arg
  - Not all settings should be layered (like `alias` or `long`)
- `App::override_arg(arg: Arg)` or `App::mut_override_arg` is layered on top of each arg
  - We'd have to implement support for tracking what arguments are explicitly set or not to know what is set in the "override arg"
  - Not all settings should be layered (like `alias` or `long`)

For when to apply these to subcommands, we could
- Have a `global_` variant of any function, like today's ``global_settings`
- Accept a `global: bool` or `scope: enum Scope { Local, Inherit }` argument in any function
- Layer Subcommands on top of their immediate parent App (see also https://github.com/clap-rs/clap/issues/2717)
  - Like with default and override args, this will require tracking what configuration is explicitly set
  - Not all settings should be layered, like `alias` (used for subcommands)

---

_Label `T: new feature` added by @epage on 2021-11-08 14:14_

---

_Label `C: args` added by @epage on 2021-11-08 14:14_

---

_Label `C: app` added by @epage on 2021-11-08 14:14_

---

_Label `C: derive macros` added by @epage on 2021-11-08 14:14_

---

_Comment by @epage on 2021-11-08 14:15_

@repi in case you have any further thoughts on this topic

---

_Referenced in [epage/clapng#234](../../epage/clapng/issues/234.md) on 2021-12-06 22:24_

---

_Label `C: args` removed by @epage on 2021-12-08 20:27_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:08_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:08_

---

_Label `S-waiting-on-decision` added by @epage on 2021-12-13 22:11_

---

_Referenced in [aobatact/clap-serde#25](../../aobatact/clap-serde/issues/25.md) on 2022-01-31 11:59_

---

_Comment by @pksunkara on 2022-02-12 09:04_

> Only works with ArgSettings, so doesn't help with AppSettings::AllArgsOverrideSelf or display_order

Hmm.. it would work if we make an `ArgSetting::ArgOverrideSelf` and then `AllArgsOverrideSelf` is basically setting that on all args in the app.

I am not sure what the scenario with display_order is though.

---

_Comment by @epage on 2022-02-14 14:06_

> Hmm.. it would work if we make an ArgSetting::ArgOverrideSelf and then AllArgsOverrideSelf is basically setting that on all args in the app.

I keep going back and forth on whether that should be the default behavior.

> I am not sure what the scenario with display_order is though.

From the use cases:

> One option for sorting arguments in help after https://github.com/clap-rs/clap/issues/2808 is to set the same display order on all arguments

---

_Referenced in [clap-rs/clap#5051](../../clap-rs/clap/pulls/5051.md) on 2023-08-01 00:54_

---

_Closed by @epage on 2023-08-01 00:55_

---
