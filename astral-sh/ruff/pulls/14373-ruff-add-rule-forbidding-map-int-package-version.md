```yaml
number: 14373
title: "[`ruff`] Add rule forbidding `map(int, package.__version__.split('.'))` (`RUF048`)"
type: pull_request
state: merged
author: InSyncWithFoo
labels:
  - rule
  - preview
assignees: []
merged: true
base: main
head: RUF048
created_at: 2024-11-16T00:52:03Z
updated_at: 2024-11-20T03:46:08Z
url: https://github.com/astral-sh/ruff/pull/14373
synced_at: 2026-01-10T20:50:57Z
```

# [`ruff`] Add rule forbidding `map(int, package.__version__.split('.'))` (`RUF048`)

---

_Pull request opened by @InSyncWithFoo on 2024-11-16 00:52_

## Summary

Resolves #12961.

## Test Plan

`cargo nextest run` and `cargo insta test`.


---

_Comment by @github-actions[bot] on 2024-11-16 01:06_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/tuple_map_int_version_parsing.rs`:53 on 2024-11-16 15:43_

Do we need to check whether the `map()` call is nested inside a `tuple` or a `list` call? I feel like both of these would be victim to the same latent bug:

```py
from foo import __version__
from collections import deque

x = deque(map(int, __version__.split('.')))

for element in map(int, __version__.split('.')):
    print(element)
```

If we just flag all `map(int, __version__.split('.'))` calls, regardless of whether they're nested inside `list()` or `tuple()` calls, the rule is more general and we catch more bugs.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/tuple_map_int_version_parsing.rs`:16 on 2024-11-16 15:43_

```suggestion
/// import matplotlib  # `__version__ == "3.9.1.post-1"` in our environment
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/tuple_map_int_version_parsing.rs`:36 on 2024-11-16 15:45_

If users follow this link, there's a warning up at the top of the page telling them that the PEP is a historical document rather than living documentation that's meant to be kept up-to-date. Let's link to the up-to-date docs instead, which are at https://packaging.python.org/en/latest/specifications/version-specifiers/#version-specifiers

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/tuple_map_int_version_parsing.rs`:118 on 2024-11-16 15:47_

```suggestion
    if attr != "split" {
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/tuple_map_int_version_parsing.rs`:127 on 2024-11-16 15:48_

```suggestion
        return id == "__version__";
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/tuple_map_int_version_parsing.rs`:92 on 2024-11-16 15:54_

It's generally good to avoid `.unwrap()` calls where possible. Even in cases like this where you can see that they're safe, it's a sign that you're fighting the type system rather than using it to your advantage. Here you can do this to achieve the same thing without any `.unwrap()`s.

```suggestion
    if !semantic.match_builtin_expr(func, "map") {
        return None;
    };

    if let [first, second] = positionals {
        Some((first, second))
    } else {
        None
    }
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/tuple_map_int_version_parsing.rs`:107 on 2024-11-16 15:54_

Here as well, you can easily avoid the `.unwrap()`:

```suggestion
    let [argument] = arguments else {
        return false;
    };
```

---

_@AlexWaygood reviewed on 2024-11-16 15:55_

Thanks!

I wonder if the rule should also flag `[int(x) for x in __version__]` and `(int(x) for x in __version__)`? But that could always be done as a followup.

---

_Comment by @InSyncWithFoo on 2024-11-17 00:50_

@AlexWaygood Thanks! All fixed.

---

_Label `rule` added by @dhruvmanila on 2024-11-18 06:24_

---

_Label `preview` added by @dhruvmanila on 2024-11-18 06:24_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-11-18 07:36_

---

_@AlexWaygood approved on 2024-11-18 13:40_

Thanks!

---

_Renamed from "[`ruff`] `tuple(map(int, package.__version__.split('.')))` (`RUF048`)" to "[`ruff`] Add rule forbidding `map(int, package.__version__.split('.'))` (`RUF048`)" by @AlexWaygood on 2024-11-18 13:40_

---

_Merged by @AlexWaygood on 2024-11-18 13:43_

---

_Closed by @AlexWaygood on 2024-11-18 13:43_

---

_Branch deleted on 2024-11-20 03:46_

---
