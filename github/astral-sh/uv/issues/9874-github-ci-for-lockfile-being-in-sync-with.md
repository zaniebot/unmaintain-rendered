---
number: 9874
title: GitHub CI for lockfile being in sync with pyproject.toml fails
type: issue
state: closed
author: superlopuh
labels: []
assignees: []
created_at: 2024-12-13T16:34:25Z
updated_at: 2024-12-15T14:21:49Z
url: https://github.com/astral-sh/uv/issues/9874
synced_at: 2026-01-07T13:12:18-06:00
---

# GitHub CI for lockfile being in sync with pyproject.toml fails

---

_Issue opened by @superlopuh on 2024-12-13 16:34_

I am trying to set up a CI that will prevent merging lockfiles out of sync with pyproject.toml:

https://github.com/xdslproject/xdsl/pull/3639/files#diff-2c6e103334c617ab830e3e4463daaafe724b1e9290af26cd928b8b8eb6df55e9R31-R35

What am I doing wrong? Locally, `uv sync --extra gui --extra dev --extra jax --extra riscv --locked` exits successfully, even outside the venv.

---

_Comment by @superlopuh on 2024-12-13 16:35_

This is the failing job: https://github.com/xdslproject/xdsl/actions/runs/12319379803/job/34386281895?pr=3639

It errors out with an uninformative message:
```
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

---

_Comment by @charliermarsh on 2024-12-13 16:44_

If you run with `--verbose`, it will tell you what specifically is out-of-date.

---

_Comment by @superlopuh on 2024-12-13 17:06_

Thank you, I added --verbose, and have no idea what's out of date:

https://github.com/xdslproject/xdsl/actions/runs/12319900775/job/34388006772?pr=3639

---

_Comment by @superlopuh on 2024-12-13 17:09_

Is this even the recommended way to do this? I want to make it impossible to merge a PR, either from Dependabot, or another contributor, that updates pyproject.toml in a way that conflicts with the lockfile. Maybe there's already a guide for this?

---

_Comment by @charliermarsh on 2024-12-13 17:11_

It says:

```
Ignoring existing lockfile due to mismatched version: `xdsl` (expected: `0.25.0+50.g582ac851`, found: `0+untagged.1.g3b3a249`)
```

---

_Comment by @superlopuh on 2024-12-13 17:17_

OK so I'm using it wrong? This is the project I'm currently running the CI for, the lockfile should be of the current commit with the current tags etc, no?

---

_Comment by @superlopuh on 2024-12-13 17:32_

How is the version calculated in the lockfile? We use versioneer, whose dynamic version calculation may well not work in the CI. Would one possible solution be to add an option to ignore the version mismatch?

---

_Comment by @superlopuh on 2024-12-13 17:34_

Is the +1/+50 the number of commits since the version tag? If so, it seems like this approach will definitely not work in the way that I'd like it to, as the lockfile would have to be updated on every commit.

---

_Comment by @zanieb on 2024-12-13 17:51_

See #7533 

---

_Comment by @superlopuh on 2024-12-13 18:07_

Thank you, I had a feeling this should have come up already, but couldn't find it.

---

_Closed by @superlopuh on 2024-12-13 18:07_

---

_Comment by @superlopuh on 2024-12-13 19:18_

Is this a Very Bad Idea?

I added an override to our makefile command that returns a dynamic version when installing locally, and a similar one on the CI. It correctly passes on the branch without a dependency update, and fails on the branch that requires one:

https://github.com/xdslproject/xdsl/actions/runs/12321584036/job/34393305969?pr=3639
https://github.com/xdslproject/xdsl/actions/runs/12321623323/job/34393428033?pr=3634

These are the relevant changes:

https://github.com/xdslproject/xdsl/pull/3639/files

I read through both the thread linked and the PEP discussion, and it looks like there's an aspect that I don't quite understand: that lockfiles are supposed to pin both the dependencies and the currently edited project. In my understanding, the objective of the lockfile is a declarative way to recreate the exact venv for everything to work, but it doesn't exist in a vacuum, and the surrounding context from the git commit should let uv recreate the venv of editable installs from only the dependencies, and not the version of the project itself. Am I wrong in my understanding? Would this question be better for the linked discussion?

---

_Comment by @thcrt on 2024-12-14 17:41_

> Is this even the recommended way to do this? I want to make it impossible to merge a PR, either from Dependabot, or another contributor, that updates pyproject.toml in a way that conflicts with the lockfile. Maybe there's already a guide for this?

Sorry for stepping in, but I'm curious as to why you would want this? Surely that would make it impossible for any contributor to update or add any dependencies?

---

_Comment by @superlopuh on 2024-12-15 14:21_

It's still possible if the PR contains the changes to both pyproject.toml and uv.lock. The objective here is a guarantee that if the dependency changes in the pyproject.toml this is reflected in the lockfile. 

---
