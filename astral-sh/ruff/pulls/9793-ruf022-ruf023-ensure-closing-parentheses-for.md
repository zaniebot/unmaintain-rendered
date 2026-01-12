```yaml
number: 9793
title: "RUF022, RUF023: Ensure closing parentheses for multiline sequences are always on their own line"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - rule
assignees: []
merged: true
base: main
head: closing-paren
created_at: 2024-02-02T18:54:53Z
updated_at: 2024-02-09T21:27:48Z
url: https://github.com/astral-sh/ruff/pull/9793
synced_at: 2026-01-12T15:55:30Z
```

# RUF022, RUF023: Ensure closing parentheses for multiline sequences are always on their own line

---

_@AlexWaygood_

## Summary

Currently these rules apply the heuristic that if the original sequence doesn't have a newline in between the final sequence item and the closing parenthesis, the autofix won't add one for you. The feedback from @ThiefMaster, however, was that this was producing slightly unusual formatting -- things like this:

```py
__all__ = [
    "b", "c",
    "a", "d"]
```

were being autofixed to this:

```py
__all__ = [
    "a",
    "b",
    "c",
    "d"]
```

When, if it was _going_ to be exploded anyway, they'd prefer something like this (with the closing parenthesis on its own line):

```py
__all__ = [
    "a",
    "b",
    "c",
    "d"
]
```

I'm still pretty skeptical that we'll be able to please everybody here with the formatting choices we make; _but_, on the other hand, this _specific_ change is pretty easy to make.

## Test Plan

`cargo test`. I also ran the autofixes for RUF022 and RUF023 on CPython to check how they looked; they looked fine to me.


---

_Comment by @github-actions[bot] on 2024-02-02 19:08_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@AlexWaygood reviewed on 2024-02-02 19:37_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:934 on 2024-02-02 19:37_

If we wanted to _always_ add a trailing comma to the last item here (@ThiefMaster does), we could do something like this instead:

```suggestion
        let parenthesis_re = regex::Regex::new(r"^,?([\])}])$").unwrap();
        if let Some(captures) = parenthesis_re.captures(postlude) {
            let closing_paren = &captures[1];
            return Cow::Owned(format!(",{newline}{leading_indent}{closing_paren}"));
        }
        return Cow::Borrowed(postlude);
```

---

_Review requested from @charliermarsh by @charliermarsh on 2024-02-09 14:06_

---

_Label `rule` added by @charliermarsh on 2024-02-09 14:06_

---

_@charliermarsh reviewed on 2024-02-09 20:38_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:905 on 2024-02-09 20:38_

Is it possible to write this via string find / filter checks, rather than using a regex? Regex tends to be overkill and slower for simple operations like this.

---

_@AlexWaygood reviewed on 2024-02-09 20:43_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:905 on 2024-02-09 20:43_

Yes, definitely doable -- just _slightly_ less elegant here :)

I'll make the change!

---

_@charliermarsh reviewed on 2024-02-09 20:48_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:905 on 2024-02-09 20:48_

As a tip: if we _did_ use a regex, we should ensure that it's only compiled once (like in Python). It can make a _huge_ difference. You can grep for `Lazy<Regex>` as an example of that pattern.

---

_@AlexWaygood reviewed on 2024-02-09 21:12_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sequence_sorting.rs`:905 on 2024-02-09 21:12_

Got rid of the regex in 6fa2c43c8629094ae4b918cbb97b94bee432e4ac!

---

_Comment by @codspeed-hq[bot] on 2024-02-09 21:18_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/AlexWaygood:closing-paren)

### Merging #9793 will **improve performances by 44.39%**

<sub>Comparing <code>AlexWaygood:closing-paren</code> (6fa2c43) with <code>main</code> (c53aae0)</sub>



### Summary

`⚡ 18` improvements
`✅ 12` untouched benchmarks




### Benchmarks breakdown

|     | Benchmark | `main` | `AlexWaygood:closing-paren` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[numpy/globals.py]` | 1,117.4 µs | 963.5 µs | +15.98% |
| ⚡ | `parser[large/dataset.py]` | 69.1 ms | 63.5 ms | +8.78% |
| ⚡ | `lexer[numpy/globals.py]` | 223.5 µs | 154.8 µs | +44.39% |
| ⚡ | `linter/all-rules[numpy/globals.py]` | 2.8 ms | 2.6 ms | +7.65% |
| ⚡ | `lexer[unicode/pypinyin.py]` | 571.5 µs | 526.5 µs | +8.55% |
| ⚡ | `linter/all-rules[pydantic/types.py]` | 44.3 ms | 39.8 ms | +11.4% |
| ⚡ | `lexer[numpy/ctypeslib.py]` | 1.8 ms | 1.6 ms | +15.26% |
| ⚡ | `linter/all-rules[unicode/pypinyin.py]` | 11.8 ms | 10.2 ms | +15.65% |
| ⚡ | `parser[pydantic/types.py]` | 26.4 ms | 24.4 ms | +8.1% |
| ⚡ | `parser[unicode/pypinyin.py]` | 4.3 ms | 3.9 ms | +9.89% |
| ⚡ | `linter/all-rules[numpy/ctypeslib.py]` | 22 ms | 18.9 ms | +16.46% |
| ⚡ | `linter/all-with-preview-rules[unicode/pypinyin.py]` | 12.8 ms | 11.3 ms | +13.33% |
| ⚡ | `linter/all-with-preview-rules[numpy/globals.py]` | 3.1 ms | 3 ms | +4.09% |
| ⚡ | `linter/all-with-preview-rules[pydantic/types.py]` | 51.9 ms | 47.2 ms | +9.9% |
| ⚡ | `linter/all-with-preview-rules[numpy/ctypeslib.py]` | 24.6 ms | 22 ms | +11.8% |
| ⚡ | `parser[numpy/ctypeslib.py]` | 11.8 ms | 10.8 ms | +9.85% |
| ⚡ | `linter/all-rules[large/dataset.py]` | 94.1 ms | 82.6 ms | +13.83% |
| ⚡ | `linter/all-with-preview-rules[large/dataset.py]` | 106.9 ms | 96 ms | +11.39% |


---

_Comment by @charliermarsh on 2024-02-09 21:19_

Lol

---

_@charliermarsh approved on 2024-02-09 21:20_

---

_Merged by @AlexWaygood on 2024-02-09 21:27_

---

_Closed by @AlexWaygood on 2024-02-09 21:27_

---

_Branch deleted on 2024-02-09 21:27_

---
