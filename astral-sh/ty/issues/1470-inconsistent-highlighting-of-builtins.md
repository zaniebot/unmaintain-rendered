```yaml
number: 1470
title: Inconsistent highlighting of builtins
type: issue
state: closed
author: MichaReiser
labels:
  - help wanted
  - server
assignees: []
created_at: 2025-11-03T14:10:32Z
updated_at: 2025-11-11T19:15:43Z
url: https://github.com/astral-sh/ty/issues/1470
synced_at: 2026-01-12T15:54:25Z
```

# Inconsistent highlighting of builtins

---

_@MichaReiser_

ty's builtins highlighting is inconsistent in a few types:

* `float` isn't highlighted as a builtin (because it's a union internally)
* builtins in type aliases (value position) aren't highlighted as builtins


ty:

<img width="313" height="316" alt="Image" src="https://github.com/user-attachments/assets/1fe20e0d-dbf2-4a58-825e-3befcac6d278" />

pylance

<img width="313" height="316" alt="Image" src="https://github.com/user-attachments/assets/a92f3475-d925-410d-a5e5-5edc59b3000e" />

---

_Label `server` added by @MichaReiser on 2025-11-03 14:10_

---

_Label `help wanted` added by @MichaReiser on 2025-11-03 14:10_

---

_Comment by @MatthewMckee4 on 2025-11-03 15:18_

Don't ty and pylance both highlight in the same way. What language server do you have activated in that screenshot?

---

_Comment by @MichaReiser on 2025-11-03 15:41_

Hmm, right, it does. I must have messed up my extension settings. 

---

_Renamed from "Use different highlighting for builtin types" to "Inconsistent highlighting of builtins" by @MichaReiser on 2025-11-04 14:02_

---

_Comment by @MichaReiser on 2025-11-04 14:05_

I updated the issue description

---

_Comment by @MichaReiser on 2025-11-11 19:14_

This is slightly more involved and the underlying issue really is that we do classification based on the type rather than definition because `definition_kind_for_name` is very limited (it only looks at the current scope). 

My very naive approach of using `definitions_for_name` instead lead to some nice improvements in tests but it also regressed some other tests.

Ultimately the fix here is to address this todo from [Follow up on TODOs in semantic token provider #791](https://github.com/astral-sh/ty/issues/791)

> Improve classification for name tokens that are imported from other modules. Currently, these are classified based on their types, which often means they're classified as variables when they should be classes, etc.


So I think we can close this as a duplicate

---

_Closed by @MichaReiser on 2025-11-11 19:15_

---
