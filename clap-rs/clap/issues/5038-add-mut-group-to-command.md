---
number: 5038
title: "Add `mut_group` to `Command`"
type: issue
state: closed
author: rgiot
labels:
  - C-enhancement
assignees: []
created_at: 2023-07-21T23:32:25Z
updated_at: 2023-12-04T18:15:33Z
url: https://github.com/clap-rs/clap/issues/5038
synced_at: 2026-01-10T01:28:05Z
---

# Add `mut_group` to `Command`

---

_Issue opened by @rgiot on 2023-07-21 23:32_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the [open](https://github.com/clap-rs/clap/issues) and [rejected](https://github.com/clap-rs/clap/issues?q=is%3Aissue+label%3AS-wont-fix+is%3Aclosed) issues

### Clap Version

4.3.12

### Describe your use case

I use a `Command` object that has been defined in another crate. This command has a group defined this way:

```rust
    .group(
        ArgGroup::new("ANY_INPUT")
            .args(&["INLINE", "INPUT", "LIST_EMBEDDED", "VIEW_EMBEDDED"])
    )
```

I modify this object in my crate to add some additional arguments and especially this one (I do not want to relly on the default implementation of version) :

```rust
            .arg(
                Arg::new("version")
                    .long("version")
                    .short('V')
                    .help("Print version")
                    .action(ArgAction::SetTrue)
                    .exclusive(true) // does not seem to work
                    .conflicts_with_all(["ANY_INPUT", "INLINE", "INPUT", "LIST_EMBEDDED", "VIEW_EMBEDDED"])
            )
```

both `exclusive` and `conflicts_with_all` and seems to be ignored, so as a workaround I have temporarily modified the `Command` definition in the other crate (so it does not fit the need of this initial crate

```rust
    .group(
        ArgGroup::new("ANY_INPUT")
            .args(&["INLINE", "INPUT", "LIST_EMBEDDED", "VIEW_EMBEDDED"])
          //  .required(true)
            .conflicts_with("version")
    )
```

### Describe the solution you'd like

Clap already has [mut_arg](https://docs.rs/clap/latest/clap/struct.Command.html#method.mut_arg) and [mut_subcommand](https://docs.rs/clap/latest/clap/struct.Command.html#method.mut_subcommand).
It would be nice to add `mut_group` to be consistent.

The solution to my problem would not require anymore to modify the former crate; only to code something similar to

```rust
command.mut_group("ANY_INPUT", |group| group.required(false).conflicts_with("version'))
```

I would say that the implementation of `mut_group` would be close to 

```rust
pub fn mut_group<F>(mut self, arg_id: impl AsRef<str>, f: F) -> Self
    where
        F: FnOnce(ArgGroup) -> ArgGroup {
        let group = ... code to get the position, remove from the appropriate vec, handle absence ...
        self.groups.push(f(group));
        self
    }
```

### Alternatives, if applicable

_No response_

### Additional Context

For sure, this `mut_group` is of interest as it is missing from the library. But I wonder if in my use case it is just a workaround around a bug in the management of conflicting arguments

---

_Label `C-enhancement` added by @rgiot on 2023-07-21 23:32_

---

_Comment by @epage on 2023-07-22 00:28_

> ```rust
> ```rust
>             .arg(
>                 Arg::new("version")
>                     .long("version")
>                     .short('V')
>                     .help("Print version")
>                     .action(ArgAction::SetTrue)
>                     .exclusive(true) // does not seem to work
>                     .conflicts_with_all(["ANY_INPUT", "INLINE", "INPUT", "LIST_EMBEDDED", "VIEW_EMBEDDED"])
>             )
> ```
> both exclusive and conflicts_with_all and seems to be ignored, so as a workaround I have temporarily modified the Command definition in the other crate (so it does not fit the need of this initial crate
> 
> ...
>
> For sure, this mut_group is of interest as it is missing from the library. But I wonder if in my use case it is just a workaround around a bug in the management of conflicting arguments

I feel like the first discussion to be having is about conflicts and `exclusive`...

---

_Comment by @rgiot on 2023-07-22 07:35_

You mean by added a dedicated issue with a minimal working example? Or I am misusing something?
 (Jtlyk, I added the conflict when I noticed the exclusive was not working)

---

_Comment by @epage on 2023-07-22 11:38_

That'd work, thanks!

---

_Comment by @rgiot on 2023-07-22 16:01_

here is the accompanying issue #5041

---

_Comment by @Hawk777 on 2023-12-03 08:37_

I would like to reiterate that this would be really nice to have. My use case is that I want to use the derive API, but I want a specific `ArgGroup` to sometimes be required and sometimes be optional depending on information discovered at runtime. I’m currently working around it by not using a group and then conditionally using `arg_mut` to create `required_unless_present` relationships between the two arguments; however, the help output is not as nice that way (it just claims that both arguments are required, even though really only one or the other is), so I would prefer to use a group—but I can’t make a derived group conditionally required, because I can’t mutate the group at runtime!

---

_Referenced in [clap-rs/clap#5247](../../clap-rs/clap/pulls/5247.md) on 2023-12-04 18:03_

---

_Comment by @epage on 2023-12-04 18:03_

@Hawk777 thanks for the use case!  Opened #5247 to address it.

---

_Closed by @epage on 2023-12-04 18:15_

---
