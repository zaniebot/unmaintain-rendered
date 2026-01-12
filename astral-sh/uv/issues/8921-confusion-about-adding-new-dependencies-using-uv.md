```yaml
number: 8921
title: Confusion about adding new dependencies using uv add
type: issue
state: closed
author: TonyYanOnFire
labels:
  - question
assignees: []
created_at: 2024-11-08T06:13:56Z
updated_at: 2024-11-11T15:36:46Z
url: https://github.com/astral-sh/uv/issues/8921
synced_at: 2026-01-12T15:59:38Z
```

# Confusion about adding new dependencies using uv add

---

_@TonyYanOnFire_

Hello. I'm new to uv and trying to incorporate it into my existing project.

I executed the following command and got an error:
```
$uv add rdrobust==1.2.1
× No solution found when resolving dependencies for split (python_full_version >= '3.8' and python_full_version < '3.10'):
╰─▶ Because rdrobust==1.2.1 depends on scikit-learn>=1.2.0 and your project depends on rdrobust==1.2.1, we can conclude that your project depends on scikit-learn>=1.2.0.
And because your project depends on scikit-learn==1.0.2, we can conclude that your project's requirements are unsatisfiable.
help: If you want to add the package regardless of the failed resolution, provide the `--frozen` flag to skip locking and syncing.
```

I understand the reason for the error. What I am confused about is: I set `resolution = "lowest-direct"`. In such a situation that "the current foo dependency version is too low to meet the new bar dependency conditions", I expected that uv would Update the foo dependency to the minimum necessary version and update the lock file. Am I misunderstanding `uv add`? Or what should be the best practice for uv in this case?

---

_Comment by @charliermarsh on 2024-11-08 13:45_

`uv add` won't modify any of the _existing_ constraints in the `pyproject.toml`, and it will respect the constraint you provided. So if you already have `scikit-learn==1.0.2` in your project, and you `uv add rdrobust==1.2.1`, which requires `scikit-learn>=1.2.0`, there's no way for us to resolve the dependencies.

---

_Comment by @charliermarsh on 2024-11-08 13:45_

A difference would be if you did like... `uv add rdrobust>=1 --resolution lowest-direct`. Then we'd try to find the _lowest_ version of `rdrobust` that's greater than or equal to version 1, and still satisfies the rest of the requirements.

---

_Label `question` added by @charliermarsh on 2024-11-08 13:45_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-11-08 13:46_

---

_Closed by @charliermarsh on 2024-11-11 15:36_

---
