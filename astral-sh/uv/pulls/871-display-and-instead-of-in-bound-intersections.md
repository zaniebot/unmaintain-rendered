```yaml
number: 871
title: "Display \"and\" instead of \",\" in bound intersections"
type: pull_request
state: closed
author: zanieb
labels:
  - error messages
assignees: []
base: main
head: zb/range-and
created_at: 2024-01-10T17:23:05Z
updated_at: 2024-01-11T21:00:25Z
url: https://github.com/astral-sh/uv/pull/871
synced_at: 2026-01-10T15:39:02Z
```

# Display "and" instead of "," in bound intersections

---

_Pull request opened by @zanieb on 2024-01-10 17:23_

Alternative to #870

Instead of saying `a>0,a<2` we say `a>0 and a<2`; making the operator clear but diverging from syntax users would write in a requirements file.

---

_@zanieb reviewed on 2024-01-10 17:24_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:2237 on 2024-01-10 17:24_

The danger of "and" is that now clauses like this are a little ambiguous

---

_Label `error messages` added by @zanieb on 2024-01-10 17:27_

---

_@BurntSushi reviewed on 2024-01-10 17:54_

---

_Review comment by @BurntSushi on `crates/puffin-cli/tests/pip_compile.rs`:2237 on 2024-01-10 17:54_

Oh hmmm, I was only thinking about it in the context of version constraints each on their own line. I agree that when things are on one line, `and` doesn't work as well.

---

_@zanieb reviewed on 2024-01-10 19:03_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:2237 on 2024-01-10 19:03_

I'm hesitant to have different formatting for each one. Perhaps && works better for that reason.

---

_Marked ready for review by @zanieb on 2024-01-10 22:01_

---

_Comment by @konstin on 2024-01-11 08:44_

Here i like the old style because it's PEP 440 that many users will be familiar with

---

_Comment by @zanieb on 2024-01-11 21:00_

Okay. Let's leave this as-is for now. We can revisit later if we want.

---

_Closed by @zanieb on 2024-01-11 21:00_

---
