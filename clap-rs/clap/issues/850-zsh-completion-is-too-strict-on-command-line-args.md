---
number: 850
title: zsh completion is too strict on command line args
type: issue
state: closed
author: laishulu
labels:
  - C-enhancement
  - A-completion
  - E-help-wanted
assignees: []
created_at: 2017-02-15T00:39:14Z
updated_at: 2020-04-08T17:22:42Z
url: https://github.com/clap-rs/clap/issues/850
synced_at: 2026-01-10T01:26:37Z
---

# zsh completion is too strict on command line args

---

_Issue opened by @laishulu on 2017-02-15 00:39_

### Rust Version

rustc 1.17.0-nightly (29dece1c8 2017-02-08)

### Affected Version of clap
 "clap 2.20.3 (registry+https://github.com/rust-lang/crates.io-index)",
name = "clap"
"checksum clap 2.20.3 (registry+https://github.com/rust-lang/crates.io-index)" = "f89819450aa94325998aa83ce7ea142db11ad24c725d6bc48459845e0d6d9f18"
### Expected Behavior Summary
`myapp --foo1 bar1 --foo2 bar2 --foo3 bar3 --<tab>` 
then list candidates for more args.

### Actual Behavior Summary
when `myapp --foo1=bar1 --foo2=bar2 --foo3=bar3 --<tab> ` , auto complete works perfect.
but  if I use `space` instead of the `=` symbol, auto complete doesn't work.

---

_Comment by @kbknapp on 2017-02-15 15:36_

I'm not sure I understand the issue entirely. Are you saying the candidates don't include the `=` but you'd like them to, or there are missing valid candidates?

---

_Comment by @laishulu on 2017-02-15 15:38_

when `myapp --foo1=bar1 --foo2=bar2 --foo3=bar3 --<tab> ` , auto complete works perfect.
but  if I use `space` instead of the `=` symbol, auto complete doesn't work.

---

_Comment by @kbknapp on 2017-02-15 15:45_

Ah ok! I understand, thanks.

I think this has to do with how Zsh completion works and isn't specific to `clap`. In Bash completion scripts there's a way to tell the interpreter that a value follows certain options, but I'm not aware of a way to do this in Zsh.

I'm open to suggestions on how to make the completion generation better.

---

_Label `C: completion gen` added by @kbknapp on 2017-02-15 15:45_

---

_Label `D: intermediate` added by @kbknapp on 2017-02-15 15:45_

---

_Label `M: help wanted` added by @kbknapp on 2017-02-15 15:45_

---

_Label `P3: want to have` added by @kbknapp on 2017-02-15 15:45_

---

_Label `T: enhancement` added by @kbknapp on 2017-02-15 15:45_

---

_Label `W: 2.x` added by @kbknapp on 2017-02-15 15:45_

---

_Label `W: 3.x blocker` added by @kbknapp on 2017-05-09 18:59_

---

_Label `W: 2.x` removed by @kbknapp on 2017-05-09 18:59_

---

_Referenced in [clap-rs/clap#1141](../../clap-rs/clap/pulls/1141.md) on 2018-01-06 11:02_

---

_Comment by @segevfiner on 2018-01-14 05:03_

This might have been fixed by #1141. Needs testing.

---

_Label `W: 3.x` added by @kbknapp on 2018-07-22 01:14_

---

_Label `W: 3.x blocker` removed by @kbknapp on 2018-07-22 01:14_

---

_Comment by @pksunkara on 2020-03-03 21:21_

@segevfiner @laishulu  I don't have zsh and I tried testing it with mac's default zsh (5.3) and I keep getting `_arguments not found`. If one of you could test and respond back, that would be great.

---

_Referenced in [clap-rs/clap#1793](../../clap-rs/clap/pulls/1793.md) on 2020-04-08 06:25_

---

_Comment by @intgr on 2020-04-08 16:48_

By pksunkara's request, I tested with the example program in PR #1793, this issue is fixed.

---

_Closed by @pksunkara on 2020-04-08 17:22_

---
