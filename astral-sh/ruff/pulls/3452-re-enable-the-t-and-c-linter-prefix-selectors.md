```yaml
number: 3452
title: Re-enable the T and C linter prefix selectors
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
assignees: []
merged: true
base: main
head: charlie/t
created_at: 2023-03-11T20:38:59Z
updated_at: 2023-03-13T12:20:31Z
url: https://github.com/astral-sh/ruff/pull/3452
synced_at: 2026-01-12T15:55:12Z
```

# Re-enable the T and C linter prefix selectors

---

_@charliermarsh_

## Summary

In one of the prior refactors around lint rule prefixes, we removed the `T` and `C` selectors.

To take `T` as an example, the issue is that we have two "linters" that both use the `T` prefix -- `flake8-debugger`, whose rules are (e.g.) `T100`, and `flake8-print`, whose rules are (e.g.) `T201`. In the refactored model, `flake8-debugger` was represented as having a prefix of `T10`, and a rule under that prefix with code `0`, which combines to an overall selector of `T100`.

Under this scheme, if the user provides the `T` prefix, we don't know where to map that code. Prior to this PR, it was redirected to `T10`, so if a user provided `--select T`, that aliased to `--select T10`, which enabled rule `T100`, thus ignoring the `flake8-print` rules entirely. I believe this was unintentional.

The same problem exists with the `C` prefix, where we have both the McCabe complexity and `flake8-comprehensions` rules. The McCabe rules no longer run under `--select C`, because it was redirected to `C40`.

This PR special-cases those two prefixes, introducing custom selectors for `T` and `C` to re-enable the expected user behavior.

Closes #3052.

---

_Label `bug` added by @charliermarsh on 2023-03-11 20:39_

---

_Comment by @github-actions[bot] on 2023-03-11 20:48_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Review requested from @konstin by @charliermarsh on 2023-03-11 20:50_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-11 20:50_

---

_Review comment by @charliermarsh on `crates/ruff/src/rule_redirects.rs`:21 on 2023-03-11 20:50_

No longer need to redirect these at all, since they have their own custom selectors.

---

_@charliermarsh reviewed on 2023-03-11 20:50_

---

_Comment by @charliermarsh on 2023-03-11 20:53_

There are likely further refactors we can do here, but I'd like to get this out in the next release as it's probably surprising to users that certain codes are no longer enforced despite no change in their selectors (e.g., `select = ["C"]` no longer runs the complexity checks).

---

_@MichaReiser approved on 2023-03-13 08:04_

---

_@konstin approved on 2023-03-13 09:36_

---

_Merged by @charliermarsh on 2023-03-13 12:20_

---

_Closed by @charliermarsh on 2023-03-13 12:20_

---

_Branch deleted on 2023-03-13 12:20_

---
