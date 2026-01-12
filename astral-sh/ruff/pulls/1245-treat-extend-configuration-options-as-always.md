```yaml
number: 1245
title: "Treat `extend-*` configuration options as \"always extended\""
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/extends
created_at: 2022-12-15T01:12:52Z
updated_at: 2022-12-15T01:22:41Z
url: https://github.com/astral-sh/ruff/pull/1245
synced_at: 2026-01-12T15:55:06Z
```

# Treat `extend-*` configuration options as "always extended"

---

_@charliermarsh_

Now, every `--extend-ignore` and `--extend-select` pair is applied iteratively (rather than "overriding" the `--extend-ignore` and `--extend-select` of something else).

This sounds complicated, but I think the semantics are more intuitive.

As an example, say you have this in your `pyproject.toml`:

```toml
[tool.ruff]
select = ["F"]
ignore = ["F401"]
```

Today, if you run `ruff /path/to/file.py --extend-select F401`, we won't actually detect `F401`, because _today_, it's treated as equivalent to:

```toml
[tool.ruff]
select = ["F", "F401"]
ignore = ["F401"]
```

And ignores "beat" selects.

As of this PR, the same setup _would_ trigger `F401`, since we now resolve the first (select, ignore) pair, then apply the next (select, ignore) pair to the resolved codes, and so on.

This is particularly useful with configuration extension via `extend`, as the child `pyproject.toml` can _always_ enable and disable codes (whereas before, if a parent disabled a _specific_ code, it was impossible for the child to re-enable it).

Resolves #1216.

---

_Merged by @charliermarsh on 2022-12-15 01:22_

---

_Closed by @charliermarsh on 2022-12-15 01:22_

---

_Branch deleted on 2022-12-15 01:22_

---
