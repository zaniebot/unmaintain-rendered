```yaml
number: 8688
title: "Dataclasses: Detect subclasses trying setting a classvar for an inherited field"
type: issue
state: open
author: frenck
labels:
  - rule
  - type-inference
assignees: []
created_at: 2023-11-15T07:55:08Z
updated_at: 2024-10-24T14:33:01Z
url: https://github.com/astral-sh/ruff/issues/8688
synced_at: 2026-01-10T11:09:50Z
```

# Dataclasses: Detect subclasses trying setting a classvar for an inherited field

---

_Issue opened by @frenck on 2023-11-15 07:55_

It would be nice if Ruff was able to detect this case:

![image](https://github.com/astral-sh/ruff/assets/195327/6b725ca3-fff4-481a-8657-00f80e1924b0)


No linter available detects the case in B, which can lead to unexpected behavior.

../Frenck


---

_Renamed from "Detect subclasses trying setting a classvar for an inherited field" to "Dataclasses: Detect subclasses trying setting a classvar for an inherited field" by @frenck on 2023-11-15 07:56_

---

_Label `multifile-analysis` added by @zanieb on 2023-11-15 15:49_

---

_Comment by @zanieb on 2023-11-15 15:50_

This seems sensible, but it's worth noting we are currently limited to looking for base classes in the same file so if your base dataclass type is defined somewhere else we will not be able to say if the field exists on it.

---

_Comment by @MichaReiser on 2024-03-21 13:23_

@zanieb, what I understand is that this is a request for a new rule, and what you're saying is that implementing the rule today would be limited by the fact that Ruff doesn't support multi-file analysis. Is my understanding correct?

---

_Label `rule` added by @MichaReiser on 2024-03-21 13:23_

---

_Comment by @zanieb on 2024-03-21 15:53_

@MichaReiser that sounds right to me.

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:32_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
