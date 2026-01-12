```yaml
number: 145
title: "properly handle '\\n' in argument help messages"
type: issue
state: closed
author: kbknapp
labels:
  - C-enhancement
  - A-help
assignees: []
created_at: 2015-07-06T01:21:16Z
updated_at: 2018-08-02T03:29:40Z
url: https://github.com/clap-rs/clap/issues/145
synced_at: 2026-01-12T16:14:08Z
```

# properly handle '\n' in argument help messages

---

_@kbknapp_

`\n` should be translated to `\n\t\t\t` or whatever number of tabs need to be used so as to properly align with the preceding line.


---

_Label `enhancement` added by @kbknapp on 2015-07-06 01:21_

---

_Added to milestone `v1.1` by @kbknapp on 2015-07-06 01:23_

---

_Label `P2: need to have` added by @kbknapp on 2015-07-15 04:18_

---

_Label `C: help message` added by @kbknapp on 2015-07-15 04:18_

---

_Label `D: easy` added by @kbknapp on 2015-07-15 04:18_

---

_Comment by @kbknapp on 2015-07-16 05:32_

This is implemented via #160 

Note that if you want a newline in your help string, use `{n}` and _not_ `\n`

i.e.

``` rust
.arg_from_usage("<some_arg> 'this is a really long help string that{n}I would like to have broken over two lines'")
```


---

_Closed by @kbknapp on 2015-07-16 05:43_

---
