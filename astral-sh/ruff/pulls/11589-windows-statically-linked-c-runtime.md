```yaml
number: 11589
title: "Windows: Statically linked C runtime"
type: pull_request
state: merged
author: T-256
labels:
  - windows
assignees: []
merged: true
base: main
head: static-crt
created_at: 2024-05-28T22:33:38Z
updated_at: 2024-05-29T12:00:12Z
url: https://github.com/astral-sh/ruff/pull/11589
synced_at: 2026-01-10T21:56:00Z
```

# Windows: Statically linked C runtime

---

_Pull request opened by @T-256 on 2024-05-28 22:33_

## Summary
Here is a comparison between [0.4.6 released binaries](https://github.com/astral-sh/ruff/actions/runs/9274921378) (dynamic-crt) and [current PR binaries](https://github.com/T-256/ruff/actions/runs/9283204908) (static-crt):

| TARGET | dynamic size | static size | increased size |
| - | - | - | - |
| aarch64-pc-windows-msvc | 7.32 MB | 7.36 MB | 0.04 MB |
| i686-pc-windows-msvc | 7.14 MB | 7.2 MB | 0.06 MB |
| x86_64-pc-windows-msvc | 7.85 MB | 7.91 MB | 0.06 MB |

Uncompressed binaries:
| TARGET | dynamic size | static size | increased size |
| - | - | - | - |
| aarch64-pc-windows-msvc | 19.5 MB | 19.6 MB | 0.1 MB |
| i686-pc-windows-msvc | 18.5 MB | 18.6 MB | 0.1 MB |
| x86_64-pc-windows-msvc | 23 MB | 23.1 MB | 0.1 MB |


Closes #11503

---

_Marked ready for review by @T-256 on 2024-05-28 23:20_

---

_@T-256 reviewed on 2024-05-28 23:25_

---

_Review comment by @T-256 on `.cargo/config.toml`:17 on 2024-05-28 23:25_

It seems it still does not migrate to dynamic crt, so for now I'll drop it.

```suggestion
```

---

_Renamed from "Statically linked C runtime" to "Windows: Statically linked C runtime" by @T-256 on 2024-05-28 23:27_

---

_Review requested from @BurntSushi by @charliermarsh on 2024-05-29 00:30_

---

_Review requested from @MichaReiser by @charliermarsh on 2024-05-29 00:30_

---

_Comment by @charliermarsh on 2024-05-29 00:30_

@BurntSushi - tagging you in here too.

---

_Label `windows` added by @charliermarsh on 2024-05-29 00:30_

---

_Comment by @charliermarsh on 2024-05-29 00:30_

That size increase looks totally reasonable to me.

---

_@MichaReiser approved on 2024-05-29 06:34_

Thanks @T-256 for working on this. 

> We also support aarch64 windows target, should I include it in this PR?

Yes, I think it would be good if all windows versions are built the seme way.

---

_@BurntSushi approved on 2024-05-29 11:56_

---

_Merged by @MichaReiser on 2024-05-29 12:00_

---

_Closed by @MichaReiser on 2024-05-29 12:00_

---
