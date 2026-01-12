```yaml
number: 12174
title: "feat(rules): detect `httpx` for `S113`"
type: pull_request
state: merged
author: mkniewallner
labels:
  - rule
assignees: []
merged: true
base: main
head: feat/detect-httpx-S113
created_at: 2024-07-03T22:31:43Z
updated_at: 2024-07-06T16:16:52Z
url: https://github.com/astral-sh/ruff/pull/12174
synced_at: 2026-01-12T15:55:40Z
```

# feat(rules): detect `httpx` for `S113`

---

_@mkniewallner_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Bandit now also reports `B113` on `httpx` (https://github.com/PyCQA/bandit/pull/1060). This PR implements the same logic, to detect missing or `None` timeouts for `httpx` alongside `requests`.

## Test Plan

Snapshot tests.

---

_@charliermarsh approved on 2024-07-03 22:38_

Thanks -- this looks good to me!

---

_Label `rule` added by @charliermarsh on 2024-07-03 22:38_

---

_Marked ready for review by @mkniewallner on 2024-07-03 22:45_

---

_Merged by @charliermarsh on 2024-07-03 23:26_

---

_Closed by @charliermarsh on 2024-07-03 23:26_

---

_Branch deleted on 2024-07-04 07:00_

---

_Comment by @jyggen on 2024-07-05 17:22_

Not sure a missing timeout is a problem since `httpx` uses 5s as default. The rule is about preventing the client from waiting indefinitely, which it never will in the case of `httpx` and a missing timeout. Setting `timeout` to `None` is still problematic though.

---

_Comment by @trim21 on 2024-07-06 02:44_

httpx has default timeout， this PR would be a false positive

https://www.python-httpx.org/advanced/timeouts/

>HTTPX is careful to enforce timeouts everywhere by default.
>
> The default behavior is to raise a TimeoutException after 5 seconds of network inactivity.

> B113: Test for missing requests timeout
This plugin test checks for requests or httpx calls without a timeout specified.
>
> Nearly all production code should use this parameter in nearly all requests, **Failure to do so can cause your program to hang indefinitely.**

---

_Comment by @mkniewallner on 2024-07-06 15:55_

> Not sure a missing timeout is a problem since `httpx` uses 5s as default. The rule is about preventing the client from waiting indefinitely, which it never will in the case of `httpx` and a missing timeout. Setting `timeout` to `None` is still problematic though.

Indeed, really sorry for missing that obvious information, I should have better checked that.

---

_Comment by @trim21 on 2024-07-06 16:16_

false positive fix https://github.com/astral-sh/ruff/pull/12213

---
