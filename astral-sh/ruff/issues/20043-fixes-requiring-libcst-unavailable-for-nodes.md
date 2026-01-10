---
number: 20043
title: Fixes requiring LibCST unavailable for nodes containing t-strings
type: issue
state: closed
author: dylwil3
labels:
  - fixes
assignees: []
created_at: 2025-08-22T14:12:31Z
updated_at: 2025-09-11T19:49:10Z
url: https://github.com/astral-sh/ruff/issues/20043
synced_at: 2026-01-10T01:23:01Z
---

# Fixes requiring LibCST unavailable for nodes containing t-strings

---

_Issue opened by @dylwil3 on 2025-08-22 14:12_

Until https://github.com/Instagram/LibCST/issues/1343 is resolved, all fixes that use `libcst` to generate a fix will not work on any expression or statement containing a t-string.

Examples of affected rules (not exhaustive):

- [explicit-f-string-type-conversion (RUF010)](https://docs.astral.sh/ruff/rules/explicit-f-string-type-conversion/#explicit-f-string-type-conversion-ruf010)
- [multiple-with-statements (SIM117)](https://docs.astral.sh/ruff/rules/multiple-with-statements/#multiple-with-statements-sim117)
- [collapsible-if (SIM102)](https://docs.astral.sh/ruff/rules/collapsible-if/#collapsible-if-sim102)
- [unnecessary-collection-call (C408)](https://docs.astral.sh/ruff/rules/unnecessary-collection-call/#unnecessary-collection-call-c408)
- [percent-format-extra-named-arguments (F504)](https://docs.astral.sh/ruff/rules/percent-format-extra-named-arguments/#percent-format-extra-named-arguments-f504)

Unfortunately it is not obvious to users which rules use `libcst` in the backend. To see if this may be responsible for the absence of your fix, run `ruff check` with the flag `-v`/`--verbose` and look for debug logs containing the phrase "Failed to extract". For example:

```console
❯ echo 'f"{ascii(t"")}"' \
  | ruff check --no-cache --isolated --select RUF010 --target-version py314 --preview \
  -v --output-format concise - \
  2>&1 1>/dev/null | grep "Failed to extract"
[2025-08-22][09:26:17][ruff_linter::checkers::ast][DEBUG] Failed to create fix for explicit-f-string-type-conversion: Failed to extract expression from source
```


---

_Referenced in [astral-sh/ruff#19895](../../astral-sh/ruff/issues/19895.md) on 2025-08-22 14:14_

---

_Label `fixes` added by @AlexWaygood on 2025-08-22 14:18_

---

_Comment by @MichaReiser on 2025-08-22 15:18_

Hmm, would it be "hard" to handle libCST parsing errors differently and display a custom error message at least (maybe with a link to an issue?)

---

_Comment by @dylwil3 on 2025-09-11 19:49_

This has been resolved by https://github.com/astral-sh/ruff/pull/20321

---

_Closed by @dylwil3 on 2025-09-11 19:49_

---
