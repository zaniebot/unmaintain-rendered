---
number: 1282
title: Allow all arguments to be valid (or at least for a subcommand)
type: issue
state: closed
author: laplaceon
labels: []
assignees: []
created_at: 2018-06-01T01:58:09Z
updated_at: 2018-08-02T03:30:24Z
url: https://github.com/clap-rs/clap/issues/1282
synced_at: 2026-01-07T13:12:19-06:00
---

# Allow all arguments to be valid (or at least for a subcommand)

---

_Issue opened by @laplaceon on 2018-06-01 01:58_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

rustc 1.26.1 (827013a31 2018-05-25)

### Affected Version of clap

clap 2.31.2

### Expected Behavior Summary
I want my app to allow all arguments and then I would put additional arguments that I didn't need into a map for use later. One part of my program would use these extra arguments to replace variables in a string to then be used as a command. I could use a SubCommand for all these extra arguments so that it's easier to get them.

### Actual Behavior Summary
I get errors of this format "Found argument '--out-file' which wasn't expected, or isn't valid in this context". I assume it's the same for SubCommands.


---

_Comment by @kbknapp on 2018-06-04 21:22_

You could use a positional argument, which accepts [multiple values](https://docs.rs/clap/2.31.2/clap/struct.Arg.html#method.multiple) and [`allow_hyphen_values(true)`](https://docs.rs/clap/2.31.2/clap/struct.Arg.html#method.allow_hyphen_values) (i.e. to start with a `-`). Alternatively you may be able to use [`AppSettings::TrailingVarArg`](https://docs.rs/clap/2.31.2/clap/enum.AppSettings.html#variant.TrailingVarArg).

---

_Closed by @kbknapp on 2018-06-04 21:22_

---

_Label `T: RFC / question` added by @kbknapp on 2018-06-04 21:23_

---

_Comment by @kbknapp on 2018-06-04 21:23_

If this doesn't work, feel free to pop in the gitter chat room or ping me here and I can help more :wink: 

---
