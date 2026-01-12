```yaml
number: 4563
title: "Handled dict and set inside f-string (#4249)"
type: pull_request
state: merged
author: DavideCanton
labels:
  - bug
assignees: []
merged: true
base: main
head: 4249
created_at: 2023-05-21T18:32:06Z
updated_at: 2023-06-09T06:59:52Z
url: https://github.com/astral-sh/ruff/pull/4563
synced_at: 2026-01-12T15:55:15Z
```

# Handled dict and set inside f-string (#4249)

---

_@DavideCanton_

This fixes some C40* rules by adding spaces around dict and set literals inside f-string, and quotes correctly string literals inside f-string, since dict kwargs must be converted into string literals.

Closes #4249.


---

_Comment by @github-actions[bot] on 2023-05-21 18:44_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      7.5±0.03ms     5.4 MB/sec    1.00      7.6±0.04ms     5.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1333.7±8.52µs    12.5 MB/sec    1.01   1345.6±2.43µs    12.4 MB/sec
formatter/numpy/globals.py                 1.00    149.9±1.22µs    19.7 MB/sec    1.01    151.6±3.62µs    19.5 MB/sec
formatter/pydantic/types.py                1.00      3.1±0.02ms     8.4 MB/sec    1.01      3.1±0.02ms     8.3 MB/sec
linter/all-rules/large/dataset.py          1.00     16.5±0.17ms     2.5 MB/sec    1.00     16.6±0.12ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.9±0.02ms     4.2 MB/sec    1.01      4.0±0.02ms     4.2 MB/sec
linter/all-rules/numpy/globals.py          1.00    486.4±2.14µs     6.1 MB/sec    1.01    491.2±3.28µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.9±0.05ms     3.7 MB/sec    1.01      6.9±0.07ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.00      8.0±0.04ms     5.1 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1708.5±9.82µs     9.7 MB/sec    1.01   1719.1±7.35µs     9.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    188.6±0.87µs    15.6 MB/sec    1.01    190.5±1.74µs    15.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.6±0.02ms     7.1 MB/sec    1.00      3.6±0.02ms     7.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.4±0.32ms     4.3 MB/sec    1.00      9.3±0.28ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1637.6±54.61µs    10.2 MB/sec    1.00  1641.5±58.33µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    179.5±8.38µs    16.4 MB/sec    1.01   181.3±13.14µs    16.3 MB/sec
formatter/pydantic/types.py                1.00      3.9±0.16ms     6.6 MB/sec    1.00      3.8±0.24ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.01     20.4±0.54ms  2037.2 KB/sec    1.00     20.3±0.52ms     2.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      5.2±0.16ms     3.2 MB/sec    1.00      5.1±0.16ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.00   602.4±25.63µs     4.9 MB/sec    1.01   607.7±35.92µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      8.6±0.27ms     3.0 MB/sec    1.00      8.5±0.25ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.01     10.1±0.29ms     4.0 MB/sec    1.00     10.1±0.25ms     4.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01      2.2±0.10ms     7.7 MB/sec    1.00      2.1±0.09ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   241.5±13.01µs    12.2 MB/sec    1.00   242.2±13.11µs    12.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      4.6±0.14ms     5.6 MB/sec    1.00      4.5±0.17ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@charliermarsh reviewed on 2023-05-21 19:27_

How would this handle a case like `f"{set(['a', 'b']) - set(['a'])}"`, where the second `set` needs to be converted, but doesn't require any padding?

---

_Comment by @DavideCanton on 2023-05-21 23:18_

> How would this handle a case like `f"{set(['a', 'b']) - set(['a'])}"`, where the second `set` needs to be converted, but doesn't require any padding?

You are right, I missed that case.

I reworked a bit the fix part by checking where the current expression is related to the f-string boundaries

---

_@charliermarsh reviewed on 2023-05-22 14:35_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:585 on 2023-05-22 14:35_

Nit: I'd probably rather parenthesize than add whitespace.

---

_@charliermarsh reviewed on 2023-05-22 14:37_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:596 on 2023-05-22 14:37_

I'm not sure that this will be sufficient -- I think we have to do something more advanced here. The `{` could be part of a different formatted expression within the same f-string, or it could be part of a curly parentheses literal:

```py
f"{{this is a literal, not an expression}}"
f"{1 + 2} and then {dict()}"
```

---

_@charliermarsh reviewed on 2023-05-22 14:38_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:596 on 2023-05-22 14:38_

(I'm being a bit vague because I don't know how exactly to solve this problem. We need to track additional state to detect the parent formatted expression.)

---

_@DavideCanton reviewed on 2023-05-22 23:07_

---

_Review comment by @DavideCanton on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:596 on 2023-05-22 23:07_

oh, ok, guess I'll have to study the f-string literal syntax a bit more

---

_@DavideCanton reviewed on 2023-05-22 23:18_

---

_Review comment by @DavideCanton on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:585 on 2023-05-22 23:18_

probably it's more robust, but I think it renders uglier expressions in many simple cases, what do you think?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:598 on 2023-05-23 06:36_

Nit: Can we directly push to the string instead of building a vec and then joining at the end?

---

_@MichaReiser reviewed on 2023-05-23 06:36_

---

_@DavideCanton reviewed on 2023-05-23 07:43_

---

_Review comment by @DavideCanton on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:598 on 2023-05-23 07:43_

right, I always think in python and forget strings are mutable in rust. Thanks

---

_@DavideCanton reviewed on 2023-05-23 10:04_

---

_Review comment by @DavideCanton on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:596 on 2023-05-23 10:04_

Ok, I see.

It seems I assumed (wrongly) that an expression wrapped in braces is always being formatted, but that's not the case.

---

_@charliermarsh reviewed on 2023-05-23 12:56_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:596 on 2023-05-23 12:56_

The other assumption here is that the expression is the first such expression in the f-string. But `f_string` here is just the start of the string, so above, we'd match on the `{1 + 2}` rather than the `{dict()}`.

---

_@DavideCanton reviewed on 2023-05-23 12:58_

---

_Review comment by @DavideCanton on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:596 on 2023-05-23 12:58_

No, they would be two separate matches. That code finds the first brace in the whole fstring, in order to detect if between the start of the f-string and the start of the matched expression there are only whitespaces. If there are just whitespaces then a left padding is not required.

---

_@DavideCanton reviewed on 2023-05-24 18:17_

---

_Review comment by @DavideCanton on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:596 on 2023-05-24 18:17_

ok now I see your point, the code is wrong

---

_@DavideCanton reviewed on 2023-05-25 07:31_

---

_Review comment by @DavideCanton on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:596 on 2023-05-25 07:31_

> (I'm being a bit vague because I don't know how exactly to solve this problem. We need to track additional state to detect the parent formatted expression.)

the problem here is that in `f"{1 + 2} and then {dict()} else {{foo}}"` the parent of the `dict()` expression is the whole string, so I cannot use it to detect the boundaries of the formatted token.

I've resorted to seeking the first `{` by iterating from the start of the matched token to the left and the first `}` from the end of the matched token to the right, so in the above case I match the two correct braces:
```
f"{1 + 2} and then {dict()} else {{foo}}"
                   ^      ^
```

The `{1 + 2}` node should not be a problem since it is not the target of these set of rules, as well as the `{{foo}}` one.

---

_Comment by @DavideCanton on 2023-06-08 20:42_

Hi, it's quite a while this is around, I'm still keeping it updated with the main branch. Is there any interest in merging this?

---

_Comment by @charliermarsh on 2023-06-08 20:59_

Apologies and thank you for the ping. I'll re-review this tonight.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-08 20:59_

---

_@charliermarsh reviewed on 2023-06-09 02:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:496 on 2023-06-09 02:33_

Would this not detect the wrong braces given that the expression is `a` in: `f"{ {a} }"`?

That would be: an f-string with a single expression, which is a set literal that contains the element `a`.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:496 on 2023-06-09 02:39_

Hmm, yeah it does get this wrong:

```diff
        249 │+23    |-print(f"{ {set(a for a in 'abc')} }")
        250 │+   23 |+print(f"{ { {a for a in 'abc'} } }")
```

But, it might be solvable. Will play around with it.

---

_@charliermarsh reviewed on 2023-06-09 02:39_

---

_@charliermarsh reviewed on 2023-06-09 04:09_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_comprehensions/fixes.rs`:496 on 2023-06-09 04:09_

Hmm, it's probably not solvable without a better f-string parser and representation.

---

_Label `bug` added by @charliermarsh on 2023-06-09 04:42_

---

_Merged by @charliermarsh on 2023-06-09 04:53_

---

_Closed by @charliermarsh on 2023-06-09 04:53_

---

_Branch deleted on 2023-06-09 06:59_

---

_Comment by @DavideCanton on 2023-06-09 06:59_

thanks for the update!

---
