```yaml
number: 19535
title: "[ty] Fix empty spans following a line terminator and unprintable character spans in diagnostics"
type: pull_request
state: merged
author: ntBre
labels:
  - ty
  - diagnostics
assignees: []
merged: true
base: main
head: brent/empty-span-after-line-terminator
created_at: 2025-07-24T18:42:25Z
updated_at: 2025-07-29T12:26:01Z
url: https://github.com/astral-sh/ruff/pull/19535
synced_at: 2026-01-10T17:58:13Z
```

# [ty] Fix empty spans following a line terminator and unprintable character spans in diagnostics

---

_Pull request opened by @ntBre on 2025-07-24 18:42_

## Summary

This was previously the last commit in #19415, split out to make it easier to review. This applies the fixes from c9b99e4, 5021f32, and 2922490cb8 to the new rendering code in `ruff_db`. I initially intended only to fix the empty span after a line terminator (as you can see in the branch name), but the two fixes were tied pretty closely together, and my initial fix for the empty spans needed a big change after trying to handle unprintable characters too. I can still split this up if it would help with review. I would just start with the unprintable characters first.

The implementation here is essentially copy-pasted from `ruff_linter::message::text.rs`, with the `SourceCode` struct renamed to `EscapedSourceCode` since there's already a `SourceCode` in scope in `render.rs`. It's also updated slightly to account for the multiple annotations for a single snippet. The original implementation used some types from the `line_width` module from `ruff_linter`. I copied over heavily stripped-down versions of these instead of trying to import them. We could inline the remaining code entirely, if we want, but I thought it was nice enough to keep.

I also moved over `ceil_char_boundary`, which is unchanged except to make it a free function taking a `&str` instead of a `Locator` method. All of this code could be deleted from `ruff_linter` if we also move over the `grouped` output format, which will be the last user after #19415.

## Test Plan

I added new tests in `ruff_linter` that call into the new rendering code to snapshot the diagnostics for the affected cases. These are copies of existing snapshots in Ruff, so it's helpful to compare them. These are a bit noisy because of the other rendering differences in the header, but all of the `^^^` indicators should be the same.

<details><summary>`empty_span_after_line_terminator` diff</summary>

```diff
diff --git a/crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E112_E11.py.snap b/crates/ruff_linter/src/message/snapshots/ruff_linter__message__text__tests__empty_span_after_line_terminator.snap
index 5ade4346e0..6df75c16f0 100644
--- a/crates/ruff_linter/src/rules/pycodestyle/snapshots/ruff_linter__rules__pycodestyle__tests__E112_E11.py.snap
+++ b/crates/ruff_linter/src/message/snapshots/ruff_linter__message__text__tests__empty_span_after_line_terminator.snap
@@ -1,17 +1,20 @@
 ---
-source: crates/ruff_linter/src/rules/pycodestyle/mod.rs
+source: crates/ruff_linter/src/message/text.rs
+expression: value.to_string()
 ---
-E11.py:9:1: E112 Expected an indented block
+error[no-indented-block]: Expected an indented block
+  --> E11.py:9:1
    |
  7 | #: E112
  8 | if False:
  9 | print()
-   | ^ E112
+   | ^
 10 | #: E113
 11 | print()
    |
 
-E11.py:9:1: SyntaxError: Expected an indented block after `if` statement
+error[invalid-syntax]: SyntaxError: Expected an indented block after `if` statement
+  --> E11.py:9:1
    |
  7 | #: E112
  8 | if False:
@@ -21,7 +24,8 @@ E11.py:9:1: SyntaxError: Expected an indented block after `if` statement
 11 | print()
    |
 
-E11.py:12:1: SyntaxError: Unexpected indentation
+error[invalid-syntax]: SyntaxError: Unexpected indentation
+  --> E11.py:12:1
    |
 10 | #: E113
 11 | print()
@@ -31,7 +35,8 @@ E11.py:12:1: SyntaxError: Unexpected indentation
 14 | mimetype = 'application/x-directory'
    |
 
-E11.py:14:1: SyntaxError: Expected a statement
+error[invalid-syntax]: SyntaxError: Expected a statement
+  --> E11.py:14:1
    |
 12 |     print()
 13 | #: E114 E116
@@ -41,17 +46,19 @@ E11.py:14:1: SyntaxError: Expected a statement
 16 | create_date = False
    |
 
-E11.py:45:1: E112 Expected an indented block
+error[no-indented-block]: Expected an indented block
+  --> E11.py:45:1
    |
 43 | #: E112
 44 | if False:  #
 45 | print()
-   | ^ E112
+   | ^
 46 | #:
 47 | if False:
    |
 
-E11.py:45:1: SyntaxError: Expected an indented block after `if` statement
+error[invalid-syntax]: SyntaxError: Expected an indented block after `if` statement
+  --> E11.py:45:1
    |
 43 | #: E112
 44 | if False:  #
```

</details>

<details><summary>`unprintable_characters` diff</summary>

```diff
diff --git a/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2512_invalid_characters.py.snap b/crates/ruff_linter/src/message/snapshots/ruff_linter__message__text__tests__unprintable_characters.snap
index 52cfdf9cce..fcfa1ac9f1 100644
--- a/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2512_invalid_characters.py.snap
+++ b/crates/ruff_linter/src/message/snapshots/ruff_linter__message__text__tests__unprintable_characters.snap
@@ -1,161 +1,115 @@
 ---
-source: crates/ruff_linter/src/rules/pylint/mod.rs
+source: crates/ruff_linter/src/message/text.rs
+expression: value.to_string()
 ---
-invalid_characters.py:24:12: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:24:12
    |
 22 | cr_ok = f'\\r'
 23 |
 24 | sub = 'sub '
-   |            ^ PLE2512
+   |            ^
 25 | sub = f'sub '
    |
-   = help: Replace with escape sequence
+help: Replace with escape sequence
 
-ℹ Safe fix
-21 21 | cr_ok = '\\r'
-22 22 | cr_ok = f'\\r'
-23 23 | 
-24    |-sub = 'sub '
-   24 |+sub = 'sub \x1A'
-25 25 | sub = f'sub '
-26 26 | 
-27 27 | sub_ok = '\x1a'
-
-invalid_characters.py:25:13: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:25:13
    |
 24 | sub = 'sub '
 25 | sub = f'sub '
-   |             ^ PLE2512
+   |             ^
 26 |
 27 | sub_ok = '\x1a'
    |
-   = help: Replace with escape sequence
-
-ℹ Safe fix
-22 22 | cr_ok = f'\\r'
-23 23 | 
-24 24 | sub = 'sub '
-25    |-sub = f'sub '
-   25 |+sub = f'sub \x1A'
-26 26 | 
-27 27 | sub_ok = '\x1a'
-28 28 | sub_ok = f'\x1a'
+help: Replace with escape sequence
 
-invalid_characters.py:55:25: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:55:25
    |
 53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
 54 |
 55 | nested_fstrings = f'␈{f'{f'␛'}'}'
-   |                         ^ PLE2512
+   |                         ^
 56 |
 57 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998106
    |
-   = help: Replace with escape sequence
-
-ℹ Safe fix
-52 52 | zwsp_after_multicharacter_grapheme_cluster = "ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
-53 53 | zwsp_after_multicharacter_grapheme_cluster = f"ಫ್ರಾನ್ಸಿಸ್ಕೊ ​​"
-54 54 | 
-55    |-nested_fstrings = f'␈{f'{f'␛'}'}'
-   55 |+nested_fstrings = f'␈{f'\x1A{f'␛'}'}'
-56 56 | 
-57 57 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998106
-58 58 | x = f"""}}ab"""
+help: Replace with escape sequence
 
-invalid_characters.py:58:12: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:58:12
    |
 57 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998106
 58 | x = f"""}}ab"""
-   |            ^ PLE2512
+   |            ^
 59 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998256
 60 | x = f"""}}a␛b"""
    |
-   = help: Replace with escape sequence
+help: Replace with escape sequence
 
-ℹ Safe fix
-55 55 | nested_fstrings = f'␈{f'{f'␛'}'}'
-56 56 | 
-57 57 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998106
-58    |-x = f"""}}ab"""
-   58 |+x = f"""}}a\x1Ab"""
-59 59 | # https://github.com/astral-sh/ruff/issues/7455#issuecomment-1741998256
-60 60 | x = f"""}}a␛b"""
-61 61 | 
-
-invalid_characters.py:64:12: PLE2512 Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:64:12
    |
 63 | # https://github.com/astral-sh/ruff/issues/13294
 64 | print(r"""␈␛�​
-   |            ^ PLE2512
+   |            ^
 65 | """)
 66 | print(fr"""␈␛�​
    |
-   = help: Replace with escape sequence
+help: Replace with escape sequence
 
-invalid_characters.py:66:13: PLE2512 Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:66:13
    |
 64 | print(r"""␈␛�​
 65 | """)
 66 | print(fr"""␈␛�​
-   |             ^ PLE2512
+   |             ^
 67 | """)
 68 | print(Rf"""␈␛�​
    |
-   = help: Replace with escape sequence
+help: Replace with escape sequence
 
-invalid_characters.py:68:13: PLE2512 Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:68:13
    |
 66 | print(fr"""␈␛�​
 67 | """)
 68 | print(Rf"""␈␛�​
-   |             ^ PLE2512
+   |             ^
 69 | """)
    |
-   = help: Replace with escape sequence
+help: Replace with escape sequence
 
-invalid_characters.py:73:9: PLE2512 Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:73:9
    |
 71 | # https://github.com/astral-sh/ruff/issues/18815
 72 | b = "\␈"
 73 | sub = "\"
-   |         ^ PLE2512
+   |         ^
 74 | esc = "\␛"
 75 | zwsp = "\​"
    |
-   = help: Replace with escape sequence
+help: Replace with escape sequence
 
-invalid_characters.py:80:25: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:80:25
    |
 78 | # tstrings
 79 | esc = t'esc esc ␛'
 80 | nested_tstrings = t'␈{t'{t'␛'}'}'
-   |                         ^ PLE2512
+   |                         ^
 81 | nested_ftstrings = t'␈{f'{t'␛'}'}'
    |
-   = help: Replace with escape sequence
-
-ℹ Safe fix
-77 77 | 
-78 78 | # tstrings
-79 79 | esc = t'esc esc ␛'
-80    |-nested_tstrings = t'␈{t'{t'␛'}'}'
-   80 |+nested_tstrings = t'␈{t'\x1A{t'␛'}'}'
-81 81 | nested_ftstrings = t'␈{f'{t'␛'}'}'
-82 82 | 
+help: Replace with escape sequence
 
-invalid_characters.py:81:26: PLE2512 [*] Invalid unescaped character SUB, use "\x1A" instead
+error[invalid-character-sub]: Invalid unescaped character SUB, use "\x1A" instead
+  --> invalid_characters.py:81:26
    |
 79 | esc = t'esc esc ␛'
 80 | nested_tstrings = t'␈{t'{t'␛'}'}'
 81 | nested_ftstrings = t'␈{f'{t'␛'}'}'
-   |                          ^ PLE2512
+   |                          ^
    |
-   = help: Replace with escape sequence
-
-ℹ Safe fix
-78 78 | # tstrings
-79 79 | esc = t'esc esc ␛'
-80 80 | nested_tstrings = t'␈{t'{t'␛'}'}'
-81    |-nested_ftstrings = t'␈{f'{t'␛'}'}'
-   81 |+nested_ftstrings = t'␈{f'\x1A{t'␛'}'}'
-82 82 |
+help: Replace with escape sequence
```

</details>

---

_Label `internal` added by @ntBre on 2025-07-24 18:42_

---

_Label `diagnostics` added by @ntBre on 2025-07-24 18:42_

---

_Comment by @github-actions[bot] on 2025-07-24 18:52_

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

_Comment by @ntBre on 2025-07-24 18:57_

After looking at this again,  I think we can probably do both this check and the unprintable check in `RenderableSnippet::new` as I mentioned in my other comment. I think I'll try that before taking this out of draft.

---

_@ntBre reviewed on 2025-07-25 15:54_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/snapshots/ruff_linter__message__text__tests__unprintable_characters.snap`:10 on 2025-07-25 15:54_

This matches this existing snapshot on main:

https://github.com/astral-sh/ruff/blob/b9d27c63bfb6e12695dbdedb3d813efb760c644d/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2512_invalid_characters.py.snap#L46-L50

Link to this file for direct comparison since it renders a bit differently in the PR view:

https://github.com/astral-sh/ruff/blob/b9d27c63bfb6e12695dbdedb3d813efb760c644d/crates/ruff_linter/src/message/snapshots/ruff_linter__message__text__tests__unprintable_characters.snap#L7-L10

Rather than the snapshot before the fix in e6e610c274:

https://github.com/astral-sh/ruff/blob/670fcecd1b57bf2d29e1a92fce6489bd21c01386/crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2512_invalid_characters.py.snap#L46-L50

---

_@ntBre reviewed on 2025-07-25 18:36_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/snapshots/ruff_linter__message__text__tests__unprintable_characters.snap`:10 on 2025-07-25 18:36_

Resolving this. I reorganized the tests to make them easier to diff against existing snapshots.

---

_Comment by @github-actions[bot] on 2025-07-25 18:40_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Renamed from "Fix empty spans following a line terminator in `ruff_db`" to "Fix empty spans following a line terminator and unprintable character spans in `ruff_db`" by @ntBre on 2025-07-25 19:43_

---

_Marked ready for review by @ntBre on 2025-07-25 19:47_

---

_Review requested from @carljm by @ntBre on 2025-07-25 19:47_

---

_Review requested from @MichaReiser by @ntBre on 2025-07-25 19:47_

---

_Review requested from @AlexWaygood by @ntBre on 2025-07-25 19:47_

---

_Review requested from @sharkdp by @ntBre on 2025-07-25 19:47_

---

_Review requested from @dcreager by @ntBre on 2025-07-25 19:47_

---

_Review request for @dcreager removed by @ntBre on 2025-07-25 19:47_

---

_Review request for @carljm removed by @ntBre on 2025-07-25 19:47_

---

_Review request for @sharkdp removed by @ntBre on 2025-07-25 19:47_

---

_Review request for @AlexWaygood removed by @ntBre on 2025-07-25 19:47_

---

_Review requested from @BurntSushi by @ntBre on 2025-07-25 19:47_

---

_Label `ty` added by @MichaReiser on 2025-07-26 09:42_

---

_Renamed from "Fix empty spans following a line terminator and unprintable character spans in `ruff_db`" to "Fix empty spans following a line terminator and unprintable character spans in Diagnostics" by @MichaReiser on 2025-07-26 09:42_

---

_Renamed from "Fix empty spans following a line terminator and unprintable character spans in Diagnostics" to "Fix empty spans following a line terminator and unprintable character spans in diagnostics" by @MichaReiser on 2025-07-26 09:42_

---

_@MichaReiser reviewed on 2025-07-26 09:45_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/text.rs`:480 on 2025-07-26 09:45_

Could these be tests in `render::tests` instead? 

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:833 on 2025-07-26 10:27_

The name here feels a bit off now, considering that it isn't a proper builder anymore.

Maybe just call it `Position` and I suggest renaming `get` to `width` to make it clear what it returns (or remove the accessor all together and access the field directly)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-26 10:29_

I don't think I fully understand why we need to collect the ranges first, only to then zip then here again. Can't we call `ann.range()` inside the loop?

```rust
for ann in annotations.iter_mut() {
	let original_range = ann.range();
  if index < usize::from(original_range.start()) {


---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:882 on 2025-07-26 10:31_

Nit: Move the declaration of this variable closer to where it's used. 

Same for `last_end`. It helps readability because, as a reader, I have to page less variables into my memory :)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:932 on 2025-07-26 10:32_

Given that we have to match on tabs here anyway, do you think the `LineWidthBuilder` is worth it or would it simplify the code if it were inlined instead?

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:981 on 2025-07-26 10:33_

This is probably a pre-existing issue but we should also handle `\r` here

---

_@MichaReiser approved on 2025-07-26 10:34_

Nice. I only have a few nit comments. The only thing I would change is to make these unit tests in ruff db. We do have the infrastructure to write those (I think?)

---

_Label `internal` removed by @MichaReiser on 2025-07-26 10:35_

---

_Renamed from "Fix empty spans following a line terminator and unprintable character spans in diagnostics" to "[ty] Fix empty spans following a line terminator and unprintable character spans in diagnostics" by @MichaReiser on 2025-07-26 10:35_

---

_@ntBre reviewed on 2025-07-26 13:17_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/text.rs`:480 on 2025-07-26 13:17_

Not very easily. For the tests in `ruff_db` we have to synthesize diagnostics:

https://github.com/astral-sh/ruff/blob/dc92966cc0ec10f703157262519837e197cf2da0/crates/ruff_db/src/diagnostic/render.rs#L2597-L2606

I started out doing that and then realized it would be much easier to use Ruff's actual infrastructure to get the diagnostics and not accidentally test the wrong thing.

It should be safe to delete these tests once Ruff uses the new renderer in general anyway, since they'll be duplicates of existing snapshots.

---

_@ntBre reviewed on 2025-07-26 13:24_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-26 13:24_

I think we need the initial range from the start of the function. I ran into a double-counting issue yesterday when I was using the current `ann.range` throughout (I was ending up with a range of 64-65 instead of the expected 62-63, starting from 60-61). I think capturing the initial range should match the other version of this code, which saves the input `annotation_range` and mutates the `range` copy:

https://github.com/astral-sh/ruff/blob/dc92966cc0ec10f703157262519837e197cf2da0/crates/ruff_linter/src/message/text.rs#L279-L283

though it pained me to collect here too.

---

_@ntBre reviewed on 2025-07-26 13:32_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:981 on 2025-07-26 13:32_

I thought this would be okay in this context because checking `\n` as the preceding character should cover both `\n` and `\r\n` line endings, but it's easy enough to add `\r` either way.

---

_@ntBre reviewed on 2025-07-26 13:39_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:932 on 2025-07-26 13:39_

I think it does look nicer inlined now that I tried it, thanks! You can see the whole looping logic on one screen now.

I don't see a great way to combine the two matches, but I still think it's an improvement.

---

_@ntBre reviewed on 2025-07-26 13:39_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:833 on 2025-07-26 13:39_

Inlined as in your other suggestion!

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:932 on 2025-07-26 14:01_

Oh, I see how to combine them. Obviously `\n` and `\r` won't hit the unprintable branch :facepalm: 

---

_@ntBre reviewed on 2025-07-26 14:01_

---

_@MichaReiser reviewed on 2025-07-26 16:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/text.rs`:480 on 2025-07-26 16:18_

I still think it's worth having dedicated tests for this or we'll loose the assertion if the rule changes or gets removed. I'm aware that it requires building the diagnostics manually but I'd expect that ir isn't too hard. It only requires calling a fewer diagnostic builder methods (and it's definitely easier to debug). But maybe I'm missing something?

(Looking at the test. The builder code is roughly as much code as what you have here)

---

_@ntBre reviewed on 2025-07-26 16:34_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/text.rs`:480 on 2025-07-26 16:34_

That's the builder code for one diagnostic, whereas the current test has 6 diagnostics in this case and 10 diagnostics for `unprintable_characters`. I'll strip down the two interesting cases and move them to `ruff_db`. I thought it was nice to have the full snapshot for diffing against Ruff, but now that that's resolved we can just keep minimized versions of the tests.

I'm still a bit wary of having to manually input the ranges in the builder, which could also fall out of sync with real diagnostics, but I see the benefits otherwise.

---

_@ntBre reviewed on 2025-07-26 16:56_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/text.rs`:480 on 2025-07-26 16:56_

Alright, that wasn't so bad: a4f74340fb068fb24da84f1f73af31ba29c2b5b1. I also applied the patch to `main` and checked that they failed there as expected.

The stripped down versions are also nice because they fit better as inline snapshots.

---

_@MichaReiser reviewed on 2025-07-26 17:53_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/text.rs`:480 on 2025-07-26 17:53_

> I'm still a bit wary of having to manually input the ranges in the builder

What I like to do in those cases is to use str.find to get the offset

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-27 15:37_

I realized that we didn't have a test for this in the stripped-down version, so I just added another test case. Without using `original_ranges`, this input:

```py
  # rendered as ^H^Z^[ in my editor
```

causes

```
    thread 'diagnostic::render::full::tests::multiple_unprintable_characters' panicked at crates/ruff_annotate_snippets/src/renderer/display_list.rs:1450:29:
    byte index 5 is not a char boundary; it is inside '␛' (bytes 4..7) of `␈␛`
```

From my earlier debugging, the issue was some kind of double  counting in `replace_whitespace_and_unprintable`. The input annotation range here is `1..1`, and the output in this panicking version is `5..5`, instead of the correct `3..3`, so it gets shifted twice if you compare against the current range.

---

_@ntBre reviewed on 2025-07-27 15:37_

---

_@MichaReiser reviewed on 2025-07-28 08:01_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:981 on 2025-07-28 08:01_

That's true. But Ruff still supports `\r` (because python does)

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-28 08:10_

Ohh, I see now. This is a callback function. That's why it matters. 

It seems unfortunate that we have to collect all original ranges even if the source code doesn't contain a single tab character (which should be the most common case).

Would it make sense to change the `for (index, c) in source.char_indices() {` to track the relative offset compared to the source offset. This way, you could call `update_ranges` with `index + relative_offset` and remove the original ranges thing entirely

---

_@MichaReiser reviewed on 2025-07-28 08:10_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-28 08:12_

Thinking about this: I think the relative offset is already known. It's `result.text_len() - source.text_len()`. That's how many characters where inserted by this function

---

_@MichaReiser reviewed on 2025-07-28 08:12_

---

_@ntBre reviewed on 2025-07-28 12:39_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-28 12:39_

`result` is likely empty until the very end of the function because we try to return a `Cow::Borrowed(source)` if nothing is modified, so I don't think we can get the offset from there.

The insertion position also matters since the callback differentiates between insertions before the start of the annotation and elsewhere inside the annotation, and it will be different for each annotation. Maybe an offset can still capture that, though.

I'll keep thinking about this, I agree it would be much nicer not to collect.

---

_@MichaReiser reviewed on 2025-07-28 12:48_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-28 12:48_

> result is likely empty until the very end of the function because we try to return a Cow::Borrowed(source) if nothing is modified, so I don't think we can get the offset from there.

It seems we update `result` everytime before we call `update_ranges`. Which makes sense to me. We update the source text and that, in return, requires updating the annotations. 



---

_@ntBre reviewed on 2025-07-28 12:49_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:981 on 2025-07-28 12:49_

Added!

---

_Review comment by @BurntSushi on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-28 12:53_

Alternatively, we could just do a regex search for tab and unprintable characters (looks like 5 distinct bytes?). If none are found, then you can just quit early with the `Cow::Borrowed` case. This search won't need to do UTF-8 decoding and `regex` might even vectorize it, so it could save you from allocating and UTF-8 decoding.

With all that said, even without that search to bail early, the up-front alloc here is almost certainly marginal. We are in the rendering code here, which does all sorts of allocs (including in `annotate-snippets`).

---

_@BurntSushi approved on 2025-07-28 12:53_

---

_@ntBre reviewed on 2025-07-28 12:54_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-28 12:54_

Ah maybe I'm misunderstanding the suggestion. In my naive attempt I tried `result.text_len() - source.text_len()` and kept getting underflow errors because we're building up the `result` over time, but `source` is always quite long from the beginning. Did you mean some sub-slice of `source`? Or maybe I injected this at the wrong point in the function.

What I meant to say above is "`result` doesn't have a comparable length to `source` until the very end of the function," not that it's totally empty.

---

_@MichaReiser reviewed on 2025-07-28 13:00_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-28 13:00_

Oh I see. In that case, isn't the relative offset `result.text_len() - i`?

---

_@ntBre reviewed on 2025-07-28 14:00_

---

_Review comment by @ntBre on `crates/ruff_db/src/diagnostic/render.rs`:892 on 2025-07-28 14:00_

Ah, you're right, thanks! I had just missed accounting for the width of the character being inserted when I tried this initially.

---

_Comment by @github-actions[bot] on 2025-07-28 14:03_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on typing conformance tests
No changes detected when running ty on typing conformance tests ✅


---

_Merged by @ntBre on 2025-07-29 12:25_

---

_Closed by @ntBre on 2025-07-29 12:25_

---

_Branch deleted on 2025-07-29 12:26_

---
