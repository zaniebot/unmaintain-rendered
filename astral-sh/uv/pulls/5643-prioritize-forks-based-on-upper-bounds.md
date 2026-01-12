```yaml
number: 5643
title: Prioritize forks based on upper bounds
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - preview
assignees: []
merged: true
base: main
head: charlie/prioritize-cap
created_at: 2024-07-30T23:37:10Z
updated_at: 2024-07-31T15:05:15Z
url: https://github.com/astral-sh/uv/pull/5643
synced_at: 2026-01-12T16:06:56Z
```

# Prioritize forks based on upper bounds

---

_@charliermarsh_

## Summary

Given a fork like:

```
pylint < 3 ; sys_platform == 'darwin'
pylint > 2 ; sys_platform != 'darwin'
```

Solving the top branch will typically yield a solution that also satisfies the bottom branch, due to maximum version selection (while the inverse isn't true).

To quote an example from the docs:

```rust
// If there's no difference, prioritize forks with upper bounds. We'd prefer to solve
// `numpy <= 2` before solving `numpy >= 1`, since the resolution produced by the former
// might work for the latter, but the inverse is unlikely to be true due to maximum
// version selection. (Selecting `numpy==2.0.0` would satisfy both forks, but selecting
// the latest `numpy` would not.)
```

Closes https://github.com/astral-sh/uv/issues/4926 for now.

---

_Label `enhancement` added by @charliermarsh on 2024-07-30 23:37_

---

_Label `preview` added by @charliermarsh on 2024-07-30 23:37_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-07-30 23:37_

---

_Review requested from @konstin by @charliermarsh on 2024-07-30 23:37_

---

_Marked ready for review by @charliermarsh on 2024-07-30 23:37_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:2873 on 2024-07-30 23:38_

Just a heuristic of course, and we don't really have a ton of examples to inform this work.

---

_@charliermarsh reviewed on 2024-07-30 23:38_

---

_Comment by @konstin on 2024-07-31 07:38_

While this is an improvement, it does not fix the transformers example. Fork points for transformers:

* Splitting on pydantic==2.8.2 over typing-extensions
* Splitting on opencv-python==4.10.0.84 over numpy
* Splitting on pandas==2.2.2 over numpy
* Splitting on botocore==1.34.151 over urllib3

~~I think we need to prioritize based on `python_version` for that.~~ I haven't dug in what part is missing yet.

---

_@konstin approved on 2024-07-31 07:39_

---

_@BurntSushi approved on 2024-07-31 14:06_

---

_Comment by @charliermarsh on 2024-07-31 14:28_

@konstin - Can you link to that example, and I'll explore it?

---

_Comment by @konstin on 2024-07-31 14:59_

I'Ve added it in https://github.com/astral-sh/uv/pull/5657

---

_Comment by @charliermarsh on 2024-07-31 15:05_

Thanks! I'll merge this and then explore that separately.

---

_Merged by @charliermarsh on 2024-07-31 15:05_

---

_Closed by @charliermarsh on 2024-07-31 15:05_

---

_Branch deleted on 2024-07-31 15:05_

---
