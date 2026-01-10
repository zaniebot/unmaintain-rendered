```yaml
number: 3658
title: Add notes for testing on windows
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - windows
assignees: []
merged: true
base: main
head: zb/contrib-win
created_at: 2024-05-19T18:49:51Z
updated_at: 2024-07-12T10:54:16Z
url: https://github.com/astral-sh/uv/pull/3658
synced_at: 2026-01-10T13:42:52Z
```

# Add notes for testing on windows

---

_Pull request opened by @zanieb on 2024-05-19 18:49_

_No description provided._

---

_Label `documentation` added by @zanieb on 2024-05-19 18:49_

---

_Label `windows` added by @zanieb on 2024-05-19 18:50_

---

_@zanieb reviewed on 2024-05-19 18:54_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:68 on 2024-05-19 18:54_

Should we move this into a code block with proper powershell export syntax?

---

_@charliermarsh reviewed on 2024-05-20 00:36_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:68 on 2024-05-20 00:36_

Sure, makes sense.

---

_@charliermarsh reviewed on 2024-05-20 00:36_

---

_Review comment by @charliermarsh on `CONTRIBUTING.md`:74 on 2024-05-20 00:36_

Can you capitalize Docker while you're here? Hah.

---

_@charliermarsh approved on 2024-05-20 00:36_

---

_@zanieb reviewed on 2024-05-20 01:48_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:74 on 2024-05-20 01:48_

😝 

---

_@zanieb reviewed on 2024-05-20 12:44_

---

_Review comment by @zanieb on `CONTRIBUTING.md`:68 on 2024-05-20 12:44_

fwiw I'll need to Google this. If someone else knows it off the top of there head and gets to it before me feel free to suggest / commit it :)

---

_Review comment by @konstin on `CONTRIBUTING.md`:70 on 2024-05-21 08:25_

```suggestion
```powershell
```

---

_@konstin approved on 2024-05-21 08:25_

---

_Comment by @konstin on 2024-05-21 08:31_

For anyone who comes looking at this in future: The cause is that nested futures in debug mode are massive, while they get compiled down to a reasonable size in release mode. You can debug this with some variation of (unix only). Install from https://github.com/loyd/top-type-sizes:

```bash
RUSTFLAGS=-Zprint-type-sizes cargo +nightly build -p uv -j 1 > type-sizes.txt && top-type-sizes -w -s -h 10 < type-sizes.txt > sizes.txt
```

You can debug the overflowing stack with lldb using https://forums.swift.org/t/getting-stack-size-using-lldb/63207:

```
settings set frame-format "frame #${frame.index}: sp=${frame.sp} fp=${frame.fp} pc=${frame.pc}{ ${module.file.basename}{`${function.name-with-args}${function.pc-offset}}}{ at ${line.file.basename}:${line.number}}{${function.is-optimized} [opt]}\n"
thread backtrace
session save backtrace.txt
```

Feed this to https://gist.github.com/konstin/448492d131bd346f2d387257c47fa312 to get the stack frame sizes.

---

_Merged by @zanieb on 2024-05-21 08:40_

---

_Closed by @zanieb on 2024-05-21 08:40_

---

_Branch deleted on 2024-05-21 08:40_

---
