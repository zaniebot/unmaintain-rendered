```yaml
number: 11338
title: Fix broken anchors in readme and docs index
type: pull_request
state: merged
author: letientai299
labels:
  - documentation
assignees: []
merged: true
base: main
head: tai/fix-broken-links
created_at: 2025-02-08T04:27:39Z
updated_at: 2025-02-08T15:21:08Z
url: https://github.com/astral-sh/uv/pull/11338
synced_at: 2026-01-12T16:09:48Z
```

# Fix broken anchors in readme and docs index

---

_@letientai299_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I've just fixed some broken anchors after browing ux doc site.

## Test Plan

- `index.md`: based on `mkdocs serve` log
- `readme.md`: manual check

## Aside

Do you manually keep `readme.md` and `index.md` partially sync? I've tried
checking pre-commit and other scripts but found no way to port my edits from one
to the other.

<!-- How was it tested? -->


---

_Comment by @zanieb on 2025-02-08 04:55_

Thank you!

Minor note this regressed in #11255 

I wonder if we stopped checking anchors in #11246? I tried transferring the `anchors: warn` but got an error that it was unknown and just dropped it.

---

_Label `documentation` added by @zanieb on 2025-02-08 04:55_

---

_@zanieb reviewed on 2025-02-08 04:56_

---

_Review comment by @zanieb on `README.md`:35 on 2025-02-08 04:56_

Also noting for follow-up, we should probably change this header in the README to match the docs index.

---

_Comment by @zanieb on 2025-02-08 04:58_

> Do you manually keep readme.md and index.md partially sync? I've tried
checking pre-commit and other scripts but found no way to port my edits from one
to the other.

It's manual. It could be nice to automate given it's easy to mess up — we just haven't invested in that. I think I explicitly opted for it to be manual because I figured the focus of the content should diverge.

We do have a [README transformation script](https://github.com/astral-sh/uv/blob/f992532f781fdc280805122e8f45b7fadf1f41ba/scripts/transform_readme.py) but it's just used for PyPI right now.

---

_@letientai299 reviewed on 2025-02-08 08:21_

---

_Review comment by @letientai299 on `README.md`:35 on 2025-02-08 08:21_

Updated, also fixed some external links to the docs.astral.sh site as well.

---

_@zanieb approved on 2025-02-08 15:20_

---

_Merged by @zanieb on 2025-02-08 15:21_

---

_Closed by @zanieb on 2025-02-08 15:21_

---
