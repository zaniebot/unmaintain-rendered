```yaml
number: 16704
title: describe requires-python fallback in docs
type: pull_request
state: merged
author: dylwil3
labels:
  - documentation
assignees: []
merged: true
base: main
head: config-docs
created_at: 2025-03-13T12:22:02Z
updated_at: 2025-03-13T16:48:13Z
url: https://github.com/astral-sh/ruff/pull/16704
synced_at: 2026-01-12T15:55:56Z
```

# describe requires-python fallback in docs

---

_@dylwil3_

Adds description of `requires-python` fallback to documentation for configuration file discovery.


---

_Label `documentation` added by @dylwil3 on 2025-03-13 12:22_

---

_Added to milestone `v0.10` by @dylwil3 on 2025-03-13 12:22_

---

_Review requested from @MichaReiser by @dylwil3 on 2025-03-13 12:22_

---

_Review requested from @ntBre by @dylwil3 on 2025-03-13 12:22_

---

_Comment by @codspeed-hq[bot] on 2025-03-13 12:27_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
<!-- __CODSPEED_INSTRUMENTATION_PERFORMANCE_REPORT_COMMENT__ -->

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dylwil3%3Aconfig-docs)

### Merging #16704 will **not alter performance**

<sub>Comparing <code>dylwil3:config-docs</code> (8da82b6) with <code>main</code> (abaa189)</sub>



### Summary

`✅ 32` untouched benchmarks  





---

_Comment by @github-actions[bot] on 2025-03-13 12:28_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `docs/configuration.md`:279 on 2025-03-13 12:47_

I think I'd omit this. It breaks the reading flow and is implicit by *attempt to fallback*

---

_Review comment by @MichaReiser on `docs/configuration.md`:284 on 2025-03-13 12:48_

```suggestion
1. If we are using a user-level configuration from `${config_dir}/ruff/pyproject.toml`, the `requires-python` field in the first `pyproject.toml` file found in an ancestor of the current working directory takes precedence over the `target-version` in the user-level configuration.
```

---

_@MichaReiser approved on 2025-03-13 12:49_

Thanks, this looks good

---

_Comment by @MichaReiser on 2025-03-13 12:50_

I'll change the base. We can merge this directly into main.

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-03-13 12:50_

---

_Review comment by @ntBre on `docs/configuration.md`:311 on 2025-03-13 16:10_

```suggestion
When no discovered configuration specifies a [`target-version`](settings.md#target-version), Ruff will attempt to fall back to the minimum version compatible with the `requires-python` field in a nearby `pyproject.toml`.
```

---

_@ntBre approved on 2025-03-13 16:22_

This looks great, and thanks for adding the subsection!

My only quibble is with the second rule. It seems slightly confusing to me, but I think it would be too verbose to try to add `ruff.toml` (or especially a list of file options) in place of each "configuration[ file]" occurrence to clarify things.

---

_Comment by @dylwil3 on 2025-03-13 16:45_

> My only quibble is with the second rule. It seems slightly confusing to me, but I think it would be too verbose to try to add ruff.toml (or especially a list of file options) in place of each "configuration[ file]" occurrence to clarify things.

Yeah I agree it's a bit awkward, but I can't think of a better way to word it at the moment. It's especially confusing because the found configuration file could itself be a `pyproject.toml`, just one that's missing both `requires-python` and `target-version`.

---

_Comment by @ntBre on 2025-03-13 16:48_

Agreed, I think it's good for now. We can iterate if users actually get confused.

---

_Merged by @dylwil3 on 2025-03-13 16:48_

---

_Closed by @dylwil3 on 2025-03-13 16:48_

---
