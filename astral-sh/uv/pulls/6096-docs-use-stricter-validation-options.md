```yaml
number: 6096
title: "docs: use stricter validation options"
type: pull_request
state: merged
author: mkniewallner
labels:
  - documentation
  - internal
assignees: []
merged: true
base: main
head: docs/stricter-validation-options
created_at: 2024-08-14T23:47:15Z
updated_at: 2024-08-15T00:36:07Z
url: https://github.com/astral-sh/uv/pull/6096
synced_at: 2026-01-12T16:07:12Z
```

# docs: use stricter validation options

---

_@mkniewallner_

## Summary

`mkdocs` supports [validation rules for links](https://www.mkdocs.org/user-guide/configuration/#validation), which can be tightened to report more issues than the default configuration. I used the recommended "maximal strictness" configuration from the documentation.

Adding the `anchors` rule helped spot 4 errors:
```console
WARNING -  Doc file 'guides/install-python.md' contains a link '../concepts/python-versions.md#python-distributions', but the doc 'concepts/python-versions.md' does not contain an anchor '#python-distributions'.
WARNING -  Doc file 'guides/install-python.md' contains a link '../concepts/python-versions.md#discovery-order', but the doc 'concepts/python-versions.md' does not contain an anchor '#discovery-order'.
WARNING -  Doc file 'guides/projects.md' contains a link '../concepts/projects.md#lock-file', but the doc 'concepts/projects.md' does not contain an anchor '#lock-file'.
WARNING -  Doc file 'pip/environments.md' contains a link '../concepts/python-versions.md#discovery-order', but the doc 'concepts/python-versions.md' does not contain an anchor '#discovery-order'.
```

## Test Plan

Local run of the documentation + `mkdocs build --strict`.

---

_Marked ready for review by @mkniewallner on 2024-08-14 23:51_

---

_@zanieb approved on 2024-08-15 00:27_

Awesome! Thank you!

---

_Label `documentation` added by @zanieb on 2024-08-15 00:27_

---

_Label `internal` added by @zanieb on 2024-08-15 00:27_

---

_Merged by @zanieb on 2024-08-15 00:27_

---

_Closed by @zanieb on 2024-08-15 00:27_

---

_Branch deleted on 2024-08-15 00:28_

---

_Comment by @mkniewallner on 2024-08-15 00:30_

https://github.com/astral-sh/uv/actions/runs/10396684003/job/28791143067 whoops, this broke the main build. That's because `anchors` is mkdocs 1.6+, and apparently insiders dependencies are installing 1.5.0.

---

_Comment by @zanieb on 2024-08-15 00:34_

Ah just noticed this broke on my branch. Thanks for noting it too :D

We'll need to upgrade insiders, but I recall there were some regressions we were blocked by...

---

_Comment by @mkniewallner on 2024-08-15 00:36_

> Ah just noticed this broke on my branch. Thanks for noting it too :D
> 
> We'll need to upgrade insiders, but I recall there were some regressions we were blocked by...

Ah, too bad! We can remove `anchors` for now if that's problematic, it's the only option that is 1.6+ only.

---
