```yaml
number: 3266
title: "Support `CommandWithArguments` hint for options"
type: issue
state: open
author: tmccombs
labels:
  - C-enhancement
  - A-completion
  - S-triage
assignees: []
created_at: 2022-01-07T09:24:49Z
updated_at: 2025-12-10T07:30:05Z
url: https://github.com/clap-rs/clap/issues/3266
synced_at: 2026-01-12T16:14:14Z
```

# Support `CommandWithArguments` hint for options

---

_@tmccombs_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.5

### Describe your use case

I have an argument like:

```rust
            Arg::new("exec-batch")
                .long("exec-batch")
                .short('X')
                .min_values(1)
                .allow_hyphen_values(true)
                .value_terminator(";")
                .value_name("cmd")
```

Ideally, I would be able to use `.value_hint(ValueHint::CommandWithArguments)` for this, but since it isn't a positional argument, I can't.

### Describe the solution you'd like

Allow using `ValueHint::CommandWithArguments` for non-positional arguments, as long as:

1. It accepts multiple values
2. there is a `value_terminator` supplied

### Alternatives, if applicable

possibly have a seperate hint type for this use case?

### Additional Context

_No response_

---

_Label `C-enhancement` added by @tmccombs on 2022-01-07 09:24_

---

_Comment by @epage on 2022-01-07 14:45_

In what way are you not seeing this supported?

---

_Label `A-completion` added by @epage on 2022-01-07 14:45_

---

_Label `S-triage` added by @epage on 2022-01-07 14:45_

---

_Comment by @tmccombs on 2022-01-08 07:02_

If I add `.value_hint(ValueHint::CommandWithArguments)` I get:

``` 
thread 'main' panicked at 'App fd: Argument 'exec-batch' has hint CommandWithArguments and must be positional.', /home/thayne/.cargo/registry/src/github.com-1ecc6299db9ec823/clap-3.0.5/src/build/app/debug_asserts.rs:210:13
```

There is a debug_assert that requires the argument to be positional if it has the `CommandWithArguments` hint.
This is also mentioned in the documentation:


> the argument must be a positional argument and have `.multiple_values(true)` and App must use `AppSettings::TrailingVarArg`



---

_Comment by @tmccombs on 2022-01-08 07:24_

I believe in this case the zsh completion would need to be different. Instead of using `_cmdambivalent` it would need to use something like:

`" _command_names -e:*{}::args: _normal"` where `{}` is replaced with the escaped value of the terminator. 

That is, the first value is completed as a command name, and then every value after up to and including the value that matches the terminator are completed using `_normal` but using excluding the command up to the first value passed to the option.

See https://github.com/clap-rs/clap/blob/453356e044db898d6ad4dd4578c2c50583913615/clap_complete/src/shells/zsh.rs#L392

---

_Comment by @epage on 2022-01-11 16:56_

Sorry, I somehow had missed that part of the code when searching

> I believe in this case the zsh completion would need to be different. Instead of using `_cmdambivalent` it would need to use something like:
> 
> `" _command_names -e:*{}::args: _normal"` where `{}` is replaced with the escaped value of the terminator.
> 
> That is, the first value is completed as a command name, and then every value after up to and including the value that matches the terminator are completed using `_normal` but using excluding the command up to the first value passed to the option.
> 
> See
> 
> https://github.com/clap-rs/clap/blob/453356e044db898d6ad4dd4578c2c50583913615/clap_complete/src/shells/zsh.rs#L392

Sounds like loosening `CommandWithArguments` won't help unless we manually implement the handling.

As an alternative, would per-value `ValueHint`s help with this (something brought up in https://github.com/clap-rs/clap/issues/2683)?  The first value would be `_cmdambivalent` with the next being `_normal`.  That being the last value hint, we'd then repeat it for all future value hints in that occurrence.

---

_Comment by @tmccombs on 2022-01-13 06:41_

> As an alternative, would per-value ValueHints help with this (something brought up in #2683)? The first value would be _cmdambivalent with the next being _normal. That being the last value hint, we'd then repeat it for all future value hints in that occurrence.

I'm guessing you ment `_command_names -` not `_cmdambivalent`? 

That would probably be tricky to do. The arguments that use `_normal` should use the command name argument in order to determine completion. In the example I gave above that is accomplished by using the double colon form. It would also need to be aware of the terminator in order to know when to stop accepting arguments.

---

_Referenced in [clap-rs/clap#4612](../../clap-rs/clap/pulls/4612.md) on 2023-01-09 10:58_

---

_Comment by @epage on 2025-11-03 21:59_

For native completions, our issue for this is #5653.

---

_Referenced in [clap-rs/clap#6016](../../clap-rs/clap/issues/6016.md) on 2025-11-03 21:59_

---

_Renamed from "Support `CommandWithArguments` hint for " to "Support `CommandWithArguments` hint for options" by @tmccombs on 2025-12-10 07:30_

---

_Referenced in [sharkdp/fd#1857](../../sharkdp/fd/issues/1857.md) on 2025-12-10 07:34_

---
