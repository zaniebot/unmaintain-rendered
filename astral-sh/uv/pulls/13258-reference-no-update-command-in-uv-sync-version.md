```yaml
number: 13258
title: "Reference no update command in `uv sync` version mismatch"
type: pull_request
state: closed
author: maxmynter
labels: []
assignees: []
base: main
head: self-update-msg
created_at: 2025-05-02T03:15:16Z
updated_at: 2025-05-08T00:10:15Z
url: https://github.com/astral-sh/uv/pull/13258
synced_at: 2026-01-10T11:10:41Z
```

# Reference no update command in `uv sync` version mismatch

---

_Pull request opened by @maxmynter on 2025-05-02 03:15_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes: #13253 
## Summary
(Very) small change to refer to update command `uv self update` when the uv version is mismatched. 

I found only this one occurrence where a reference is suitable. Only other contenders are here 
https://github.com/astral-sh/uv/blob/21b9f62dbf0901e6ff453f11afb79a7ab89f1af1/crates/uv/src/commands/project/mod.rs#L81-L85
But I think this would rather convolute the message and they will run into the other message when running `uv sync`. 


## Note 
Happy to also try my look with implementing your feedback on #11869 if that's ok for you, @aarondobbing?
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Local test
<!-- How was it tested? -->


---

_Comment by @maxmynter on 2025-05-02 03:38_

I don't think that the CI failures are related to this change. Locally, I get them on main (041c7a5) too. 

---

_Comment by @konstin on 2025-05-02 08:04_

Fixed the CI error in #13259

---

_Assigned to @zanieb by @konstin on 2025-05-02 08:07_

---

_Review requested from @zanieb by @konstin on 2025-05-04 20:53_

---

_Comment by @charliermarsh on 2025-05-08 00:10_

Unfortunately this also got PR'd in https://github.com/astral-sh/uv/pull/13305, so sorry for the overlap!

---

_Closed by @charliermarsh on 2025-05-08 00:10_

---
