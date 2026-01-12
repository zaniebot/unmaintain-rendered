```yaml
number: 1915
title: "Difference between `lowest` and `lowest-direct`?"
type: issue
state: closed
author: radoering
labels:
  - documentation
  - good first issue
assignees: []
created_at: 2024-02-23T15:06:53Z
updated_at: 2024-02-24T22:15:02Z
url: https://github.com/astral-sh/uv/issues/1915
synced_at: 2026-01-12T15:58:33Z
```

# Difference between `lowest` and `lowest-direct`?

---

_@radoering_

> However, uv's resolution strategy can be configured to prefer the lowest compatible version of each package (--resolution=lowest), or even the lowest compatible version of any direct dependencies (--resolution=lowest-direct)

Reading this paragraph, I'm not sure I understand the difference between these two strategies correctly. My understanding is that `lowest` will try to choose the lowest version of each direct and transitive dependency if possible. Will `lowest-direct` only prefer the lowest version for each direct dependency but prefer the highest version for transitive dependencies? (The wording of the paragraph somehow makes me think that `lowest-direct` is an augmentation of `lowest` but my understanding is the opposite.)

---

_Comment by @charliermarsh on 2024-02-23 15:13_

Your understanding is correct. I'll take a crack at rewording it.

---

_Comment by @zanieb on 2024-02-23 15:13_

I believe you are correct. Happy to review an improvement for this.

---

_Comment by @zanieb on 2024-02-23 15:14_

Ugh Charlie is so fast :)

---

_Label `documentation` added by @zanieb on 2024-02-23 15:14_

---

_Label `good first issue` added by @zanieb on 2024-02-23 15:14_

---

_Comment by @samypr100 on 2024-02-24 18:48_

How about this:


By default, uv follows the standard Python dependency resolution strategy of preferring the latest compatible version of each package. For example, `uv pip install flask>=2.0.0` will install the latest version of Flask (at time of writing: `3.0.0`).

However, you can configure uv's resolution strategy to support alternative workflows. By using `--resolution=lowest`, uv will install the **lowest** compatible versions for all dependencies, both **direct** and **transitive**. Alternatively, `--resolution=lowest-direct` focuses only on **direct** dependencies, opting for their **lowest** compatible versions, while using **latest** compatible versions for **transitive** dependencies. This distinction can be particularly useful for library authors who wish to test against the lowest supported versions of direct dependencies without restricting the versions of transitive dependencies.

---

_Closed by @charliermarsh on 2024-02-24 22:15_

---
