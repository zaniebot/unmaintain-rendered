```yaml
number: 19137
title: "[ty] Add documentation for server traits"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
  - server
assignees: []
merged: true
base: main
head: dhruv/server-trait-doc
created_at: 2025-07-04T03:51:02Z
updated_at: 2025-07-09T12:40:20Z
url: https://github.com/astral-sh/ruff/pull/19137
synced_at: 2026-01-10T18:33:12Z
```

# [ty] Add documentation for server traits

---

_Pull request opened by @dhruvmanila on 2025-07-04 03:51_

This PR adds some basic documentation for the traits in the server implementation.


---

_Label `internal` added by @dhruvmanila on 2025-07-04 03:51_

---

_Review requested from @carljm by @dhruvmanila on 2025-07-04 03:51_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-07-04 03:51_

---

_Review requested from @AlexWaygood by @dhruvmanila on 2025-07-04 03:51_

---

_Review requested from @sharkdp by @dhruvmanila on 2025-07-04 03:51_

---

_Review requested from @dcreager by @dhruvmanila on 2025-07-04 03:51_

---

_Review request for @dcreager removed by @dhruvmanila on 2025-07-04 03:51_

---

_Review request for @carljm removed by @dhruvmanila on 2025-07-04 03:51_

---

_Review request for @sharkdp removed by @dhruvmanila on 2025-07-04 03:51_

---

_Review request for @AlexWaygood removed by @dhruvmanila on 2025-07-04 03:51_

---

_Review requested from @BurntSushi by @dhruvmanila on 2025-07-04 03:51_

---

_Comment by @github-actions[bot] on 2025-07-04 03:55_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~88MB
+ TOTAL MEMORY USAGE: ~97MB

colour (https://github.com/colour-science/colour)
- TOTAL MEMORY USAGE: ~405MB
+ TOTAL MEMORY USAGE: ~445MB

pytest (https://github.com/pytest-dev/pytest)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~251MB

scrapy (https://github.com/scrapy/scrapy)
-     memo fields = ~207MB
+     memo fields = ~189MB

bokeh (https://github.com/bokeh/bokeh)
- TOTAL MEMORY USAGE: ~276MB
+ TOTAL MEMORY USAGE: ~251MB

aiohttp (https://github.com/aio-libs/aiohttp)
-     memo fields = ~129MB
+     memo fields = ~117MB

openlibrary (https://github.com/internetarchive/openlibrary)
-     memo fields = ~171MB
+     memo fields = ~189MB

scipy (https://github.com/scipy/scipy)
- TOTAL MEMORY USAGE: ~1399MB
+ TOTAL MEMORY USAGE: ~1271MB

manticore (https://github.com/trailofbits/manticore)
-     memo fields = ~652MB
+     memo fields = ~593MB

```
</details>


---

_Label `ty` added by @dylwil3 on 2025-07-04 14:38_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/traits.rs`:23 on 2025-07-07 13:40_

You may want to rename this section if you end up renaming `WorkspaceSnapshot` to `SessionSnapshot`.

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/traits.rs`:105 on 2025-07-07 13:41_

Same here. You may want to reprhase this to *on the entire session* if you decide to rename `WorkspaceSnapshot` to `SessionSnapshot`

---

_@MichaReiser approved on 2025-07-07 13:41_

This is great. Thank you

---

_@MichaReiser reviewed on 2025-07-07 13:42_

---

_Review comment by @MichaReiser on `crates/ty_server/src/server/api/traits.rs`:1 on 2025-07-07 13:42_

It could be useful to copy the same documentation to Ruff.

---

_Label `ty` removed by @dhruvmanila on 2025-07-07 14:24_

---

_Label `documentation` added by @dhruvmanila on 2025-07-07 14:24_

---

_Label `server` added by @dhruvmanila on 2025-07-07 14:24_

---

_Label `documentation` removed by @dhruvmanila on 2025-07-07 14:24_

---

_Merged by @dhruvmanila on 2025-07-07 14:26_

---

_Closed by @dhruvmanila on 2025-07-07 14:26_

---

_Branch deleted on 2025-07-07 14:26_

---

_Comment by @github-actions[bot] on 2025-07-07 14:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@BurntSushi reviewed on 2025-07-09 12:40_

Nice, thank you! This is very helpful.

---
