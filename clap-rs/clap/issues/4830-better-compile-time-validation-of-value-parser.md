```yaml
number: 4830
title: better compile time validation of value_parser types
type: issue
state: open
author: rbtcollins
labels:
  - C-enhancement
assignees: []
created_at: 2023-04-10T20:31:18Z
updated_at: 2023-04-10T20:55:40Z
url: https://github.com/clap-rs/clap/issues/4830
synced_at: 2026-01-12T16:14:16Z
```

# better compile time validation of value_parser types

---

_@rbtcollins_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

3.2.23

### Describe your use case

Trying to parse `toolchain` arguments for `rustup` as a custom type rather than just String. We have about 5 different sorts of toolchains that can be parsed. Official ones from channels, custom names, official-or-custom, 'none'-or-official-or-custom.

When the clap value parser type (e.g. CustomToolchainNameParser) and the code querying the argument have a mismatched type, there is a silent failure (or perhaps we're misusing things?)

e.g. in the install code path I had this:
```
    if let Ok(Some(names)) = m.try_get_many::<PartialToolchainDesc>("toolchain") {
```

and the definition for install has this :
```
                        .value_parser(NonEmptyStringValueParser)
```

Then the type id check in `verify_arg_t` fails, but this is then caught in `try_get_many` - which I *think* we're using to handle optional values. But this is a different sort of failure - the failure is in the code that was compiled, and should be detectable at compile time.

### Describe the solution you'd like

Our buggy code should fail to compile. Or perhaps we're doing things wrong and the docs should help us understand that.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @rbtcollins on 2023-04-10 20:31_

---

_Comment by @epage on 2023-04-10 20:46_

Except in some specific cases (#3912, #3864), the derive API handles all of this for you at compile-time

When using `ArgMatches::get_*`, we will panic at runtime.  This just requires exercising all `ArgMatches::get_*` calls in tests and you know things are valid.  You could use `try_get_one::<CustomToolchainNameParser as TypedValueParser>::Value>("toolchain")` to help (if those were even the right types and args involved, I also don't know if you can shorten that for associated value accesses)

Instead of either of the above, rustup is using `ArgMatches::try_get_*` and its unclear from the Issue what rustup does in the case of[`Err(MatchesError::Downcast {...} }) `](https://docs.rs/clap/latest/clap/parser/enum.MatchesError.html).

I'm not too sure what more clap can do to help in this case or how the docs could be modified to help people who have explicitly chosen to go off the beaten path and then (making assumptions here) ignoring the error we are reporting about this case.

---

_Comment by @rbtcollins on 2023-04-10 20:55_

I'm very happy to change rustup to use clap better - its a very old codebase and we have a lot of places things could be better :).

Poking at this further, I've found 

```
Mismatch between definition and access of `toolchain`. Could not downcast to rustup::toolchain::ResolvableToolchainName, need to downcast to rustup::dist::dist::PartialToolchainDesc 
```

happens at runtime, which is great.

I think the original failure occurred after I changed the get_* calls, but had not yet added the value_parser() calls to the clap App definition.

---

_Referenced in [clap-rs/clap#4626](../../clap-rs/clap/issues/4626.md) on 2023-06-15 20:25_

---
