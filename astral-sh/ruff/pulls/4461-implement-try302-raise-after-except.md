```yaml
number: 4461
title: "Implement TRY302 - `raise` after `except`"
type: pull_request
state: merged
author: john-h-k
labels:
  - rule
assignees: []
merged: true
base: main
head: rule/useless-try
created_at: 2023-05-16T22:36:14Z
updated_at: 2023-05-18T21:25:20Z
url: https://github.com/astral-sh/ruff/pull/4461
synced_at: 2026-01-12T03:50:03Z
```

# Implement TRY302 - `raise` after `except`

---

_Pull request opened by @john-h-k on 2023-05-16 22:36_

Fixes #4308 

This implements TRY302 from tryceratops.

```py
try:
	foo()
except:
	raise # TRY302
```



---

_Review comment by @john-h-k on `crates/ruff/src/rules/tryceratops/rules/pointless_raise.rs`:54 on 2023-05-16 22:37_

Ideally I should probably widen this diagnostic to the handler, not the raise. Either that or provide a better message

---

_Review comment by @john-h-k on `crates/ruff/src/rules/tryceratops/rules/pointless_raise.rs`:51 on 2023-05-16 22:37_

I am used to the luxury of `if_chain!` in the rust compiler, but if the double-nested lets are bad I can refactor

---

_Review comment by @john-h-k on `ruff.schema.json`:1 on 2023-05-16 22:37_

for some reason a test is failing on this file, it looks like my editor added a newline at the end when saving

---

_@john-h-k reviewed on 2023-05-16 22:38_

---

_@charliermarsh reviewed on 2023-05-16 22:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/tryceratops/rules/pointless_raise.rs`:51 on 2023-05-16 22:41_

If you take a look at the logic in [tryceratops](https://github.com/guilatrova/tryceratops/commit/f7a56dd337acda060aa643c80f20b2e780ad2fa2#diff-a2fd5eddbabe84a3b436a6be3501fa89c8d23023c77771f3d7e84b9e19ce9c05R158), they do a couple nice things:

1. They check if _all_ handlers are useless. I think this is correct, since you can use a "useless" handler to omit exceptions from subsequent handlers.
2. They check that the body is _only_ a raise, and that the argument is either the same as the "cause" (`except ... as e: raise e`) or implicit (`except ... as e: raise`). I think we should check those too.

Might also make sense to take a look at their test suite to ensure compatibility.


---

_@john-h-k reviewed on 2023-05-16 22:47_

---

_Review comment by @john-h-k on `crates/ruff/src/rules/tryceratops/rules/pointless_raise.rs`:51 on 2023-05-16 22:47_

point 1 and the latter half of point 2 makes sense, I handle the latter style but not the former, shall fix that.

About the body only being a raise, what's the reasoning for that? Surely
```py
except:
    raise
    print("something went wrong!")
```
would warrant both this error and some sort of dead-code error?

Although make sense if the aim is preserving exact behaviour

---

_@charliermarsh reviewed on 2023-05-16 22:51_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/tryceratops/rules/pointless_raise.rs`:51 on 2023-05-16 22:51_

Ah yeah, I think you're right. It's probably fine to retain the "first statement is a raise" behavior you have here.

---

_Label `rule` added by @charliermarsh on 2023-05-16 22:53_

---

_Review comment by @john-h-k on `crates/ruff/src/rules/tryceratops/rules/pointless_raise.rs`:51 on 2023-05-16 23:51_

:+1: addressed the comments

---

_@john-h-k reviewed on 2023-05-16 23:51_

---

_Comment by @github-actions[bot] on 2023-05-17 01:28_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
linter/all-rules/large/dataset.py          1.00     18.0±0.79ms     2.3 MB/sec     1.03     18.6±0.58ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.20ms     3.8 MB/sec     1.04      4.5±0.20ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   558.7±39.84µs     5.3 MB/sec     1.04   581.0±40.43µs     5.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.9±0.50ms     3.2 MB/sec     1.01      8.0±0.36ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.00      8.7±0.41ms     4.7 MB/sec     1.04      9.1±0.44ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1918.4±107.99µs     8.7 MB/sec    1.03  1976.4±70.82µs     8.4 MB/sec
linter/default-rules/numpy/globals.py      1.00   223.3±14.13µs    13.2 MB/sec     1.06   237.1±15.11µs    12.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.19ms     6.3 MB/sec     1.03      4.2±0.18ms     6.0 MB/sec
parser/large/dataset.py                    1.00      7.2±0.35ms     5.6 MB/sec     1.04      7.5±0.26ms     5.4 MB/sec
parser/numpy/ctypeslib.py                  1.01  1443.5±71.82µs    11.5 MB/sec     1.00  1434.2±70.41µs    11.6 MB/sec
parser/numpy/globals.py                    1.00    139.3±7.24µs    21.2 MB/sec     1.01    141.2±9.69µs    20.9 MB/sec
parser/pydantic/types.py                   1.00      3.0±0.15ms     8.4 MB/sec     1.01      3.1±0.15ms     8.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.02     19.6±0.33ms     2.1 MB/sec    1.00     19.3±0.55ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.0±0.15ms     3.4 MB/sec    1.00      4.9±0.12ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.01   573.4±56.59µs     5.1 MB/sec    1.00   567.6±18.33µs     5.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.1±0.24ms     3.2 MB/sec    1.01      8.2±0.22ms     3.1 MB/sec
linter/default-rules/large/dataset.py      1.15     11.5±2.56ms     3.5 MB/sec    1.00     10.0±0.36ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1946.6±71.03µs     8.6 MB/sec    1.08      2.1±0.06ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    222.2±9.03µs    13.3 MB/sec    1.06    234.6±9.29µs    12.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.1±0.12ms     6.2 MB/sec    1.08      4.4±0.15ms     5.8 MB/sec
parser/large/dataset.py                    1.00      7.9±0.14ms     5.2 MB/sec    1.15      9.1±0.24ms     4.5 MB/sec
parser/numpy/ctypeslib.py                  1.00  1496.3±40.38µs    11.1 MB/sec    1.12  1677.9±45.00µs     9.9 MB/sec
parser/numpy/globals.py                    1.00    156.6±7.88µs    18.8 MB/sec    1.07    167.5±5.74µs    17.6 MB/sec
parser/pydantic/types.py                   1.00      3.3±0.07ms     7.7 MB/sec    1.14      3.8±0.10ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Merged by @charliermarsh on 2023-05-17 01:36_

---

_Closed by @charliermarsh on 2023-05-17 01:36_

---

_@jankatins reviewed on 2023-05-18 19:49_

---

_Review comment by @jankatins on `crates/ruff/resources/test/fixtures/tryceratops/TRY302.py`:75 on 2023-05-18 19:49_

Does that even compile? Catch e, but raise ex?

---

_@charliermarsh reviewed on 2023-05-18 19:58_

---

_Review comment by @charliermarsh on `crates/ruff/resources/test/fixtures/tryceratops/TRY302.py`:75 on 2023-05-18 19:58_

You would get a runtime error, since `ex` can't be resolved. But it wouldn't trigger TRY302.

---

_@john-h-k reviewed on 2023-05-18 21:25_

---

_Review comment by @john-h-k on `crates/ruff/resources/test/fixtures/tryceratops/TRY302.py`:75 on 2023-05-18 21:25_

Yeah it's intentionally invalid. It won't compile but also shouldn't trigger the lint

---
