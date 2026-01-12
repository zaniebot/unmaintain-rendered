```yaml
number: 13783
title: "add `--show-extras` to `uv tool list` to list extra requirements installed with tools"
type: pull_request
state: merged
author: gaardhus
labels:
  - enhancement
assignees: []
merged: true
base: main
head: main
created_at: 2025-06-02T13:07:12Z
updated_at: 2025-06-02T14:59:40Z
url: https://github.com/astral-sh/uv/pull/13783
synced_at: 2026-01-12T16:10:52Z
```

# add `--show-extras` to `uv tool list` to list extra requirements installed with tools

---

_@gaardhus_

## Summary

Implemented as suggested in #13761 

eg.

```
$ uv tool install 'harlequin[postgres]'
$ uv tool list --show-extras
harlequin v2.1.2 [extras: postgres]
- harlequin
```

## Test Plan

Added a new test with the argument along with the others from the `uv tool list` cli.


---

_@zanieb reviewed on 2025-06-02 13:38_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_list.rs`:492 on 2025-06-02 13:38_

I wonder if we should just show extras by default? e.g.,

```
flask[async, dotenv] v3.02
```

What do you think?



---

_@zanieb reviewed on 2025-06-02 13:38_

---

_Review comment by @zanieb on `crates/uv/tests/it/tool_list.rs`:492 on 2025-06-02 13:38_

(It's not very pretty)

---

_Assigned to @zanieb by @zanieb on 2025-06-02 13:39_

---

_Review comment by @gaardhus on `crates/uv/tests/it/tool_list.rs`:492 on 2025-06-02 14:02_

That was actually my first hunch as well, but seemed like @charliermarsh preferred the flag https://github.com/astral-sh/uv/issues/13761#issuecomment-2928175706. And I guess extras aren't always that important which I could see as an argument to hide it behind the flag.

---

_@gaardhus reviewed on 2025-06-02 14:02_

---

_Label `enhancement` added by @zanieb on 2025-06-02 14:49_

---

_@zanieb approved on 2025-06-02 14:51_

---

_Merged by @zanieb on 2025-06-02 14:59_

---

_Closed by @zanieb on 2025-06-02 14:59_

---
