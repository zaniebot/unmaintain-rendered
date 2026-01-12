```yaml
number: 1932
title: ty cannot find packages in the python environment lib subfolder when l is lowercase
type: issue
state: open
author: martinResearch
labels:
  - needs-mre
  - windows
  - imports
assignees: []
created_at: 2025-12-16T14:01:40Z
updated_at: 2025-12-16T15:38:54Z
url: https://github.com/astral-sh/ty/issues/1932
synced_at: 2026-01-12T15:54:26Z
```

# ty cannot find packages in the python environment lib subfolder when l is lowercase

---

_@martinResearch_

### Summary

I When I create Python environments with conda, the subfolder is named "lib" (lowercase), but ty expects it to start with a capital "L". This prevents ty from finding the Python packages. It would help if ty could recognize the "lib" folder regardless of case.

### Version

 0.0.1-alpha.34 (ef3d48ac4 2025-12-12)

---

_Comment by @AlexWaygood on 2025-12-16 14:03_

Thanks! I'm guessing you're on Windows?

---

_Comment by @martinResearch on 2025-12-16 14:15_

yes on windows 11 x86_64

---

_Comment by @martinResearch on 2025-12-16 14:16_

note that not all my conda environments end up with lib lower case. It depends on which conda packages I install.

---

_Comment by @MichaReiser on 2025-12-16 14:21_

I'm surprised this is an issue, given that all Windows file systems are case-insensitive. But maybe the real issue is that we successfully resolve the environment with `Lib`, but then fail to resolve the imports, where we perform a case-sensitive match.

@martinResearch would you mind sharing the logs when running ty with `ty check -v`?

---

_Label `windows` added by @MichaReiser on 2025-12-16 14:21_

---

_Label `imports` added by @MichaReiser on 2025-12-16 14:21_

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-16 14:21_

---

_Comment by @martinResearch on 2025-12-16 15:17_

I just tried to reproduce with a minimal conda environment and I am not getting the issue. I will need to investigate more to get a minimal example to reproduce.

---

_Label `needs-mre` added by @MichaReiser on 2025-12-16 15:38_

---
