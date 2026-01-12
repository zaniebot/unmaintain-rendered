```yaml
number: 12376
title: "Add `--show-version` to `uv python find`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/find-version
created_at: 2025-03-21T18:05:34Z
updated_at: 2025-04-03T13:34:47Z
url: https://github.com/astral-sh/uv/pull/12376
synced_at: 2026-01-12T16:10:15Z
```

# Add `--show-version` to `uv python find`

---

_@zanieb_

@jtfmumm mentioned a desire for this. I'm not sure how we should do this. I kind of want to change this to something like...

```
$ uv python find
CPython 3.13 @ <path>
$ uv python find --only-path
<path>
$ uv python find --short
<path>
$ uv python find --only-version 
3.13
```

The change in defaults would be breaking though.

---

_Label `enhancement` added by @zanieb on 2025-03-21 18:05_

---

_Review comment by @Gankra on `crates/uv/src/commands/python/find.rs`:100 on 2025-03-21 18:19_

While you're here can you migrate this to writeln's with a printer?

---

_@Gankra approved on 2025-03-21 18:23_

The implementation is fine (also oops just realized this is a draft)

---

_Comment by @Gankra on 2025-03-21 18:26_

I agree with everything in your main comment. I'd be inclined to use -v for the nicer output but we use that for some very different things that you don't want here.

---

_Comment by @zanieb on 2025-03-21 20:15_

Open to additional thoughts on a transition plan or flag name.

---

_Marked ready for review by @zanieb on 2025-03-21 20:15_

---

_@charliermarsh reviewed on 2025-03-21 22:37_

---

_Review comment by @charliermarsh on `crates/uv-cli/src/lib.rs`:4765 on 2025-03-21 22:37_

Or `--version`? (Defer to you.)

---

_@charliermarsh approved on 2025-03-21 22:37_

---

_@zanieb reviewed on 2025-03-23 03:07_

---

_Review comment by @zanieb on `crates/uv-cli/src/lib.rs`:4765 on 2025-03-23 03:07_

Yeah I have a preference for `--version` but it's taken.

```
❯ uv python find --version
uv-python-find 0.6.9 (3d9460278 2025-03-20)
```

Do you think we should override it like that?

---

_Review requested from @jtfmumm by @zanieb on 2025-03-24 15:02_

---

_@jtfmumm approved on 2025-03-24 15:33_

---

_Merged by @zanieb on 2025-04-03 13:34_

---

_Closed by @zanieb on 2025-04-03 13:34_

---

_Branch deleted on 2025-04-03 13:34_

---
