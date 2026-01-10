```yaml
number: 15829
title: "[`pyflakes`] Visit forward annotations in `TypeAliasType` as types (`F401`)"
type: pull_request
state: merged
author: ntBre
labels:
  - bug
assignees: []
merged: true
base: main
head: brent/forward-type-alias
created_at: 2025-01-30T15:17:53Z
updated_at: 2025-01-30T23:06:39Z
url: https://github.com/astral-sh/ruff/pull/15829
synced_at: 2026-01-10T19:57:22Z
```

# [`pyflakes`] Visit forward annotations in `TypeAliasType` as types (`F401`)

---

_Pull request opened by @ntBre on 2025-01-30 15:17_

## Summary

Fixes https://github.com/astral-sh/ruff/issues/15812 by visiting the second argument as a type definition.

## Test Plan

New F401 tests based on the report.


---

_Label `bug` added by @ntBre on 2025-01-30 15:17_

---

_Review requested from @AlexWaygood by @ntBre on 2025-01-30 15:17_

---

_Comment by @github-actions[bot] on 2025-01-30 15:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_34.py`:1 on 2025-01-30 15:34_

Can you add a test like this, in case folks use the kwargs?

```
Json = TypeAliasType(
    value='Union[dict[str, Json], list[Json], str, int, float, bool, None]',
    name='Json',
)
```

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/ast/mod.rs`:1380 on 2025-01-30 15:39_

You're probably right, and I might just not be tracing the logic correctly, but is it possible that:

```
# `value` gets visited as "non type definition"
Json = TypeAliasType(
    value='Union[dict[str, Json], list[Json], str, int, float, bool, None]',
    name='Json',
)  

# whereas here it gets visited as a type definition
Json = TypeAliasType(
    'Json',
    'Union[dict[str, Json], list[Json], str, int, float, bool, None]',
)
```

---

_@dylwil3 reviewed on 2025-01-30 15:39_

This is great! I have a question about the treatement of kwargs vs positional args

---

_@dylwil3 reviewed on 2025-01-30 16:31_

---

_Review comment by @dylwil3 on `crates/ruff_linter/src/checkers/ast/mod.rs`:1380 on 2025-01-30 16:31_

Also probably good to make sure no errors or anything occur if someone does something egregious:

```
args = [
    'Json',
    'Union[dict[str, Json], list[Json], str, int, float, bool, None]',
]

kwargs = {
    'name':'Json',
    'value':'Union[dict[str, Json], list[Json], str, int, float, bool, None]',
}

Json = TypeAliasType(*args)
Json = TypeAliasType(**kwargs)
```

(I don't think it matters what the actual lint rule does in these cases, but it'd be bad if the visitor panicked, for example).

---

_@ntBre reviewed on 2025-01-30 16:36_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_34.py`:1 on 2025-01-30 16:36_

Oh good catch, this test gives the original import error. Do you think the other cases above the new code (I pretty much copied and updated the `TypeVar` case, in particular) are also susceptible to this? I can't come up with a reason why they aren't but could be missing something.

---

_@ntBre reviewed on 2025-01-30 16:40_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:1380 on 2025-01-30 16:40_

Added! We don't detect the import as used here, but at least it doesn't panic, as you said.

---

_@dylwil3 reviewed on 2025-01-30 16:48_

---

_Review comment by @dylwil3 on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_34.py`:1 on 2025-01-30 16:48_

I _think_ the `TypeVar` logic looks okay to me. The function signature (which things are positional + keyword vs keyword only) is working in your favor then https://docs.python.org/3/library/typing.html#typing.TypeVar

---

_@AlexWaygood reviewed on 2025-01-30 16:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_34.py`:1 on 2025-01-30 16:54_

Yeah, @dylwil3's right -- it's not possible to specify the first argument to the `TypeVar` constructor (`name`) by keyword if you also specify other arguments. That means that the first argument must always be the `name` argument, which means it must be interpreted as a value expression. All other valid arguments to the `TypeVar` constructor are expected to be type expressions.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_34.py`:1 on 2025-01-30 16:56_

nit: you could probably isolate these snippets by putting them all in separate functions rather than putting each snippet in a different fixture file

---

_@ntBre reviewed on 2025-01-30 16:57_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_34.py`:1 on 2025-01-30 16:57_

Ahhh, I see. Thank you both!

---

_Review comment by @AlexWaygood on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_37.py`:3 on 2025-01-30 16:59_

You could possibly add a comment next to this import and the one in `F401_38.py` saying that _strictly speaking_ it's a false positive to emit F401 here, but we can't really be expected to reasonably understand that the strings here need to be understood as type expressions (and type checkers probably wouldn't understand them as type expressions either!)

---

_@AlexWaygood approved on 2025-01-30 17:00_

nice!

---

_@ntBre reviewed on 2025-01-30 17:11_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/pyflakes/F401_34.py`:1 on 2025-01-30 17:11_

Ah of course, that's a lot better. Thank you!

---

_@dylwil3 approved on 2025-01-30 17:32_

---

_@AlexWaygood approved on 2025-01-30 17:58_

---

_Comment by @ntBre on 2025-01-30 18:00_

Thank you both again for the reviews! I'll hold off merging until after the release.

---

_Merged by @ntBre on 2025-01-30 23:06_

---

_Closed by @ntBre on 2025-01-30 23:06_

---

_Branch deleted on 2025-01-30 23:06_

---
