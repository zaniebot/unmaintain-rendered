```yaml
number: 12894
title: "correctly describe the compatible release clause \"~=\""
type: pull_request
state: closed
author: opyate
labels: []
assignees: []
base: main
head: patch-1
created_at: 2025-04-15T09:31:58Z
updated_at: 2025-04-15T09:56:13Z
url: https://github.com/astral-sh/uv/pull/12894
synced_at: 2026-01-12T16:10:26Z
```

# correctly describe the compatible release clause "~="

---

_@opyate_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Correctly describe the "compatible release clause" represented by `~=`.

## Test Plan

No test plan, but a review is required.


---

_Comment by @konstin on 2025-04-15 09:38_

While it is true that the spec defines compatible releases in terms of star releases, for the concepts page, the shorter single sentence explanation is more applicable. The goal of the page is to give users a working understanding that they can use to define dependencies in their project, without covering edge cases or formal definitions.

---

_Comment by @opyate on 2025-04-15 09:56_

Thanks, I squinted a bit and the clauses in the existing doc actually does seem equivalent 😅 


---

_Closed by @opyate on 2025-04-15 09:56_

---
