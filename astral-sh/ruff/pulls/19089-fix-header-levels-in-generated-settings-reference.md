```yaml
number: 19089
title: Fix header levels in generated settings reference
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
  - ty
assignees: []
merged: true
base: main
head: zb/header-level
created_at: 2025-07-02T13:51:18Z
updated_at: 2025-07-02T14:01:25Z
url: https://github.com/astral-sh/ruff/pull/19089
synced_at: 2026-01-12T15:56:31Z
```

# Fix header levels in generated settings reference

---

_@zanieb_

The headers were one level too deep for child items, and the top-level `rules` header was way off.

---

_Review requested from @carljm by @zanieb on 2025-07-02 13:51_

---

_Label `documentation` added by @zanieb on 2025-07-02 13:51_

---

_Label `ty` added by @zanieb on 2025-07-02 13:51_

---

_Review requested from @MichaReiser by @zanieb on 2025-07-02 13:51_

---

_Review requested from @AlexWaygood by @zanieb on 2025-07-02 13:51_

---

_Review requested from @sharkdp by @zanieb on 2025-07-02 13:51_

---

_Review requested from @dcreager by @zanieb on 2025-07-02 13:51_

---

_@zanieb reviewed on 2025-07-02 13:53_

---

_Review comment by @zanieb on `crates/ruff_dev/src/generate_ty_options.rs`:117 on 2025-07-02 13:53_

Sometimes you gotta debug!

---

_Renamed from "Fix header leves in generated settings reference" to "Fix header levels in generated settings reference" by @zanieb on 2025-07-02 13:54_

---

_Comment by @github-actions[bot] on 2025-07-02 13:54_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
black (https://github.com/psf/black)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~129MB

dulwich (https://github.com/dulwich/dulwich)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~117MB
-     memo fields = ~106MB
+     memo fields = ~97MB

boostedblob (https://github.com/hauntsaninja/boostedblob)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~97MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

static-frame (https://github.com/static-frame/static-frame)
-     memo fields = ~304MB
+     memo fields = ~334MB

rotki (https://github.com/rotki/rotki)
-     memo fields = ~445MB
+     memo fields = ~490MB

scikit-learn (https://github.com/scikit-learn/scikit-learn)
- TOTAL MEMORY USAGE: ~717MB
+ TOTAL MEMORY USAGE: ~652MB

```
</details>


---

_@sharkdp approved on 2025-07-02 13:55_

Thank you!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-02 13:56_

---

_Merged by @sharkdp on 2025-07-02 14:01_

---

_Closed by @sharkdp on 2025-07-02 14:01_

---

_Branch deleted on 2025-07-02 14:01_

---
