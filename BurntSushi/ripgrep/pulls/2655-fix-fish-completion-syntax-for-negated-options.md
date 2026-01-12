```yaml
number: 2655
title: Fix fish completion syntax for negated options
type: pull_request
state: closed
author: blyxxyz
labels:
  - rollup
assignees: []
base: master
head: fix-fish-negation-completion
created_at: 2023-11-27T10:19:54Z
updated_at: 2023-11-29T14:53:44Z
url: https://github.com/BurntSushi/ripgrep/pull/2655
synced_at: 2026-01-12T18:23:14Z
```

# Fix fish completion syntax for negated options

---

_@blyxxyz_

The fish completion generator currently forgets to add `-l` to negation options, leading to a list of these errors:
```
complete: too many arguments

~/.config/fish/completions/rg.fish (line 146):
complete -c rg -n '__fish_use_subcommand'  no-sort-files -d '(DEPRECATED) Sort results by file path.'
^
from sourcing file ~/.config/fish/completions/rg.fish

(Type 'help complete' for related documentation)
```
(To reproduce: `fish -c 'rg --generate=complete-fish | source'`)

It also potentially suggests a list of choices for negation options, even though those never take arguments. That case doesn't occur with any of the current options but it's an easy fix.

Fixes #2659.

---

_Label `bug` added by @BurntSushi on 2023-11-28 00:57_

---

_@BurntSushi approved on 2023-11-28 00:59_

Nice, thank you!

---

_Label `bug` removed by @BurntSushi on 2023-11-28 00:59_

---

_Label `rollup` added by @BurntSushi on 2023-11-28 01:01_

---

_Closed by @BurntSushi on 2023-11-28 02:17_

---

_Branch deleted on 2023-11-29 14:53_

---
