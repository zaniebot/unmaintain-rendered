```yaml
number: 20588
title: "[`refurb`] Preserve digit separators in `Decimal` constructor (`FURB157`)"
type: pull_request
state: merged
author: danparizher
labels:
  - rule
  - fixes
assignees: []
merged: true
base: main
head: fix-20572
created_at: 2025-09-26T03:11:46Z
updated_at: 2025-10-28T21:55:11Z
url: https://github.com/astral-sh/ruff/pull/20588
synced_at: 2026-01-12T15:57:05Z
```

# [`refurb`] Preserve digit separators in `Decimal` constructor (`FURB157`)

---

_@danparizher_

## Summary

Fixes #20572


---

_Comment by @github-actions[bot] on 2025-09-26 03:20_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@amyreese approved on 2025-09-30 19:51_

---

_Review requested from @ntBre by @amyreese on 2025-09-30 19:52_

---

_Label `rule` added by @amyreese on 2025-09-30 19:53_

---

_Label `fixes` added by @amyreese on 2025-09-30 19:53_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:139 on 2025-09-30 20:11_

I looked through some of the previous issues with this rule, and this check is not sufficient: 
- https://github.com/astral-sh/ruff/issues/13807
- https://github.com/astral-sh/ruff/issues/14204
- https://github.com/astral-sh/ruff/issues/14587

The first of these, especially [this comment](https://github.com/astral-sh/ruff/issues/13807#issuecomment-2422680644), notes a couple of cases where `has_digit_separators` would lead to an invalid `original_str`. This replacement in the snapshots below is an example of that: `Decimal(__1____000) `:

```pycon
>>> from decimal import Decimal
>>> Decimal(__1____000)
Traceback (most recent call last):
  File "<python-input-5>", line 1, in <module>
    Decimal(__1____000)
            ^^^^^^^^^^
NameError: name '__1____000' is not defined
>>> Decimal("__1____000")
Decimal('1000')
```

---

_@ntBre requested changes on 2025-09-30 20:13_

Thanks! But I think we need to be more careful here. At least one of the new snapshots is incorrect.

---

_Review requested from @ntBre by @danparizher on 2025-09-30 23:50_

---

_@amyreese reviewed on 2025-10-01 01:30_

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:138 on 2025-10-01 01:30_

Would it be better to just strip leading underscores, and otherwise preserve the placement and frequency, rather than assume that thousands are the only use of underscores in numeric literals?

---

_@ntBre reviewed on 2025-10-01 01:35_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:138 on 2025-10-01 01:35_

Leading underscores aren't the only problem. Duplicates in the middle cause a syntax error too, but not in the `Decimal` constructor:

```pycon
>>> from decimal import Decimal
>>> 1__2
  File "<python-input-2>", line 1
    1__2
     ^
SyntaxError: invalid decimal literal
>>> Decimal("1__2")
Decimal('12')
```

But I agree in principle, `add_thousand_separators` does not seem like the right approach to me.

I'm starting to think we should just mark the fix as unsafe if the value we're replacing the original with is different. The edge cases here seem pretty tricky.

---

_@danparizher reviewed on 2025-10-01 01:47_

---

_Review comment by @danparizher on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:138 on 2025-10-01 01:47_

Thanks for catching that; I agree that marking it unsafe is probably the way to go to not break too much.

---

_@m-aciek reviewed on 2025-10-01 09:32_

---

_Review comment by @m-aciek on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:138 on 2025-10-01 09:32_

Wouldn't stripping leading underscores and replacing duplicates with a single underscore be enough?

In my opinion keeping `Decimal("10_000_000")` -> `Decimal(10_000_000)` transition as safe would be very convenient.

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:138 on 2025-10-01 12:32_

It would be enough to strip leading underscores, strip trailing underscores, strip leading zeros (unless all the digits are zeros), and collapse medial underscore sequences: `Decimal(" _-_0_01__2_ ")` would become `Decimal(-1_2)`.

---

_@dscorbett reviewed on 2025-10-01 12:32_

---

_@m-aciek reviewed on 2025-10-01 17:00_

---

_Review comment by @m-aciek on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:138 on 2025-10-01 17:00_

That's the behavior I fight for ❤️ 🚀 :)

---

_@ntBre reviewed on 2025-10-01 17:02_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:138 on 2025-10-01 17:02_

That sounds reasonable to me, I gave up a bit too soon :) @danparizher do you want to give this a shot? We can still revert to marking the fix unsafe if the code gets too complicated, but this sounds feasible with the transformations we're already doing.

---

_@amyreese approved on 2025-10-01 23:42_

lgtm

---

_Comment by @dscorbett on 2025-10-02 01:34_

The current state of this PR is that underscores are normalized to be thousands separators, but cf. @amyreese’s [earlier comment](https://github.com/astral-sh/ruff/pull/20588#discussion_r2393221709). If a user writes `Decimal("1__23_45_000")` that formatting was probably intentional so it should become `Decimal(1_23_45_000)`. I think normalizing the underscores more than the minimum needed to avoid a syntax error should be left for a different rule as in #18221.

---

_Comment by @MichaReiser on 2025-10-21 07:27_

What's the status of this PR? Is there something left that needs addressing or is it good to go?

---

_Comment by @ntBre on 2025-10-21 13:06_

I don't think we should normalize the digit separators to thousands separators, as @dscorbett pointed out.

---

_Review comment by @amyreese on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:251 on 2025-10-24 19:56_

I think this ends up being overly verbose and defensive, and lines 225-251 could be replaced with just:

```
let result = trimmed
    .chars()
    .dedup_by(|a, b| {*a == '_' && a == b})
    .collect::<String>();
```

Leading/trailing underscores were already trimmed by line 218, so there shouldn't be a need to both check that there are some number of digits *and* the result after trimming/deduplication is not empty.

---

_Review comment by @amyreese on `crates/ruff_linter/resources/test/fixtures/refurb/FURB157.py`:78 on 2025-10-24 19:57_

Would be nice to include test cases showing that non-thousands separators are maintained accordingly, eg `"12_34_56_78"` and `"1234_5678"`.

---

_@amyreese reviewed on 2025-10-24 19:59_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:159 on 2025-10-24 20:45_

We should use `Fix::applicable_edit` here.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:159 on 2025-10-24 20:47_

Or wait, if we're going to preserve the original formatting, should it just be safe again?

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:209 on 2025-10-24 20:48_

We should either remove `_numeric_part` if it's unused or rename it to `numeric_part`, if it's used.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:231 on 2025-10-24 20:51_

These `is_empty` checks don't seem right to me at all. I don't think we should return an arbitrary value for invalid input, and I think we've already validated the input in `verbose_decimal_constructor`, but I could also be missing something.

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/refurb/rules/verbose_decimal_constructor.rs`:215 on 2025-10-24 20:52_

We already stripped the prefix once on line 99. Can we reuse more of the work from the parent function?

---

_@ntBre reviewed on 2025-10-24 20:53_

---

_Review requested from @ntBre by @danparizher on 2025-10-24 22:22_

---

_@ntBre approved on 2025-10-28 20:31_

I pushed a few commits simplifying things slightly to reuse the earlier unary prefix removal, but this looks good to me.

---

_Comment by @ntBre on 2025-10-28 21:35_

After a bit more thought, I came up with some additional edge cases with separators and leading zeros. Hopefully everything is covered now.

---

_Merged by @ntBre on 2025-10-28 21:47_

---

_Closed by @ntBre on 2025-10-28 21:47_

---

_Branch deleted on 2025-10-28 21:55_

---
