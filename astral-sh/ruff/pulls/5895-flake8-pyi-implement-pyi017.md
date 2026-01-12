```yaml
number: 5895
title: "[`flake8-pyi`] Implement PYI017"
type: pull_request
state: merged
author: qdegraaf
labels:
  - rule
assignees: []
merged: true
base: main
head: feature/PYI017
created_at: 2023-07-19T19:25:17Z
updated_at: 2023-07-27T08:12:13Z
url: https://github.com/astral-sh/ruff/pull/5895
synced_at: 2026-01-12T15:55:19Z
```

# [`flake8-pyi`] Implement PYI017

---

_@qdegraaf_

## Summary

Implements `PYI017` or `Y017` from `flake8-pyi` plug-in. Mirrors [upstream implementation](https://github.com/PyCQA/flake8-pyi/blob/ceab86d16b822d12abae1d8087850d0bc66d2f52/pyi.py#L1039-L1048). It checks for any assignment with more than 1 target or an assignment to anything other than a name, and raises a violation for these in stub files. 

Couldn't find a clear and concise explanation for why this is to be avoided and what is preferred for attribute cases like: 

```python
a.b = int
```
So welcome some input there, to learn and to finish up the docs.

## Test Plan

Added test cases from upstream plug-in in a fixture (both `.py` and `.pyi`). Added a few more.

## Issue link

Refers: https://github.com/astral-sh/ruff/issues/848

---

_Comment by @github-actions[bot] on 2023-07-19 19:56_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     12.2±0.66ms     3.3 MB/sec    1.00     12.1±0.49ms     3.4 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.4±0.12ms     6.9 MB/sec    1.00      2.4±0.14ms     7.0 MB/sec
formatter/numpy/globals.py                 1.01   276.3±17.75µs    10.7 MB/sec    1.00   273.3±19.96µs    10.8 MB/sec
formatter/pydantic/types.py                1.00      5.2±0.30ms     4.9 MB/sec    1.01      5.2±0.27ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.02     17.8±0.57ms     2.3 MB/sec    1.00     17.5±0.65ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.4±0.25ms     3.8 MB/sec    1.02      4.5±0.24ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.04   614.4±70.23µs     4.8 MB/sec    1.00   591.3±39.91µs     5.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      8.1±0.31ms     3.2 MB/sec    1.00      7.9±0.30ms     3.2 MB/sec
linter/default-rules/large/dataset.py      1.02      9.1±0.31ms     4.5 MB/sec    1.00      9.0±0.38ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1866.4±87.37µs     8.9 MB/sec    1.01  1883.5±78.19µs     8.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   217.4±12.15µs    13.6 MB/sec    1.03   223.6±15.41µs    13.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.16ms     6.5 MB/sec    1.02      4.0±0.17ms     6.3 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06     11.9±0.16ms     3.4 MB/sec    1.00     11.2±0.15ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.06      2.3±0.05ms     7.2 MB/sec    1.00      2.2±0.03ms     7.7 MB/sec
formatter/numpy/globals.py                 1.02    248.4±5.37µs    11.9 MB/sec    1.00    242.8±8.26µs    12.2 MB/sec
formatter/pydantic/types.py                1.05      5.1±0.06ms     5.0 MB/sec    1.00      4.8±0.10ms     5.3 MB/sec
linter/all-rules/large/dataset.py          1.00     15.8±0.19ms     2.6 MB/sec    1.00     15.7±0.17ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.07ms     4.1 MB/sec    1.00      4.1±0.05ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    493.1±8.33µs     6.0 MB/sec    1.00    491.2±6.81µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.00      7.1±0.08ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     4.9 MB/sec    1.00      8.2±0.19ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1707.6±18.65µs     9.8 MB/sec    1.00  1699.3±15.27µs     9.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    191.9±3.27µs    15.4 MB/sec    1.00    191.9±3.06µs    15.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.04ms     7.0 MB/sec    1.00      3.6±0.03ms     7.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@zanieb reviewed on 2023-07-19 21:31_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/complex_assignment.rs`:33 on 2023-07-19 21:31_

I agree this message is a little tricky. I'm not sure users will understand what "non-name targets" means but I'm struggling to find a better message. Perhaps?

```suggestion
        format!("Stubs should not contain assignments to multiple targets or attributes.")
```

---

_@zanieb reviewed on 2023-07-19 21:31_

Thanks for working on this! It looks like a good start. Would you mind marking this as a draft until the documentation is done?

---

_@zanieb reviewed on 2023-07-19 21:32_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/complex_assignment.rs`:33 on 2023-07-19 21:32_

or perhaps clearer to say "to attributes or multiple targets" to avoid confusion about "multiple attributes"

---

_Comment by @qdegraaf on 2023-07-19 21:58_

> Thanks for working on this! It looks like a good start. Would you mind marking this as a draft until the documentation is done?

Sure! I'm probably going to need some help with that from someone who knows more about `flake8-pyi`or stub files though. Could you maybe mark it as such with a label of some sorts? Upstream plug-in does not have much more to say on it than:
```
Stubs should not contain assignments with multiple targets or non-name targets. E.g. T, S = TypeVar("T"), TypeVar("S") is disallowed, as is foo.bar = TypeVar("T").
```

And browsing through https://peps.python.org/pep-0484/#stub-files and https://typing.readthedocs.io/en/latest/source/stubs.html I can find no mention of it either

---

_Converted to draft by @qdegraaf on 2023-07-19 21:59_

---

_Comment by @zanieb on 2023-07-19 23:01_

@AlexWaygood might have some more details on the intent of the rule.

---

_Comment by @charliermarsh on 2023-07-19 23:16_

I can do it when I’m back on my computer, but I’d recommend looking through the Git history upstream. There‘s often additional context in the PR summaries and release notes.

---

_Comment by @qdegraaf on 2023-07-19 23:26_

Found this: https://github.com/PyCQA/flake8-pyi/pull/76 which links to https://github.com/python/typeshed/issues/4913 and then https://peps.python.org/pep-0613/ which seem to hint at 
```python
a.b = int
a = b = int
```
possibly not being valid explicit type aliases and therefore problematic but I'm not sure. 

Maybe @hauntsaninja can clarify? 

---

_Comment by @konstin on 2023-07-20 10:10_

To me, `a.b = int` in a stub files looks like an anti-pattern, it's definitely more readable if you'd `b: int` inside the class or module `a`

---

_Comment by @AlexWaygood on 2023-07-20 10:13_

In general, stub files should be thought of as "data files" for a type checker. They're not intended to ever be executed. As such, it's useful in general to enforce that only a subset of Python syntax is allowed in a stub file, to ensure that everything in the stub is 100% unambiguous when it comes to how the type checker is supposed to interpret it. (If it's not, there's not much point in having the stub files at all!)

So, without doing too much historical research into why we originally implemented this check at flake8-pyi, I feel like the rule it's enforcing is of a piece with this general principle.

---

_Comment by @AlexWaygood on 2023-07-20 10:40_

In other words: there's basically no reason anybody should ever _need_ to use multiple assignment, or assignments to non-`Name` targets, in a stub -- and if somebody's trying to do that for whatever reason, it likely indicates some kind of misunderstanding on their part.

We could probably improve the error message and docs over at flake8-pyi!

---

_Comment by @qdegraaf on 2023-07-20 13:40_

Thanks for the explanation! I'm happy with that as a `## Why is this bad?` snippet. Will add and open up for review again.

---

_Marked ready for review by @qdegraaf on 2023-07-20 13:56_

---

_Review comment by @AlexWaygood on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI017.pyi`:1 on 2023-07-20 13:59_

This isn't a _great_ example of something that's an okay assignment in a stub, since 99/100 times in a stub file this should _probably_ be:

```py
a: TypeAlias = int
```

Better examples of things that are okay in a stub file would be:

```py
var: int
a = var
```

or

```py
class Foo:
    def method(self) -> list[int]: ...

foo: Foo
a = foo.method
```

---

_Review comment by @AlexWaygood on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI017.pyi`:16 on 2023-07-20 13:59_

This would be flagged by ruff's existing PYI026 check (it should be `i: TypeAlias = int | str`), so again it's not a _great_ example of an assignment that's okay in a stub file

---

_@AlexWaygood reviewed on 2023-07-20 14:00_

---

_@AlexWaygood reviewed on 2023-07-20 14:12_

Tests and docs look good! (I'm not qualified to review rust code)

---

_@zanieb approved on 2023-07-20 15:43_

Thanks for looking it over @AlexWaygood !

This looks good to me. I'd like to include another maintainer to give it a once over but then it should be ready cc @charliermarsh 

---

_Review requested from @charliermarsh by @zanieb on 2023-07-20 15:44_

---

_Label `rule` added by @charliermarsh on 2023-07-20 16:29_

---

_Merged by @charliermarsh on 2023-07-20 16:35_

---

_Closed by @charliermarsh on 2023-07-20 16:35_

---

_Branch deleted on 2023-07-27 08:12_

---
