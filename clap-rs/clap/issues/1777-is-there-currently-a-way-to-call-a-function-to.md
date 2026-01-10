---
number: 1777
title: Is there currently a way to call a function to provide a default value (instead of a static string)?
type: issue
state: closed
author: CreepySkeleton
labels: []
assignees: []
created_at: 2020-04-01T21:26:37Z
updated_at: 2024-04-16T12:55:37Z
url: https://github.com/clap-rs/clap/issues/1777
synced_at: 2026-01-10T01:27:05Z
---

# Is there currently a way to call a function to provide a default value (instead of a static string)?

---

_Issue opened by @CreepySkeleton on 2020-04-01 21:26_

Is there currently a way to call a function to provide a default value (instead of a static string)? This is in the context of Clap's proc_macro.

We have some resolution logic (config file, plus global configuration, plus some environment stuff) for default values, and that would be very useful.

For a concrete example, we have a `--file` argument for a server, which takes a filename and validates it's a valid path (user can write to it). If it's not passed in, it should be read from an environment variable. If that's not specified, it should be in a configuration file. If it's not there, it should be at the root of the project (if the user is running inside a project). All else, it should be a temporary file (using `temp_file()`). We could use an `Option<PathBuf>` and set it after arg matching, but it complicates the code using it (unwrapping everywhere). Simplest is to panic during arg parsing (e.g. if user has no right to the fs), else just be a `PathBuf`, from the clap code directly.

I know there is a `parse` argument to the proc_macro, but AFAIK it only applies if there is a value passed by the user, not for default values.

_Originally posted by @hansl in https://github.com/clap-rs/clap/discussions/1776_

---

_Label `C: other` added by @CreepySkeleton on 2020-04-01 21:27_

---

_Label `W: 3.x` added by @CreepySkeleton on 2020-04-01 21:27_

---

_Comment by @pksunkara on 2020-04-01 21:35_

What does clap need to do here? If we are talking about default values for args being provided by external stuff, we should already have an issue about this called hooks somewhere.

---

_Comment by @TeXitoi on 2020-04-01 22:40_

@hansl does using a lazy static allow you to do what you want.

An example: https://github.com/CanalTP/mimirsbrunn/blob/master/libs/bragi/src/lib.rs#L67

---

_Comment by @pksunkara on 2020-04-07 15:54_

This should be covered by the hooks suggestion in https://github.com/clap-rs/clap/issues/1634

---

_Closed by @pksunkara on 2020-04-07 15:54_

---

_Comment by @lynn on 2024-04-16 12:55_

You can now use `#[clap(default_value_t = my_fn())]` for this. [Example on Rust Playground](https://play.rust-lang.org/?version=stable&mode=debug&edition=2021&gist=731f0008d73402fd93f44c54b5c7e2f0)

---

_Referenced in [clap-rs/clap#1453](../../clap-rs/clap/issues/1453.md) on 2025-01-20 03:41_

---
