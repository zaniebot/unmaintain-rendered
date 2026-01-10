```yaml
number: 22313
title: "[ty] `cargo insta test --force-update-snapshots`"
type: pull_request
state: merged
author: oconnor663
labels:
  - testing
assignees: []
merged: true
base: main
head: cargo-insta-force-update
created_at: 2025-12-30T23:13:14Z
updated_at: 2026-01-05T15:55:49Z
url: https://github.com/astral-sh/ruff/pull/22313
synced_at: 2026-01-10T16:30:32Z
```

# [ty] `cargo insta test --force-update-snapshots`

---

_Pull request opened by @oconnor663 on 2025-12-30 23:13_

Snapshot tests recently started reporting this warning:

> Snapshot test passes but the existing value is in a legacy format.
> Please run cargo insta test --force-update-snapshots to update to a
> newer format.

This PR is the result of that forced update.

One file (crates/ruff_db/src/diagnostic/render/full.rs) seems to get corrupted, because it contains strings with unprintable characters that trigger some bug in cargo-insta. I've manually reverted that file, but kept everything else.


---

_Review requested from @carljm by @oconnor663 on 2025-12-30 23:13_

---

_Review requested from @AlexWaygood by @oconnor663 on 2025-12-30 23:13_

---

_Review requested from @sharkdp by @oconnor663 on 2025-12-30 23:13_

---

_Review requested from @dcreager by @oconnor663 on 2025-12-30 23:13_

---

_Review requested from @MichaReiser by @oconnor663 on 2025-12-30 23:13_

---

_Review requested from @dhruvmanila by @oconnor663 on 2025-12-30 23:13_

---

_Label `testing` added by @AlexWaygood on 2025-12-30 23:23_

---

_Comment by @astral-sh-bot[bot] on 2025-12-30 23:25_


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

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip5.py.snap`:3 on 2025-12-31 07:38_

Hmm, why did it remove all the input files. I would very much like to retain this information in some form or another as it is very convenient to navigate to the test file.

---

_@MichaReiser reviewed on 2025-12-31 07:48_

---

_Comment by @AlexWaygood on 2026-01-02 16:12_

It would be nice to merge a solution to the warnings soon, because the warnings are extremely noisy when running tests for ty locally right now

---

_Review comment by @ntBre on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__fmtskip5.py.snap`:3 on 2026-01-02 16:29_

Was this added by `insta::glob` and not added by `datatest`? Just a guess

---

_@ntBre reviewed on 2026-01-02 16:29_

---

_Comment by @oconnor663 on 2026-01-02 19:34_

I couldn't figure out a clever way to restore all the `input_file` lines, so I just had Claude do it :) I can confirm that all the "legacy format" warnings from `main` are still gone if I run `cargo insta test`.

---

_Comment by @oconnor663 on 2026-01-02 19:38_

For context, when I looked into why exactly these legacy format warnings were printing, at least some of them were because the old snapshots were only equal to current output after trimming leading newlines. Specifically:

- https://github.com/mitsuhiko/insta/blob/fd40cf7d5197a8ac30779c376c0d36a9ac420da8/insta/src/snapshot.rs#L664-L665
- https://github.com/mitsuhiko/insta/blob/fd40cf7d5197a8ac30779c376c0d36a9ac420da8/insta/src/snapshot.rs#L824-L828

---

_@MichaReiser approved on 2026-01-03 16:45_

---

_Merged by @oconnor663 on 2026-01-05 15:55_

---

_Closed by @oconnor663 on 2026-01-05 15:55_

---

_Branch deleted on 2026-01-05 15:55_

---
