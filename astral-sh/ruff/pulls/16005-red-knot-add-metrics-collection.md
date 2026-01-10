```yaml
number: 16005
title: "[red-knot] Add metrics collection"
type: pull_request
state: open
author: dcreager
labels:
  - ty
assignees: []
draft: true
base: main
head: dcreager/metrics
created_at: 2025-02-06T22:29:57Z
updated_at: 2025-02-07T16:49:32Z
url: https://github.com/astral-sh/ruff/pull/16005
synced_at: 2026-01-10T19:57:22Z
```

# [red-knot] Add metrics collection

---

_Pull request opened by @dcreager on 2025-02-06 22:29_

This adds metrics collection to red-knot.

[[rendered README](https://github.com/astral-sh/ruff/blob/dcreager/metrics/crates/ruff_metrics/README.md)]

**Work in progress**

---

_Label `red-knot` added by @dcreager on 2025-02-06 22:29_

---

_Comment by @dcreager on 2025-02-06 22:35_

I still want to add more actual metrics (and verify that there aren't any performance regressions), but I'm pushing this up for visibility.

Overview documentation available in the [README](https://github.com/astral-sh/ruff/blob/dcreager/metrics/crates/ruff_metrics/README.md).

Collect metrics by passing the `--metrics` flag:

```console
$ ./red_knot check --project ~/git/py/black --venv-path .venv --extra-search-path src --metrics
```

Then generate some graphs!

Total scopes created over time:

```console
$ uv run crates/ruff_metrics/plot_metrics.py counter semantic_index.scope_count
```

![counter](https://github.com/user-attachments/assets/b03145b9-c962-4645-9dad-ea4d29c263ee)

Total scopes over time, per file:

```console
$ uv run crates/ruff_metrics/plot_metrics.py counter semantic_index.scope_count --group-by file
```

![counter-by-file](https://github.com/user-attachments/assets/ad135be5-bbaf-4ded-9661-39b93e6c9d7e)

Histogram of total scopes per file:

```console
$ uv run crates/ruff_metrics/plot_metrics.py histogram semantic_index.scope_count --group-by file
```

![histogram](https://github.com/user-attachments/assets/dc127069-bfbb-46e3-b122-f8ace54a8c3a)


---

_Comment by @github-actions[bot] on 2025-02-06 22:46_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---
