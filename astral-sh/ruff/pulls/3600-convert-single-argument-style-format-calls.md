```yaml
number: 3600
title: Convert single-argument %-style format calls
type: pull_request
state: merged
author: charliermarsh
labels:
  - rule
assignees: []
merged: true
base: main
head: charlie/printf
created_at: 2023-03-18T19:25:03Z
updated_at: 2023-03-21T04:02:27Z
url: https://github.com/astral-sh/ruff/pull/3600
synced_at: 2026-01-12T15:55:13Z
```

# Convert single-argument %-style format calls

---

_@charliermarsh_

## Summary

Today, we only convert %-style format calls if the right-hand side is a dictionary or tuple literal. However, I _think_ we can support a larger set of expressions on the right-hand side.

There are three cases to consider...

1. The template string contain at least one named value (`"%(foo)s" % x`). In that case, the right-hand side _has_ to resolve to a map, so we treat it as kwargs (e.g., `"{foo}".format(**x)`).
2. The template string contains multiple positional values (`"%s %s" % x`). In that case, the right-hand side _has_ to resolve to a tuple, so we treat it as kwargs (e.g., `"{} {}".format(*x)`).
3. The template string contains a single value (`"%s" % x`). In that case, we can't be certain whether the right-hand side is a single expression, or a tuple containing a single expression -- so unfortunately, we have to continue ignoring those.

There's one case that's not formally covered above, which is a template string that mixes named and positional values, like `"%(foo)s %s"`. But I'm unable to come up with a right-hand side that's valid given mixed named and positional values. Does such a case exist? I believe it's impossible.

Closes #3549.

---

_Label `rule` added by @charliermarsh on 2023-03-18 19:25_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-18 19:25_

---

_Review requested from @konstin by @charliermarsh on 2023-03-18 19:25_

---

_Comment by @github-actions[bot] on 2023-03-18 19:39_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+5, -0, 0 error(s))

<details><summary>airflow (+1, -0)</summary>
<p>

```diff
+ airflow/providers/google/cloud/hooks/bigtable.py:252:32: UP031 [*] Use format specifiers instead of percent format
```

</p>
</details>
<details><summary>bokeh (+3, -0)</summary>
<p>

```diff
+ src/bokeh/plotting/_figure.py:738:41: UP031 [*] Use format specifiers instead of percent format
+ tests/unit/bokeh/models/test_plots.py:486:38: UP031 [*] Use format specifiers instead of percent format
+ tests/unit/bokeh/models/test_plots.py:492:38: UP031 [*] Use format specifiers instead of percent format
```

</p>
</details>
<details><summary>scikit-build (+1, -0)</summary>
<p>

```diff
+ skbuild/platform_specifics/cygwin.py:33:27: UP031 [*] Use format specifiers instead of percent format
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.04ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.7±0.01ms     4.5 MB/sec    1.00      3.7±0.00ms     4.5 MB/sec
linter/all-rules/numpy/globals.py          1.01    433.7±1.11µs     6.8 MB/sec    1.00    430.9±2.23µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.01ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.7±0.02ms     5.3 MB/sec    1.01      7.7±0.03ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1697.9±5.06µs     9.8 MB/sec    1.00   1706.0±2.25µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.2±0.29µs    16.8 MB/sec    1.01    176.9±2.56µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.01ms     7.1 MB/sec    1.00      3.6±0.00ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     17.6±0.64ms     2.3 MB/sec    1.00     17.0±0.48ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.8±0.21ms     3.5 MB/sec    1.01      4.8±0.26ms     3.5 MB/sec
linter/all-rules/numpy/globals.py          1.07   645.5±44.15µs     4.6 MB/sec    1.00   600.7±46.57µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.2±0.36ms     3.1 MB/sec    1.00      8.0±0.39ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.05      9.9±0.38ms     4.1 MB/sec    1.00      9.5±0.58ms     4.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.13ms     7.7 MB/sec    1.00      2.1±0.10ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.07   265.4±16.18µs    11.1 MB/sec    1.00   248.4±28.31µs    11.9 MB/sec
linter/default-rules/pydantic/types.py     1.12      5.0±0.16ms     5.1 MB/sec    1.00      4.4±0.22ms     5.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @andersk on 2023-03-18 22:19_

Case 3 does not hold.

```python
x = (1,)
print("%s" % x)
print("{}".format(x))
```

---

_Comment by @charliermarsh on 2023-03-18 23:05_

Ah that's a shame, I'd guess that's a small minority of cases.

---

_Comment by @charliermarsh on 2023-03-18 23:07_

I can't think of any way to avoid that, so I guess we have to continue omitting those cases.

---

_Comment by @charliermarsh on 2023-03-19 03:20_

(Removed, and amended the PR summary.)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/printf_string_formatting.rs`:398 on 2023-03-20 08:43_

I love the comments!

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyupgrade/rules/printf_string_formatting.rs`:339 on 2023-03-20 08:44_

Nit: A boolean for keywords seems sufficient
```suggestion
    let mut has_keyword_arguments = false;
```

---

_@MichaReiser approved on 2023-03-20 08:44_

LGTM. I can't say much about the inference rules. 

---

_Comment by @konstin on 2023-03-20 09:36_

I think the proper way to ensure the cases are correctly covered would be reading the cpython source code, but i couldn't find the percent formatting source code, only the tests: https://github.com/python/cpython/blob/5e6661bce968173fa45b74fa2111098645ff609c/Lib/test/test_format.py

---

_@konstin approved on 2023-03-20 09:38_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyupgrade/rules/printf_string_formatting.rs`:339 on 2023-03-21 03:30_

Ah, we need to know if there's _more than_ one positional argument. If we have exactly one positional argument, we can't fix it unfortunately :sob:

---

_@charliermarsh reviewed on 2023-03-21 03:30_

---

_Merged by @charliermarsh on 2023-03-21 03:35_

---

_Closed by @charliermarsh on 2023-03-21 03:35_

---

_Branch deleted on 2023-03-21 03:35_

---
