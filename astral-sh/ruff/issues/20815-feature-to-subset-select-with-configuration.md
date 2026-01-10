---
number: 20815
title: "Feature to subset `--select` with configuration ignores"
type: issue
state: closed
author: fmigneault
labels:
  - configuration
  - wish
assignees: []
created_at: 2025-10-12T03:41:00Z
updated_at: 2025-10-14T04:37:32Z
url: https://github.com/astral-sh/ruff/issues/20815
synced_at: 2026-01-10T01:23:01Z
---

# Feature to subset `--select` with configuration ignores

---

_Issue opened by @fmigneault on 2025-10-12 03:41_

### Summary

## Description

When a project defines a `pyproject.toml` with a large list of `select`, `ignore` and `per-file-ignores` linting conditions, essentially documenting how this project is expected to be validated, running `ruff` on it with a lot of changes can potentially create a very large amount of errors. 

A developer that would have to address the errors would likely prefer to address them "contextually". 
For example, focusing on documentation issues by themselves (codes: D, DOC, etc.), before moving to other kind of issues, like recommendations (codes: SIM, UP, etc.).

However, the current options to "filter" a set of rules (ie: `--select` or `--extend-select`) do not seem to consider the configuration (`--config`). In other words, doing the following will re-enable all `D` rules, regardless of any `ignore = ["D..."]` values defined in the `pyproject.toml` file.

```shell
ruff check --config pyproject.toml --select D
```

It would be useful to have an option, such as a `--select-filter D` or similar, that would essentially perform the same checks as if running `ruff check` with the detected or provided configuration, while limiting the rules only to the given subset (but not overriding ignores). If using a specific code (eg: `--select D100`), the current behavior would work. This "filter" feature is relevant along the behavior of partial rule codes that activates a large amount of codes.

## Example

I run `ruff check --config pyproject.toml` on this project branch (https://github.com/fmigneault/aiu/tree/modernize), I receive 0 errors, since all `select`/`ignore` are applied as intended.

If I run `ruff check --config pyproject.toml --select D`, the multiple D ignores (https://github.com/fmigneault/aiu/blob/e5ae36ce17b9102f3a838398e2423f6cfd133d6d/pyproject.toml#L141-L153) are re-enabled (rightly so since "D" selected), but causing many unintended detection of "D"-codes expected to be ignored. 

Current workaround involves listing all D... codes in `--select`, which does not scale and is a burden.




---

_Comment by @fmigneault on 2025-10-12 03:45_

Similar to #20625

---

_Label `configuration` added by @MichaReiser on 2025-10-12 06:41_

---

_Label `wish` added by @MichaReiser on 2025-10-12 06:41_

---

_Comment by @MichaReiser on 2025-10-12 06:41_

Thanks for the detailed write-up! It made it easy to understand your use case. 

I'm a bit hesitant of adding more `--select` options as they already tend to confuse users.

I could see a few possibilities:

* Support negated `ignore`. That would allow you to write `--ignore ALL --ignore !D` to ignore all rules except `D`, while respecting the rules selected with `--select` and `--extend-select`. I think this should also be compatible with the rule selection that we introduced in ty (which we might port over to ruff at some point)
* Support this as part of an interactive CLI review mode. 



---

_Comment by @fmigneault on 2025-10-12 18:51_

If using `--ignore ALL --ignore !D`, I think I would end up with the inverse problem, that is, that I have to list all `!D...` I want to consider (the ones not ignored in the config), which is also a long list.

Another possibility would be for `--select ...` not to override other field indicated by `--config ...` (or auto-detected), but I think that could break expectations for users relying on the current behavior, which is why I think another option would be required. That being said, there is also an `--isolated` option, so users that would want the current behavior could use `--select ... --isolated` explicitly. 

---

_Comment by @MichaReiser on 2025-10-13 07:15_

> If using --ignore ALL --ignore !D, I think I would end up with the inverse problem, that is, that I have to list all !D... I want to consider (the ones not ignored in the config), which is also a long list.

Yeah, I think you're right

I thought more about this and I don't think we should make rule selection even more complicated, as it already is one of the most complicated parts. This approach might also no longer be supported after #1774 where rules are no longer categorized by linter.

What I'd do is to update the `select` in the configuration and comment out all rules that aren't `D`. I can then check all `D` rules. Once done, revert the configuration change and comment out another batch of rules. 

While not ideal, I don't think it's too bad. 

---

_Closed by @MichaReiser on 2025-10-13 07:15_

---

_Closed by @MichaReiser on 2025-10-13 07:15_

---

_Comment by @fmigneault on 2025-10-14 04:37_

Is there any chance the categorization logic could be reused to define "per-linter" categories as well? 
While I think #1774 is also a great addition for broader categories, when errors happen, they are associated with certain error codes. If the codes are associated to certain kind of errors, they are natively a good way to regroup them. 

There are some linters like flake8 (and its many plugins) that are definitely multi-purpose, and might not be that great for categories, but others like flynt, mypy, isort, pydocstyle for example have very specific purpose that fit nicely into categories (i.e.: fix strings, typings, imports, docstrings respectively).

---
