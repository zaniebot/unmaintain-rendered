---
number: 37
title: Display all required arguments in usage
type: issue
state: closed
author: kbknapp
labels:
  - C-bug
assignees: []
created_at: 2015-03-28T04:43:39Z
updated_at: 2018-08-02T03:29:37Z
url: https://github.com/clap-rs/clap/issues/37
synced_at: 2026-01-07T13:12:19-06:00
---

# Display all required arguments in usage

---

_Issue opened by @kbknapp on 2015-03-28 04:43_

Currently, if an argument is "required by" a "required by default" argument it will not be shown in the usage string.

i.e. If I set up two positional arguments "input" and "output" mark only input as `required(true)`, but specify `requires("output")` on the input argument, "output" will not be listed in the usage string which can be confusing to users:

```
$ myprog infile.txt
One or more required arguments weren't specified
USAGE:
    myprog [FLAGS] <INPUT>
[...]
```

It SHOULD display:

```
USAGE:
    myprog [FLAGS] <INPUT> <OUTPUT>
```


---

_Label `bug` added by @kbknapp on 2015-03-28 04:43_

---

_Closed by @kbknapp on 2015-03-28 17:36_

---
