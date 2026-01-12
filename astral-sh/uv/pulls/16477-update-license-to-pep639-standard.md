```yaml
number: 16477
title: Update license to pep639 standard
type: pull_request
state: open
author: wagenrace
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-10-28T09:48:43Z
updated_at: 2025-10-28T15:11:04Z
url: https://github.com/astral-sh/uv/pull/16477
synced_at: 2026-01-12T16:12:16Z
```

# Update license to pep639 standard

---

_@wagenrace_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

In [pep 639](https://peps.python.org/pep-0639/) the classifier for licenses gets deprecated in favor of the license field.
The benefits for UV:
- Clear that you can pick a license, not have to have both
- Clear it is version 2 for the Apache license

https://peps.python.org/pep-0639/

## Test Plan

Builds, works.
If the license shows up in pypi.org with the SPDX operator, it works


---

_Comment by @zanieb on 2025-10-28 12:16_

We've tried updating this before and it broke our release process.

---

_Comment by @wagenrace on 2025-10-28 12:43_

I see the red in the workflows 
Is it the pipelines or the older versions of Python?

Might want to look at it but do not want to reinvent to much

---

_Comment by @wagenrace on 2025-10-28 12:51_

The error I see is "Error: failed with: Error: missing API token, please run `depot login`"
I do not think this is code-related, right?

---

_Comment by @zanieb on 2025-10-28 13:09_

The Depot / Docker thing is unrelated.

---

_Comment by @zanieb on 2025-10-28 13:09_

I can't seem to find the failure from the previous attempt. @konstin may remember.

---

_Comment by @konstin on 2025-10-28 13:15_

It's in https://github.com/astral-sh/ruff/pull/19499 and the revert PRs. It should be fixed in maturin now but we should double check so we don't fail at the PyPI upload stage.

---

_Comment by @wagenrace on 2025-10-28 15:11_

Can that be tested without merging?
Uploading to [test.pypi.com](https://test.pypi.org/)?

---
