```yaml
number: 19073
title: Remove new codspeed dependencies
type: pull_request
state: merged
author: ntBre
labels:
  - internal
assignees: []
merged: true
base: main
head: brent/codspeed-deps
created_at: 2025-07-01T16:07:35Z
updated_at: 2025-07-01T16:20:33Z
url: https://github.com/astral-sh/ruff/pull/19073
synced_at: 2026-01-10T18:39:09Z
```

# Remove new codspeed dependencies

---

_Pull request opened by @ntBre on 2025-07-01 16:07_

Summary
--

Updates to codspeed 3.0.2, removing some of the new dependencies introduced in 3.0. See https://github.com/astral-sh/uv/pull/14396 and https://github.com/CodSpeedHQ/codspeed-rust/pull/108 for the similar PR in uv and the upstream fix, respectively.

Test Plan
--

N/a

---

_Label `internal` added by @ntBre on 2025-07-01 16:08_

---

_Comment by @github-actions[bot] on 2025-07-01 16:11_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
async-utils (https://github.com/mikeshardmind/async-utils)
- TOTAL MEMORY USAGE: ~49MB
+ TOTAL MEMORY USAGE: ~45MB

beartype (https://github.com/beartype/beartype)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

black (https://github.com/psf/black)
- TOTAL MEMORY USAGE: ~129MB
+ TOTAL MEMORY USAGE: ~142MB

graphql-core (https://github.com/graphql-python/graphql-core)
-     memo fields = ~129MB
+     memo fields = ~117MB

dulwich (https://github.com/dulwich/dulwich)
- TOTAL MEMORY USAGE: ~142MB
+ TOTAL MEMORY USAGE: ~156MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~207MB
+     memo fields = ~189MB

alerta (https://github.com/alerta/alerta)
- TOTAL MEMORY USAGE: ~106MB
+ TOTAL MEMORY USAGE: ~117MB

mkosi (https://github.com/systemd/mkosi)
- TOTAL MEMORY USAGE: ~117MB
+ TOTAL MEMORY USAGE: ~129MB
-     memo fields = ~97MB
+     memo fields = ~106MB

tornado (https://github.com/tornadoweb/tornado)
-     memo fields = ~142MB
+     memo fields = ~129MB

paasta (https://github.com/yelp/paasta)
-     memo fields = ~171MB
+     memo fields = ~156MB

sphinx (https://github.com/sphinx-doc/sphinx)
-     memo fields = ~228MB
+     memo fields = ~251MB

freqtrade (https://github.com/freqtrade/freqtrade)
-     memo fields = ~304MB
+     memo fields = ~276MB

static-frame (https://github.com/static-frame/static-frame)
-     memo fields = ~304MB
+     memo fields = ~334MB

rotki (https://github.com/rotki/rotki)
-     memo fields = ~445MB
+     memo fields = ~490MB

```
</details>


---

_@AlexWaygood approved on 2025-07-01 16:12_

---

_Comment by @github-actions[bot] on 2025-07-01 16:19_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Merged by @ntBre on 2025-07-01 16:20_

---

_Closed by @ntBre on 2025-07-01 16:20_

---

_Branch deleted on 2025-07-01 16:20_

---
