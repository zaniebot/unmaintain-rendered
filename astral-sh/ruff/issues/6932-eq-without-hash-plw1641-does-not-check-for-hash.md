```yaml
number: 6932
title: "`eq-without-hash` (`PLW1641`) does not check for `__hash__` in superclasses"
type: issue
state: closed
author: DetachHead
labels:
  - bug
  - type-inference
assignees: []
created_at: 2023-08-28T07:16:45Z
updated_at: 2024-12-10T00:26:26Z
url: https://github.com/astral-sh/ruff/issues/6932
synced_at: 2026-01-10T11:09:49Z
```

# `eq-without-hash` (`PLW1641`) does not check for `__hash__` in superclasses

---

_Issue opened by @DetachHead on 2023-08-28 07:16_

i noticed this is mentioned [in the rule's docs](https://beta.ruff.rs/docs/rules/eq-without-hash/) but i'm raising it because i couldn't see an issue for it

---

_Comment by @charliermarsh on 2023-08-28 14:44_

👍 Yeah this is why the rule is in the nursery -- as long as we don't support multi-file analysis, we can't implement it reliably. I'm gonna close as this is basically a known issue that will eventually be solved by multi-file analysis.

---

_Closed by @charliermarsh on 2023-08-28 14:44_

---

_Label `bug` added by @charliermarsh on 2023-08-28 14:44_

---

_Label `type-inference` added by @zanieb on 2023-08-28 14:54_

---

_Label `type-inference` removed by @zanieb on 2023-08-28 14:54_

---

_Label `multifile-analysis` added by @zanieb on 2023-08-28 14:55_

---

_Comment by @DetachHead on 2023-08-29 00:53_

wouldn't it make sense to keep the issue open to track it if it's a known issue?

---

_Comment by @charliermarsh on 2023-08-29 01:34_

We could consider creating a central tracking issue (or a tag like @zanieb did here). I just want to avoid piling up a bunch of known issues related to multi-file analysis that can't be worked on individually and make it harder to navigate the issue tracker.

---

_Comment by @LefterisJP on 2023-09-16 20:59_

> We could consider creating a central tracking issue (or a tag like @zanieb did here). I just want to avoid piling up a bunch of known issues related to multi-file analysis that can't be worked on individually and make it harder to navigate the issue tracker.

Also hit this one while enabling `--preview`. It's a useful rule. It found out 1 place we were missing `__hash__` but then had 6 false positives due to superclasses.

Added ```# noqa: PLW1641  # hash implemented by superclass``` at the false positives for now and keeping the rule.

---

_Comment by @DetachHead on 2023-09-17 00:07_

> We could consider creating a central tracking issue

raised #7447

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:34_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:34_

---

_Comment by @KotlinIsland on 2024-12-10 00:26_

- see #14883, it shouldn't look in superclasses

---
