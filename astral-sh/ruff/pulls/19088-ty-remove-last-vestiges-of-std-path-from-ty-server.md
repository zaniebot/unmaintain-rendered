```yaml
number: 19088
title: "[ty] Remove last vestiges of `std::path` from `ty_server`"
type: pull_request
state: merged
author: Gankra
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: gankra/syspath
created_at: 2025-07-02T13:38:42Z
updated_at: 2025-07-03T09:48:32Z
url: https://github.com/astral-sh/ruff/pull/19088
synced_at: 2026-01-12T15:56:31Z
```

# [ty] Remove last vestiges of `std::path` from `ty_server`

---

_@Gankra_

Fixes https://github.com/astral-sh/ty/issues/603

---

_Review requested from @carljm by @Gankra on 2025-07-02 13:38_

---

_Review requested from @MichaReiser by @Gankra on 2025-07-02 13:38_

---

_Review requested from @AlexWaygood by @Gankra on 2025-07-02 13:38_

---

_Review requested from @sharkdp by @Gankra on 2025-07-02 13:38_

---

_Review requested from @dcreager by @Gankra on 2025-07-02 13:38_

---

_Label `internal` added by @Gankra on 2025-07-02 13:38_

---

_Label `server` added by @AlexWaygood on 2025-07-02 13:41_

---

_Label `ty` added by @AlexWaygood on 2025-07-02 13:41_

---

_Comment by @github-actions[bot] on 2025-07-02 13:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

isort (https://github.com/pycqa/isort)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~80MB

static-frame (https://github.com/static-frame/static-frame)
-     memo fields = ~334MB
+     memo fields = ~304MB

meson (https://github.com/mesonbuild/meson)
-     memo fields = ~304MB
+     memo fields = ~334MB

```
</details>


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-02 13:47_

---

_@sharkdp approved on 2025-07-02 15:43_

Thank you!

---

_@dhruvmanila approved on 2025-07-03 05:01_

Looks good, thanks!

---

_Merged by @dhruvmanila on 2025-07-03 09:48_

---

_Closed by @dhruvmanila on 2025-07-03 09:48_

---

_Branch deleted on 2025-07-03 09:48_

---
