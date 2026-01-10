---
number: 16694
title: "Ignoring `import-private-name` (`PLC2701`) for first party module imports"
type: issue
state: open
author: Avasam
labels:
  - rule
  - needs-design
assignees: []
created_at: 2025-03-13T02:47:23Z
updated_at: 2025-03-13T20:29:20Z
url: https://github.com/astral-sh/ruff/issues/16694
synced_at: 2026-01-10T01:22:58Z
---

# Ignoring `import-private-name` (`PLC2701`) for first party module imports

---

_Issue opened by @Avasam on 2025-03-13 02:47_

### Summary

I recently reran Ruff in preview mode on the typeshed codebase. And noticed that [import-private-name (PLC2701)](https://docs.astral.sh/ruff/rules/import-private-name/#import-private-name-plc2701) whines about importing private first-party modules.

That rule can still make sense as a best-practice to avoid importing private *symbols* from first-party modules, but the point of private modules is that they should be imported in the same project/package that defines them right?

In typeshed's case, we have:
```toml
[tool.ruff.lint.isort]
known-first-party = ["_utils", "ts_utils"]
```

Yet PLC2701 triggers on lines like `from _utils import MYPY_PROTOBUF_VERSION, download_file, extract_archive, run_protoc`. Where `_utils` is a local module, which I think would always be undesirable to flag.
I understand that [known-first-party](https://docs.astral.sh/ruff/settings/#lint_isort_known-first-party) is an isort rule, but:
1. Can [import-private-name (PLC2701)](https://docs.astral.sh/ruff/rules/import-private-name/#import-private-name-plc2701) ignore first-party private module imports
2. Can the logic of "what's a first-party module import", including manual additions to `known-first-party`, be shared across isort and Pylint rules?

---

Ruff: 0.9.3

---

_Referenced in [astral-sh/ruff#16691](../../astral-sh/ruff/issues/16691.md) on 2025-03-13 03:03_

---

_Label `rule` added by @MichaReiser on 2025-03-13 14:48_

---

_Renamed from "Ignoring `import-private-name (PLC2701)` for first party module imports" to "Ignoring `import-private-name` (`PLC2701`) for first party module imports" by @MichaReiser on 2025-03-13 14:49_

---

_Comment by @MichaReiser on 2025-03-13 14:52_

Could you share an example that is raised only in preview mode? I did a quick search and couldn't find any mention of preview behavior specific to PLC2701

>  but the point of private modules is that they should be imported in the same project/package that defines them right?

I think that depends on how strict you want to be. I understand from https://github.com/astral-sh/ruff/issues/16691 that they want to allow private imports from the direct parent module but not across packages in the same project or across multiple ancestors. 

Eitherway. I do think that it makes sense to have a more intelligent rule to enforce visibility restrictions. However, I think this requires some design about what are common project structures and how we want to enforce them with the limited information that Ruff has available or if we have to wait for Red Knot.

---

_Label `needs-design` added by @MichaReiser on 2025-03-13 14:52_

---

_Comment by @Avasam on 2025-03-13 20:25_

> Could you share an example that is raised only in preview mode? I did a quick search and couldn't find any mention of preview behavior specific to PLC2701

I think the entire rule might be in preview?
Typeshed selects the `PLC` group. So running with the `--preview` flag just adds the rule to the selection.

---

Btw this issue isn't directly related to #16691 . As in I opened this issue first, then found #16691 . And found some overlap whilst reading it.

---
