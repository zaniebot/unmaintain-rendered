```yaml
number: 13631
title: "[refurb] Count codepoints not bytes for `slice-to-remove-prefix-or-suffix (FURB188)`"
type: pull_request
state: merged
author: dylwil3
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: affix-unicode
created_at: 2024-10-04T16:24:43Z
updated_at: 2024-10-07T15:36:24Z
url: https://github.com/astral-sh/ruff/pull/13631
synced_at: 2026-01-12T15:55:45Z
```

# [refurb] Count codepoints not bytes for `slice-to-remove-prefix-or-suffix (FURB188)`

---

_@dylwil3_

This PR fixes the calculation of string length for the purposes of verifying when to suggest `removeprefix`/`removesuffix` (FURB188). Before, we used `text_len` which was counting bytes rather than codepoints (chars) and therefore disagreed with Python's `len` for non-ASCII text.

Closes #13620


---

_Comment by @github-actions[bot] on 2024-10-04 16:38_

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

_Review requested from @MichaReiser by @zanieb on 2024-10-04 17:27_

---

_Review requested from @dhruvmanila by @zanieb on 2024-10-04 17:27_

---

_Comment by @dscorbett on 2024-10-04 17:56_

Another subtlety worth testing is strings with surrogates. In Python, each surrogate counts as 1 and surrogate pairs are not special so they count as 2; for example, `len("\ud800\udc00") == 2` whereas `len("\U00010000") == 1`. I don’t know how Ruff distinguishes those two example strings or if `chars().count()` matches what Python does.

---

_Comment by @dylwil3 on 2024-10-04 18:26_

> Another subtlety worth testing is strings with surrogates. In Python, each surrogate counts as 1 and surrogate pairs are not special so they count as 2; for example, `len("\ud800\udc00") == 2` whereas `len("\U00010000") == 1`. I don’t know how Ruff distinguishes those two example strings or if `chars().count()` matches what Python does.

TIL @dscorbett - neat! Added a test for this, and it appears to be handled correctly (I think this happens in the guts of the parser, so by the time I'm looking at `string_val` here, the subtleties have been smoothed out).

---

_Comment by @dscorbett on 2024-10-05 02:06_

I think the reason it works is that Ruff’s representation of a Python string as a Rust string replaces surrogates with replacement characters. That is fine for counting the code points but could be a problem for other rules.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:339 on 2024-10-07 08:28_

I suggest converting to a `u64` considering that you have to use `usize::try_from` anyways (for 32 bit platforms)
```suggestion
            .and_then(ast::Int::as_u64)
            .and_then(|x| usize::try_from(x).ok())
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:339 on 2024-10-07 08:29_

Or you could consider adding a `as_usize` method to `ast::Int`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:340 on 2024-10-07 08:30_

```suggestion
            .is_some_and(|x| x == string_val.chars().count()),
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:376 on 2024-10-07 08:30_

```suggestion
                    .is_some_and(|x| x == string_val.chars().count())
```

---

_@MichaReiser approved on 2024-10-07 08:31_

Nice, thanks. I only have two nit comments. 

---

_Label `fixes` added by @MichaReiser on 2024-10-07 08:31_

---

_Label `preview` added by @MichaReiser on 2024-10-07 08:31_

---

_Label `preview` removed by @MichaReiser on 2024-10-07 08:31_

---

_Label `fixes` removed by @MichaReiser on 2024-10-07 08:31_

---

_Label `rule` added by @MichaReiser on 2024-10-07 08:31_

---

_Label `preview` added by @MichaReiser on 2024-10-07 08:31_

---

_@dylwil3 reviewed on 2024-10-07 13:32_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/rules/refurb/rules/slice_to_remove_prefix_or_suffix.rs`:339 on 2024-10-07 13:32_

went with the latter

---

_@MichaReiser approved on 2024-10-07 14:13_

---

_Merged by @MichaReiser on 2024-10-07 14:13_

---

_Closed by @MichaReiser on 2024-10-07 14:13_

---

_Branch deleted on 2024-10-07 15:36_

---
