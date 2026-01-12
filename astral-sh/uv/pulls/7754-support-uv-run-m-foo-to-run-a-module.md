```yaml
number: 7754
title: "Support `uv run -m foo` to run a module"
type: pull_request
state: merged
author: j178
labels:
  - cli
assignees: []
merged: true
base: main
head: run-module
created_at: 2024-09-28T07:19:43Z
updated_at: 2024-10-01T13:45:46Z
url: https://github.com/astral-sh/uv/pull/7754
synced_at: 2026-01-12T16:07:58Z
```

# Support `uv run -m foo` to run a module

---

_@j178_

## Summary

This is another attempt using `module: bool` instead of `module: Option<String>` following #7322. 
The original PR can't be reopened after a force-push to the branch, I've created this new PR.

Resolves #6638

---

_Renamed from "Support uv run -m foo to run a module" to "Support `uv run -m foo` to run a module" by @j178 on 2024-09-28 07:19_

---

_Marked ready for review by @j178 on 2024-09-28 07:46_

---

_Comment by @bluss on 2024-09-28 11:41_

Here's a tricky test case: does the equivalent of `uv run python -m pdb -m tarfile` work?  pdb is an example of a module that accepts `-m` in its own command (to launch debugging for a module..)

---

_Comment by @j178 on 2024-09-28 11:47_

`uv run -m pdb -m tarfile` does work:
```console
$ cargo run -- run -m pdb -m tarfile
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.38s
     Running `target\debug\uv.exe run -m pdb -m tarfile`
> c:\users\nigel\appdata\roaming\uv\data\python\cpython-3.12.1-windows-x86_64-none\lib\tarfile.py(29)<module>()
-> """Read from and write to tar format archives.
```

---

_@zanieb approved on 2024-09-28 15:07_

Thanks!

---

_Merged by @zanieb on 2024-09-28 15:07_

---

_Closed by @zanieb on 2024-09-28 15:07_

---

_Label `cli` added by @zanieb on 2024-09-28 15:07_

---

_Branch deleted on 2024-09-29 02:53_

---

_Comment by @a-reich on 2024-10-01 13:02_

I came to this repo to ask for adding a shortcut to `uv run python -m`, and to my delight found this was already merged 😃 wonderful job!

---

_Comment by @bluss on 2024-10-01 13:45_

I guess that it allows reordering -m, anywhere before the module name, but maybe that doesn't matter (?)

---
