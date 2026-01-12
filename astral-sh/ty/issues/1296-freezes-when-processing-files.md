```yaml
number: 1296
title: Freezes when processing files
type: issue
state: closed
author: qarmin
labels:
  - hang
assignees: []
created_at: 2025-10-01T18:26:31Z
updated_at: 2025-10-17T09:10:16Z
url: https://github.com/astral-sh/ty/issues/1296
synced_at: 2026-01-12T15:54:24Z
```

# Freezes when processing files

---

_@qarmin_

### Summary

```
ty check file.py
```
causes freezes with such files

[Freezes.zip](https://github.com/user-attachments/files/22646085/Freezes.zip)

Flamegraph (~10 seconds)

[flamegraph.svg.zip](https://github.com/user-attachments/files/22646233/flamegraph.svg.zip)

### Version

ty unknown (cae37bddd 2025-08-25)

---

_Comment by @AlexWaygood on 2025-10-01 18:33_

Thanks! I skimmed through some of these and it looks like some of them may be https://github.com/astral-sh/ty/issues/1111. It's probably worth us checking back in on this after that's fixed, though (which should hopefully be soon).

---

_Label `hang` added by @AlexWaygood on 2025-10-01 18:33_

---

_Comment by @MichaReiser on 2025-10-17 09:10_

I ran ty on the entire directory and it now completes in 500ms.

---

_Closed by @MichaReiser on 2025-10-17 09:10_

---
