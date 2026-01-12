```yaml
number: 10080
title: Set PowerPC Page Size to 64KB
type: pull_request
state: merged
author: MichaReiser
labels:
  - bug
assignees: []
merged: true
base: main
head: ppc-page-size
created_at: 2024-02-22T13:35:42Z
updated_at: 2024-02-23T07:32:22Z
url: https://github.com/astral-sh/ruff/pull/10080
synced_at: 2026-01-12T15:55:31Z
```

# Set PowerPC Page Size to 64KB

---

_@MichaReiser_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/10073

We had a similar issue with aarch64 and the fix was to increase the page size to 64KB: https://github.com/astral-sh/ruff/issues/3791#issuecomment-1488100121

> The issue seems to be that jemalloc is compiled with fixed page sizes and using a larger page size than what it is compiled for segfaults. But jemalloc supports running on systems with smaller page sizes then configured at compile time. 
  Sources: [[1](https://github.com/jemalloc/jemalloc/issues/467), [2](https://gitlab.com/gitlab-org/omnibus-gitlab/-/merge_requests/4800)]


This PR does the same for PowerPC because widely used PowerPC systems use a default page size of 64KB [source](https://www.talospace.com/2020/10/where-did-64k-page-size-come-from.html)


## Test Plan
@nwf tested the built binary on their power pc



---

_Label `bug` added by @MichaReiser on 2024-02-22 13:36_

---

_@charliermarsh approved on 2024-02-22 17:44_

---

_Comment by @MichaReiser on 2024-02-22 18:46_

@nwf could you try one of the power pc binaries from (either wheel or from binaries) and report back if they are now working for you? 

https://github.com/astral-sh/ruff/actions/runs/8005385997#artifacts

I can't test it myself because I don't have a power pc cpu

---

_Comment by @nwf on 2024-02-23 05:35_

Much better, thanks!

```
0abaa1dc35a7% ./ruff 
Ruff: An extremely fast Python linter.
```

---

_Comment by @MichaReiser on 2024-02-23 07:32_

Awesome. Thank you @nwf for testing the change

---

_Merged by @MichaReiser on 2024-02-23 07:32_

---

_Closed by @MichaReiser on 2024-02-23 07:32_

---

_Branch deleted on 2024-02-23 07:32_

---
