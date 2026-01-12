```yaml
number: 12159
title: Fix cache key collisions for paths with separators
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/hash-path
created_at: 2024-07-03T00:39:16Z
updated_at: 2024-07-03T12:36:48Z
url: https://github.com/astral-sh/ruff/pull/12159
synced_at: 2026-01-12T15:55:40Z
```

# Fix cache key collisions for paths with separators

---

_@zanieb_

Closes https://github.com/astral-sh/ruff/issues/12158

Hashing `Path` does not take into account path separators so `foo/bar` is the same as `foobar` which is no good for our case. I'm guessing this is an upstream bug, perhaps introduced by https://github.com/rust-lang/rust/commit/45082b077b4991361f451701d0a9467ce123acdf? I'm investigating that further.

---

_Label `bug` added by @zanieb on 2024-07-03 00:39_

---

_Comment by @github-actions[bot] on 2024-07-03 00:52_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **encountered linter errors**. (no lint changes; 1 project error)

<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (error)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

```
warning: The top-level linter settings are deprecated in favour of their counterparts in the `lint` section. Please update the following options in `pyproject.toml`:
  - 'ignore' -> 'lint.ignore'
  - 'select' -> 'lint.select'
  - 'unfixable' -> 'lint.unfixable'
  - 'per-file-ignores' -> 'lint.per-file-ignores'
warning: `PGH001` has been remapped to `S307`.
warning: `PGH002` has been remapped to `G010`.
warning: `PLR1701` has been remapped to `SIM101`.
ruff failed
  Cause: Selection of deprecated rule `E999` is not allowed when preview is enabled.
```

</p>
</details>




---

_Comment by @zanieb on 2024-07-03 01:23_

Filed a bug report at https://github.com/rust-lang/rust/issues/127254

---

_Comment by @zanieb on 2024-07-03 04:34_

I think this fix is safe because it's what was done upstream until this regression was introduced https://github.com/rust-lang/rust/commit/a083dd653af0f7f46ba6058ab51e1f9d6a2aca7d

---

_Marked ready for review by @zanieb on 2024-07-03 04:34_

---

_Review requested from @MichaReiser by @zanieb on 2024-07-03 04:34_

---

_Review requested from @charliermarsh by @zanieb on 2024-07-03 04:34_

---

_@MichaReiser approved on 2024-07-03 06:27_

Thank you! 

We could also consider to hash the [`as_os_str`](https://doc.rust-lang.org/std/path/struct.Path.html#method.as_os_str) because all we really care about is if the path changed and invalidating if the path changed but, technically is still equal, isn't a real concern.

---

_Merged by @zanieb on 2024-07-03 12:36_

---

_Closed by @zanieb on 2024-07-03 12:36_

---

_Branch deleted on 2024-07-03 12:36_

---
