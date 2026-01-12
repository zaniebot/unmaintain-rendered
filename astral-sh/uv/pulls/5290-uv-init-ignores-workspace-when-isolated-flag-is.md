```yaml
number: 5290
title: "`uv init` ignores workspace when `--isolated` flag is used"
type: pull_request
state: merged
author: j178
labels:
  - cli
  - preview
assignees: []
merged: true
base: main
head: isolated
created_at: 2024-07-22T14:33:55Z
updated_at: 2024-07-22T21:07:28Z
url: https://github.com/astral-sh/uv/pull/5290
synced_at: 2026-01-12T16:06:44Z
```

# `uv init` ignores workspace when `--isolated` flag is used

---

_@j178_

## Summary

Per https://github.com/astral-sh/uv/pull/5250#issuecomment-2242137762

> It would also be great to have an argument (perhaps leveraging the global isolated option) that allows us to disable workspace discovery when we don't want to add a project as a member.


## Test Plan

```sh

$cargo run -- init work
$ cargo run -- init work/pkg-a --isolated
warning: `uv init` is experimental and may change without warning
Initialized project sub-c in /tmp/work
```


---

_@zanieb approved on 2024-07-22 14:41_

Thanks! Can you add a test case?

---

_Label `cli` added by @zanieb on 2024-07-22 14:41_

---

_Label `preview` added by @zanieb on 2024-07-22 14:41_

---

_Comment by @j178 on 2024-07-22 15:15_

Sure!

---

_Assigned to @charliermarsh by @zanieb on 2024-07-22 16:19_

---

_Comment by @zanieb on 2024-07-22 16:20_

I'm going to have @charliermarsh take a look here — we're thinking about the future of the `--isolated` flag.

---

_Review requested from @charliermarsh by @zanieb on 2024-07-22 16:35_

---

_@charliermarsh approved on 2024-07-22 19:12_

I think this is fine for now, since it's consistent with `uv run` and we need to revisit this alongside that usage.

---

_Merged by @charliermarsh on 2024-07-22 19:13_

---

_Closed by @charliermarsh on 2024-07-22 19:13_

---

_Branch deleted on 2024-07-22 21:07_

---
