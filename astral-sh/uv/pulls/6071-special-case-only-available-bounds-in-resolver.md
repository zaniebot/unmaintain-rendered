```yaml
number: 6071
title: "Special case \"only available\" bounds in resolver errors"
type: pull_request
state: closed
author: zanieb
labels:
  - error messages
assignees: []
base: main
head: zb/only-available-exclusive
created_at: 2024-08-13T23:08:38Z
updated_at: 2024-08-15T16:27:46Z
url: https://github.com/astral-sh/uv/pull/6071
synced_at: 2026-01-10T13:09:50Z
```

# Special case "only available" bounds in resolver errors

---

_Pull request opened by @zanieb on 2024-08-13 23:08_

So, this is a little nuanced, but it confused me a couple times and I'm poking around in this code so I figured I'd give it a try.

In the following:

```
❯  uv pip install 'httpx>9999'
  × No solution found when resolving dependencies:
  ╰─▶ Because only httpx<=9999 is available and you require httpx>9999, we can conclude that the requirements are
      unsatisfiable.
```

We say **`only httpx<=9999 is available`**, which _suggests_ that `httpx 9999` is available — but it's not, and we know that. The problem is that we naively take the complement of the requirement (`httpx>9999`).

Here, we special case inclusive bounds to check if the version in question is actually available — if not, we switch to an exclusive bound. 

Note this also applies to the reverse case

```
❯ uv pip install 'httpx<0.000001'
  × No solution found when resolving dependencies:
  ╰─▶ Because only httpx>=0.1 is available and you require httpx<0.1, we can conclude that the requirements are
      unsatisfiable.
```

However, for simplicity, I haven't tackled the case where we display multiple version segments yet. So for more complex requests, our messaging is still confusing:

```
❯ uv pip install 'httpx>99,<999'
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of httpx are available:
          httpx<=99
          httpx>=999
      and you require httpx>99,<999, we can conclude that the requirements are unsatisfiable.
```

This will require further refactoring, which I'll track in a new issue.

---

_Label `error messages` added by @zanieb on 2024-08-13 23:08_

---

_Marked ready for review by @zanieb on 2024-08-13 23:17_

---

_@zanieb reviewed on 2024-08-13 23:18_

---

_Review comment by @zanieb on `crates/uv-resolver/src/pubgrub/report.rs`:92 on 2024-08-13 23:18_

Suggestions welcome on the pattern here, but I'm going to open an issue to follow-up to make this reusable in the multi-segment case as well.

---

_Review requested from @charliermarsh by @zanieb on 2024-08-14 00:28_

---

_@charliermarsh approved on 2024-08-14 22:25_

Seems reasonable!

---

_Comment by @zanieb on 2024-08-15 16:27_

https://github.com/astral-sh/uv/pull/6118 is more comprehensive and would conflict.

---

_Closed by @zanieb on 2024-08-15 16:27_

---
