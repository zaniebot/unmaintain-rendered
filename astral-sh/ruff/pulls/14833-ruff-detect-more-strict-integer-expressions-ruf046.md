```yaml
number: 14833
title: "[`ruff`] Detect more strict-integer expressions (`RUF046`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF046-2
created_at: 2024-12-08T04:18:59Z
updated_at: 2024-12-21T19:00:11Z
url: https://github.com/astral-sh/ruff/pull/14833
synced_at: 2026-01-12T15:55:49Z
```

# [`ruff`] Detect more strict-integer expressions (`RUF046`)

---

_@InSyncWithFoo_

## Summary

Part 2 of the big change introduced in #14828 and the follow-up to #14832.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Renamed from "[ruff] Do not simplify round() calls (RUF046)" to "[`ruff`] Detect more strict-integer expressions (`RUF046`)" by @InSyncWithFoo on 2024-12-08 04:20_

---

_Comment by @github-actions[bot] on 2024-12-08 04:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 54 projects unchanged)

<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/rotki/rotki/blob/da76cb8e347543bb2f539a6731c8666bc382eacb/rotkehlchen/tests/utils/ethereum.py#L231'>rotkehlchen/tests/utils/ethereum.py:231:15:</a> RUF046 [*] Value being casted is already an integer
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF046 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>




---

_Comment by @MichaReiser on 2024-12-09 08:48_

Hmm, I can't change the base branch if the target is from a fork. I'll wait reviewing this PR until the target is merged. 

---

_Comment by @MichaReiser on 2024-12-09 08:52_

Would you mind writing a short summary of what you're changing in the PR description? It's hard to review code if the change is unclear. 

Did you review through the ecosystem changes? It's unclear to me why we're no longer flagging 

https://github.com/RasaHQ/rasa/blob/b8de3b231126747ff74b2782cb25cb22d2d898d7/rasa/shared/core/training_data/loading.py#L86


---

_Comment by @InSyncWithFoo on 2024-12-09 12:40_

@MichaReiser These changes are all expected, even if they don't seem correct at first sight. For example:

```py
#                                                   vvvvvvvvvvvvv Not `int`.
def load_data_from_files(..., exclusion_percentage: Optional[int] = None) -> ...:
    ...
#                   vvvvvvvvvvvvvvvvvvvvvvvvvvvv This is classified as "Maybe" rather than "Likely".
    idx = int(round(exclusion_percentage / 100.0 * len(story_steps)))
#                   \______This thus might also not be `int`______/
```

However, I also agree that this is suboptimal. `round()` with no `ndigits` is indeed likely to produce an `int`. In any case, since #14832 and this have diverged, I can only work on improving this after the former is merged.

---

_Closed by @InSyncWithFoo on 2024-12-11 19:12_

---

_Branch deleted on 2024-12-11 19:12_

---

_Branch restored on 2024-12-11 19:14_

---

_Reopened by @InSyncWithFoo on 2024-12-11 19:14_

---

_@MichaReiser requested changes on 2024-12-12 09:01_

Would you mind updating your PR with a short summary outlining the change? It's otherwise very difficult to review the PR without even knowing what the intended behavior is.

---

_Comment by @InSyncWithFoo on 2024-12-12 13:25_

The goal of this PR is to correctly detect `1 & 1` and other kinds of expressions to be strict instances of `int`. Other cases can be found in `RUF046.py`.

To give an example, `int(len(foo) // 2)` would not be detected as a violation prior to this PR. It now is. Existing violations are also preserved.

There are, however, limitations. `foo is not bar` is classified as `IsStrictlyInt::False`, resulting in `1 + (foo is not bar)` being `IsStrictlyInt::Maybe`, even though `1 + True` is `2`.

---

_Comment by @MichaReiser on 2024-12-12 13:35_

Thanks for the extra explanation. That's helpful. 

I don't think the pattern is common enough to justify the increase in complexity. Unless we move some of the logic into `typing::is_int` where it can be used for other rules as well. 

---

_Comment by @InSyncWithFoo on 2024-12-12 14:05_

@MichaReiser On the other hand, it is:

* [`\bint\(\s*\d+\s*(?:[-+*^&|]|//)\s*\d+\s*\)`](https://sourcegraph.com/search?q=context:global+lang:Python+%5Cbint%5C%28%5Cs*%5Cd%2B%5Cs*%28%3F:%5B-%2B*%5E%26%7C%5D%7C//%29%5Cs*%5Cd%2B%5Cs*%5C%29&patternType=regexp&case=yes&sm=0): 500+ results
* [`\bint\(\s*\d+\s*\)`](https://sourcegraph.com/search?q=context:global+lang:Python+%5Cbint%5C%28%5Cs*%5Cd%2B%5Cs*%5C%29&patternType=regexp&case=yes&sm=0): 10000+ results

That's without function calls, variables and such.

There's also [`\bint\(\s*\d+\s*/\s*\d+\s*\)`](https://sourcegraph.com/search?q=context:global+lang:Python+%5Cbint%5C%28%5Cs*%5Cd%2B%5Cs*/%5Cs*%5Cd%2B%5Cs*%5C%29&patternType=regexp&case=yes&sm=0) with ~900 results, but I suppose that should be another rule.

---

_Comment by @MichaReiser on 2024-12-13 08:05_

I'm not suggesting that we should not improve the rule at all, but I'm wary of making rules unnecessarily complex by implementing too much ad hoc type inference. It results in inconsistent behavior between rules and makes Ruff harder to maintain. 

Can we focus on the most important cases you outlined above: single numbers, mathematical expressions, and a few known function calls? It also makes me wonder if we could move some or most of the logic into `typing::is_int` (we can only move the "strict" part). The logic would then benefit all rules instead of just a few.  

---

_Comment by @InSyncWithFoo on 2024-12-14 06:19_

> Can we focus on the most important cases you outlined above: single numbers, mathematical expressions, and a few known function calls?

But I'm already focused on such expressions. All three kinds you mention, yes. `expr_is_strictly_int()` just recurses into subexpressions, nothing more. I really don't think I can simplify it any further (save for the `Expr::Compare` branch?).

`typing::is_int()` attempts to infer the type of a binding, not an arbitrary expression. I could move `expr_is_strictly_int()` to somewhere inside `typing`, but that's about it.

---

_Comment by @MichaReiser on 2024-12-15 16:35_

A `is_int_expr` next to `is_int` that returns `true` for expressions that are guaranteed to be `int` seems like a good outcome. What do you think? The maybe-int cases can then be handled inside the rule itself. 

---

_Comment by @InSyncWithFoo on 2024-12-15 17:40_

`expr_is_strictly_int()` is now located in `typing.rs` with its name changed to `is_strictly_int_expr`. I'm not sure if that changes anything, though.

---

_Comment by @MichaReiser on 2024-12-19 12:49_

@AlexWaygood made me aware that we already have [ResolvedPythonType](https://github.com/astral-sh/ruff/blob/8d9e408dbb0f1876e3b56c8b4e2adb93258fb713/crates/ruff_python_semantic/src/analyze/type_inference.rs#L9) which implements a very similar infrastructure. I think we should integrate your changes there **but** without the `True`, `Maybe` distinction. Instead, it only infers `Int` if the type is guaranteed to be an int and it returns `Unknown` otherwise. 


My suggestion for **this** PR is to simply use `ResolvedPythonType` without changing its implementation. We can improve `ResolvedPythonType` if needed in a separate PR

That means that the handling of `round` must remain in this rule.

---

_Comment by @InSyncWithFoo on 2024-12-19 18:19_

@MichaReiser I'm not sure if I follow. How would I detect `int(round(...) * 42)` with `ResolvedPythonType`? As I see, it doesn't handle calls, which means `round(...) * 42` would be resolved to `Unknown`. Or do you mean we should not fix such cases for now?

---

_Comment by @MichaReiser on 2024-12-20 07:58_

@InSyncWithFoo 

We shouldn't fix those cases for now. We can handle `round` calls directly in `RUF046` and anything more involved using `round` won't be detected because `ResolvedPythonType` doesn't support it.

---

_Review requested from @MichaReiser by @InSyncWithFoo on 2024-12-20 23:07_

---

_Label `rule` added by @MichaReiser on 2024-12-21 14:19_

---

_Label `preview` added by @MichaReiser on 2024-12-21 14:19_

---

_Comment by @MichaReiser on 2024-12-21 14:19_

Perfect, thank you! I think this is a great improvement to the rule without significantly increasing its complexity. 

---

_@MichaReiser approved on 2024-12-21 14:20_

---

_Merged by @MichaReiser on 2024-12-21 14:23_

---

_Closed by @MichaReiser on 2024-12-21 14:23_

---

_Branch deleted on 2024-12-21 18:58_

---

_Comment by @InSyncWithFoo on 2024-12-21 19:00_

@MichaReiser Why was the file renamed to `round_applicability`? That's not the name of the rule.

---
