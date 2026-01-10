---
number: 2335
title: Implement visible aliases for args in completions.
type: issue
state: closed
author: pksunkara
labels:
  - C-enhancement
  - A-completion
  - E-easy
  - ":money_with_wings: $5"
assignees: []
created_at: 2021-02-07T14:14:57Z
updated_at: 2021-02-22T09:09:25Z
url: https://github.com/clap-rs/clap/issues/2335
synced_at: 2026-01-10T01:27:16Z
---

# Implement visible aliases for args in completions.

---

_Issue opened by @pksunkara on 2021-02-07 14:14_

Currently, completions correctly support arg's short visible aliases but not the arg's long visible aliases. This issue is about rectifying it.

#### Implementation

Follow the same pattern from `shorts` and implement for `longs` too in `clap_generate`

---

_Label `T: enhancement` added by @pksunkara on 2021-02-07 14:14_

---

_Label `C: args` added by @pksunkara on 2021-02-07 14:14_

---

_Label `D: easy` added by @pksunkara on 2021-02-07 14:14_

---

_Label `C: generator` added by @pksunkara on 2021-02-07 14:14_

---

_Label `Z: good first issue` added by @pksunkara on 2021-02-07 14:14_

---

_Label `C: alias` added by @pksunkara on 2021-02-07 14:14_

---

_Label `:money_with_wings: $5` added by @pksunkara on 2021-02-07 14:14_

---

_Added to milestone `3.1` by @pksunkara on 2021-02-07 14:15_

---

_Referenced in [clap-rs/clap#2349](../../clap-rs/clap/pulls/2349.md) on 2021-02-15 08:58_

---

_Closed by @pksunkara on 2021-02-22 09:09_

---
