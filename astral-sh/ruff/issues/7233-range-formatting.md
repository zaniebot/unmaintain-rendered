```yaml
number: 7233
title: Range formatting
type: issue
state: closed
author: MichaReiser
labels:
  - formatter
assignees: []
created_at: 2023-09-08T07:46:33Z
updated_at: 2024-02-05T19:21:47Z
url: https://github.com/astral-sh/ruff/issues/7233
synced_at: 2026-01-10T11:09:49Z
```

# Range formatting

---

_Issue opened by @MichaReiser on 2023-09-08 07:46_

For IDE integration, it would be great if it was possible to select a certain range of code and only format that range.

---

_Assigned to @konstin by @MichaReiser on 2023-09-08 07:46_

---

_Label `formatter` added by @MichaReiser on 2023-09-08 07:46_

---

_Added to milestone `Formatter: Beta` by @MichaReiser on 2023-09-08 07:46_

---

_Comment by @T-256 on 2023-09-08 16:26_

Thats cool, not just one range, but [batch range formatting](https://code.visualstudio.com/updates/v1_82#_support-for-batch-range-formatting).

---

_Comment by @MichaReiser on 2023-09-08 17:20_

> Thats cool, not just one range, but [batch range formatting](https://code.visualstudio.com/updates/v1_82#_support-for-batch-range-formatting).

Oh, I didn't know about that. We'll start with one range but being aware of multi range formatting may help us to decide on the design

---

_Comment by @MichaReiser on 2023-09-08 17:22_

Relevant code in Rome/Biome 

https://github.com/biomejs/biome/blob/92be3f8fc3c13bd3ab7827b828bc4167db1d5ee0/crates/rome_formatter/src/lib.rs#L1059-L1307

---

_Comment by @konstin on 2023-09-08 18:26_

do you know what's the use case for this from the editor side?

---

_Comment by @MichaReiser on 2023-09-08 18:36_

I assume it's to speed up "Format changed code" when saving a file with multiple non-overlapping changed ranges. 

---

_Comment by @KraXen72 on 2023-10-03 07:33_

> do you know what's the use case for this from the editor side?
  
For example, VSCode's format selection command. i use it most of the time instead of formatting the entire document.

---

_Removed from milestone `Formatter: Beta` by @MichaReiser on 2023-10-17 00:58_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2023-10-17 00:58_

---

_Comment by @konstin on 2023-10-20 10:33_

Current state (only very basic statement range formatting): https://github.com/astral-sh/ruff/tree/range-formatting

---

_Unassigned @konstin by @MichaReiser on 2023-10-27 01:58_

---

_Comment by @skykasko on 2023-11-08 12:56_

FYI, this capability has recently been added to Black (https://github.com/psf/black/pull/4020).

---

_Comment by @LazyRen on 2023-12-21 04:29_

> do you know what's the use case for this from the editor side?

Addition to the `format selection` command, it seems `Format on Save Mode: modifications` also depend on this feature.

Currently, Ruff doesn't format the document automatically even if format on save is enabled if `Format on Save Mode` is set to `modifications`.

---

_Assigned to @MichaReiser by @MichaReiser on 2024-01-24 12:39_

---

_Closed by @MichaReiser on 2024-02-05 19:21_

---
