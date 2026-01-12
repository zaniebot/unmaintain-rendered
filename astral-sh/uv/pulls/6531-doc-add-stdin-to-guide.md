```yaml
number: 6531
title: "doc: add stdin to guide"
type: pull_request
state: merged
author: billy-doyle
labels:
  - documentation
assignees: []
merged: true
base: main
head: billy/additional-stdin-docs
created_at: 2024-08-23T16:03:03Z
updated_at: 2024-08-23T16:18:54Z
url: https://github.com/astral-sh/uv/pull/6531
synced_at: 2026-01-12T16:07:24Z
```

# doc: add stdin to guide

---

_@billy-doyle_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
Document in guide stdin usage

alllll the easter eggs can do as well, but declined to keep consistent with the other examples 😆 

Additions to https://github.com/astral-sh/uv/pull/6481

```bash
$ uv run - <<EOF               
import antigravity
EOF
```

## Test Plan

<!-- How was it tested? -->


https://github.com/astral-sh/uv/pull/6519#issuecomment-2307371063 new PR

---

_Review comment by @zanieb on `docs/guides/scripts.md`:65 on 2024-08-23 16:06_

Minor problem, the copy paste button only grabs `uv run - <<EOF` because the rest is treated as output. Maybe we should do

```suggestion
```bash
uv run - <<EOF
print("hello world!")
EOF
```
```

---

_@zanieb reviewed on 2024-08-23 16:06_

---

_@zanieb reviewed on 2024-08-23 16:07_

---

_Review comment by @zanieb on `docs/guides/scripts.md`:65 on 2024-08-23 16:07_

(GitHub removed the trailing ``` in the suggestion, will need to be added)

---

_@billy-doyle reviewed on 2024-08-23 16:07_

---

_Review comment by @billy-doyle on `docs/guides/scripts.md`:65 on 2024-08-23 16:07_

oh gotcha ok. and should we leave the actual output off the doc?

---

_@zanieb reviewed on 2024-08-23 16:13_

---

_Review comment by @zanieb on `docs/guides/scripts.md`:65 on 2024-08-23 16:13_

I think that's the best option here.

---

_@billy-doyle reviewed on 2024-08-23 16:14_

---

_Review comment by @billy-doyle on `docs/guides/scripts.md`:65 on 2024-08-23 16:14_

all g, done

---

_@zanieb approved on 2024-08-23 16:18_

Thanks!

---

_Merged by @zanieb on 2024-08-23 16:18_

---

_Closed by @zanieb on 2024-08-23 16:18_

---

_Label `documentation` added by @zanieb on 2024-08-23 16:18_

---
