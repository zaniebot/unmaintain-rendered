```yaml
number: 14828
title: "[`ruff`] Unnecessary rounding (`RUF057`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF057
created_at: 2024-12-07T00:34:15Z
updated_at: 2025-01-02T14:05:50Z
url: https://github.com/astral-sh/ruff/pull/14828
synced_at: 2026-01-12T15:55:49Z
```

# [`ruff`] Unnecessary rounding (`RUF057`)

---

_@InSyncWithFoo_

## Summary

Resolves #14793.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-12-07 00:39_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-12-07 18:20_

Thanks for following up on this! 

Would you mind splitting the PR into two if it isn't too much work:

1. Changes the existing rule
2. Adding the new rule

Two separate PRs have the advantage that we can list the two changes separately in the changelog. It also makes it easier for me to review your PR

---

_Comment by @InSyncWithFoo on 2024-12-07 18:31_

@MichaReiser I would prefer not having to do that. `RUF057` depends on a `RUF046` API (`expr_is_strictly_int()`), which itself comprises most of the change to `RUF046`.

---

_Comment by @MichaReiser on 2024-12-07 18:36_

Yeah, that's a bit annoying. However, I just skimmed over the changes, and there's too much going on in this PR, which makes it difficult for me and future readers to understand what it is about. That's why I have to insist that you split the PR. I suggest three PRs:

1. Scope change of `RUF046`
2.. Improving the integer inference of `RUF046`
3. Adding the new `RUF057`

I'd suggest you start with one PR and then submit the next once that is merged. Or you can create stacked PRs, but they can be a bit painful when it comes to rebasing them.

---

_Renamed from "[`ruff`] Detect more strict-integer expressions and move `round()` logic to new rule (`RUF046`, `RUF057`)" to "[`ruff`] Unnecessary ndigits argument to `round()` (`RUF057`)" by @InSyncWithFoo on 2024-12-08 04:21_

---

_Renamed from "[`ruff`] Unnecessary ndigits argument to `round()` (`RUF057`)" to "[`ruff`] Unnecessary `ndigits` argument to `round()` (`RUF057`)" by @InSyncWithFoo on 2024-12-08 04:21_

---

_Comment by @InSyncWithFoo on 2024-12-08 04:22_

All done. I don't mind a little bit of rebasing, so stacked PRs it is.

---

_Comment by @MichaReiser on 2024-12-08 17:47_

Thank you.

Hmm. This is not the rule I expected. From our conversation in #14793 I'd expected a rule that detects unnecessary `round` calls rather than unnecessary `ndigits`. 

E.g. `round(5)` should be converted to just `5`. The same for `round(5, None)`

---

_Renamed from "[`ruff`] Unnecessary `ndigits` argument to `round()` (`RUF057`)" to "[`ruff`] Unnecessary rounding (`RUF057`)" by @InSyncWithFoo on 2024-12-25 21:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_rounding.rs`:36 on 2024-12-26 09:46_

```suggestion
        "Remove unnecessary `round` call".to_string()
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_rounding.rs`:90 on 2024-12-26 09:49_

Can you tell me more about the motivation for distinguishing between `ResolvedInt` and `InferredInt`? I think this is also worth documenting because it's not obvious to readers why the distinction is necessary. 

It might be better to have more semantic names rather than the source  because both `typing::int` and `ResolvedPythonType` are doing type inference, which makes `InferredInt` rather confusing

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_rounding.rs`:13 on 2024-12-26 09:52_

Rounding a value that's already an integer is unnecessary. It's more clear to use the value directly.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_rounding.rs`:27 on 2024-12-26 09:53_

I'm leaning towards naming the method `UnnecessaryRound` 

---

_@MichaReiser reviewed on 2024-12-26 09:53_

---

_Label `rule` added by @MichaReiser on 2024-12-26 09:53_

---

_Label `preview` added by @MichaReiser on 2024-12-26 09:53_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_rounding.rs`:90 on 2024-12-26 10:42_

The inference results of `ResolvedPythonType` is much more reliable than that of `InferredInt`, so `ResolvedInt` leads to `Safe` applicability while that of `InferredInt` is `Unsafe`. In hindsight, `ResolvedInt` should probably be merged with `LiteralInt`.

---

_@InSyncWithFoo reviewed on 2024-12-26 10:42_

---

_@MichaReiser reviewed on 2024-12-28 09:18_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_rounding.rs`:90 on 2024-12-28 09:18_

As a reader, I still wonder what the difference between `InferredInt` and `ResolvedInt` is. I think we should rename the variants or add documentation (the best option is probably to do both). 

Can you share an example where `InferredInt` returns `true` for something that shouldn't be an `int`? Is it because booleans are assignable to `int`: 

E.g. this is fine according to pyright

```python
def test(a: int) -> int:
    return a

test(True)  # Returns `True`
```

But `round(True)` evaluates to `1`

```
>>> round(True)
1
```

Are there more cases like this? If not, then how about renaming the `InferredInt` variant to `AnnotatedInt` or `Int` vs `LiteralInt` with a comment saying that one is equivalent to an `:int` annotation and the other `Literal[int]`?


---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_rounding.rs`:90 on 2024-12-28 11:25_

`LiteralInt` has the false implication that the argument was given as a literal in the source code, even though it might very well be a complex expression. `AnnotatedInt` has the false implication that the variable was annotated, but `typing::is_int()` returns `true` even for names that are only bound to an assignment whose value is an integer literal:

```py
a = 1
_ = round(a)
#   ^^^^^^^^ RUF057
```

That we also need to account for subclasses of `int` is the reason to make the fix for `InferredInt` unsafe, yes.

---

_@InSyncWithFoo reviewed on 2024-12-28 11:25_

---

_@MichaReiser reviewed on 2024-12-30 10:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_rounding.rs`:90 on 2024-12-30 10:08_

Thanks for adding some documentation. I did read it, but I'm not sure it's very helpful because it only documents what's already apparent from the code. What's unclear to a reader is why the distinction is important. The names are also somewhat confusing, as you mentioned in your last comment. For example, `LiteralInt` doesn't mean that it's a `Literal`; it could also be a complex expression. 

I still only have a limited understanding but my impression is that the variants for `RoundKind` are all about *how likely the expression is an `int`*. Or possibly whether the value is equivalent to `int` or only assignable to int. How about we introduce a new enum to `InferedType` with the variants `Equivalent` (it's exactly `int`), and `AssignableTo` (5 or `True`). `RoundKind` then has the variants `Int(InferredType)`, `Float(InferredType)`, and `Other` and `NdigitsKind` has `Absent`, `LiteralNone`, `Int(InferredType)`, and `Other`. 

We can then add documentation to where we construct the `InferredType` why e.g. `typing::is_int` is `InferredType::AssignableTo` and why `ResolvedPythonType` is `Equivalent`



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:59 on 2024-12-31 15:51_

Some examples will help future readers

```suggestion
				// ```python
        // round(3)
        // round(3, 0)
        // round(3, ..)
        // ```
        (RoundedKind::Int(InferredType::Equivalent), _) => Applicability::Safe,
```

It would also be great if we can add examples to the other branches

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:64 on 2024-12-31 15:53_

It's not clear to me why the round call should be unnecessary if the value is a float:

```
>>> round(3.0)
3
>>> 3.0
3.0
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:71 on 2024-12-31 15:54_

Same here, i don't think we can ever remove round calls if the value is a `float`. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:122 on 2024-12-31 15:55_

```suggestion
    let rounded = arguments.find_argument_value("number", 0)?;
    let ndigits = arguments.find_argument_value("ndigits", 1);
```

---

_@MichaReiser reviewed on 2024-12-31 15:56_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:94 on 2024-12-31 15:57_

Nit: Rename to `RoundedValue`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:102 on 2024-12-31 15:57_

Rename to `NdigitsValue`. Value is somewhat more specific than `Kind`

---

_@MichaReiser reviewed on 2024-12-31 15:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:14 on 2024-12-31 15:58_

```suggestion
/// It's clearer to use the value directly.
```

---

_@MichaReiser reviewed on 2024-12-31 15:58_

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF057.py`:11 on 2025-01-01 11:07_

Should we make this an error too if the value is known to be equivalent to an int? The only semantic change that I could see is that `round` no longer throws if `foo` isn't an int. But we could make the fix unsafe to account for this.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_round.rs`:118 on 2025-01-01 11:19_

I removed the type alias because it wasn't used consistently and I found it more confusing than helpful (I had to lookup what its type is). In this case, I think adding some documentation might be more helpful

---

_@MichaReiser approved on 2025-01-01 11:25_

Thanks and happy new year. This is looking great. I've one last comment on where I think the rule should flag a `round` call but currently isn't. I'm interested in why you chose to not flag that `round` call site. 

---

_@InSyncWithFoo reviewed on 2025-01-01 11:49_

---

_Review comment by @InSyncWithFoo on `crates/ruff_linter/resources/test/fixtures/ruff/RUF057.py`:11 on 2025-01-01 11:49_

(Happy new year to you too.) Done.

---

_Merged by @MichaReiser on 2025-01-02 09:00_

---

_Closed by @MichaReiser on 2025-01-02 09:00_

---

_Branch deleted on 2025-01-02 14:05_

---
