```yaml
number: 9454
title: clarify best practice for python matrix strategy github workflow
type: pull_request
state: merged
author: zkurtz
labels:
  - documentation
assignees: []
merged: true
base: main
head: clarify-python-matrix-strategy-gh-workflow
created_at: 2024-11-26T22:21:49Z
updated_at: 2024-12-16T19:38:06Z
url: https://github.com/astral-sh/uv/pull/9454
synced_at: 2026-01-12T16:08:49Z
```

# clarify best practice for python matrix strategy github workflow

---

_@zkurtz_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Existing documentation on how to apply a matrix strategy over the python version in a github workflow is incomplete and possibly misleading. This PR deletes the existing section and creates a new one to reflect current best practice. See https://github.com/astral-sh/uv/issues/9376.

## Test Plan

N/A, only docs.


---

_Converted to draft by @zkurtz on 2024-11-26 23:15_

---

_Marked ready for review by @zkurtz on 2024-11-26 23:25_

---

_@zkurtz reviewed on 2024-11-27 00:58_

---

_Review comment by @zkurtz on `docs/guides/integration/github.md`:98 on 2024-11-27 00:58_

IIUC https://github.com/astral-sh/uv/issues/9376#issuecomment-2501200380, this method would be inneffective in case a `.python-version` file exists.

---

_Comment by @eifinger on 2024-11-28 20:52_

Your PR triggered me to include this directly into `setup-uv`: https://github.com/astral-sh/setup-uv/releases/tag/v4.1.0

Would you mind updating the PR with the new input instead of the extra step? 😉 

---

_Comment by @zkurtz on 2024-11-28 23:07_

@eifinger thanks for the updated `setup-uv`. My PR is updated accordingly.

---

_@eifinger reviewed on 2024-11-29 06:47_

The previous version highlighted the part where `${{ matrix.python-version }}` was used. Otherwise looks good to me.

---

_Review requested from @eifinger by @zkurtz on 2024-11-29 12:12_

---

_Comment by @zkurtz on 2024-11-29 12:13_

Added highlighting. I counted lines carefully, but I don't see how to render the file to visually confirm the highlighting

---

_@eifinger approved on 2024-11-29 12:14_

@zanieb Looks good to me

---

_Comment by @zkurtz on 2024-12-15 12:51_

This has sat for a couple of weeks ... I wonder if that's normal or if I should ping a maintainer.

---

_Comment by @zanieb on 2024-12-16 19:37_

Sorry I have quite the review backlog lately.

---

_@zanieb approved on 2024-12-16 19:37_

---

_Label `documentation` added by @zanieb on 2024-12-16 19:38_

---

_Merged by @zanieb on 2024-12-16 19:38_

---

_Closed by @zanieb on 2024-12-16 19:38_

---
