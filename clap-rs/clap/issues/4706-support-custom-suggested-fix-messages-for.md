---
number: 4706
title: Support custom suggested fix messages for specific flags
type: issue
state: closed
author: weihanglo
labels:
  - C-enhancement
  - A-help
  - S-waiting-on-design
assignees: []
created_at: 2023-02-13T15:03:48Z
updated_at: 2023-08-17T13:58:14Z
url: https://github.com/clap-rs/clap/issues/4706
synced_at: 2026-01-10T01:28:00Z
---

# Support custom suggested fix messages for specific flags

---

_Issue opened by @weihanglo on 2023-02-13 15:03_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.1.4

### Describe your use case

Clap has a good fix suggestion mechanism when a user types something wrong. It seems to be based on [Jaro distance](https://github.com/clap-rs/clap/blob/ad5d67623a89415c7f5f7e1f7a47bf5abdfd0b6c/src/parser/features/suggestions.rs#L20). 

A developer sometimes wants to provide an additional context on a specific situation to users. In order to do so, clap needs to support things like **“when a user passes a non-existent flag/command/value/whatever, suggest a flag with this custom message instead.”**

### Describe the solution you'd like

There are lots of different ways and layers to support this feature. 

I'll try to dump only one silly idea here: Clap seems to have [several kinds of suggested fix](https://docs.rs/clap/4.1.4/clap/?search=suggested). I don't have any knowledge of clap internals, but feel like at least `Arg` and `Command` can have a method, say `suggests`, accepting a flag name and an alternate message, such as

```diff
 Arg::new("directory")
     .help("Change to DIRECTORY before doing anything")
     .short('C')
     .value_name("DIRECTORY")
+   .suggests("--cwd", "long flag --cwd is not yet supported. Please use `-C` instead")
```

Not a good idea but you've got the gist.




### Alternatives, if applicable

Do it manually on application-side. Like

- https://github.com/rust-lang/cargo/blob/39c13e67a5962466cc7253d41bc1099bbcb224c3/src/bin/cargo/commands/tree.rs#L230-L235
- https://github.com/rust-lang/cargo/blob/39c13e67a5962466cc7253d41bc1099bbcb224c3/src/bin/cargo/commands/read_manifest.rs#L4-L13

They are not really in the same situation of this feature requests, as Cargo keeps a stable flag way longer than usual. However, if Cargo decide to remove those subcommand/flags, it may utilize this feature to provide good suggestions.

### Additional Context

This idea was originally from <https://github.com/rust-lang/cargo/issues/11702>

---

_Label `C-enhancement` added by @weihanglo on 2023-02-13 15:03_

---

_Comment by @epage on 2023-02-13 15:23_

In rust-lang/cargo#11702, my original idea was for this to allow the user to provide custom suggestions, similar to the automatic ones.

This would look like
```diff
 Arg::new("directory")
     .help("Change to DIRECTORY before doing anything")
     .short('C')
     .value_name("DIRECTORY")
-   .suggests("--cwd", "long flag --cwd is not yet supported. Please use `-C` instead")
+   .suggest(["--cwd", "--directory", "--cd"])
```
with the error message:
```console
$ cargo --cwd ../foo
error: unexpected argument '--cwd' found

  note: argument '-C' exists

Usage: cargo [COMMAND]

For more information, try '--help'.
```

---

_Comment by @epage on 2023-02-13 15:23_

Aside: In a way, this reminds me of #3321, especially with how this issue is written with providing custom error messages

---

_Comment by @epage on 2023-02-13 15:25_

Implementation note: we'll need to be careful that we can discover these for suggestions without
- Being used for inferring
- Being provided as dynamic suggestions so long as valid arguments are suggested

I wonder if we could use this in completions so we complete `--directory` as `-C`?

---

_Label `A-help` added by @epage on 2023-02-23 21:24_

---

_Label `S-waiting-on-design` added by @epage on 2023-02-23 21:24_

---

_Referenced in [rust-lang/cargo#12009](../../rust-lang/cargo/issues/12009.md) on 2023-04-20 17:17_

---

_Referenced in [rust-lang/cargo#10362](../../rust-lang/cargo/issues/10362.md) on 2023-05-24 17:47_

---

_Referenced in [rust-lang/cargo#12467](../../rust-lang/cargo/issues/12467.md) on 2023-08-09 12:55_

---

_Referenced in [rust-lang/cargo#12478](../../rust-lang/cargo/pulls/12478.md) on 2023-08-11 13:33_

---

_Comment by @epage on 2023-08-11 14:04_

What if we instead supported this as
```rust
 Arg::new("directory")
     .help("Change to DIRECTORY before doing anything")
     .short('C')
     .value_name("DIRECTORY"),
 Arg::new("cwd")
     .long('cwd')
     .value_name("DIRECTORY")
     .hide(true)
     .action(ArgAction::Unsupported("long flag --cwd is not yet supported. Please use `-C` instead".into()))
```
- debug_asserts will ensure `ArgAction::Unsupported` is used with `hide(true)
- If completions show hidden arguments, then they'll need to hide these

---

_Comment by @weihanglo on 2023-08-11 14:57_

That looks nicer and more consistent! What is the behavior in your mind when receiving such an arg, bail out immediately?

---

_Comment by @epage on 2023-08-11 15:00_

I'm leaning towards showing an unexpected argument error with the message inside of the tip

---

_Comment by @weihanglo on 2023-08-11 15:16_

Things Cargo also wants:

* Suggesting a new subcommand for a deprecated subcommand.
* Instead of erroring out, some flags just need a deprecation warning and continue working.

I guess for the first one, cargo needs to match the Result by itself. For the latter, `ArgAction` doesn't apply to the situation.

Also, should we talk about custom validation error message as a whole? I don't really understand clap internals so they might be different stuff.

---

_Comment by @epage on 2023-08-11 15:26_

Native deprecation support is being tracked in #3321

> Also, should we talk about custom validation error message as a whole? I don't really understand clap internals so they might be different stuff.

[`Arg::value_parser`](https://docs.rs/clap/latest/clap/struct.Arg.html#method.value_parser) allows validating an argument and converting it into a application-domain type

Is that what you are looking for?

---

_Comment by @weihanglo on 2023-08-11 15:43_

I see. 
Yep. `value_parser` might work. Will look into how to integrate.

---

_Referenced in [rust-lang/cargo#12494](../../rust-lang/cargo/issues/12494.md) on 2023-08-15 14:24_

---

_Referenced in [clap-rs/clap#5075](../../clap-rs/clap/pulls/5075.md) on 2023-08-16 19:55_

---

_Comment by @epage on 2023-08-16 19:56_

Could people look over #5075 to see if it works for their needs

---

_Closed by @epage on 2023-08-17 13:58_

---
