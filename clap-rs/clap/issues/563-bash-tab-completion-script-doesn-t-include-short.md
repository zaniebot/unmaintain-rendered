```yaml
number: 563
title: "Bash tab-completion script doesn't include short version of `--help` and `--verbose`"
type: issue
state: closed
author: dzamlo
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2016-07-02T13:50:03Z
updated_at: 2018-08-02T03:29:51Z
url: https://github.com/clap-rs/clap/issues/563
synced_at: 2026-01-12T16:14:09Z
```

# Bash tab-completion script doesn't include short version of `--help` and `--verbose`

---

_@dzamlo_

The script generated with `gen_completions` doesn't include the short version of the `--help` and `--verbose` arguments (`-h` and `-V` respectively).


---

_Comment by @kbknapp on 2016-07-02 21:49_

Thanks noticing and filing this! This should be a quick fix.


---

_Label `T: bug` added by @kbknapp on 2016-07-02 21:52_

---

_Label `P2: need to have` added by @kbknapp on 2016-07-02 21:52_

---

_Label `D: easy` added by @kbknapp on 2016-07-02 21:52_

---

_Label `W: 2.x` added by @kbknapp on 2016-07-02 21:52_

---

_Label `C: completion gen` added by @kbknapp on 2016-07-02 21:52_

---

_Comment by @kbknapp on 2016-07-03 16:02_

#565 fixes this.


---

_Closed by @homu on 2016-07-03 16:15_

---
