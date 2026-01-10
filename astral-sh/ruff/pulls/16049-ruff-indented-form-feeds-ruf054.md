```yaml
number: 16049
title: "[`ruff`] Indented form feeds (`RUF054`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF054
created_at: 2025-02-09T00:39:38Z
updated_at: 2025-02-10T12:26:49Z
url: https://github.com/astral-sh/ruff/pull/16049
synced_at: 2026-01-10T19:57:22Z
```

# [`ruff`] Indented form feeds (`RUF054`)

---

_Pull request opened by @InSyncWithFoo on 2025-02-09 00:39_

## Summary

Resolves #12321.

The physical-line-based `RUF054` checks for form feed characters that are preceded by only tabs and spaces, but not any other characters, including form feeds.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2025-02-09 00:45_

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

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/mod.rs`:539 on 2025-02-09 02:14_

This might be a good use for `test_case` like some of the other tests up above, if these don't get moved into a `.py` file.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/indented_form_feed.rs`:12 on 2025-02-09 02:16_

nit: it might be nice to have a link to the reference here

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/indented_form_feed.rs`:22 on 2025-02-09 02:20_

Does this render properly in the docs? I could imagine both of these just looking like

```python
if foo():
    bar()
```

but I haven't checked myself.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/indented_form_feed.rs`:64 on 2025-02-09 02:37_

I guess you could use `memchr` again for `SPACE` and `TAB` and compare the indices to `index_relative_to_line`, but this is probably fine too.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/mod.rs`:537 on 2025-02-09 02:50_

Emacs might be the outlier here, but it's quite easy for me to insert form feed characters. If it wouldn't be too much hassle in other editors (and thus for other contributors), it might be nice to have some normal snapshot tests instead of, or at least in addition to, these. We're not currently testing the actual diagnostic output, for example.

---

_Review comment by @ntBre on `crates/ruff_linter/src/registry.rs`:261 on 2025-02-09 02:52_

very small nit: I think these were sorted in alphabetical order before, so `IndentedFormFeed` would need to move up under `DocLineTooLong` to preserve that.

---

_@ntBre reviewed on 2025-02-09 02:55_

Looks good, thanks! I have a few nits and one slightly larger question/suggestion about the tests, but overall this looks good.

---

_@InSyncWithFoo reviewed on 2025-02-09 03:20_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/indented_form_feed.rs`:22 on 2025-02-09 03:20_

It does. We are talking three different flavors here, but consider GFM:

> ```python
> if foo():\n    \fbar()
> ```

---

_@InSyncWithFoo reviewed on 2025-02-09 03:20_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/indented_form_feed.rs`:64 on 2025-02-09 03:20_

I don't think the first index of a space or tab says anything. For example, `def f():\n\tpass \f` is not an error, even though `\f` is preceded by a space and both of them are placed on a tab-indented line.

---

_@InSyncWithFoo reviewed on 2025-02-09 03:20_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/mod.rs`:537 on 2025-02-09 03:20_

> [Dammit, Emacs.](https://xkcd.com/378/)

---

`\f` is not normally visible, so such a file would either trip up the CI checks or that of the editors. I can try, though.

---

_@ntBre reviewed on 2025-02-09 16:49_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/indented_form_feed.rs`:64 on 2025-02-09 16:49_

Oh I see now, I guess I ignored the `!=` checks!

---

_@ntBre reviewed on 2025-02-09 17:19_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/mod.rs`:537 on 2025-02-09 17:19_

> Dammit, Emacs.

:laughing: 

Well it's definitely unfortunate that these are completely invisible on GitHub, but I like seeing the diagnostics and reusing the other test code. Thanks!

---

_@ntBre approved on 2025-02-09 17:19_

---

_Merged by @ntBre on 2025-02-10 00:23_

---

_Closed by @ntBre on 2025-02-10 00:23_

---

_Branch deleted on 2025-02-10 00:26_

---

_Label `rule` added by @dhruvmanila on 2025-02-10 12:26_

---

_Label `preview` added by @dhruvmanila on 2025-02-10 12:26_

---
