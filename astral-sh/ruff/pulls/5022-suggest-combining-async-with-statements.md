```yaml
number: 5022
title: Suggest combining async with statements
type: pull_request
state: merged
author: Thomasdezeeuw
labels: []
assignees: []
merged: true
base: main
head: thomas/issue-3025
created_at: 2023-06-12T10:34:58Z
updated_at: 2023-06-12T14:33:20Z
url: https://github.com/astral-sh/ruff/pull/5022
synced_at: 2026-01-12T03:43:29Z
```

# Suggest combining async with statements

---

_Pull request opened by @Thomasdezeeuw on 2023-06-12 10:34_

## Summary

Previously the rule for SIM117 explicitly ignored `async with` statements as it would incorrectly suggestion to merge `async with` and regular `with` statements as reported in issue #1902.

This partially reverts the fix for that (commit     396be5edeac0c5724de87e93c5a885dacf201f05) by enabling the rules for `async with` statements again, but with a check ensuring that the statements are both of the same kind, i.e. both `async with` or both (just) `with` statements.

Closes #3025

## Test Plan

Updated and existing test and added a new test case from #3025.

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:63 on 2023-06-12 10:38_

```suggestion
/// Returns a boolean indicating whether it's an async with statement, the items and
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:65 on 2023-06-12 10:45_

How would this deal with
```python
with a as a2:
    async with b as b2:
        with c as c2:
            async with d as d2:
                f(a2, b2, c2, d2)
```

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:101 on 2023-06-12 11:09_

Took me way too long to figure this out:
```suggestion
            // Make sure with fix from top to bottom for nested with statements, e.g. for
            // ```python
            // with A():
            //     with B():
            //         with C():
            //             print("hello")
            // ```
            // suggest
            // ```python
            // with A(), B():
            //     with C():
            //         print("hello")
            // ```
            // but not the following
            // ```python
            // with A():
            //     with B(), C():
            //         print("hello")
            // ```
            return;
```
I'd place it outside the if let but github only allows me to place comments here

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:71 on 2023-06-12 11:11_

What's the purpose of the recursion if we only fix one level in each ruff fix iteration?

---

_@konstin reviewed on 2023-06-12 11:12_

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:65 on 2023-06-12 11:30_

Currently it makes no suggestion.

We could probably suggest the following:

> ```python
> with a as a2, c as c2:
>     async with b as b2, d as d2:
>         f(a2, b2, c2, d2)
> ```

However, I'm not sure if that always produces equivalent code. If I read the docs correctly (https://docs.python.org/3/whatsnew/2.6.html#pep-343-the-with-statement) the syntax is `with expression`, where `expression` could fail in some way.

Consider

> ```python
> with open('/etc/file1', 'r') as f1:
>     async with open('/etc/file2', 'r') as f2:
>         with open('/etc/file3', 'r') as f3:
>             async with open('/etc/file3', 'r') as f4:
>                 f(f1, f2, f3, f4)
> ```

I don't think we can rewrite that to 

> ```python
> with open('/etc/file1', 'r') as f1, open('/etc/file3', 'r') as f3:
>     async with open('/etc/file2', 'r') as f2, open('/etc/file3', 'r') as f4:
>         f(f1, f2, f3, f4)
> ```

Because that changes the order in which the files are opened.

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:71 on 2023-06-12 11:34_

I'm not exactly sure, I didn't want to change the existing code too much possibly breaking something in the process.

It does slightly improve the suggestion span (`-` is old code, `+` is without the recursive call):

```
---- rules::flake8_simplify::tests::rules::rule_multiplewithstatements_path_new_sim117_py_expects stdout ----
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ Snapshot Summary ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Snapshot file: crates/ruff/src/rules/flake8_simplify/snapshots/ruff__rules__flake8_simplify__tests__SIM117_SIM117.py.snap
Snapshot: SIM117_SIM117.py
Source: crates/ruff/src/rules/flake8_simplify/mod.rs:52
────────────────────────────────────────────────────────────────────────────────────────────────────────
-old snapshot
+new results
────────────┬───────────────────────────────────────────────────────────────────────────────────────────
   22    22 │    |
   23    23 │  6 |   # SIM117
   24    24 │  7 | / with A():
   25    25 │  8 | |     with B():
   26       │- 9 | |         with C():
   27       │-   | |_________________^ SIM117
         26 │+   | |_____________^ SIM117
         27 │+ 9 |           with C():
   28    28 │ 10 |               print("hello")
   29    29 │    |
   30    30 │    = help: Combine `with` statements
   31    31 │
────────────┴───────────────────────────────────────────────────────────────────────────────────────────

```

But perhaps we can resolve that without recursion.

---

_@Thomasdezeeuw reviewed on 2023-06-12 11:36_

---

_@konstin reviewed on 2023-06-12 11:51_

---

_Review comment by @konstin on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:65 on 2023-06-12 11:51_

sorry, bad example. If i use
```python
async with b as b2:
    with c as c2:
        async with d as d2:
            f(b2, c2, d2)
```
i get 
```
SIM117.py:112:1: SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
    |
110 |       return 0
111 |   
112 | / async with b as b2:
113 | |     with c as c2:
114 | |         async with d as d2:
    | |___________________________^ SIM117
115 |               f(b2, c2, d2)
    |
    = help: Combine `with` statements

ℹ Suggested fix
109 109 | 
110 110 |     return 0
111 111 | 
112     |-async with b as b2:
113     |-    with c as c2:
114     |-        async with d as d2:
115     |-            f(b2, c2, d2)
    112 |+async with b as b2, c as c2:
    113 |+    async with d as d2:
    114 |+        f(b2, c2, d2)

```
which is incorrect.

Could you add a test case for both the already working example and this example?

---

_@charliermarsh reviewed on 2023-06-12 12:46_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:65 on 2023-06-12 12:46_

Yeah if they're out-of-order (with, then async, then with, etc.), we shouldn't re-order the context managers IMO -- only consecutive sync or async managers.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:71 on 2023-06-12 12:46_

> What's the purpose of the recursion if we only fix one level in each ruff fix iteration?

Is this true? I was under the impression that we did collapse multiple levels (haven't looked back at the code).

---

_@charliermarsh reviewed on 2023-06-12 12:46_

---

_@Thomasdezeeuw reviewed on 2023-06-12 13:56_

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:71 on 2023-06-12 13:56_

After following some red hearings I think I can say that we don't completely collapse multiple levels. This is the output of the current code (before this pr):

```
SIM117.py:2:1: SIM117 [*] Use a single `with` statement with multiple contexts instead of nested `with` statements
  |
1 |   # SIM117
2 | / with A():
3 | |     with B():
4 | |         with C():
  | |_________________^ SIM117
5 |               print("hello")
  |
  = help: Combine `with` statements

ℹ Suggested fix
1 1 | # SIM117
2   |-with A():
3   |-    with B():
4   |-        with C():
5   |-            print("hello")
  2 |+with A(), B():
  3 |+    with C():
  4 |+        print("hello")
```

Note that the error indicator (first part) includes all three `with` statements, but the suggested fix only includes the first two `with` statement. Which is not ideal because when applied Ruff will complain the next run to also collapse `with C()`.

I could work on the fix suggestion to rewrite it to `with A(), B(), C()`, but that will have a larger scope than issue https://github.com/astral-sh/ruff/issues/3025 is aiming to fix, so perhaps we should do it in another pr.

---

_@Thomasdezeeuw reviewed on 2023-06-12 14:00_

---

_Review comment by @Thomasdezeeuw on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:65 on 2023-06-12 14:00_

I've resolved this in the last commit (169c0fffaacf4386bde36416d389187c744fc521), but that changed the range of text the error points to, see the change in the snapshot and https://github.com/astral-sh/ruff/pull/5022#discussion_r1226710316.

---

_@charliermarsh reviewed on 2023-06-12 14:11_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_simplify/rules/ast_with.rs`:71 on 2023-06-12 14:11_

Ahh ok, so we flag them all, but only fix one level... This is not _terrible_ because Ruff runs iteratively until there are no more fixable errors, so from the user's perspective, it will appear as if these were fixed in one pass. But it does mean that the suggested fix is confusing as depicted above.

Let's fix it if possible, but in a separate PR.

---

_@charliermarsh approved on 2023-06-12 14:12_

---

_@konstin approved on 2023-06-12 14:23_

---

_Comment by @github-actions[bot] on 2023-06-12 14:26_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.17      8.1±0.05ms     5.1 MB/sec    1.00      6.9±0.05ms     5.9 MB/sec
formatter/numpy/ctypeslib.py               1.14   1580.6±5.26µs    10.5 MB/sec    1.00   1391.4±2.39µs    12.0 MB/sec
formatter/numpy/globals.py                 1.10    151.4±0.44µs    19.5 MB/sec    1.00    137.9±0.98µs    21.4 MB/sec
formatter/pydantic/types.py                1.15      3.2±0.01ms     8.0 MB/sec    1.00      2.8±0.01ms     9.1 MB/sec
linter/all-rules/large/dataset.py          1.00     14.8±0.13ms     2.7 MB/sec    1.02     15.1±0.10ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.6±0.01ms     4.7 MB/sec    1.02      3.6±0.04ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    368.4±1.19µs     8.0 MB/sec    1.01    371.5±0.99µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.05ms     4.1 MB/sec    1.03      6.5±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.5±0.04ms     5.4 MB/sec    1.01      7.6±0.04ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1552.0±3.10µs    10.7 MB/sec    1.01  1570.1±43.79µs    10.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    166.2±1.19µs    17.7 MB/sec    1.00    166.4±0.30µs    17.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.3±0.01ms     7.6 MB/sec    1.00      3.3±0.01ms     7.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.7±0.21ms     4.2 MB/sec    1.00      9.7±0.23ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.0±0.12ms     8.3 MB/sec    1.00  1952.6±52.38µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    188.6±7.78µs    15.6 MB/sec    1.00    187.8±8.38µs    15.7 MB/sec
formatter/pydantic/types.py                1.01      3.9±0.11ms     6.5 MB/sec    1.00      3.9±0.10ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     20.3±0.41ms     2.0 MB/sec    1.00     20.4±0.37ms  2044.0 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.15ms     3.2 MB/sec    1.01      5.2±0.12ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   608.8±16.88µs     4.8 MB/sec    1.01   612.2±20.40µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.7±0.22ms     2.9 MB/sec    1.01      8.8±0.20ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00     10.3±0.20ms     4.0 MB/sec    1.00     10.3±0.18ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.06ms     7.7 MB/sec    1.00      2.2±0.05ms     7.7 MB/sec
linter/default-rules/numpy/globals.py      1.01   244.6±11.44µs    12.1 MB/sec    1.00    241.7±8.17µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.13ms     5.5 MB/sec    1.00      4.6±0.12ms     5.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @Thomasdezeeuw on 2023-06-12 14:33_

---

_Closed by @Thomasdezeeuw on 2023-06-12 14:33_

---

_Branch deleted on 2023-06-12 14:33_

---
