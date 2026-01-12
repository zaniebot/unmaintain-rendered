```yaml
number: 19084
title: "[ty] Log target names at trace level"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - server
  - ty
assignees: []
merged: true
base: main
head: dhruv/server-target-logging
created_at: 2025-07-02T04:46:36Z
updated_at: 2025-07-02T04:50:13Z
url: https://github.com/astral-sh/ruff/pull/19084
synced_at: 2026-01-12T15:56:31Z
```

# [ty] Log target names at trace level

---

_@dhruvmanila_

Follow-up to https://github.com/astral-sh/ruff/pull/19083, also log the target names like `ty_python_semantic::module_resolver::resolver` in `2025-07-02 10:12:20.188697000 DEBUG ty_python_semantic::module_resolver::resolver: Adding first-party search path '/Users/dhruv/playground/ty_server'` at trace level.

---

_Review requested from @carljm by @dhruvmanila on 2025-07-02 04:46_

---

_Label `internal` added by @dhruvmanila on 2025-07-02 04:46_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-02 04:46_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-02 04:46_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-02 04:46_

---

_Label `server` added by @dhruvmanila on 2025-07-02 04:46_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-02 04:46_

---

_Label `ty` added by @dhruvmanila on 2025-07-02 04:46_

---

_Merged by @dhruvmanila on 2025-07-02 04:49_

---

_Closed by @dhruvmanila on 2025-07-02 04:49_

---

_Branch deleted on 2025-07-02 04:49_

---

_Comment by @github-actions[bot] on 2025-07-02 04:50_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~97MB
+ TOTAL MEMORY USAGE: ~88MB

dulwich (https://github.com/dulwich/dulwich)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~106MB

tornado (https://github.com/tornadoweb/tornado)
-     memo fields = ~129MB
+     memo fields = ~142MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo fields = ~228MB
+     memo fields = ~251MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~171MB
+     memo fields = ~189MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~276MB
+     memo fields = ~304MB

```
</details>


---
