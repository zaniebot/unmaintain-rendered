```yaml
number: 11124
title: Fix link to license in README
type: pull_request
state: merged
author: KPCOFGS
labels:
  - documentation
assignees: []
merged: true
base: main
head: main
created_at: 2024-04-24T12:35:54Z
updated_at: 2024-04-26T02:08:13Z
url: https://github.com/astral-sh/ruff/pull/11124
synced_at: 2026-01-10T22:37:01Z
```

# Fix link to license in README

---

_Pull request opened by @KPCOFGS on 2024-04-24 12:35_

## Summary

Updated the readme file, so the license shields.io tag appeared in the README file would actually take users to the LICENSE file


---

_@MichaReiser approved on 2024-04-24 13:50_

Thanks

---

_Label `documentation` added by @MichaReiser on 2024-04-24 13:50_

---

_Renamed from "Update README.md" to "Fix link to license in README" by @MichaReiser on 2024-04-24 13:51_

---

_Merged by @MichaReiser on 2024-04-24 13:51_

---

_Closed by @MichaReiser on 2024-04-24 13:51_

---

_Comment by @charliermarsh on 2024-04-24 13:51_

Wait, I think this won't work properly when we publish to PyPI.

---

_Comment by @MichaReiser on 2024-04-24 13:53_

Hmm, I always forget about PyPI's markdown support. We can revert? Although I think the new version is better than the old one where it just linked back to PyPI?

---

_Comment by @charliermarsh on 2024-04-24 13:55_

We do have a `transform_readme.py` step where we could implement "rewrite for PyPI".

---

_Comment by @KPCOFGS on 2024-04-26 02:08_

Hello, I see what you all are talking about now. Please check out PR #11155 as an attempt to solve the issue

---
