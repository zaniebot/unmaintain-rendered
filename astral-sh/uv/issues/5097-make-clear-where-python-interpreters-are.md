```yaml
number: 5097
title: Make clear where Python interpreters are downloaded from
type: issue
state: open
author: matterhorn103
labels:
  - documentation
assignees: []
created_at: 2024-07-16T09:17:34Z
updated_at: 2024-07-16T14:16:53Z
url: https://github.com/astral-sh/uv/issues/5097
synced_at: 2026-01-12T15:58:54Z
```

# Make clear where Python interpreters are downloaded from

---

_@matterhorn103_

Just a thought that for transparency it would be nice if it was made obvious what the source is for `uv python install`.

At the very least it would be good to show it when `uv python list` is called.

Equally `uv help python` or `uv python -h` might give a short explanation as to where the interpreters come from, who compiles them and is responsible for them, etc.

It'd also be good if the source URL was printed during installation. The information is of course available by running with the `--verbose` flag but naturally this is not normally turned on.

Distro package managers also usually tell you what they have planned before they do it. That is not the approach you have taken for `uv pip install` or `uv add` either, which I assume is a design decision, but it would still be good if it was made more clear post-execution what was done and the URL was at least printed during the installation process.

---

_Label `documentation` added by @zanieb on 2024-07-16 14:16_

---
