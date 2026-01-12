```yaml
number: 5062
title: Migrate from MdBook to MkDocs
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/docs-base
created_at: 2024-07-15T01:51:53Z
updated_at: 2024-07-15T22:22:09Z
url: https://github.com/astral-sh/uv/pull/5062
synced_at: 2026-01-12T16:06:36Z
```

# Migrate from MdBook to MkDocs

---

_@charliermarsh_

## Summary

We want to have consistency between the Ruff and uv documentation for the upcoming release. We don't love the Ruff docs, but we'd rather have consistency and then work towards improving them both, rather than have two very-different documentation sites that both have weaknesses.

The setup here is simpler than in Ruff as: (1) we don't yet generate any docs from Rust and (2) we don't try to reuse the README in the uv documentation (which adds a lot of complexity in Ruff). So the change here is mostly a 1-to-1 port to MkDocs.

## Test Plan

![Screenshot 2024-07-14 at 9 49 15 PM](https://github.com/user-attachments/assets/8bfb5b06-08ff-4329-b368-d9087b78996e)


---

_@charliermarsh reviewed on 2024-07-15 01:52_

---

_Review comment by @charliermarsh on `.github/workflows/publish-docs.yml`:50 on 2024-07-15 01:52_

I may create a project here for deployment, or I'll revert back to pages for now.

---

_Review requested from @zanieb by @charliermarsh on 2024-07-15 02:00_

---

_Label `documentation` added by @charliermarsh on 2024-07-15 02:00_

---

_@charliermarsh reviewed on 2024-07-15 19:16_

---

_Review comment by @charliermarsh on `docs/guides/install-python.md`:39 on 2024-07-15 19:16_

Fixes.

---

_@charliermarsh reviewed on 2024-07-15 19:16_

---

_Review comment by @charliermarsh on `docs/first-steps.md`:36 on 2024-07-15 19:16_

I think?

---

_@charliermarsh reviewed on 2024-07-15 19:17_

---

_Review comment by @charliermarsh on `docs/preview/introduction.md`:1 on 2024-07-15 19:17_

Renamed to `index.md`.

---

_@charliermarsh reviewed on 2024-07-15 19:17_

---

_Review comment by @charliermarsh on `docs/pip/compatibility.md`:186 on 2024-07-15 19:17_

I think? Will be annoying to keep this in-sync with `PIP_COMPATIBILITY.md`, though maybe the goal is to remove that.

---

_@zanieb reviewed on 2024-07-15 20:21_

---

_Review comment by @zanieb on `docs/pip/compatibility.md`:186 on 2024-07-15 20:21_

Yes we'll move `PIP_COMPATIBILITY.md` to here and leave a placeholder with a link,

---

_@zanieb reviewed on 2024-07-15 20:22_

---

_Review comment by @zanieb on `docs/first-steps.md`:36 on 2024-07-15 20:22_

Maybe?

```suggestion
See the documentation on [Python version management](./python-versions.md) for more details on getting started.
```

Or maybe the Python guide? I can own fixing this eventually — lots to do in this doc.

---

_@zanieb reviewed on 2024-07-15 21:55_

---

_Review comment by @zanieb on `docs/requirements.txt`:1 on 2024-07-15 21:55_

Can we use an `requirements.in` or `pyproject.toml` for these too? Can we dog food here?

---

_@zanieb approved on 2024-07-15 21:56_

---

_@charliermarsh reviewed on 2024-07-15 22:10_

---

_Review comment by @charliermarsh on `docs/requirements.txt`:1 on 2024-07-15 22:10_

Yeah... I need to figure out what the input requirements were (this is copied from Ruff).

---

_Merged by @charliermarsh on 2024-07-15 22:22_

---

_Closed by @charliermarsh on 2024-07-15 22:22_

---

_Branch deleted on 2024-07-15 22:22_

---
