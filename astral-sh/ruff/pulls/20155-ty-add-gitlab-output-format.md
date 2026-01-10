```yaml
number: 20155
title: "[ty] Add GitLab output format"
type: pull_request
state: merged
author: ntBre
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: brent/ty-gitlab
created_at: 2025-08-29T17:00:49Z
updated_at: 2025-09-03T13:08:15Z
url: https://github.com/astral-sh/ruff/pull/20155
synced_at: 2026-01-10T17:46:21Z
```

# [ty] Add GitLab output format

---

_Pull request opened by @ntBre on 2025-08-29 17:00_

## Summary

This wires up the GitLab output format moved into `ruff_db` in https://github.com/astral-sh/ruff/pull/20117 to the ty CLI.

While I was here, I made one unrelated change to the CLI docs. Clap was rendering the escapes around the `\[default\]` brackets for the `full` output, so I just switched those to parentheses:

```
--output-format <OUTPUT_FORMAT>
    The format to use for printing diagnostic messages

    Possible values:
    - full:    Print diagnostics verbosely, with context and helpful hints \[default\]
    - concise: Print diagnostics concisely, one per line
    - gitlab:  Print diagnostics in the JSON format expected by GitLab Code Quality reports
```

## Test Plan

New CLI test, and a manual test with `--config 'terminal.output-format = "gitlab"'` to make sure this works  as a configuration option too. I also tried piping the output through jq to make sure it's at least valid JSON


---

_Label `cli` added by @ntBre on 2025-08-29 17:01_

---

_Label `ty` added by @ntBre on 2025-08-29 17:01_

---

_Comment by @github-actions[bot] on 2025-08-29 17:02_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Review comment by @ntBre on `crates/ty/tests/cli/main.rs`:642 on 2025-08-29 17:03_

One minor note, I know this looks redundant, but GitLab doesn't render the `check_name` anywhere, so we manually prepend this to the description now (https://github.com/astral-sh/ruff/issues/19881).

---

_@ntBre reviewed on 2025-08-29 17:03_

---

_Comment by @github-actions[bot] on 2025-08-29 17:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Marked ready for review by @ntBre on 2025-08-29 17:14_

---

_Review requested from @carljm by @ntBre on 2025-08-29 17:14_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-29 17:14_

---

_Review requested from @sharkdp by @ntBre on 2025-08-29 17:14_

---

_Review requested from @dcreager by @ntBre on 2025-08-29 17:14_

---

_Review comment by @dhruvmanila on `crates/ty/src/lib.rs`:331 on 2025-09-03 04:57_

I'm assuming that we do the same thing for Ruff as well?

---

_@dhruvmanila approved on 2025-09-03 04:58_

---

_@ntBre reviewed on 2025-09-03 13:03_

---

_Review comment by @ntBre on `crates/ty/src/lib.rs`:331 on 2025-09-03 13:03_

Yep, pretty similar. Ruff has some `Flags` for the `Printer` that toggle showing these messages.

https://github.com/astral-sh/ruff/blob/bbfcf6e111648052783bdb68ec71706fc69b714f/crates/ruff/src/printer.rs#L85-L93

---

_Merged by @ntBre on 2025-09-03 13:08_

---

_Closed by @ntBre on 2025-09-03 13:08_

---

_Branch deleted on 2025-09-03 13:08_

---
