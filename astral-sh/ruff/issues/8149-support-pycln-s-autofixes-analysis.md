```yaml
number: 8149
title: "Support pycln's autofixes analysis"
type: issue
state: open
author: Avasam
labels:
  - rule
  - type-inference
assignees: []
created_at: 2023-10-23T18:40:53Z
updated_at: 2024-10-24T14:33:43Z
url: https://github.com/astral-sh/ruff/issues/8149
synced_at: 2026-01-10T11:09:50Z
```

# Support pycln's autofixes analysis

---

_Issue opened by @Avasam on 2023-10-23 18:40_

The core functionality of pycln (that is, cleaning up imports) is already supported by Ruff.
However, there's two specific features that I would like to see. And I know they won't be possible until Ruff has multi-file analysis (see #7447):
- Side-effect detection: https://hadialqattan.github.io/pycln/#/?id=side-effects
  - This is about being able to detect when an import removal is safe, or when it it may cause side effects (for example: importing `setuptools` changes how `distutils` work, so a stray `import setuptools` should be left in). This is also used a lot in pywin32: https://github.com/mhammond/pywin32/blob/main/pycln.toml
- star-import unpacking: https://hadialqattan.github.io/pycln/#/?id=-x-expand-stars-flag
  - Replaces
    ```py
    from foo import *
    bar()
    bar_two()
    ```
    With:
    ```py
    from foo import bar, bar_two
    bar()
    bar_two()
    ```
    (assuming of course the `foo` module can be statically analysed)
- Detect implicit imports from sub-packages: https://hadialqattan.github.io/pycln/#/?id=implicit-imports-from-sub-packages
  - IMO this is a code smell in itself. But the current "unknown symbol" checks could be improved. And a new error code added for implicit imports usage.
    
The first two could probably be integrated into existing checks `unused-import (F401)` and `undefined-local-with-import-star[-usage] (F403/F405)` as possible "safe autofixes".

---

_Label `rule` added by @dhruvmanila on 2023-10-25 05:26_

---

_Label `multifile-analysis` added by @dhruvmanila on 2023-10-25 05:26_

---

_Label `type-inference` added by @MichaReiser on 2024-10-24 14:33_

---

_Label `multifile-analysis` removed by @MichaReiser on 2024-10-24 14:33_

---
