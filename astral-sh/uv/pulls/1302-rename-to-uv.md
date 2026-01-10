```yaml
number: 1302
title: "Rename to `uv`"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: zb/uv
created_at: 2024-02-14T22:40:19Z
updated_at: 2024-02-15T17:19:47Z
url: https://github.com/astral-sh/uv/pull/1302
synced_at: 2026-01-10T15:33:24Z
```

# Rename to `uv`

---

_Pull request opened by @zanieb on 2024-02-14 22:40_

First, replace all usages in files in-place. I used my editor for this. If someone wants to add a one-liner that'd be fun.

Then, update directory and file names:

```
# Run twice for nested directories
find . -type d -print0 | xargs -0 rename s/puffin/uv/g
find . -type d -print0 | xargs -0 rename s/puffin/uv/g

# Update files
find . -type f -print0 | xargs -0 rename s/puffin/uv/g
```

Then add all the files again

```
# Add all the files again
git add crates
git add python/uv

# This one needs a force-add
git add -f crates/uv-trampoline
```

---

_Comment by @zanieb on 2024-02-15 17:08_

To make my life easier, merged with:

```
git merge main -X theirs
```

Then replaced all the "new" "Puffin" usages with "uv".

Then repaired some duplicated imports.



---

_@charliermarsh approved on 2024-02-15 17:10_

---

_Marked ready for review by @zanieb on 2024-02-15 17:11_

---

_Merged by @zanieb on 2024-02-15 17:19_

---

_Closed by @zanieb on 2024-02-15 17:19_

---

_Branch deleted on 2024-02-15 17:19_

---
