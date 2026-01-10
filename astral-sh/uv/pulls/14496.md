```yaml
number: 14496
title: "Add `--workspace` flag to `uv add`"
type: pull_request
state: merged
author: charliermarsh
labels:
  - enhancement
  - cli
assignees: []
merged: true
base: main
head: charlie/w
created_at: 2025-07-07T20:56:26Z
updated_at: 2025-07-09T17:46:22Z
url: https://github.com/astral-sh/uv/pull/14496
synced_at: 2026-01-10T06:53:02Z
```

# Add `--workspace` flag to `uv add`

---

_Pull request opened by @charliermarsh on 2025-07-07 20:56_

## Summary

You can now pass `--workspace` to `uv add` to add a path dependency as a workspace member.

Closes https://github.com/astral-sh/uv/issues/14464.


---

_Comment by @charliermarsh on 2025-07-07 21:29_

~I think this doesn't properly revert the changes to the root `pyproject.toml`.~ (Fixed)

---

_Marked ready for review by @charliermarsh on 2025-07-09 00:04_

---

_Review requested from @zanieb by @charliermarsh on 2025-07-09 00:04_

---

_Review requested from @konstin by @charliermarsh on 2025-07-09 00:04_

---

_Label `enhancement` added by @charliermarsh on 2025-07-09 00:04_

---

_Label `cli` added by @charliermarsh on 2025-07-09 00:04_

---

_@zanieb approved on 2025-07-09 15:03_

Should this just be the default behavior for path dependencies in the current source tree?

---

_Comment by @zanieb on 2025-07-09 15:04_

(i.e., since `uv init` in a project will make it a workspace member by default)

---

_Comment by @charliermarsh on 2025-07-09 15:05_

Yeah, like anything under the workspace root?

---

_Comment by @zanieb on 2025-07-09 15:06_

Yeah, with `--no-workspace` to opt-out?

---

_Comment by @charliermarsh on 2025-07-09 15:07_

Yeah, that makes sense. Do you think that specific change should go out in v0.8 though, since it's a change in behavior?

---

_Comment by @zanieb on 2025-07-09 15:18_

I think it'd be okay either way, but that would be reasonable.

---

_Merged by @charliermarsh on 2025-07-09 15:46_

---

_Closed by @charliermarsh on 2025-07-09 15:46_

---

_Branch deleted on 2025-07-09 15:46_

---

_Comment by @zhuoqun-chen on 2025-07-09 17:46_

Hi @charliermarsh and @zanieb, sorry to bother here, I'm wondering if `uv add --workspace` supports adding existing paths as workspace members via relative paths. (I'm not familiar with `rust` so not sure my question is actually related or not, correct me if I'm wrong. :D)

say I have such tree on my host machine:

```log
/home/username/work
├── mono-a/
│   ├── .devcontainer/
│   ├── libs/
│   └── pyproject.toml
├── mono-b/
│   └── pyproject.toml
├── orphan-1/
│   └── pyproject.toml
├── orphan-2/
│   └── pyproject.toml
└── orphan-3/
    └── pyproject.toml
```

if I want to add existing `orphan-1` project as a workspace member of `mono-a` into `mono-a/libs/orphan-1`, will `uv` add it as `/home/username/work/orphan-1` or `../../orphan-1`?

My use case prefers the `../../orphan-1`.

My current practice before this commit is:

1. I open `mono-a` as a vscode project and
2. I `reopen it in devcontainer` and mount `/home/username/work` as `/workspace` inside the container, which makes all the required members visible inside `/workspace/mono-a` after properly creating all the needed relative symlinks before entering the container.

Or if there is any better practice I'm missing here?

---
