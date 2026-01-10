```yaml
number: 17864
title: migrate to dist-workspace.toml, use new workspace.packages config
type: pull_request
state: merged
author: Gankra
labels: []
assignees: []
merged: true
base: main
head: gankra/dist-sep
created_at: 2025-05-05T17:19:38Z
updated_at: 2025-05-05T18:07:48Z
url: https://github.com/astral-sh/ruff/pull/17864
synced_at: 2026-01-10T18:57:03Z
```

# migrate to dist-workspace.toml, use new workspace.packages config

---

_Pull request opened by @Gankra on 2025-05-05 17:19_

_No description provided._

---

_Review comment by @Gankra on `dist-workspace.toml`:4 on 2025-05-05 17:20_

This (and the version bump) should be the only real change

---

_@Gankra reviewed on 2025-05-05 17:22_

---

_Comment by @Gankra on 2025-05-05 17:22_

~~(Failing CI is expected, I haven't actually cut the prerelease yet 😉)~~

Prerelease cut.

---

_Comment by @github-actions[bot] on 2025-05-05 17:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/mesonbuild/meson-python">mesonbuild/meson-python</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read tests/packages/symlinks/baz.py: No such file or directory (os error 2)
error: Failed to read tests/packages/symlinks/qux.py: No such file or directory (os error 2)
```

</p>
</details>




---

_Marked ready for review by @Gankra on 2025-05-05 17:34_

---

_@zanieb approved on 2025-05-05 17:38_

---

_Merged by @Gankra on 2025-05-05 18:07_

---

_Closed by @Gankra on 2025-05-05 18:07_

---

_Branch deleted on 2025-05-05 18:07_

---
