```yaml
number: 3501
title: "Prefer `itertools.pairwise()` over `zip()` for successive pairs (`RUF007`)"
type: pull_request
state: merged
author: evanrittenhouse
labels:
  - rule
assignees: []
merged: true
base: main
head: 3297_itertools_pairwise
created_at: 2023-03-14T03:58:24Z
updated_at: 2023-03-17T13:42:45Z
url: https://github.com/astral-sh/ruff/pull/3501
synced_at: 2026-01-12T15:55:13Z
```

# Prefer `itertools.pairwise()` over `zip()` for successive pairs (`RUF007`)

---

_@evanrittenhouse_

Fixes #3297. 

We ensure that the two arguments to `zip()` match and that the lower bound of the slice is 1, if the second argument is a slice. Otherwise, we wouldn't be able to guarantee that the request being made is for successive pairs. 

---

_Review comment by @andersk on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:26 on 2023-03-14 04:05_

Also check `checker.ctx.is_builtin("zip")`.

---

_@andersk reviewed on 2023-03-14 04:07_

[`itertools.pairwise`](https://docs.python.org/3/library/itertools.html#itertools.pairwise) is new in Python 3.10, so this rule should be restricted to `target_version >= PyVersion::Py310`.

Should we also check for `zip(a[:-1], a[1:])`?

---

_Converted to draft by @evanrittenhouse on 2023-03-14 04:17_

---

_@evanrittenhouse reviewed on 2023-03-14 04:17_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:26 on 2023-03-14 04:17_

Makes sense, I can implement these changes tomorrow

---

_Comment by @github-actions[bot] on 2023-03-15 03:35_

✅ ecosystem check detected no changes.

<!-- thollander/actions-comment-pull-request "ecosystem-results" -->

---

_Marked ready for review by @evanrittenhouse on 2023-03-15 03:51_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:25 on 2023-03-15 07:51_

I'm not as familiar with python but should these be `i64` to support a broader range of valid int indices?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:83 on 2023-03-15 07:53_

Checking if `ctx.is_builtin` is probably the most expensive operation (not very, but still):

We can help the compiler to generate more optimized code by changing the order of checks:

1. Test if the python version is valid 
2. Test if it only has one argument
3. Test if the id is `zip`
4. Only then, test if it is the built-in zip

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:105 on 2023-03-15 07:57_

Let's avoid the `first_arg_info_opt.unwrap()`
```suggestion
            // If it's not one of those, return
            let Some(first_arg_info) = first_arg_info_opt else  { return; };

            // Second arg can only be a subscript
            let ExprKind::Subscript { .. } = &args[1].node else {
                return;
            };
            let second_arg_info = get_slice_info(&args[1], checker.stylist).unwrap();

            let args_are_successive = (first_arg_info.slice_start == 0
                || first_arg_info.slice_end == -1)
                && second_arg_info.slice_start == 1;
```

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:100 on 2023-03-15 07:59_

Can we add `// SAFETY` comment describing why it's safe to call `unwrap` here (or why it is guaranteed that `get_slice_info` always returns `Some` for `args[1])?

---

_@MichaReiser reviewed on 2023-03-15 07:59_

---

_@andersk reviewed on 2023-03-15 08:41_

---

_Review comment by @andersk on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:58 on 2023-03-15 08:41_

We can’t just default unrecognized things to `0`. `a[0:-1]` and `a[2147483648:-1]` are quite different but this function treats them identically.

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:58 on 2023-03-15 18:09_

Moved the `unwrap()` to the equality check to force no diagnostic if there was a parsing error

---

_@evanrittenhouse reviewed on 2023-03-15 18:09_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:100 on 2023-03-15 18:09_

Added a `return` if `get_slice_info` is `None` for `args[1]` which should remove the potential for panic

---

_@evanrittenhouse reviewed on 2023-03-15 18:09_

---

_@evanrittenhouse reviewed on 2023-03-15 18:10_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:105 on 2023-03-15 18:10_

Done

---

_@evanrittenhouse reviewed on 2023-03-15 18:10_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:83 on 2023-03-15 18:10_

Done

---

_@evanrittenhouse reviewed on 2023-03-15 18:19_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:25 on 2023-03-15 18:19_

Done

---

_@andersk reviewed on 2023-03-15 23:55_

---

_Review comment by @andersk on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:55 on 2023-03-15 23:55_

We can’t default *these* unrecognized things to 0 either: `a[x:-1]` is different from `a[0:-1]`.

So is `a[0:-1:2]`, by the way.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:34 on 2023-03-16 08:13_

It seems that we only care about whether parsing was successful or not, but not what the actual parsing error was. That means we could use `Option<i64>` here (you can use `result.ok()` to get an option) and safe a few bytes of memory. 

---

_@MichaReiser approved on 2023-03-16 08:14_

---

_@MichaReiser reviewed on 2023-03-16 08:15_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:55 on 2023-03-16 08:15_

@andersk brought up a few good edge cases. Could you add those to the test suite?

---

_@evanrittenhouse reviewed on 2023-03-16 17:19_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:55 on 2023-03-16 17:19_

Yup, will do

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/ruff/rules/pairwise_over_zipped.rs`:34 on 2023-03-16 17:21_

Yeah, that's the path I'm going down in the latest commit. Seems like it'll help with the default issues that @andersk raised, since we can just default to `None` instead of a number.

---

_@evanrittenhouse reviewed on 2023-03-16 17:21_

---

_Converted to draft by @evanrittenhouse on 2023-03-16 19:47_

---

_Comment by @github-actions[bot] on 2023-03-16 23:40_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.
### Benchmark
#### Linux
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      9.5±0.27ms     4.3 MB/sec    1.20     11.4±0.38ms     3.6 MB/sec
linter/numpy/ctypeslib.py    1.00      2.4±0.11ms   139.8 MB/sec    1.01      2.4±0.08ms   138.1 MB/sec
linter/numpy/globals.py      1.00  1233.2±44.12µs   144.5 MB/sec    1.00  1235.5±43.22µs   144.3 MB/sec
linter/pydantic/types.py     1.00      4.4±0.17ms     5.8 MB/sec    1.18      5.2±0.16ms     4.9 MB/sec
```

#### Windows
```
group                        main                                   pr
-----                        ----                                   --
linter/large/dataset.py      1.00      8.7±0.18ms     4.7 MB/sec    1.02      8.9±0.67ms     4.6 MB/sec
linter/numpy/ctypeslib.py    1.04      2.9±0.05ms   116.2 MB/sec    1.00      2.8±0.02ms   121.2 MB/sec
linter/numpy/globals.py      1.03  1510.6±27.55µs   118.0 MB/sec    1.00  1464.6±35.84µs   121.7 MB/sec
linter/pydantic/types.py     1.01      4.1±0.11ms     6.3 MB/sec    1.00      4.0±0.07ms     6.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-03-16 23:46_

Should have time to merge this tonight if it's in good shape!

---

_Comment by @evanrittenhouse on 2023-03-17 00:14_

@charliermarsh just have to get the new changes re-reviewed - I changed the logic for determining successive pairs to simply be a difference of 1 between start of first/second slices. That'll allow us to pick up cases like `zip(x[2:], x[3:])` which should still trigger the violation, e.g. `pairwise(x[2:])`.

---

_Marked ready for review by @evanrittenhouse on 2023-03-17 00:14_

---

_Renamed from "Prefer `itertools.pairwise()` over `zip()` for successive pairs" to "Prefer `itertools.pairwise()` over `zip()` for successive pairs (`RUF007`)" by @charliermarsh on 2023-03-17 03:37_

---

_Merged by @charliermarsh on 2023-03-17 03:50_

---

_Closed by @charliermarsh on 2023-03-17 03:50_

---

_Comment by @charliermarsh on 2023-03-17 03:50_

Thanks @evanrittenhouse!

---

_Label `rule` added by @charliermarsh on 2023-03-17 03:50_

---

_Branch deleted on 2023-03-17 13:42_

---
