```yaml
number: 14408
title: "[`flake8-bugbear`] `itertools.batched()` without explicit `strict` (`B911`)"
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
created_at: 2024-11-18T00:33:21Z
updated_at: 2024-12-11T13:04:35Z
url: https://github.com/astral-sh/ruff/pull/14408
synced_at: 2026-01-12T15:55:47Z
```

# [`flake8-bugbear`] `itertools.batched()` without explicit `strict` (`B911`)

---

_@InSyncWithFoo_

## Summary

Resolves #14387.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-18 00:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Label `rule` added by @dhruvmanila on 2024-11-18 06:01_

---

_Label `preview` added by @dhruvmanila on 2024-11-18 06:01_

---

_Comment by @MichaReiser on 2024-11-18 07:26_

I think I prefer to wait with adding this rule and see if it gets accepted upstream to avoid having to re-code in a few days/weeks. See https://github.com/PyCQA/flake8-bugbear/issues/498

---

_Label `needs-decision` added by @MichaReiser on 2024-11-19 07:42_

---

_Closed by @InSyncWithFoo on 2024-11-20 03:45_

---

_Branch deleted on 2024-11-20 03:45_

---

_Branch restored on 2024-11-20 12:28_

---

_Reopened by @InSyncWithFoo on 2024-11-20 12:28_

---

_Label `needs-decision` removed by @MichaReiser on 2024-12-08 17:38_

---

_Renamed from "[`ruff`] `itertools.batched()` without explicit `strict` (`RUF054`)" to "[`flake8-bugbear`] `itertools.batched()` without explicit `strict` (`B911`)" by @InSyncWithFoo on 2024-12-08 18:17_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:33 on 2024-12-09 08:40_

I think we should not provide a fix if the call already has a `kwarg` argument or mark it as a manual fix. I'm even leaning towards not raising the diagnostic in that case because we can't know if `strict` is set or not. What do you think? (@AlexWaygood ?)

The fix should be unsafe nonetheless because adding `strict` changes the runtime semantic and the right fix might be to add `strict=False` after all. 



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:67 on 2024-12-09 08:42_

I understand that this is a deviation from the upstream rule. We should document it

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:63 on 2024-12-09 08:43_

It seems we need to be careful around starred expressions

```suggestion
    let first_positional = arguments.find_positional(0);
```

---

_@MichaReiser approved on 2024-12-09 08:45_

This is great. 

We have to decide on the fix safety and polish the documentation a bit.

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-12-09 15:53_

---

_Comment by @MichaReiser on 2024-12-09 15:54_

@AlexWaygood requesting review from you regarding the open question around fix safety (and whether we should raise a diagnostic). I already reviewed the code changes

---

_@AlexWaygood reviewed on 2024-12-09 16:49_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:33 on 2024-12-09 16:49_

I strongly agree that we should always mark the fix as unsafe; `strict=True` might not be appropriate for all applications (https://github.com/PyCQA/flake8-bugbear/issues/498#issuecomment-2481323632).

I also agree that we shouldn't offer a fix (or it should be display-only) if there's `kwargs` since there's this risk:

```pycon
>>> kwargs = {"strict": True}
>>> import itertools
>>> itertools.batched([], strict=True, **kwargs)
Traceback (most recent call last):
  File "<python-input-2>", line 1, in <module>
    itertools.batched([], strict=True, **kwargs)
    ~~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^
TypeError: itertools.batched() got multiple values for keyword argument 'strict'
```

However, I'd lean towards still emitting the diagnostic if there's kwargs. In most cases, they can probably explicitly pass in `strict=True` or `strict=False`. In the very rare situations where they really need dynamic keyword arguments, I think they can just `# noqa` the rule.

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2024-12-09 16:49_

---

_@InSyncWithFoo reviewed on 2024-12-09 17:10_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:33 on 2024-12-09 17:10_

Just to be clear, the fix should be marked as unsafe even if it doesn't change runtime behaviour? Currently, it just adds `strict=False`, but `False` is already the default value.

---

_@AlexWaygood reviewed on 2024-12-09 17:14_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:33 on 2024-12-09 17:14_

Oh, the fix just adds `strict=False`? I'm not sure that's a very useful autofix TBH. Maybe we should just not offer a fix for this rule? The point of the rule is that you should think about your `itertools.batched()` calls and be explicit about whether or not you want the strict behaviour. But you can only tell which is appropriate by doing a manual audit. If Ruff auto-adds `strict=False` everywhere, you haven't really gained anything from the rule

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:33 on 2024-12-09 17:15_

(Sorry for not checking what the fix actually did...)

---

_@AlexWaygood reviewed on 2024-12-09 17:15_

---

_@InSyncWithFoo reviewed on 2024-12-09 17:18_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:33 on 2024-12-09 17:18_

I think I copied this from [`B905`](https://docs.astral.sh/ruff/rules/zip-without-explicit-strict/#fix-safety). Its fix is safe by default and only unsafe when `**kwargs` is present.

---

_@AlexWaygood reviewed on 2024-12-09 17:19_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:33 on 2024-12-09 17:19_

I don't really find `B905`'s fix particularly useful either, so I'm not sure it's a great precedent to follow 😆 but interested to hear @MichaReiser's thoughts.

---

_@MichaReiser reviewed on 2024-12-10 08:33_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_bugbear/rules/batched_without_explicit_strict.rs`:33 on 2024-12-10 08:33_

I agree that the fix has limited use here and it somewhat defeats the purpose of: Think about what you want. 

Because of that, I went ahead and removed it. It simplifies the implementation

---

_Merged by @MichaReiser on 2024-12-10 08:39_

---

_Closed by @MichaReiser on 2024-12-10 08:39_

---

_Branch deleted on 2024-12-11 13:04_

---
