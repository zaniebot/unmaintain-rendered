```yaml
number: 20012
title: Fix rust feature activation
type: pull_request
state: merged
author: ntBre
labels:
  - testing
assignees: []
merged: true
base: main
head: brent/get-size-feature
created_at: 2025-08-20T22:38:47Z
updated_at: 2025-08-21T07:26:08Z
url: https://github.com/astral-sh/ruff/pull/20012
synced_at: 2026-01-10T17:46:21Z
```

# Fix rust feature activation

---

_Pull request opened by @ntBre on 2025-08-20 22:38_

Summary
--

I ran into a problem earlier when trying to run this command:

```shell
$ cargo test -p ruff_source_file --all-features
error[E0277]: the trait bound `TextSize: GetSize` is not satisfied
  --> crates/ruff_source_file/src/line_index.rs:23:41
```

It looks like we just needed to activate the `get-size` feature of `ruff_text_size` when `ruff_source_file` activates its feature of the same name.

While I was at it, I looked for other cases with the same problem:

```shell
for crate in crates/*; do
    if ! cargo test -p ${crate#*/} --all-features > /dev/null; then
        echo "$crate failed"
        break
    fi
done
```

and fixed those too. I also tried with no features enabled, since I know we've run into similar problems with that before, but that didn't find anything except for some unreachable `pub` warnings for `TestDb` in `ty_project`. But `TestDb` is conditionally re-exported, so I don't think we can get rid of those warnings easily, unless we just want to `allow` the warnings.

Should we consider throwing something like the script above into a daily/weekly cron job in CI? I don't think we necessarily need to run it on every PR.

Test Plan
--

Same commands above, which now succeed


---

_Review requested from @carljm by @ntBre on 2025-08-20 22:38_

---

_Review requested from @MichaReiser by @ntBre on 2025-08-20 22:38_

---

_Review requested from @sharkdp by @ntBre on 2025-08-20 22:38_

---

_Review requested from @dcreager by @ntBre on 2025-08-20 22:38_

---

_Label `testing` added by @ntBre on 2025-08-20 22:38_

---

_Review request for @dcreager removed by @ntBre on 2025-08-20 22:39_

---

_Review request for @carljm removed by @ntBre on 2025-08-20 22:39_

---

_Review request for @sharkdp removed by @ntBre on 2025-08-20 22:39_

---

_Comment by @github-actions[bot] on 2025-08-20 22:40_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-08-20 22:42_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-08-20 22:50_

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

_Renamed from "Fix a few features" to "Fix rust feature activation" by @MichaReiser on 2025-08-21 07:25_

---

_Merged by @MichaReiser on 2025-08-21 07:26_

---

_Closed by @MichaReiser on 2025-08-21 07:26_

---

_Branch deleted on 2025-08-21 07:26_

---
