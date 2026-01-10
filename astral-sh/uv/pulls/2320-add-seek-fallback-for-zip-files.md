```yaml
number: 2320
title: "Add `Seek` fallback for zip files"
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - compatibility
assignees: []
merged: true
base: main
head: charlie/data-descriptors
created_at: 2024-03-10T02:26:10Z
updated_at: 2024-03-10T16:33:52Z
url: https://github.com/astral-sh/uv/pull/2320
synced_at: 2026-01-10T14:54:43Z
```

# Add `Seek` fallback for zip files

---

_Pull request opened by @charliermarsh on 2024-03-10 02:26_

## Summary

Some zip files can't be streamed; in particular, `rs-async-zip` doesn't support data descriptors right now (though it may in the future). This PR adds a fallback path for such zips that downloads the entire zip file to disk, then unzips it from disk (which gives us `Seek`).

Closes https://github.com/astral-sh/uv/issues/2216.

## Test Plan

`cargo run pip install --extra-index-url https://buf.build/gen/python hashb_foxglove_protocolbuffers_python==25.3.0.1.20240226043130+465630478360 --force-reinstall -n`


---

_Label `bug` added by @charliermarsh on 2024-03-10 02:26_

---

_Label `compatibility` added by @charliermarsh on 2024-03-10 02:26_

---

_Comment by @charliermarsh on 2024-03-10 02:33_

Somewhat torn on merging this. It's good behavior but it does add complexity, and `rs-async-zip` _might_ add data descriptor support soon (TBD) -- see: https://github.com/Majored/rs-async-zip/issues/122.

---

_Comment by @charliermarsh on 2024-03-10 02:54_

Eh, ultimately, this is a good thing to do.

---

_Comment by @charliermarsh on 2024-03-10 03:01_

All these Windows tests are randomly overflowing their stacks is confusing to me. This shouldn't have changed the stack at all?

---

_Merged by @charliermarsh on 2024-03-10 15:39_

---

_Closed by @charliermarsh on 2024-03-10 15:39_

---

_Branch deleted on 2024-03-10 15:39_

---

_Comment by @konstin on 2024-03-10 15:58_

> All these Windows tests are randomly overflowing their stacks is confusing to me. This shouldn't have changed the stack at all?

Every local variable increases the stack frame size :sob: 

---

_Comment by @charliermarsh on 2024-03-10 16:03_

Nooooo

---

_Comment by @charliermarsh on 2024-03-10 16:04_

It must be _so_ marginal

---

_Comment by @konstin on 2024-03-10 16:33_

debug is just super inefficient with stack space, this never happens on release

---
