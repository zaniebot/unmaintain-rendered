```yaml
number: 18139
title: Docs on global var produce different outputs
type: issue
state: closed
author: jm-positron
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-05-16T17:07:24Z
updated_at: 2025-05-22T05:55:38Z
url: https://github.com/astral-sh/ruff/issues/18139
synced_at: 2026-01-10T11:09:58Z
```

# Docs on global var produce different outputs

---

_Issue opened by @jm-positron on 2025-05-16 17:07_

### Summary

The docs on this page are inaccurate: https://docs.astral.sh/ruff/rules/global-statement/

The two examples produce slightly different outputs.

The original example produces:

```
10
10
```
vs
```
1
10
```

The examples should produce identical output.


Note I spent some time trying to find where in the project those docs are produced, but unfortuntately wasn't able to find it quickly. So this report just references the url.

### Version

_No response_

---

_Label `documentation` added by @ntBre on 2025-05-16 17:17_

---

_Comment by @ntBre on 2025-05-16 17:56_

Yeah, it can be a bit tricky to map the rules back to their source code. The docs are extracted from the docstring [here](https://github.com/astral-sh/ruff/blob/main/crates/ruff_linter/src/rules/pylint/rules/global_statement.rs#L6) if you're interested in updating it :)

I'll work on it or add the `help-wanted` label if not.

---

_Comment by @jm-positron on 2025-05-19 16:05_

I think it would be better for someone more familiar with ruff and its objectives to update it. Thank you for the link to the doc, I believe I failed to find it when searching for a string from the docs due to the markdown code backticks.

---

_Label `help wanted` added by @ntBre on 2025-05-19 17:12_

---

_Closed by @MichaReiser on 2025-05-22 05:55_

---
