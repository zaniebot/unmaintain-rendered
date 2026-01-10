---
number: 18656
title: RET503 false positive on exhaustive type check
type: issue
state: open
author: MeGaGiGaGon
labels:
  - rule
  - type-inference
assignees: []
created_at: 2025-06-13T01:38:44Z
updated_at: 2025-06-13T07:00:34Z
url: https://github.com/astral-sh/ruff/issues/18656
synced_at: 2026-01-10T01:22:59Z
---

# RET503 false positive on exhaustive type check

---

_Issue opened by @MeGaGiGaGon on 2025-06-13 01:38_

### Summary

From the same codebase as https://github.com/astral-sh/ruff/issues/5474#issuecomment-2968706593 , RET503 still reports an error after an exhaustive type check. If the fix is applied, type checkers will warn/error about unreachable code.
[playground link](https://play.ruff.rs/98b1a534-fca2-4f66-859c-6be9a207f73f)

```py
import tarfile
import zipfile

type ArchiveKind = tarfile.TarFile | zipfile.ZipFile

def get_first_archive_member(archive: ArchiveKind) -> str:
    if isinstance(archive, tarfile.TarFile):
        return archive.getnames()[0]
    elif isinstance(archive, zipfile.ZipFile):
        return archive.namelist()[0]
    # RET503 error
```

### Version

ruff 0.11.13 (5faf72a4d 2025-06-05) + playground

---

_Label `rule` added by @MichaReiser on 2025-06-13 06:58_

---

_Label `type-inference` added by @MichaReiser on 2025-06-13 06:58_

---

_Comment by @MichaReiser on 2025-06-13 07:00_

I can see how it would be desired that Ruff wouldn't raise `RET503` here. However, this requires that Ruff understands that `ArchiveKind` only has two variants and they're all handled. This is far beyond what Ruff's semantic model can handle today, but should be possible once we start using the semantic model from our in progress type checker ty in Ruff. 

For now, the best you can do here is to suppress `RET503`

---
