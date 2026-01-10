---
number: 5293
title: "Caller-defined `complete` subcommand for rust-native completions"
type: issue
state: open
author: liuhangbin
labels:
  - C-enhancement
  - M-breaking-change
  - E-medium
  - A-completion
assignees: []
created_at: 2024-01-09T03:19:58Z
updated_at: 2024-08-12T17:24:28Z
url: https://github.com/clap-rs/clap/issues/5293
synced_at: 2026-01-10T01:28:09Z
---

# Caller-defined `complete` subcommand for rust-native completions

---

_Issue opened by @liuhangbin on 2024-01-09 03:19_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.4.11

### Describe your use case

Currently, the dynamic completion hides the parameter from the program's parameters and only shows with `$ cmd complete -h`, which is good. But it hard-codes the parameter with `complete`, which may conflict with or be similar to the program's existing parameter. This makes `$ cmd c` not work. It would be good if the user could define the complete parameter themself.

### Describe the solution you'd like

Maybe add a new function `fn set_comp_words` in `impl CompleteCommand`. Then we can define the `comp_words` ourself.

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @liuhangbin on 2024-01-09 03:19_

---

_Referenced in [retis-org/retis#329](../../retis-org/retis/pulls/329.md) on 2024-01-09 08:43_

---

_Comment by @epage on 2024-01-09 16:39_

To clarify, you are wanting to replace the `complete` subcommand name, right?

That isn't too clear from the proposed solution as its unclear what `comp_words` is.

> The clap maintainer also said that dynamic completion will replace static completion in future. So I chose to use dynamic completion, although it only supports bash and fish right now.

dynamic completions are also very early in their development so I also generally recommend people use static completions at this time but I encourage fixes and improvements on dynamic completions.

---

_Comment by @liuhangbin on 2024-01-10 02:23_

> To clarify, you are wanting to replace the `complete` subcommand name, right?

Yes
> 
> That isn't too clear from the proposed solution as it's unclear what `comp_words` is.

In `clap_complete/src/dynamic/shells/mod.rs` it defines
```rust
/// Generally used via [`CompleteCommand`]
#[derive(clap::Args)]
#[command(arg_required_else_help = true)]
#[command(group = clap::ArgGroup::new("complete").multiple(true).conflicts_with("register"))]
#[allow(missing_docs)]
#[derive(Clone, Debug)]
pub struct CompleteArgs {
    /// Specify shell to complete for
    #[arg(long)]
    shell: Shell,

    /// Path to write completion-registration to
    #[arg(long, required = true)]
    register: Option<std::path::PathBuf>,

    #[arg(raw = true, hide_short_help = true, group = "complete")]
    comp_words: Vec<OsString>,
}
```

So I think adding a function in `impl CompleteCommand` to set the `comp_words` may fix this issue.

> 
> > The clap maintainer also said that dynamic completion will replace static completion in future. So I chose to use dynamic completion, although it only supports bash and fish right now.
> 
> dynamic completions are also very early in their development so I also generally recommend people use static completions at this time but I encourage fixes and improvements on dynamic completions.

The dynamic completions could `hide` from the existing parameters, which is very useful. If we use the static completions and add a, e.g. `generate` parameter, we need to remove it if switch to dynamic completions.


---

_Comment by @epage on 2024-01-10 03:53_

> So I think adding a function in impl CompleteCommand to set the comp_words may fix this issue.

Its not clear to me why someone would want to use this struct but override `comp_words`.  `comp_words` is meant to come from the shell, not the calling program, to define what we should complete.

Are you trying to use `CompleteCommand` as a completion API-only and not for flattening into your CLI?  If so, that isn't its role.  The logic it implements is fairly light with most of the behavior coming from elsewhere.

> The dynamic completions could hide from the existing parameters, which is very useful. If we use the static completions and add a, e.g. generate parameter, we need to remove it if switch to dynamic completions.

It sounds like the concern is with compatibility being broken with how you currently do completions?  I can understand that.  However, the current built-in interface isn't stable yet either.  Its also likely that you could make a generate parameter used for static completions that will then work for dynamic completions.  The shell support might be a little different but you can choose now for a subset of shells (fig is one that I know won't work with dynamic completions).

---

_Comment by @liuhangbin on 2024-01-10 05:38_

> > So I think adding a function in impl CompleteCommand to set the comp_words may fix this issue.
> 
> Its not clear to me why someone would want to use this struct but override `comp_words`. `comp_words` is meant to come from the shell, not the calling program, to define what we should complete.

It's just my guess. Not the way you should follow. And looks like I didn't read the code correctly. You can choose the best way to implement this feature.

> 
> Are you trying to use `CompleteCommand` as a completion API-only and not for flattening into your CLI? If so, that isn't its role. The logic it implements is fairly light with most of the behavior coming from elsewhere.

I use the way like [dynamic example](https://github.com/clap-rs/clap/blob/master/clap_complete/examples/dynamic.rs) does.

> 
> > The dynamic completions could hide from the existing parameters, which is very useful. If we use the static completions and add a, e.g. generate parameter, we need to remove it if switch to dynamic completions.
> 
> It sounds like the concern is with compatibility being broken with how you currently do completions? I can understand that. However, the current built-in interface isn't stable yet either. Its also likely that you could make a generate parameter used for static completions that will then work for dynamic completions. The shell support might be a little different but you can choose now for a subset of shells (fig is one that I know won't work with dynamic completions).

Yes, I tried the static way. What I am concerned about is that the `generate` completion is not related to the program functions. It's mainly used by the package maintainer. 

So I like the idea that dynamic completion hides the `complete` parameter by default, which doesn't affect the normal program parameters.



---

_Comment by @liuhangbin on 2024-01-10 07:45_

> So I like the idea that dynamic completion hides the `complete` parameter by default, which doesn't affect the normal program parameters.

OK, I just saw that I can use the `.hide(true)` for the static completion parameter.

---

_Label `E-medium` added by @epage on 2024-01-11 16:55_

---

_Label `A-completion` added by @epage on 2024-01-11 16:55_

---

_Renamed from "[clap_complete] Please support user defined dynamic complete command" to "Please support user defined dynamic complete command" by @epage on 2024-01-11 16:55_

---

_Comment by @epage on 2024-01-11 16:55_

Overall, anyone is able to define their own `CompleteCommand`.  The core issue imo is that the registration script does not allow overriding it.

---

_Comment by @liuhangbin on 2024-01-12 00:52_

> Overall, anyone is able to define their own `CompleteCommand`. The core issue imo is that the registration script does not allow overriding it.

Indeed.

---

_Label `M-breaking-change` added by @epage on 2024-08-11 00:06_

---

_Renamed from "Please support user defined dynamic complete command" to "Caller-defined `complete` subcommand for rust-native completions" by @epage on 2024-08-11 00:06_

---

_Referenced in [clap-rs/clap#5670](../../clap-rs/clap/issues/5670.md) on 2024-08-12 13:41_

---

_Comment by @epage on 2024-08-12 17:24_

In #5671, we added a new way to control the running of completions through environment variables.  Due to the benefits mentioned in that issue, I'm inclined to have that be our "primary" way of doing things.  It includes being able to customize the environment variable.

For those that want a subcommand and want to customize it, I lean towards saying people should copy/paste the subcommand we provide.

---
