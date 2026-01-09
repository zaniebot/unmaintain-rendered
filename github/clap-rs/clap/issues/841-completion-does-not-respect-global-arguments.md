---
number: 841
title: completion does not respect global arguments
type: issue
state: closed
author: laishulu
labels:
  - C-bug
  - A-completion
assignees: []
created_at: 2017-02-03T11:11:46Z
updated_at: 2018-08-02T03:30:01Z
url: https://github.com/clap-rs/clap/issues/841
synced_at: 2026-01-07T13:12:19-06:00
---

# completion does not respect global arguments

---

_Issue opened by @laishulu on 2017-02-03 11:11_

### Rust Version

rustc -V

### Affected Version of clap

clap 2.20.0

### Expected Behavior Summary
1. `global-arg` is a global arg of `myapp` which can be used in `subcmd`, i.e both `myapp --global-arg subcmd` and `myapp subcmd --global-arg` work
2. when I type `myapp subcmd --[tab]`, `global-arg` should pops up.

### Actual Behavior Summary
when I type `myapp subcmd --[tab]`, `global-arg` does not show up

---

_Comment by @kbknapp on 2017-02-03 16:15_

Three questions for you:

 * Can you post a link to the code, or a snippet of the code you're trying to use? 
 * Which shell are you using?
 * Does the `myapp` or `subcmd` have an underscore in the name? (`_`). If so, this could be from #581 which I'm currently working on and have almost ready to PR.

---

_Label `T: RFC / question` added by @kbknapp on 2017-02-03 16:16_

---

_Comment by @laishulu on 2017-02-03 16:21_

```
                    app.gen_completions_to("myapp",
                                           Shell::Zsh,
                                           &mut io::stdout())
```
I use `zsh`.  underscore is in nowhere.

---

_Label `C: completion gen` added by @kbknapp on 2017-02-03 16:28_

---

_Label `D: easy` added by @kbknapp on 2017-02-03 16:28_

---

_Label `P2: need to have` added by @kbknapp on 2017-02-03 16:28_

---

_Label `T: bug` added by @kbknapp on 2017-02-03 16:28_

---

_Label `W: 2.x` added by @kbknapp on 2017-02-03 16:28_

---

_Label `T: RFC / question` removed by @kbknapp on 2017-02-03 16:28_

---

_Added to milestone `2.20.3` by @kbknapp on 2017-02-03 16:28_

---

_Comment by @kbknapp on 2017-02-03 17:53_

Alright, I have this fixed, once #842 merges I'll start the new PR

---

_Closed by @kbknapp on 2017-02-03 22:43_

---
