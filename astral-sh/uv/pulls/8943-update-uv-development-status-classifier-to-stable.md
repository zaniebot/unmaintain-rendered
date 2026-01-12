```yaml
number: 8943
title: "Update uv development status classifier to \"Stable\" on PyPI"
type: pull_request
state: merged
author: zanieb
labels:
  - releases
assignees: []
merged: true
base: main
head: zb/uv-stable
created_at: 2024-11-08T14:38:04Z
updated_at: 2024-11-09T02:21:03Z
url: https://github.com/astral-sh/uv/pull/8943
synced_at: 2026-01-12T16:08:34Z
```

# Update uv development status classifier to "Stable" on PyPI

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/8941

---

_Label `releases` added by @zanieb on 2024-11-08 14:38_

---

_@charliermarsh reviewed on 2024-11-08 14:53_

---

_Review comment by @charliermarsh on `pyproject.toml`:15 on 2024-11-08 14:53_

I think this needs to be "Development Status :: 5 - Production/Stable"

---

_@zanieb reviewed on 2024-11-08 15:17_

---

_Review comment by @zanieb on `pyproject.toml`:15 on 2024-11-08 15:17_

Thanks, I should have looked that up.

---

_Review comment by @zanieb on `pyproject.toml`:15 on 2024-11-08 15:17_

For reference https://pypi.org/classifiers/

---

_@zanieb reviewed on 2024-11-08 15:17_

---

_Review requested from @charliermarsh by @zanieb on 2024-11-08 18:15_

---

_Review comment by @mkniewallner on `pyproject.toml`:15 on 2024-11-08 21:17_

Maybe overkill, but https://pypi.org/project/validate-pyproject/ can validate a `pyproject.toml`, including classifiers. It would have indeed detected the issue here:

```console
$ uvx --from 'validate-pyproject[all]' validate-pyproject pyproject.toml
Invalid file: pyproject.toml
[ERROR] `project.classifiers[0]` must be trove-classifier
```

There's even a [`pre-commit` hook](https://validate-pyproject.readthedocs.io/en/stable/readme.html#pre-commit), so maybe that can be run locally only when `pyproject.toml` is updated, and every time on the CI as it should be quick to run? Happy to add it if you think it's worth doing.

---

_@mkniewallner reviewed on 2024-11-08 21:17_

---

_@zanieb reviewed on 2024-11-08 23:30_

---

_Review comment by @zanieb on `pyproject.toml`:15 on 2024-11-08 23:30_

I don't really feel the need for a pre-commit hook but a CI check seems nice to have! 

---

_@charliermarsh approved on 2024-11-09 02:20_

---

_Merged by @charliermarsh on 2024-11-09 02:21_

---

_Closed by @charliermarsh on 2024-11-09 02:21_

---

_Branch deleted on 2024-11-09 02:21_

---
