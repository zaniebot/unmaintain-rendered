```yaml
number: 4694
title: "[`pycodestyle`] Add blank line(s) rules (`E301`, `E302`, `E303`, `E304`, `E305`, `E306`)"
type: pull_request
state: closed
author: hoel-bagard
labels: []
assignees: []
draft: true
base: main
head: add_blank_lines_E30
created_at: 2023-05-28T17:08:35Z
updated_at: 2023-11-16T13:55:49Z
url: https://github.com/astral-sh/ruff/pull/4694
synced_at: 2026-01-10T23:40:55Z
```

# [`pycodestyle`] Add blank line(s) rules (`E301`, `E302`, `E303`, `E304`, `E305`, `E306`)

---

_Pull request opened by @hoel-bagard on 2023-05-28 17:08_

## Summary

This PR is part of https://github.com/charliermarsh/ruff/issues/2402, it adds the `E301`, `E302`, `E303`, `E304`, `E305`, `E306` error rules along with their fixes.

A first attempt at implementing `E305` using physical lines was done [here](https://github.com/charliermarsh/ruff/pull/4635), however since some operations are expensive when using physical lines, this implementation uses logical lines.
 
To be able to use `NonLogicalNewline` to detect blank lines, I have removed the check explicitly skipping them. I have modified the existing rules so that their behavior remains unchanged.

## Test Plan
The test fixture uses [the one from pycodestyle](https://github.com/PyCQA/pycodestyle/blob/main/testsuite/E30.py
) as is, but the notes in the file about which line should generate which error do not fully match the rules nor the implementation. I therefore did my best to manually check that what I did matches the rules/implementation in a way that makes sense.

For example, the rule [E306](https://www.flake8rules.com/rules/E306.html)'s example is not detected by pycodestyle, but this [false positive](https://github.com/PyCQA/pycodestyle/issues/1011) is.

---

_Comment by @github-actions[bot] on 2023-05-28 17:43_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.22      8.4±0.05ms     4.9 MB/sec    1.00      6.8±0.02ms     6.0 MB/sec
formatter/numpy/ctypeslib.py               1.28   1790.5±4.21µs     9.3 MB/sec    1.00   1401.5±3.59µs    11.9 MB/sec
formatter/numpy/globals.py                 1.40    195.5±2.26µs    15.1 MB/sec    1.00    139.4±1.83µs    21.2 MB/sec
formatter/pydantic/types.py                1.44      4.0±0.01ms     6.4 MB/sec    1.00      2.8±0.01ms     9.2 MB/sec
linter/all-rules/large/dataset.py          1.00     14.2±0.15ms     2.9 MB/sec    1.02     14.5±0.05ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.7 MB/sec    1.01      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    363.5±1.66µs     8.1 MB/sec    1.01    366.6±1.78µs     8.0 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.3±0.06ms     4.1 MB/sec    1.00      6.2±0.01ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      7.2±0.02ms     5.7 MB/sec    1.02      7.3±0.01ms     5.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1467.8±3.55µs    11.3 MB/sec    1.05   1545.3±3.13µs    10.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.0±0.26µs    18.7 MB/sec    1.04    164.4±0.12µs    18.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.01ms     8.0 MB/sec    1.04      3.3±0.01ms     7.7 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.20     12.1±0.54ms     3.4 MB/sec    1.00     10.1±0.39ms     4.0 MB/sec
formatter/numpy/ctypeslib.py               1.29      2.6±0.12ms     6.4 MB/sec    1.00      2.0±0.09ms     8.2 MB/sec
formatter/numpy/globals.py                 1.41   287.3±16.46µs    10.3 MB/sec    1.00   203.7±11.31µs    14.5 MB/sec
formatter/pydantic/types.py                1.38      5.6±0.24ms     4.5 MB/sec    1.00      4.1±0.17ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.00     19.2±0.64ms     2.1 MB/sec    1.03     19.8±0.64ms     2.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.1±0.24ms     3.3 MB/sec    1.01      5.1±0.23ms     3.3 MB/sec
linter/all-rules/numpy/globals.py          1.02   618.3±32.51µs     4.8 MB/sec    1.00   608.2±27.85µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.5±0.34ms     3.0 MB/sec    1.00      8.5±0.30ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00     10.4±0.67ms     3.9 MB/sec    1.06     11.0±0.58ms     3.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.06ms     7.5 MB/sec    1.00      2.2±0.08ms     7.5 MB/sec
linter/default-rules/numpy/globals.py      1.04   271.6±15.17µs    10.9 MB/sec    1.00    261.1±9.81µs    11.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.7±0.16ms     5.4 MB/sec    1.01      4.8±0.22ms     5.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @MichaReiser on 2023-05-28 19:44_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:324 on 2023-05-30 07:35_

Nit: `len` gives you the number of bytes and not the number of blank characters. The two are different for non-ascii characters. E.g. emojis are commonly represented as 4 UTF-8 bytes, but they only form 1-2 characters.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:350 on 2023-05-30 07:37_

Please avoid introducing new calls to `deprecated` methods. Use `Fix::automatic`, `Fix::manual` or Fix::suggested` instead.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:343 on 2023-05-30 07:38_

I believe this check will fail if `prev_line` is an empty line even if it is the first method in the class. Or is this intended?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:101 on 2023-05-30 07:42_

Is this example correct? Isn't the rule testing for extraneous blank lines (and not too few blank lines)?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:84 on 2023-05-30 07:42_

Is the title correct? Isn't it testing for missing blank lines?

---

_@MichaReiser reviewed on 2023-05-30 07:50_

Thanks for working on this. I haven't reviewed the changes in full details yet because I first want to discuss the overall direction. 

I think using tokens to detect the blank lines is preferred over the physical lines. However, I would refrain from changing the definition of `LogicalLine`s to include empty lines too because it would then contradict [Python's official definition of logical lines](https://docs.python.org/3/reference/lexical_analysis.html#logical-lines) (logical lines cannot be empty), which would be rather confusing. 

Is my understanding correct, that the rule only tests for blank lines before and after functions, methods, and classes? 

If so, then I think it might be *easiest* to rewrite the rule as an AST based rule that runs for every top level function (you'll need to pass the next statement too, to avoid duplicates), and classes. It can use the `Locator` and then count the leading/trailing new lines.  

If the rules instead test for proper spacing between all statements, then I think it's best if we would build out a new `TokenVisitor` infrastructure that allows to runs some logic for every token in the program. Both `LogicalLine`s and your new rule could implement the `TokenVisitor` trait that has a single `visit_token(&mut self, token: &Tok, range: TextRange)` method. The visitor can track its own state internally and can trigger its rules when reaching a certain token (end of a logical line, end of a blank line). I would prefer this over changing the semantics of logical line because it still visits the tokens only once and provides us with a more extensible solution that could potentially even allow the other physical lines rule to work on tokens, rather than using the `UniversalNewlinesIterator`. @charliermarsh what do you think, would you have time to build such infrastructure? 

---

_Review requested from @charliermarsh by @MichaReiser on 2023-05-30 07:51_

---

_Comment by @hoel-bagard on 2023-05-30 08:06_

> Is my understanding correct, that the rule only tests for blank lines before and after functions, methods, and classes?


The rules check for proper spacing between all statements. For example, the following would generate an error:

```python
a = 1



print(a)
```

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:343 on 2023-05-30 08:18_

Are you talking about a case like this ?

```python
class A:

    def b():
        pass
```

---

_@hoel-bagard reviewed on 2023-05-30 08:18_

---

_@hoel-bagard reviewed on 2023-05-30 08:19_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:324 on 2023-05-30 08:19_

Fixed in 47063d4b7536e5624cf96b6cc150a62ad54a417f

---

_@hoel-bagard reviewed on 2023-05-30 08:19_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:350 on 2023-05-30 08:19_

Fixed in 2b4e95954dd8f47317eaf7c3e46ca125bb133ffd

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:101 on 2023-05-30 08:24_

My understanding is that some rules test for too few blank lines ([E301](https://www.flake8rules.com/rules/E301.html), [E302](https://www.flake8rules.com/rules/E302.html), [E305](https://www.flake8rules.com/rules/E305.html) and [E306](https://www.flake8rules.com/rules/E306.html)) and others for extraneous blank lines ( [E303](https://www.flake8rules.com/rules/E303.html), [E304](https://www.flake8rules.com/rules/E304.html))

I'm going to fix the titles/descriptions accordingly.

---

_@hoel-bagard reviewed on 2023-05-30 08:24_

---

_@hoel-bagard reviewed on 2023-05-30 09:05_

---

_Review comment by @hoel-bagard on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/blank_lines.rs`:84 on 2023-05-30 09:05_

Fixed in [2fc94f8](https://github.com/charliermarsh/ruff/pull/4694/commits/2fc94f80fcaed9bc599842b91172c8a322671699)

---

_@charliermarsh reviewed on 2023-06-02 15:40_

I haven't reviewed `blank_lines.rs` extensively, but in pycodestyle, it seems like they just track the number of preceding blank lines in the logical lines builder (but don't emit empty lines), and then when they test the next non-blank logical line, they use that "number of preceding blank lines" state to enforce these rules.

Is that a viable approach for us? To avoid emitting blank lines, and instead track state as we build + include the blank-line context on the next logical line?

---

_Comment by @hoel-bagard on 2023-06-03 09:48_

That's essentially what I did (when the logical line is empty I just increment a counter and return), so I think that would work.
I tracked quite a few variables, so that might need to be refined.

---

_Review requested from @charliermarsh by @charliermarsh on 2023-06-13 02:08_

---

_Comment by @charliermarsh on 2023-06-13 03:20_

I looked into merging this but it doesn't seem to properly handle nested definitions -- this snippet is clean under pycodestyle, but yields violations here:

```py
class C:
    def f():
        pass


def f():
    def f():
        pass
```

Yields:

```
foo.py:2:5: E306 [*] Expected 1 blank line before a nested definition, found 0
foo.py:7:5: E302 [*] Expected 2 blank lines, found 0
foo.py:7:5: E306 [*] Expected 1 blank line before a nested definition, found 0
```


---

_Comment by @hoel-bagard on 2023-06-13 03:27_

@charliermarsh Yes, the rules and examples of pycodestyles do not match the actual implementation.

For example, the rule [E306](https://www.flake8rules.com/rules/E306.html)'s example is not detected by pycodestyle (as you've pointed out above), but this [false positive](https://github.com/PyCQA/pycodestyle/issues/1011) is.

---

_Comment by @hoel-bagard on 2023-06-13 03:29_

I haven't had time to look into tracking the number of blank lines in the logical lines builder yet, but I'll try to do it once I have time.

---

_Comment by @hoel-bagard on 2023-06-16 14:35_

I've moved the tracking of blank lines into the logical lines builder, so blank lines are no longer emitted as logical lines.
However, I'm not sure how to track the number of blank characters there. I used to have `line.text().chars().count()`, but I don't think I can access the actual text from within the builder, so instead I used the tokens.

---

_Comment by @hoel-bagard on 2023-07-07 03:00_

I started reviewing the output on the pycodestyle fixture, and found a major issue with this PR. Pycodestyle's E3 rules take into account comments. For example:
```python
a = 1




# a



# a
class A:
    pass
```

Pycodestyle output:
```
example.py:6:1: E303 too many blank lines (4)
example.py:10:1: E303 too many blank lines (3)
example.py:11:1: E302 expected 2 blank lines, found 4
```

However, since comment only lines are not logical lines, I can't reproduce this output. Moreover, this causes the implementation of the autofix to mess up the code.

Without making empty lines/comment only lines into logical lines (or using physical lines), I don't think these rules can be implemented.
@charliermarsh Would you have a recommendation on how I could proceed ?

---

_Comment by @MichaReiser on 2023-07-07 09:16_

> If the rules instead test for proper spacing between all statements, then I think it's best if we would build out a new TokenVisitor infrastructure that allows to runs some logic for every token in the program. Both LogicalLines and your new rule could implement the TokenVisitor trait that has a single visit_token(&mut self, token: &Tok, range: TextRange) method. The visitor can track its own state internally and can trigger its rules when reaching a certain token (end of a logical line, end of a blank line). I would prefer this over changing the semantics of logical line because it still visits the tokens only once and provides us with a more extensible solution that could potentially even allow the other physical lines rule to work on tokens, rather than using the UniversalNewlinesIterator. @charliermarsh what do you think, would you have time to build such infrastructure?

@charliermarsh what do you think?


---

_Comment by @akx on 2023-07-24 11:52_

Could we move this forward somehow? I was about to start implementing these rules but luckily checked whether there was momentum on it already :)

---

_Comment by @hoel-bagard on 2023-07-24 14:25_

I'll finish cleaning the fixture to make it easier to check that the implementation works (or not).

But without implementing something like the proposed TokenVisitor, it's difficult to progress.

---

_Converted to draft by @hoel-bagard on 2023-08-26 12:12_

---

_Comment by @hoel-bagard on 2023-09-25 10:57_

@MichaReiser Did something like the `TokenVisitor` get implemented since you talked about it ? I would like to finish the PR, but I can't do it using logical lines, since some cases do not include any logical line and should still emit an error (see the example below).

```python
# comment




# comment
```

---

_Comment by @MichaReiser on 2023-09-25 12:09_

> @MichaReiser Did something like the `TokenVisitor` get implemented since you talked about it ? I would like to finish the PR, but I can't do it using logical lines, since some cases do not include any logical line and should still emit an error (see the example below).
> 
> ```python
> # comment
> 
> 
> 
> 
> # comment
> ```

Not the way I described it but there is a set of token-based rules that operate only on the tokens/indexer. Would that meet your needs?

https://github.com/astral-sh/ruff/blob/b34278e0cd25a24b69f4786897ca80c19fc3d5f1/crates/ruff_linter/src/checkers/tokens.rs#L22-L197

---

_Comment by @hoel-bagard on 2023-09-25 12:27_

I think so, thank you.
Is there something I should keep in mind while using the tokens ? For example, in my attempt using physical lines it ended up too slow to be usable.

---

_Comment by @MichaReiser on 2023-09-25 13:44_

> I should keep in mind while using the tokens ? For example, in my attempt using physical lines it ended up too slow to be usable.

Try to perform as many operations as possible on the tokens directly. Only fall back to inspect the source code (or token content) if you must.


---

_Comment by @hoel-bagard on 2023-11-16 13:55_

Closed in favor of https://github.com/astral-sh/ruff/pull/8720.

---

_Closed by @hoel-bagard on 2023-11-16 13:55_

---
