```yaml
number: 1027
title: support -oflag style arguments
type: issue
state: closed
author: kahing
labels: []
assignees: []
created_at: 2017-08-14T04:41:43Z
updated_at: 2018-08-02T03:30:10Z
url: https://github.com/clap-rs/clap/issues/1027
synced_at: 2026-01-12T16:14:10Z
```

# support -oflag style arguments

---

_@kahing_

Some programs allow both -o key=value and -okey=value and -oflag arguments (ex: mount). How would one do the same thing in clap?


---

_Comment by @kbknapp on 2017-08-15 00:45_

clap supports each of those already, minus the automatic parsing of `key=value`. Instead it will make `key=value` a single "value" that user code is then expected to parse further (such as splitting on `=`, etc.).

---

_Label `T: RFC / question` added by @kbknapp on 2017-08-15 00:45_

---

_Comment by @kahing on 2017-08-17 02:52_

Ah, I had a problem with it because multiple(true) would slurp positional arguments, which can be worked around by passing `--` before the positionals.

---

_Closed by @kahing on 2017-08-17 02:52_

---
