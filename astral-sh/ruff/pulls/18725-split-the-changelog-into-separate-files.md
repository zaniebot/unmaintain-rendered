```yaml
number: 18725
title: Split the changelog into separate files
type: pull_request
state: merged
author: ntBre
labels:
  - documentation
assignees: []
merged: true
base: main
head: brent/split-changelog
created_at: 2025-06-17T15:34:11Z
updated_at: 2025-06-17T17:27:38Z
url: https://github.com/astral-sh/ruff/pull/18725
synced_at: 2026-01-12T15:56:24Z
```

# Split the changelog into separate files

---

_@ntBre_

Summary
--

During the release today, I noticed that the changelog is finally too long to
render at all on GitHub. This PR follows the same splitting procedure as in
uv (astral-sh/uv#11510, astral-sh/uv#12099): first splitting the file into one
per minor version, and then reversing the contents of each file to start with
the breaking release (`changelogs/0.11.x.md` starts with 0.11.0 instead of
0.11.13 as in the old changelog).

For the second part, I used [`reverse-changelog.py`](https://github.com/astral-sh/uv/blob/main/scripts/reverse-changelog.py) from the uv repo, so hopefully everything is correct. I spot-checked 0.7.0 at least.

---

_Label `documentation` added by @ntBre on 2025-06-17 15:34_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-17 15:34_

---

_@MichaReiser approved on 2025-06-17 16:23_

Thanks. Can we add some instructions to the release step in the CONTRIBUTING.md. Just so that I won't forget splitting the CHANGELOG when I make the next minor release

---

_Merged by @ntBre on 2025-06-17 17:27_

---

_Closed by @ntBre on 2025-06-17 17:27_

---

_Branch deleted on 2025-06-17 17:27_

---
