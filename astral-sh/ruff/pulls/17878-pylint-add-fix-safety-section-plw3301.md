```yaml
number: 17878
title: "[`pylint`] add fix safety section (`PLW3301`)"
type: pull_request
state: merged
author: yunchipang
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/fix-safety-nested-min-max
created_at: 2025-05-06T03:34:32Z
updated_at: 2025-05-12T21:34:45Z
url: https://github.com/astral-sh/ruff/pull/17878
synced_at: 2026-01-10T18:51:01Z
```

# [`pylint`] add fix safety section (`PLW3301`)

---

_Pull request opened by @yunchipang on 2025-05-06 03:34_

parent: #15584 
issue: #16163 


---

_Comment by @github-actions[bot] on 2025-05-06 03:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `documentation` added by @AlexWaygood on 2025-05-06 06:41_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:31 on 2025-05-06 19:17_

This isn't really a fix safety issue, this is a bug with an open issue that needs to be fixed. 

I went through the git blame on this one, and I'm not really sure it actually needs to be unsafe. It was first converted from `Fix::unspecified` to `Fix::suggested` in https://github.com/astral-sh/ruff/pull/5251, which became `Fix::sometimes_applies`, which became `Fix::unsafe_edit`. I didn't see any specific discussion of the fix being unsafe, so I think it may have just been the more conservative option.

All that to say, it's not clear to me why this rule would need to be unsafe, at least from its description and development history. We even check for comment ranges and avoid a fix entirely if comments are present, so let's hold off on this for now.

---

_@ntBre reviewed on 2025-05-06 19:17_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:31 on 2025-05-06 20:25_

The `min` fix is unsafe when `__lt__` is not commutative. The `max` fix is unsafe when `__gt__` is not commutative.
```console
$ cat >plw3301.py <<'# EOF'
print(min(2.0, min(float("nan"), 1.0)))
print(max(1.0, max(float("nan"), 2.0)))
# EOF

$ python plw3301.py
2.0
1.0

$ ruff check --isolated --select PLW3301 plw3301.py --unsafe-fixes --fix
Found 2 errors (2 fixed, 0 remaining).

$ python plw3301.py
1.0
2.0
```

---

_@dscorbett reviewed on 2025-05-06 20:25_

---

_@yunchipang reviewed on 2025-05-06 23:45_

---

_Review comment by @yunchipang on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:31 on 2025-05-06 23:45_

@ntBre @dscorbett thanks so much for the input! I've updated the docs, though I'm not exactly sure, referencing the above case, is this rule always or sometimes unsafe? In general, what's makes a rule always/sometimes unsafe?

thanks a lot.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:28 on 2025-05-07 18:24_

```suggestion
/// print(min(2.0, float("nan"), 1.0))      # after fix: 1.0
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:31 on 2025-05-07 18:25_

```suggestion
/// print(max(1.0, float("nan"), 2.0))      # after fix: 2.0
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:25 on 2025-05-07 18:27_

I'd probably phrase this more like

> This fix is always unsafe because it can change the program's behavior in cases where the underlying dunder methods (`__lt__` for `min` and `__gt__` for `max`) are not commutative, as is the case for floats, for example:

---

_@ntBre reviewed on 2025-05-07 18:28_

Thanks, this is looking good.

---

_@ntBre reviewed on 2025-05-07 18:30_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:31 on 2025-05-07 18:30_

Ah great catch as always, @dscorbett! I was hoping you might see this one.

I may have mentioned this on another PR, but the way I check for safety is to search the file for `safe_edit`. If you find cases of `Fix::safe_edit` _and_ `Fix::unsafe_edit`, then the rule is sometimes safe. If you only find `Fix::unsafe_edit`, as I believe is the case here, the rule is always unsafe. You'll have to be slightly more careful with your search range in files with multiple rules, but the general strategy still applies.

---

_@dscorbett reviewed on 2025-05-07 18:55_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:25 on 2025-05-07 18:55_

“Commutative” is the wrong word: I meant “associative”. Sorry for the confusion. Actually, I’m not sure associativity is precisely what is necessary and sufficient here. It’s probably best to avoid the technical jargon unless someone proves it’s correct.

---

_Comment by @yunchipang on 2025-05-07 23:46_

@nefrob @dscorbett thanks for the reviews! I’ve adjusted the wording slightly based on my understanding—hope it’s clearer and aligns with the intent. Let me know if there’s any part that could be further improved!

---

_@ntBre reviewed on 2025-05-09 20:57_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/pylint/rules/nested_min_max.rs`:27 on 2025-05-09 20:57_

```suggestion
/// This fix is always unsafe and may change the program's behavior for
/// types without full equivalence relations, such as float comparisons involving `NaN`.
```

I _think_ associativity is the correct property, but this is another possibility that I borrowed from the Rust [`PartialEq`](https://doc.rust-lang.org/std/cmp/trait.PartialEq.html) docs. I think we could also phrase it as "types without a total ordering" or something. Maybe associative is close enough to stick with that :sweat_smile: 

---

_Review requested from @ntBre by @yunchipang on 2025-05-09 21:35_

---

_@ntBre approved on 2025-05-12 20:34_

Looks good, thanks! I just pushed one small change putting the section back at the end.

---

_Merged by @ntBre on 2025-05-12 20:51_

---

_Closed by @ntBre on 2025-05-12 20:51_

---

_Branch deleted on 2025-05-12 21:34_

---
