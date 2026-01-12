```yaml
number: 12963
title: "Improve formatting for `\"all\"` `default-groups` setting documentation"
type: pull_request
state: merged
author: johnthagen
labels: []
assignees: []
merged: true
base: main
head: patch-1
created_at: 2025-04-18T11:46:14Z
updated_at: 2025-04-18T16:44:59Z
url: https://github.com/astral-sh/uv/pull/12963
synced_at: 2026-01-12T16:10:28Z
```

# Improve formatting for `"all"` `default-groups` setting documentation

---

_@johnthagen_

## Summary

Make the documentation for `"all"` `defauilt-groups` a little easier to read by monospacing the literal.


---

_@samypr100 approved on 2025-04-18 12:44_

---

_Comment by @charliermarsh on 2025-04-18 13:04_

I think we need to change the source here since this file is generated.

---

_Comment by @charliermarsh on 2025-04-18 13:29_

(Ironically the tests that validate it don't run if you _only_ edit Markdown.)

---

_Comment by @samypr100 on 2025-04-18 13:33_

> (Ironically the tests that validate it don't run if you _only_ edit Markdown.)

☝️ 😂

---

_Comment by @johnthagen on 2025-04-18 15:10_

@charliermarsh I updated the source file and ran `cargo dev generate-all`. Let me know if I did this correctly as it's my first time 😅 .

Also please squash the commits when merging if you accept.

---

_@charliermarsh approved on 2025-04-18 15:13_

---

_Comment by @johnthagen on 2025-04-18 15:14_

@charliermarsh Is the deadsnakes CI failure spurious? 🤔 

- https://github.com/astral-sh/uv/actions/runs/14537239389/job/40788046679?pr=12963

---

_@charliermarsh approved on 2025-04-18 15:34_

---

_Merged by @charliermarsh on 2025-04-18 15:34_

---

_Closed by @charliermarsh on 2025-04-18 15:34_

---

_Branch deleted on 2025-04-18 16:44_

---
