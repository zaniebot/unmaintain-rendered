```yaml
number: 14024
title: "[red-knot] Remove `Type::None`"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/remove-type-none
created_at: 2024-10-31T20:09:33Z
updated_at: 2024-11-04T13:00:08Z
url: https://github.com/astral-sh/ruff/pull/14024
synced_at: 2026-01-10T20:50:57Z
```

# [red-knot] Remove `Type::None`

---

_Pull request opened by @sharkdp on 2024-10-31 20:09_

## Summary

Removes `Type::None` in favor of `KnownClass::NoneType.to_instance(…)`.

closes astral-sh/ruff#13670

## Test Plan

Existing tests pass.

---

_Label `red-knot` added by @sharkdp on 2024-10-31 20:09_

---

_Review requested from @carljm by @sharkdp on 2024-10-31 20:09_

---

_Review requested from @MichaReiser by @sharkdp on 2024-10-31 20:09_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-10-31 20:09_

---

_Comment by @codspeed-hq[bot] on 2024-10-31 20:21_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/david/remove-type-none)

### Merging astral-sh/ruff#14024 will **degrade performances by 4.32%**

<sub>Comparing <code>david/remove-type-none</code> (c7d4bdb) with <code>main</code> (e302c2d)</sub>



### Summary

`❌ 1 (👁 1)` regressions
`✅ 31` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `david/remove-type-none` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| 👁 | `red_knot_check_file[incremental]` | 4.4 ms | 4.6 ms | -4.32% |


---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:641 on 2024-10-31 20:28_

In the long term, we should be able to remove these branches once we handle this TODO:

https://github.com/astral-sh/ruff/blob/b372fe719814c2178178e7e6ffef9ee52f528f7a/crates/red_knot_python_semantic/src/types.rs#L687-L695

Could you add a TODO comment here to that effect?

---

_Comment by @github-actions[bot] on 2024-10-31 20:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types/infer.rs`:3682 on 2024-10-31 20:30_

This is identical to the branch immediately below it

```suggestion
```

---

_@AlexWaygood reviewed on 2024-10-31 20:31_

---

_@AlexWaygood reviewed on 2024-10-31 20:32_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/types.rs`:530 on 2024-10-31 20:32_

We should be able to remove this branch too once we understand that `NoneType` is a subclass of `object` (my MRO PR is coming soon!!)

---

_@sharkdp reviewed on 2024-10-31 20:42_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types/infer.rs`:3682 on 2024-10-31 20:42_

Oh. Forgot that I had planned ahead!

---

_@sharkdp reviewed on 2024-10-31 20:47_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/types.rs`:530 on 2024-10-31 20:47_

Note that this is about `NoneType <: NoneType`. The `X <: object` case is handled further above.

I'm actually surprised that `NoneType <: NoneType` doesn't work without this. We have a `other == self` check in `is_subclass_of` on the `ClassType`s. Is that not working correctly? Is it actually checking for equal classes or for some other identity. Like just comparing salsa IDs?

> my MRO PR is coming soon!!

That will definitely help us remove some cases!

---

_Review comment by @carljm on `crates/red_knot_python_semantic/src/types.rs`:530 on 2024-10-31 21:06_

`self == other` is indeed just checking salsa IDs. But Salsa IDs are assigned by Salsa interning, which interns based on struct equality, so that should be the same thing as an `Eq` check.

It would be good to understand what's happening here. Let me know if you'd like another pair of eyes on it. Maybe it's related to the fact that there are multiple `NoneType` in typeshed? (The one in `_typeshed` and the one in `types` for more recent Python versions.)

Also, I think this branch should go into `is_equivalent_to` instead of `is_subtype_of`.

---

_@carljm approved on 2024-10-31 21:10_

Looks great!

I wonder how much of the perf regression is due to the fact that currently I think every `None` ends up being a union of the `None` defined in `_typeshed` and the one defined in`types`? This could mean we spend a lot more time in recursive union handling for things like `is_subtype_of`? If so, this should be largely mitigated by `sys.version_info` checking.

---

_Comment by @MichaReiser on 2024-11-01 07:35_

I took a look at the perf regression 

What I spotted is  that this branch now spends 5.6% of total time inside `infer_function_definition_statement` where we only spent 1.6% before. 

Expanding the tree shows that the difference comes from `infer_decorators` that isn't present in the profile for main. I suspect that it comes from 

```py
@lru_cache(maxsize=None)
def cached_tz(hour_str: str, minute_str: str, sign_str: str) -> timezone:
    sign = 1 if sign_str == "+" else -1
    return timezone(
        timedelta(
            hours=sign * int(hour_str),
            minutes=sign * int(minute_str),
        )
    )
```

Inside `infer_decorators` there's now a call to `Type::none` which starts semantic indexing and what not of `lru_cache. 

I tried to debug if we now infer a more accurate type for `lru_cache` but both main and this PR infer `Todo` for 

```py
from functools import lru_cache
from typing import reveal_type

reveal_type(lru_cache(maxsize=None))
```

The main thing that's puzzling me.... Why is the `lru_cache` type not cached? Shouldn't this bail out early instead of calling into `semantic_index`?


![image](https://github.com/user-attachments/assets/8e2c5e46-10b2-4aa9-a37c-662dd9388727)


Ohh... never mind. I looked at the warmup phase instead of the incremental path. 

This is for the incremental part

![image](https://github.com/user-attachments/assets/821f92ee-d633-4984-9ce6-f8ed64e4c2cf)

The finding is the same... and, for some reason, `KnownClass::none` is re-executing semantic indexing of the `functools` module which it should not... Not sure why. We should look into this


[incremental check_types this PR](https://share.firefox.dev/4f5MNQp)

[incremental check_types main](https://share.firefox.dev/4hpRRkh)

---

_Comment by @MichaReiser on 2024-11-01 07:44_

Looking at the salsa code. What this suggests is that we don't reuse the same salsa ids and, because of that have cache misses...

---

_Comment by @MichaReiser on 2024-11-01 08:07_

I'm not sure what's happening and/or if this is a bug in the benchmark. I copied the tomllib and manually ran the red knot CLI in watch mode and the salsa events indicate that it uses the memoized values except for `_re.py` that changed.

---

_Comment by @MichaReiser on 2024-11-01 08:50_

Okay, I might still have looked at the wrong profiles. I now created a PR to use a named closure for the incremental and warmup vs to avoid this in the future. 

* [incremental none](https://share.firefox.dev/48y8LsQ)
* [incremental main](https://share.firefox.dev/3YMAAum)

I haven't looked at it too closely because salsa's maybe changed after profiles are a bit of a pain but I strongly suspect that the difference mainly comes from that salsa now has to validate the ingredients for the module defining `None`?  I suspect that addressing https://github.com/astral-sh/ty/issues/242 could help here as well.

---

_Comment by @carljm on 2024-11-01 20:55_

> the difference mainly comes from that salsa now has to validate the ingredients for the module defining `None`

Is this a speculation or is there evidence pointing to it? I couldn't really follow which comments here are based on looking at the wrong part of the profile :)

This could be, though. I was thinking we would already have a dependence on `types` module (because `builtins` imports it), but it's possible tomllib didn't actually resolve the type of any builtin depending on `types` module, so our lazy per-definition inference might have meant we previously didn't have to semantic-index `types` module.

If this is the cause of the regression, it's mostly just a weakness in our benchmark. Real-world codebases are likely to already rely on the `types` module in some way; adding a core typechecking dependency on it should not really be considered a real-world regression, if the regression just comes from "now we have to index that module."

I still think it's worth seeing whether Salsa-caching the resolution of the type of `None` makes a difference here. If the regression is entirely due to having all the new ingredients from the `types` module, then it won't.

---

_Comment by @sharkdp on 2024-11-04 11:38_

## Performance investigation

- The original version of this PR had a -4% performance regression on the "incremental" *as well as the "cold" benchmark* ([results](https://codspeed.io/astral-sh/ruff/runs/6723f0d664ef40e4a982d256)). The "cold" benchmark was probably just slightly below the warning/error threshold.
- Consequently, a rebase to `main` (after the merge of https://github.com/astral-sh/ruff/pull/14034 by @MichaReiser) did not help with improving performance. Both versions are still at -4% ([results](https://codspeed.io/astral-sh/ruff/runs/6728abafa3e6c10bfecc5442))
- I then tried the suggestion by @carljm and added explicit salsa caching for the lookup of `NoneType` in 503aa4644d303ee8c5f411af6a07bd44b3108549. The CI run now passes, but the performance regression is still there with -3% for both versions ([results](https://codspeed.io/astral-sh/ruff/runs/6728b047a3e6c10bfecc544d)). Not sure if the resolution of these benchmarks is really sub-1%? If so, this is a small improvement.
- I checked which modules were resolved when running red-knot on tomllib; On `main`, we do not load `_typeshed`; on this branch, we do (due to the explicit lookup of `NoneType` in typeshed). This alone could probably explain the increase in runtime.

This also explains why I *didn't* see a performance regression when comparing this branch with main on a larger codebase (`black`). Because we *do* load `_typeshed` even on `main` (for some reason) when running on `black`. Results:

| Command | Mean [ms] | Min [ms] | Max [ms] | Relative |
|:---|---:|---:|---:|---:|
| `./red_knot_main --current-directory /home/shark/black` | 113.7 ± 3.4 | 105.5 | 120.0 | 1.00 ± 0.05 |
| `./red_knot_remove_none --current-directory /home/shark/black` | 113.2 ± 4.3 | 107.3 | 121.1 | 1.00 |

After talking to @MichaReiser, I removed the explicit caching of the `NoneType` lookup again.


---

_@MichaReiser approved on 2024-11-04 12:03_

---

_@AlexWaygood approved on 2024-11-04 12:07_

I've only skimmed, but this all LGTM, and I agree that a small perf "regression" here is fine given your analysis (and given the significant improvements to maintainability this gives us)

---

_Merged by @sharkdp on 2024-11-04 13:00_

---

_Closed by @sharkdp on 2024-11-04 13:00_

---

_Branch deleted on 2024-11-04 13:00_

---
