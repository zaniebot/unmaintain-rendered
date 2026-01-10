```yaml
number: 19129
title: Move Azure output format to the new diagnostic format
type: pull_request
state: closed
author: ntBre
labels:
  - ty
  - diagnostics
assignees: []
draft: true
base: main
head: brent/diagnostic-rendering
created_at: 2025-07-03T18:24:59Z
updated_at: 2025-09-03T19:07:22Z
url: https://github.com/astral-sh/ruff/pull/19129
synced_at: 2026-01-10T17:46:21Z
```

# Move Azure output format to the new diagnostic format

---

_Pull request opened by @ntBre on 2025-07-03 18:24_

## Summary

This PR adds a dummy `FileResolver` to Ruff to enable using the `ruff_db::Diagnostic` rendering code and ports one of the simplest output formats (`azure`) to a `ruff_db::DiagnosticFormat`. This also made it very easy to add support for this output format to ty.

I think this will eventually allow us to rework, or even remove, the `Emitter` infrastructure in Ruff, but my plan for now is just to port the internals of each `Emitter`.

We didn't have any tests for this, but I removed this check:

https://github.com/astral-sh/ruff/blob/333191b7f7d78473a4ee88f6a24e199b29b01070/crates/ruff_linter/src/message/azure.rs#L22-L28

because the line numbers in notebooks look sensible to me. This matches the current behavior of ty too.

## Test Plan

Existing Ruff tests and a new ty CLI test


---

_Label `internal` added by @ntBre on 2025-07-03 18:25_

---

_Label `diagnostics` added by @ntBre on 2025-07-03 18:25_

---

_Label `ty` added by @ntBre on 2025-07-03 18:25_

---

_Label `internal` removed by @ntBre on 2025-07-03 18:25_

---

_Comment by @ntBre on 2025-07-03 18:26_

Not sure about the labels here :laughing: it should be `internal` for Ruff, but I guess it's a new feature for `ty`.

---

_Comment by @github-actions[bot] on 2025-07-03 18:28_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
mypy_primer (https://github.com/hauntsaninja/mypy_primer)
-     memo fields = ~45MB
+     memo fields = ~41MB

Expression (https://github.com/cognitedata/Expression)
-     memo fields = ~54MB
+     memo fields = ~49MB

trio (https://github.com/python-trio/trio)
-     memo fields = ~156MB
+     memo fields = ~142MB

discord.py (https://github.com/Rapptz/discord.py)
-     memo fields = ~189MB
+     memo fields = ~207MB

hydra-zen (https://github.com/mit-ll-responsible-ai/hydra-zen)
- TOTAL MEMORY USAGE: ~80MB
+ TOTAL MEMORY USAGE: ~88MB

mongo-python-driver (https://github.com/mongodb/mongo-python-driver)
-     memo fields = ~171MB
+     memo fields = ~156MB

bandersnatch (https://github.com/pypa/bandersnatch)
-     memo fields = ~66MB
+     memo fields = ~72MB

mkdocs (https://github.com/mkdocs/mkdocs)
-     memo fields = ~97MB
+     memo fields = ~106MB

psycopg (https://github.com/psycopg/psycopg)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB

django-stubs (https://github.com/typeddjango/django-stubs)
- TOTAL MEMORY USAGE: ~171MB
+ TOTAL MEMORY USAGE: ~189MB

openlibrary (https://github.com/internetarchive/openlibrary)
- TOTAL MEMORY USAGE: ~228MB
+ TOTAL MEMORY USAGE: ~207MB

scipy (https://github.com/scipy/scipy)
- TOTAL MEMORY USAGE: ~1156MB
+ TOTAL MEMORY USAGE: ~1271MB

```
</details>


---

_@ntBre reviewed on 2025-07-03 18:31_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:109 on 2025-07-03 18:31_

It [looks](https://learn.microsoft.com/en-us/azure/devops/pipelines/scripts/logging-commands?view=azure-devops&tabs=bash#properties) like we could get by without nesting these, much like the `Concise` format above, but I'm not totally sure. These should both always be `Some` for Ruff.

---

_Comment by @github-actions[bot] on 2025-07-03 18:34_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:109 on 2025-07-03 18:40_

Never mind, I needed to scroll up a bit for the general [format](https://learn.microsoft.com/en-us/azure/devops/pipelines/scripts/logging-commands?view=azure-devops&tabs=bash#logging-command-format). I'm confident we can handle this like the `Concise` format now.

---

_@ntBre reviewed on 2025-07-03 18:40_

---

_Marked ready for review by @ntBre on 2025-07-03 19:14_

---

_Review requested from @carljm by @ntBre on 2025-07-03 19:14_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-03 19:14_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-03 19:14_

---

_Review requested from @sharkdp by @ntBre on 2025-07-03 19:14_

---

_Review requested from @dcreager by @ntBre on 2025-07-03 19:14_

---

_Review request for @dcreager removed by @ntBre on 2025-07-03 19:14_

---

_Review request for @carljm removed by @ntBre on 2025-07-03 19:14_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-03 19:14_

---

_Review requested from @BurntSushi by @ntBre on 2025-07-03 19:14_

---

_Review requested from @dhruvmanila by @ntBre on 2025-07-03 19:14_

---

_Comment by @ntBre on 2025-07-03 21:48_

I started working on JSON output after this, and I think we may end up needing the `NotebookIndex` anyway, so I should probably not delete that section of code. I'll convert this back to a draft while I figure it out.

---

_Converted to draft by @ntBre on 2025-07-03 21:48_

---

_Closed by @ntBre on 2025-07-04 16:05_

---

_Branch deleted on 2025-09-03 19:07_

---
