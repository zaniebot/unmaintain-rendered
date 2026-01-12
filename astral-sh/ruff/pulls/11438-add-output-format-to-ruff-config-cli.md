```yaml
number: 11438
title: "Add `--output-format` to `ruff config` CLI"
type: pull_request
state: merged
author: thatch
labels:
  - cli
assignees: []
merged: true
base: main
head: json-format-config
created_at: 2024-05-15T23:11:53Z
updated_at: 2024-05-16T02:22:52Z
url: https://github.com/astral-sh/ruff/pull/11438
synced_at: 2026-01-12T15:55:38Z
```

# Add `--output-format` to `ruff config` CLI

---

_@thatch_

This is useful for extracting the defaults in order to construct equivalent configs by external scripts.  This is my first non-hello-world rust code, comments and suggested tests appreciated.

## Summary

We already have `ruff linter --output-format json`, this provides `ruff config x --output-format json` as well.  I plan to use this to construct an equivalent config snippet to include in some managed repos, so when we update their version of ruff and it adds new lints, they get a PR that includes the commented-out new lints.

Note that the no-args form of `ruff config` ignores output-format currently, but probably should obey it (although array-of-strings doesn't seem that useful, looking for input on format).

## Test Plan

I could use a hand coming up with a typical way to write automated tests for this.

```sh-session
(.venv) [timhatch:ruff ]$ ./target/debug/ruff config lint.select
A list of rule codes or prefixes to enable. Prefixes can specify exact
rules (like `F841`), entire categories (like `F`), or anything in
between.

When breaking ties between enabled and disabled rules (via `select` and
`ignore`, respectively), more specific prefixes override less
specific prefixes.

Default value: ["E4", "E7", "E9", "F"]
Type: list[RuleSelector]
Example usage:
``toml
# On top of the defaults (`E4`, E7`, `E9`, and `F`), enable flake8-bugbear (`B`) and flake8-quotes (`Q`).
select = ["E4", "E7", "E9", "F", "B", "Q"]
``
(.venv) [timhatch:ruff ]$ ./target/debug/ruff config lint.select --output-format json
{
  "Field": {
    "doc": "A list of rule codes or prefixes to enable. Prefixes can specify exact\nrules (like `F841`), entire categories (like `F`), or anything in\nbetween.\n\nWhen breaking ties between enabled and disabled rules (via `select` and\n`ignore`, respectively), more specific prefixes override less\nspecific prefixes.",
    "default": "[\"E4\", \"E7\", \"E9\", \"F\"]",
    "value_type": "list[RuleSelector]",
    "scope": null,
    "example": "# On top of the defaults (`E4`, E7`, `E9`, and `F`), enable flake8-bugbear (`B`) and flake8-quotes (`Q`).\nselect = [\"E4\", \"E7\", \"E9\", \"F\", \"B\", \"Q\"]",
    "deprecated": null
  }
}
```

---

_Comment by @github-actions[bot] on 2024-05-15 23:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @charliermarsh on 2024-05-16 01:43_

This looks reasonable to me. Let me see if I can get the no-args form working, then can merge.

---

_@charliermarsh approved on 2024-05-16 01:59_

---

_Label `cli` added by @charliermarsh on 2024-05-16 02:00_

---

_Renamed from "Add output-format to config in cli" to "Add `--output-format` to `ruff config` CLI" by @charliermarsh on 2024-05-16 02:00_

---

_Merged by @charliermarsh on 2024-05-16 02:17_

---

_Closed by @charliermarsh on 2024-05-16 02:17_

---
