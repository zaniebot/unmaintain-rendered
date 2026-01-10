```yaml
number: 8627
title: Add option to remove elements from default exclusion
type: issue
state: open
author: ofek
labels:
  - configuration
assignees: []
created_at: 2023-11-12T04:48:27Z
updated_at: 2025-03-02T16:40:29Z
url: https://github.com/astral-sh/ruff/issues/8627
synced_at: 2026-01-10T11:09:50Z
```

# Add option to remove elements from default exclusion

---

_Issue opened by @ofek on 2023-11-12 04:48_

https://docs.astral.sh/ruff/settings/#exclude

I noticed that the following directories were being ignored:

- [src/hatch/cli/build](https://github.com/pypa/hatch/tree/master/src/hatch/cli/build)
- [src/hatch/venv](https://github.com/pypa/hatch/tree/master/src/hatch/venv)

Is there a better way (or should there be) than to redefine the entire set minus those two entries?

---

_Comment by @charliermarsh on 2023-11-12 20:38_

I don't think there's a better solution than this right now, though I'm also hesitant to further complicate the settings by having both a way to add and remove from the default set. Anyone else have good ideas on how to do better here?

---

_Label `configuration` added by @charliermarsh on 2023-11-12 20:38_

---

_Comment by @ofek on 2023-11-12 20:50_

I would bet a nontrivial amount of projects have a directory called `build`.

Perhaps for these two, only perform the check if a `pyproject.toml` is in the same directory? If that doesn't add too much overhead for file traversal

---

_Comment by @MichaReiser on 2023-11-27 07:26_

I haven't tried but adding a directory to `include` could work if it has higher precedence than `exclude`.

---

_Comment by @charliermarsh on 2023-11-27 17:14_

Include has lower precedence than exclude, so I don’t think that would work.

---

_Comment by @ofek on 2025-03-02 16:40_

For posterity, it looks like `build` is no longer excluded by default 😄 

---
