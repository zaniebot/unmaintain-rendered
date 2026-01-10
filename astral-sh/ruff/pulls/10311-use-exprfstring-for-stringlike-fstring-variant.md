```yaml
number: 10311
title: "Use `ExprFString` for `StringLike::FString` variant"
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: main
head: dhruv/string-like-f-string
created_at: 2024-03-09T11:29:30Z
updated_at: 2024-03-14T08:01:14Z
url: https://github.com/astral-sh/ruff/pull/10311
synced_at: 2026-01-10T22:47:01Z
```

# Use `ExprFString` for `StringLike::FString` variant

---

_Pull request opened by @dhruvmanila on 2024-03-09 11:29_

## Summary

This PR updates the `StringLike::FString` variant to use `ExprFString` instead of `FStringLiteralElement`.

For context, the reason it used `FStringLiteralElement` is that the node is actually the string part of an f-string ("foo" in `f"foo{x}"`). But, this is inconsistent with other variants where the captured value is the _entire_ string. 

This is also problematic w.r.t. implicitly concatenated strings. Any rules which work with `StringLike::FString` doesn't account for the string part in an implicitly concatenated f-strings. For example, we don't flag confusable character in the first part of `"𝐁ad" f"𝐁ad string"`, but only the second part (https://play.ruff.rs/16071c4c-a1dd-4920-b56f-e2ce2f69c843).

### Update `PYI053`

_This is included in this PR because otherwise it requires a temporary workaround to be compatible with the old logic._

This PR also updates the `PYI053` (`string-or-bytes-too-long`) rule for f-string to consider _all_ the visible characters in a f-string, including the ones which are implicitly concatenated. This is consistent with implicitly concatenated strings and bytes.

For example,

```python
def foo(
	# We count all the characters here
    arg1: str = '51 character ' 'stringgggggggggggggggggggggggggggggggg',
	# But not here because of the `{x}` replacement field which _breaks_ them up into two chunks
    arg2: str = f'51 character {x} stringgggggggggggggggggggggggggggggggggggggggggggg',
) -> None: ...
```

This PR fixes it to consider all _visible_ characters inside an f-string which includes expressions as well.

fixes: #10310 
fixes: #10307 

## Test Plan

Add new test cases and update the snapshots.

## Review

To facilitate the review process, the change have been split into two commits: one which has the code change while the other has the test cases and updated snapshots.


---

_Review requested from @AlexWaygood by @dhruvmanila on 2024-03-09 11:29_

---

_Review requested from @charliermarsh by @dhruvmanila on 2024-03-09 11:29_

---

_@AlexWaygood reviewed on 2024-03-09 11:42_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:1 on 2024-03-09 11:42_

I don't _object_ to the changes here, but I feel like they're more involved than they _need_ to be. There's no reason to ever use f-strings in a `.pyi` file, so I think we could honestly just skip this rule for f-strings if we wanted to.

In the past I've floated lints to ban f-strings in stubs altogether (https://github.com/PyCQA/flake8-pyi/pull/288), but the idea was (very reasonably!) rejected on the grounds that it's not somebody anybody would ever really want to do anyway -- so the proposed lint wouldn't actually have been solving any real-world problem

---

_Comment by @github-actions[bot] on 2024-03-09 11:47_

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

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:1 on 2024-03-13 08:40_

Yeah, I agree that skipping over f-strings would be ok but I think for now we can just improve the logic to take into account all the literal characters in a f-string.

---

_@dhruvmanila reviewed on 2024-03-13 08:40_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/resources/test/fixtures/flake8_bandit/S104.py`:24 on 2024-03-13 08:41_

This is not exactly correct (https://github.com/astral-sh/ruff/issues/10308) but for now it's fine.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_bind_all_interfaces.rs`:71 on 2024-03-13 08:44_

This logic is being repeated for all three rules changed in this PR. The logic being:
* Loop over each f-string part
	* If it's a string literal, then directly run the check on it
	* If it's an f-string, loop over each literal and expression element of it

I'm not exactly sure on how to avoid duplicating this logic or if it's possible without introducing additional complexity.

---

_@dhruvmanila reviewed on 2024-03-13 08:44_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-03-13 08:46_

---

_@AlexWaygood reviewed on 2024-03-13 08:52_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:1 on 2024-03-13 08:52_

The more I think about this, the less I like it in the context of this rule :/

The rule here uses a really dumb heuristic, but one that works surprisingly well. It just says that if a default value in a stub is long enough to fill of most the page, it's probably an implementation detail or has been inserted by a codemod or something — since you never _need_ default values in stubs (they're nice for IDE users, but never essential), you should probably take it out.

50 characters is a fairly arbitrary cutoff here that we use for this rule, but for the purposes of this rule, I'm not sure see I why this string should be counted as 12 characters rather than 24:

```python
x = "one" f"one{expr}one" f"one" f"{expr}"
#    ^^^    ^^^      ^^^    ^^^
```

For the purposes of this rule, I think the characters in the expression parts should count just as much towards the total length as the characters in the literal parts.

Or we could just skip over f-strings for this rule :-)

---

_@dhruvmanila reviewed on 2024-03-13 09:09_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:1 on 2024-03-13 09:09_

My main goal with this PR is to update the `StringLike::FString` variant to use `ExprFString` instead of `FStringLiteralElement` which unlocks the following PR (#10312). As a result of that change, I'm required to update the rules which uses this variant to make sure it takes the updated value into account.

Now, for the other two rules it was straightforward. But for this rule, I found out that the fix was incorrect and it was marked as safe. This is what prompt me to update it so that it considers all literal characters.

> For the purposes of this rule, I think the characters in the expression parts should count just as much towards the total length as the characters in the literal parts.

I think the reason I excluded expressions is because they could evaluate to any arbitrary amount of characters but for literal parts we're sure of the number.

That said I don't really have any strong opinions regarding this rule as it's not the main purpose of this PR. Sorry if this wasn't clear.

---

_@AlexWaygood reviewed on 2024-03-13 09:18_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:1 on 2024-03-13 09:18_

> I think the reason I excluded expressions is because they could evaluate to any arbitrary amount of characters but for literal parts we're sure of the number.

For this specific rule, I think we could just be dumb about it, and not worry about how many characters the expressions in the f-string would evaluate to at runtime. We can just count the literal characters as they appear on the page, since that's what the rule is really about anyway.

But I continue to think that the simplest way of fixing the incorrect behaviour you pointed out in https://github.com/astral-sh/ruff/issues/10307 would be to just skip over f-strings for this rule 😄

> That said I don't really have any strong opinions regarding this rule as it's not the main purpose of this PR. Sorry if this wasn't clear.

Fully understood — I can open a separate PR to skip over f-strings in this rule, if that's easier?

---

_@dhruvmanila reviewed on 2024-03-13 09:30_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:1 on 2024-03-13 09:30_

> But I continue to think that the simplest way of fixing the incorrect behaviour you pointed out in #10307 would be to just skip over f-strings for this rule 😄

Maybe just skip if it contains expressions? So that for something like `f"aaaaaaaa"` we still raise the violation. This change will only be possible _after_ this PR as it requires looking at the `ExprFString` instead of just the `FStringLiteralElement`. What do you think?

> Fully understood — I can open a separate PR to skip over f-strings in this rule, if that's easier?

I don't mind making the change in this PR itself.

---

_@AlexWaygood reviewed on 2024-03-13 11:02_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:1 on 2024-03-13 11:02_

> Maybe just skip if it contains expressions?

Hmm, not really a fan... I think we should either _always_ skip over f-strings for this rule, or count the characters according to how they literally appear on the page. I've never seen anybody use an f-string in a stub, but if they did, I think it would be really confusing for users if this triggered a violation:

```py
x: str = f'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
```

but this didn't:

```py
x: str = f'xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx{}'
```

given that the latter obviously takes up more space on the page!

---

_@charliermarsh reviewed on 2024-03-14 04:10_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/flake8_pyi/rules/string_or_bytes_too_long.rs`:1 on 2024-03-14 04:10_

I agree that "raw characters, including expressions" makes more sense but I don't feel strongly. I think either approach is fine, and it's just very unlikely to make a difference in practice.

If it were me, I might do: if the f-string contains an expression, _always_ flag it; if not, count the characters.

---

_@charliermarsh approved on 2024-03-14 04:10_

---

_@AlexWaygood approved on 2024-03-14 07:57_

the flake8-pyi changes LGTM now, thanks!

---

_Label `internal` added by @dhruvmanila on 2024-03-14 07:58_

---

_Merged by @dhruvmanila on 2024-03-14 08:00_

---

_Closed by @dhruvmanila on 2024-03-14 08:00_

---

_Branch deleted on 2024-03-14 08:00_

---
