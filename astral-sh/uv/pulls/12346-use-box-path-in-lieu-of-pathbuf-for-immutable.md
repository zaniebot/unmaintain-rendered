```yaml
number: 12346
title: "Use `Box<Path>` in lieu of `PathBuf` for immutable structs"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/path
created_at: 2025-03-20T20:25:40Z
updated_at: 2025-03-25T21:56:07Z
url: https://github.com/astral-sh/uv/pull/12346
synced_at: 2026-01-10T11:10:39Z
```

# Use `Box<Path>` in lieu of `PathBuf` for immutable structs

---

_Pull request opened by @charliermarsh on 2025-03-20 20:25_

## Summary

I don't know if I actually want to commit this, but I did it on the plane last time and just polished it off (got it to compile) while waiting to board.


---

_Comment by @zanieb on 2025-03-20 20:30_

I read the title and thought "classic Charlie on the plane PR"

---

_Review requested from @BurntSushi by @charliermarsh on 2025-03-22 15:55_

---

_Label `performance` added by @charliermarsh on 2025-03-22 15:55_

---

_Marked ready for review by @charliermarsh on 2025-03-24 14:37_

---

_Comment by @BurntSushi on 2025-03-25 13:22_

It looks like there's [no measurable change in perf](https://codspeed.io/astral-sh/uv/branches/charlie%2Fpath), but I still feel like this is something we should do for "good sense" reasons. This also eliminates the possibility of any `PathBuf` being replaced here containing "extra" capacity (which would be a kind of space leak I guess). But I don't think any of our metrics really track that, so it's hard to know whether there is an actual improvement or not.

One interesting thing to consider would be a "small path" optimization. I'm not sure a crate for that exists. (A quick search didn't turn anything up.) It might be a little trickier to build than a small string optimization.

---

_@BurntSushi approved on 2025-03-25 13:23_

---

_Merged by @charliermarsh on 2025-03-25 21:56_

---

_Closed by @charliermarsh on 2025-03-25 21:56_

---

_Branch deleted on 2025-03-25 21:56_

---
