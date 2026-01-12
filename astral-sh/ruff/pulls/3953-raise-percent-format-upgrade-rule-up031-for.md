```yaml
number: 3953
title: "Raise percent-format upgrade rule (`UP031`) for hanging modulos"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/mod
created_at: 2023-04-13T03:38:27Z
updated_at: 2023-04-13T04:09:21Z
url: https://github.com/astral-sh/ruff/pull/3953
synced_at: 2026-01-12T04:28:19Z
```

# Raise percent-format upgrade rule (`UP031`) for hanging modulos

---

_Pull request opened by @charliermarsh on 2023-04-13 03:38_

## Summary

This PR extends `UP031` to handle cases like:

```py
(
    """
    foo %s
    """
    % (x,)
)
```

I.e., cases in which the modulo symbol appears on a separate line. In the past, we avoided flagging and fixing these. I think the reasoning was that we'd end up messing with the formatting, since the `.format` and its arguments would end up on the same line as the string end-quotes themselves. But, we kind of already have this problem? For example, we _already_ apply this fix:

```diff
diff --git a/Doc/includes/mp_workers.py b/Doc/includes/mp_workers.py
index 3b92269f89..fb756d19ec 100644
--- a/Doc/includes/mp_workers.py
+++ b/Doc/includes/mp_workers.py
@@ -18,8 +18,7 @@ def worker(input, output):

 def calculate(func, args):
     result = func(*args)
-    return '%s says that %s%s = %s' % \
-        (current_process().name, func.__name__, args, result)
+    return '{} says that {}{} = {}'.format(current_process().name, func.__name__, args, result)
```

So, to me, it seems consistent to just apply the fix and expect users to address any changes in formatting (hopefully via an autoformatter).

Closes #3943.


---

_Comment by @github-actions[bot] on 2023-04-13 03:56_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+7, -0, 0 error(s))

<details><summary>bokeh (+7, -0)</summary>
<p>

```diff
+ tests/unit/bokeh/embed/test_server__embed.py:110:19: UP031 [*] Use format specifiers instead of percent format
+ tests/unit/bokeh/embed/test_server__embed.py:129:19: UP031 [*] Use format specifiers instead of percent format
+ tests/unit/bokeh/embed/test_server__embed.py:157:19: UP031 [*] Use format specifiers instead of percent format
+ tests/unit/bokeh/embed/test_server__embed.py:171:19: UP031 [*] Use format specifiers instead of percent format
+ tests/unit/bokeh/embed/test_server__embed.py:67:19: UP031 [*] Use format specifiers instead of percent format
+ tests/unit/bokeh/embed/test_server__embed.py:81:19: UP031 [*] Use format specifiers instead of percent format
+ tests/unit/bokeh/embed/test_server__embed.py:96:19: UP031 [*] Use format specifiers instead of percent format
```

</p>
</details>

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.2±0.33ms     2.5 MB/sec    1.00     16.3±0.37ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.13ms     4.1 MB/sec    1.00      4.1±0.13ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   528.4±18.07µs     5.6 MB/sec    1.00   526.5±17.75µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.1±0.22ms     3.6 MB/sec    1.00      7.0±0.15ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.04      8.3±0.39ms     4.9 MB/sec    1.00      8.0±0.22ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1855.7±71.64µs     9.0 MB/sec    1.00  1818.3±62.31µs     9.2 MB/sec
linter/default-rules/numpy/globals.py      1.00   221.5±10.46µs    13.3 MB/sec    1.00   222.5±11.48µs    13.3 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.14ms     6.6 MB/sec    1.00      3.8±0.11ms     6.8 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.1±0.20ms     2.5 MB/sec    1.00     16.0±0.16ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.04ms     4.1 MB/sec    1.00      4.1±0.04ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.3±6.51µs     6.0 MB/sec    1.00    493.1±5.61µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.8±0.06ms     3.8 MB/sec    1.01      6.8±0.13ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.1±0.04ms     5.0 MB/sec    1.01      8.2±0.06ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1767.8±15.85µs     9.4 MB/sec    1.01  1789.7±28.34µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    192.0±3.42µs    15.4 MB/sec    1.01    193.5±5.02µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     6.9 MB/sec    1.01      3.7±0.04ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-04-13 03:59_

---

_Closed by @charliermarsh on 2023-04-13 03:59_

---

_Branch deleted on 2023-04-13 03:59_

---
