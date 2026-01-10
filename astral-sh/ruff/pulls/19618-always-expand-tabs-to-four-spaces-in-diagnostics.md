```yaml
number: 19618
title: Always expand tabs to four spaces in diagnostics
type: pull_request
state: merged
author: ntBre
labels:
  - diagnostics
assignees: []
merged: true
base: main
head: brent/tab-processing
created_at: 2025-07-29T16:54:59Z
updated_at: 2025-07-30T15:00:38Z
url: https://github.com/astral-sh/ruff/pull/19618
synced_at: 2026-01-10T17:58:13Z
```

# Always expand tabs to four spaces in diagnostics

---

_Pull request opened by @ntBre on 2025-07-29 16:54_

## Summary

I was a bit stuck on some snapshot differences I was seeing in #19415, but @BurntSushi pointed out that `annotate-snippets` already normalizes tabs on its own, which was very helpful! Instead of applying this change directly to the other branch, I wanted to try applying it in `ruff_linter` first. This should very slightly reduce the number of changes in #19415 proper.

It looks like `annotate-snippets` always expands a tab to four spaces, whereas I think we were aligning to tab stops:

```diff
  6 | spam(ham[1], { eggs: 2})
  7 | #: E201:1:6
- 8 | spam(   ham[1], {eggs: 2})
-   |      ^^^ E201
+ 8 | spam(    ham[1], {eggs: 2})
+   |      ^^^^ E201
```

```diff
61 | #: E203:2:15 E702:2:16
 62 | if x == 4:
-63 |     print(x, y) ; x, y = y, x
-   |                ^ E203
+63 |     print(x, y)    ; x, y = y, x
+   |                ^^^^ E203
```

```diff
 E27.py:15:6: E271 [*] Multiple spaces after keyword
    |
-13 | True        and False
+13 | True        and    False
 14 | #: E271
 15 | a and  b
    |      ^^ E271
```

I don't think this is too bad and has the major benefit of allowing us to pass the non-tab-expanded range to `annotate-snippets` in #19415, where it's also displayed in the header. Ruff doesn't have this problem currently because it uses its own concise diagnostic output as the header for full diagnostics, where the pre-expansion range is used directly.

## Test Plan

Existing tests with a few snapshot updates


---

_Label `diagnostics` added by @ntBre on 2025-07-29 17:04_

---

_Comment by @github-actions[bot] on 2025-07-29 17:04_

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

_Marked ready for review by @ntBre on 2025-07-29 17:05_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-29 17:05_

---

_Review requested from @BurntSushi by @ntBre on 2025-07-29 17:05_

---

_@BurntSushi approved on 2025-07-29 17:10_

This LGTM. You might want Micha to sign-off on the snapshot changes. I don't mind them personally, and I think I might even prefer them. With tab-stops, the number of spaces inserted is variable and could, I think, be confusing as to what is happening. Where as I think if someone is using tabs, if they see they are consistently replaced by 4 space characters, then I think it's probably easier to intuit what's happening.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E101_E101.py.snap`:20 on 2025-07-29 19:59_

I took a quick look at what my editor does and the old rendering is closer to what editors render (just noting down)

---

_@MichaReiser reviewed on 2025-07-30 07:57_

I think this is fine. I do prefer the the old layout because it closer matches what users see in their editor. But I don't think it justifies spending much time on. 

However, don't we have the same issue with unprintable replacements? Or is that fine, because we only map them to characters that map to the same column? It might be worth adding a comment explaining why it's okay that we do whatever we do ;)

Should this PR also undo the changes introduced in https://github.com/astral-sh/ruff/pull/19535 ?

---

_@BurntSushi reviewed on 2025-07-30 12:04_

---

_Review comment by @BurntSushi on `crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E101_E101.py.snap`:20 on 2025-07-30 12:04_

That would depend on your configured tabwidth though right? Admittedly, 4 is probably the most common, but if you use a different tabwidth I would guess the rendering would be different?

---

_Comment by @BurntSushi on 2025-07-30 12:07_

> However, don't we have the same issue with unprintable replacements? Or is that fine, because we only map them to characters that map to the same column? It might be worth adding a comment explaining why it's okay that we do whatever we do ;)

For the unprintable characters, they don't have a "width": https://play.rust-lang.org/?version=stable&mode=debug&edition=2024&gist=7c5639c10e050961f2ec7b81a82abcd6

But we're replacing it with a character that has a width of 1 column.

It's possible this might work fine today if there's a `character.width().unwrap_or(1)` somewhere in `annotate-snippets`. If not, I think the right way to fix that would be in `annotate-snippets`. IDK if that's worth prioritizing right now. We could open a bug issue for it.

---

_Comment by @ntBre on 2025-07-30 12:45_

> Should this PR also undo the changes introduced in https://github.com/astral-sh/ruff/pull/19535?

Ah, yep, I should revert most of that too.

Yeah I guess we do have the same issue for unprintable characters, but it's probably a bit less prominent with a difference of 1 vs 3. And I kind of assumed unprintable characters are less common than tabs too.

I'm pretty sure `annotate-snippets` doesn't handle this already. I actually tested unprintable characters in #19535, unlike tabs, but I can double check.

Assuming not, grepping for `unwrap_or(1)` was surprisingly fruitful:

https://github.com/astral-sh/ruff/blob/c5ac998892a339be0304c7f9e69a5318b371deb8/crates/ruff_annotate_snippets/src/renderer/display_list.rs#L355-L357 

I think this is for cutting long lines, but just above here is where `normalize_whitespace` is called, so it might be a promising place to try replacing unprintable characters too. I might spend a little time trying that.

---

_Comment by @ntBre on 2025-07-30 14:11_

Actually, I looked back at the snapshots in #19415, and if the problem is "we display the wrong range in the header," this is okay, at least for the snapshots in our test suite. I'm a bit surprised by this because the `text_len` of our replacement characters is 3, so we are updating the ranges, but it doesn't cause a mismatch in the header line like the tabs. I guess `annotate-snippets` _does_ account for the length vs width difference already.

I even added a more exaggerated example:

```py
nested_fstrings = f'{f'{f''}'}'
```

where we replace 6 unprintable characters instead of 1 in the `unprintable_characters` test. The input range is `29..29`, which gets shifted to `41..41`, but the output is still correct:

```
error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
 --> example.py:1:30
  |
1 | nested_fstrings = f'␈␈␈␈␈␈{f'{f'␛'}'}'
  |                              ^
  |
```

So I think our unprintable handling is okay. I'll revert the whitespace parts of #19535!

---

_@MichaReiser approved on 2025-07-30 14:27_

---

_Review requested from @carljm by @ntBre on 2025-07-30 14:30_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-30 14:30_

---

_Review requested from @sharkdp by @ntBre on 2025-07-30 14:30_

---

_Review requested from @dcreager by @ntBre on 2025-07-30 14:30_

---

_Comment by @ntBre on 2025-07-30 14:32_

I also renamed the functions since we're not replacing whitespace anymore, updated the docs on both versions, and finally added a tab test in `render::full`.

---

_Review request for @dcreager removed by @ntBre on 2025-07-30 14:32_

---

_Review request for @carljm removed by @ntBre on 2025-07-30 14:32_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-30 14:32_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-30 14:32_

---

_Comment by @github-actions[bot] on 2025-07-30 14:33_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-07-30 14:34_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Merged by @ntBre on 2025-07-30 15:00_

---

_Closed by @ntBre on 2025-07-30 15:00_

---

_Branch deleted on 2025-07-30 15:00_

---
