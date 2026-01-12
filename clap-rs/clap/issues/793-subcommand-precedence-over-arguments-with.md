```yaml
number: 793
title: Subcommand precedence over arguments with SubcommandsNegateReqs
type: issue
state: closed
author: klemens
labels: []
assignees: []
created_at: 2016-12-29T02:19:36Z
updated_at: 2016-12-31T15:48:51Z
url: https://github.com/clap-rs/clap/issues/793
synced_at: 2026-01-12T16:14:09Z
```

# Subcommand precedence over arguments with SubcommandsNegateReqs

---

_@klemens_

When using (at least) two positional arguments and a subcommand, the subcommand takes precedence when given as the second argument.

The goal is a cli of the following form, where the subcommand is only matched if it is the first argument. This makes it possible to e.g. use `sub` (or `help`) as the value for the second argument.
```
test <arg1> <arg2>
test sub ...
```

### Example

```rust
println!("{:#?}", App::new("test")
    .setting(AppSettings::SubcommandsNegateReqs)
    .arg(Arg::with_name("arg1").required(true))
    .arg(Arg::with_name("arg2").required(true))
    .subcommand(SubCommand::with_name("sub"))
    .get_matches());
```

`test 1 sub` results in:

```js
ArgMatches {
    args: {
        "arg1": MatchedArg { ... "1" ... }
    },
    subcommand: Some(
        SubCommand { ... }
    ),
    ...
}
```

while it would be much more useful (IMHO) if it resulted in:

```js
ArgMatches {
    args: {
        "arg2": MatchedArg { ... "1" ... },
        "arg1": MatchedArg { ... "sub" ... }
    },
    subcommand: None,
    ...
}
```

Additionally, when the `suggestions` feature is enabled, any prefix or extended version of `sub` with at least length 2 triggers a "Did you mean 'sub'?" help screen. Without `suggestions`, only exact matches cause the described problem.

A workaround is to use `test -- 1 sub` or `test 1 -- sub`, which results in the second output.

Changing the meaning of `SubcommandsNegateReqs` is of course not backwards compatible, but a new setting that prefers arguments over subcommands could be introduced.

* Rust version: 1.14.0
* Clap version: 2.19.2

---

_Comment by @kbknapp on 2016-12-29 03:49_

Thanks for the details! :smile: I'm a little confused on one point though, are you asking for when a subcommand conflicts with a required argument that the argument take precedence? Subcommands taking precedence also happens without requirements.

```rust
println!("{:#?}", App::new("test")
    .arg(Arg::with_name("arg1"))
    .arg(Arg::with_name("arg2"))
    .subcommand(SubCommand::with_name("sub"))
    .get_matches());
```
Has the same issue as what's listed above.

I'd be a little leary of having args take precdence though, as there's no way to "escape" a subcommand, but there *is* a way to "escape" an argument (`test 1 -- sub` as you mentioned).

---

_Label `T: RFC / question` added by @kbknapp on 2016-12-29 03:49_

---

_Comment by @klemens on 2016-12-29 04:17_

You are right, `SubcommandsNegateReqs` and required arguments are not strictly necessary for reproducing the "problem", but omitting them allows something like `test 1 2 sub`, which is a totally different usage.

> there's no way to "escape" a subcommand

This shouldn't be the default of course, but I think there is no other way¹ to implement my desired usage pattern:
```
app <destination> <command>
app config ...
```

I first noticed the problem when I wanted to send the `help` command using my application, which instead displayed the cli help, because of the automatically added `help` subcommand.

(1) There actually *is* another way: You can "abuse" AllowExternalSubcommands, so the first argument becomes the "external subcommand" if it is no internal one. However, then you can't use any advanced functionality of `Arg` and have to parse the additional arguments manually (or using a second clap `App` instance).

---

_Comment by @kbknapp on 2016-12-30 21:00_

Ah ok, I understand now! I do think this is doable, but I'm unsure of what to call this new setting. Bascially you want, "If an arg is used, forget about subcommands entirely." i.e. down't allow the `<cmd> [args] <cmd> [args] <cmd>` format. 

Perhaps something like, `DisallowArgsBetweenSubcommands` or `ArgsNegateSubcommands` (preferred)

---

_Comment by @klemens on 2016-12-30 22:38_

> If an arg is used, forget about subcommands entirely

Exactly! I think `ArgsNegateSubcommands` sounds better and fits nicely with `SubcommandsNegateReqs`.

Edit: I think this new setting should only apply to positional arguments, so that something like `app --flag config ...` stays possible. Therefore we maybe should call it `PositionalArgsNegateSubcommands`?

---

_Label `C: settings` added by @kbknapp on 2016-12-31 02:08_

---

_Label `D: easy` added by @kbknapp on 2016-12-31 02:08_

---

_Label `P4: nice to have` added by @kbknapp on 2016-12-31 02:08_

---

_Label `T: new setting` added by @kbknapp on 2016-12-31 02:08_

---

_Label `W: 2.x` added by @kbknapp on 2016-12-31 02:08_

---

_Label `T: RFC / question` removed by @kbknapp on 2016-12-31 02:08_

---

_Added to milestone `2.20.0` by @kbknapp on 2016-12-31 02:08_

---

_Assigned to @kbknapp by @kbknapp on 2016-12-31 05:26_

---

_Closed by @homu on 2016-12-31 06:15_

---

_Comment by @klemens on 2016-12-31 15:48_

Wow, thank you for the fast implementation, it works fine!

Have you seen my edit about only applying this setting to positional arguments? I don't currently need it, but I think it could be useful in something like temporarily specifying a different config file or a flag like `-v` for verbosity that applies to the main command and all subcommands.

---

_Referenced in [klemens/rconc#1](../../klemens/rconc/issues/1.md) on 2017-02-12 21:44_

---
