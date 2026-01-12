```yaml
number: 379
title: "Cannot get help output for optional ARGS with dependencies to print as [DEPENDENCY [DEPENDENT]]"
type: issue
state: closed
author: kamalmarhubi
labels: []
assignees: []
created_at: 2016-01-12T16:32:59Z
updated_at: 2018-08-02T03:29:47Z
url: https://github.com/clap-rs/clap/issues/379
synced_at: 2026-01-12T16:14:09Z
```

# Cannot get help output for optional ARGS with dependencies to print as [DEPENDENCY [DEPENDENT]]

---

_@kamalmarhubi_

I'm creating a program that can take a command and its arguments as arguments. I'd like the usage to show up as

```
USAGE:
    clap-optional-args-help [FLAGS] [COMMAND [ARG...]]
```

I've set  up the dependency with `requires`, and have tried both with and without `TrailingVarArg` ([example source](https://github.com/kamalmarhubi/clap-optional-args-help/blob/master/src/main.rs)), but cannot change the output from:

```
clap-optional-args-help 

USAGE:
    clap-optional-args-help [FLAGS] [--] [ARGS]

FLAGS:
    -h, --help       Prints help information
    -V, --version    Prints version information

ARGS:
    COMMAND    Command to run
    ARG...     Arguments for COMMAND
```


---

_Renamed from "Cannot get help for optional ARGS with dependencies as [DEPENDENCY [DEPENDENT]]" to "Cannot get help output for optional ARGS with dependencies to print as [DEPENDENCY [DEPENDENT]]" by @kamalmarhubi on 2016-01-12 16:35_

---

_Comment by @kbknapp on 2016-01-13 07:28_

There's two things you could do in that case.
1. Use a custom usage string. But this has some downsides, namely that's the _only_ usage string that will be displayed on error, i.e. if you supply a custom usage string `clap` won't generate context sensitive usage strings based on what the user was trying to do on error. If this isn't something you care about, a custom usage string will solve the issue:

``` rust
App::new("clap-optional-args-help")
    .usage("clap-optional-args-help [COMMAND [ARG...]]")
    .setting(TrailingVarArg)
    .arg(Arg::with_name("COMMAND").help("Command to run").index(1))
    .arg(Arg::with_name("ARG")
        .help("Arguments for COMMAND")
        .multiple(true)
        .requires("COMMAND")
        .index(2))
```
1. Use a required `COMMAND` argument (this will show it's dependencies). 

Also note, even with it as you have written, if the user ran `clap-optional-args-help cmd` (i.e. no args) they'll get an error about missing `ARG`, and the usage string will show `clap-optional-args-help [COMMAND] [ARG]...` (this is the context aware usage strings I was talking about)


---

_Label `T: RFC / question` added by @kbknapp on 2016-01-13 07:31_

---

_Closed by @kbknapp on 2016-02-14 11:13_

---
