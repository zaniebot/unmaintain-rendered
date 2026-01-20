```yaml
number: 22776
title: "[ty] show final search path instead of \"and 1 more paths\""
type: pull_request
state: open
author: mswart
labels: []
assignees: []
base: main
head: show-one-more-path
created_at: 2026-01-20T20:45:11Z
updated_at: 2026-01-20T21:09:16Z
url: https://github.com/astral-sh/ruff/pull/22776
synced_at: 2026-01-20T21:54:18Z
```

# [ty] show final search path instead of "and 1 more paths"

---

_@mswart_

## Summary

If there are six search paths, five are attached as subdiagnostic of unresolved imports but the sixth is by default hidden and replaced by "... and 1 more paths. Run with `-v` to see all paths."

```
info: Searched in the following paths during module resolution:
info:   1. <temp_dir>/extra1 (extra search path specified on the CLI or in your config file)
info:   2. <temp_dir>/extra2 (extra search path specified on the CLI or in your config file)
info:   3. <temp_dir>/extra3 (extra search path specified on the CLI or in your config file)
info:   4. <temp_dir>/extra4 (extra search path specified on the CLI or in your config file)
info:   5. <temp_dir>/ (first-party code)
info:   ... and 1 more paths. Run with `-v` to see all paths.
```

By hiding a single path this truncation does not shorten the output but still requires the user to rerun ty. We can just include the final search path instead.
The subdiagnostic "and 1 more paths" isn't helpful - we can show the hidden path instead (and still have the same number of subdiagnistics).

```
info: Searched in the following paths during module resolution:
info:   1. <temp_dir>/extra1 (extra search path specified on the CLI or in your config file)
info:   2. <temp_dir>/extra2 (extra search path specified on the CLI or in your config file)
info:   3. <temp_dir>/extra3 (extra search path specified on the CLI or in your config file)
info:   4. <temp_dir>/extra4 (extra search path specified on the CLI or in your config file)
info:   5. <temp_dir>/ (first-party code)
info:   6. vendored://stdlib (stdlib typeshed stubs vendored by ty)
```

## Test Plan

* cargo test extended
* manual invocation (with the configuration that prompted me to propose this change).

---

_Review requested from @carljm by @mswart on 2026-01-20 20:45_

---

_Review requested from @AlexWaygood by @mswart on 2026-01-20 20:45_

---

_Review requested from @sharkdp by @mswart on 2026-01-20 20:45_

---

_Review requested from @dcreager by @mswart on 2026-01-20 20:45_

---

_Review requested from @MichaReiser by @mswart on 2026-01-20 20:45_

---

_@mswart reviewed on 2026-01-20 21:09_

---

_Review comment by @mswart on `crates/ty_python_semantic/src/types/infer/builder.rs`:8137 on 2026-01-20 21:09_

This could be implemented with `.next()`, too: we only count the items afterwards, but that feels a bit magic.

---
