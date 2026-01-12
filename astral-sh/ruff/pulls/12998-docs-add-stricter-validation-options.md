```yaml
number: 12998
title: "docs: add stricter validation options"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/stricter-validation-options
created_at: 2024-08-19T22:23:41Z
updated_at: 2024-08-20T05:44:29Z
url: https://github.com/astral-sh/ruff/pull/12998
synced_at: 2026-01-12T15:55:42Z
```

# docs: add stricter validation options

---

_@mkniewallner_

## Summary

Applying the same change as done in https://github.com/astral-sh/uv/pull/6096. Note that in `uv` repository, this [broke the docs build](https://github.com/astral-sh/uv/pull/6096#issuecomment-2290151150) because `anchors` is `mdkocs` 1.6+ only, and insiders used 1.5.0 while public dependencies used 1.6.0, but in this repository, both use 1.6.0 ([public](https://github.com/astral-sh/ruff/blob/049cda2ff37bcae59f7dae9af6a453075c76e635/docs/requirements.txt#L3), [insiders](https://github.com/astral-sh/ruff/blob/049cda2ff37bcae59f7dae9af6a453075c76e635/docs/requirements-insiders.txt#L3)), so this should not be an issue to have in the template.

Contrarily to `uv` repository, no violations were reported here, but this could prevent adding some in the future.

## Test Plan

Local run of the documentation + `mkdocs build --strict`.

---

_Marked ready for review by @mkniewallner on 2024-08-19 22:29_

---

_@zanieb approved on 2024-08-19 22:57_

Thanks!

---

_Merged by @zanieb on 2024-08-19 23:07_

---

_Closed by @zanieb on 2024-08-19 23:07_

---

_Label `documentation` added by @dhruvmanila on 2024-08-20 05:44_

---
