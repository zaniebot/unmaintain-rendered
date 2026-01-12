```yaml
number: 6464
title: "Don't expand groups for trailing end-of-line comments"
type: pull_request
state: closed
author: charliermarsh
labels:
  - formatter
assignees: []
base: main
head: charlie/break
created_at: 2023-08-09T23:35:11Z
updated_at: 2023-08-11T15:58:42Z
url: https://github.com/astral-sh/ruff/pull/6464
synced_at: 2026-01-12T15:55:21Z
```

# Don't expand groups for trailing end-of-line comments

---

_@charliermarsh_

## Summary

This is a PR where we need to align on desired behavior. Right now, we expand a group when writing a trailing end-of-line comment, so e.g., given:

```python
{
    a: a  # a
    for c in e  # for # c 
}
```

We format this expression as:

```python
{
    a: a  # a
    for c in e  # for # c
}
```

Black, meanwhile, doesn't let trailing end-of-line comments expand, so formats as:

```python
{a: a for c in e}  # a  # for # c
```

Looking through the snapshot diff, this seems to strictly improve Black compatibility (and our current behavior seems like a significant deviation). Even the changes in our own fixtures now better resemble Black as verified in the playground.

I don't know that I have a super strong opinion on whether to use Black's behavior or our own, but I lead towards following Black's behavior by default.

## Test Plan

Weird mix on the similarity scores -- some went down a little, some went up a little.

Before:

- `zulip`: 0.99702
- `django`: 0.99784
- `warehouse`: 0.99585
- `build`: 0.75623
- `transformers`: 0.99469
- `cpython`: 0.75988
- `typeshed`: 0.74853

After:

- `zulip`: 0.99696
- `django`: 0.99785
- `warehouse`: 0.99569
- `build`: 0.75623
- `transformers`: 0.99481
- `cpython`: 0.75987
- `typeshed`: 0.74842


---

_@charliermarsh reviewed on 2023-08-09 23:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@expression__compare.py.snap`:141 on 2023-08-09 23:35_

(This now matches Black.)

---

_@charliermarsh reviewed on 2023-08-09 23:35_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@expression__dict_comp.py.snap`:131 on 2023-08-09 23:35_

This now matches Black.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@simple_cases__comments2.py.snap`:343 on 2023-08-09 23:36_

This is much closer to Black but still wrong due to the line suffixes not being included when determining the line length:

```python
lcomp = [
    element for element in collection if element is not None  # yup  # yup  # right
]
```

---

_@charliermarsh reviewed on 2023-08-09 23:36_

---

_Label `formatter` added by @charliermarsh on 2023-08-09 23:36_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-09 23:36_

---

_Review requested from @konstin by @charliermarsh on 2023-08-09 23:36_

---

_Comment by @github-actions[bot] on 2023-08-10 00:09_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.3±0.42ms     4.4 MB/sec    1.00      9.3±0.50ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00  1885.6±83.89µs     8.8 MB/sec    1.00  1879.4±141.52µs     8.9 MB/sec
formatter/numpy/globals.py                 1.05   229.2±16.86µs    12.9 MB/sec    1.00   219.2±12.62µs    13.5 MB/sec
formatter/pydantic/types.py                1.10      4.2±0.18ms     6.0 MB/sec    1.00      3.9±0.23ms     6.6 MB/sec
linter/all-rules/large/dataset.py          1.00     11.4±0.65ms     3.6 MB/sec    1.09     12.4±0.59ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.10ms     5.5 MB/sec    1.12      3.4±0.17ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   461.9±19.82µs     6.4 MB/sec    1.05   483.4±18.76µs     6.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.2±0.28ms     4.1 MB/sec    1.06      6.6±0.39ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.04      6.4±0.23ms     6.4 MB/sec    1.00      6.1±0.23ms     6.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.02  1370.0±68.25µs    12.2 MB/sec    1.00  1345.0±86.95µs    12.4 MB/sec
linter/default-rules/numpy/globals.py      1.03    168.5±9.33µs    17.5 MB/sec    1.00   162.8±11.67µs    18.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.12ms     8.6 MB/sec    1.01      3.0±0.15ms     8.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.03ms     4.1 MB/sec    1.00      9.9±0.04ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   1879.9±7.87µs     8.9 MB/sec    1.01   1897.7±9.42µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    197.4±1.80µs    14.9 MB/sec    1.01    199.1±5.09µs    14.8 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.01ms     6.0 MB/sec    1.01      4.2±0.02ms     6.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.04ms     3.3 MB/sec    1.00     12.5±0.04ms     3.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.01ms     4.8 MB/sec    1.01      3.5±0.02ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    361.5±3.60µs     8.2 MB/sec    1.01    363.6±3.56µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.02ms     3.9 MB/sec    1.00      6.5±0.02ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.02ms     6.0 MB/sec    1.00      6.8±0.04ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1396.5±12.89µs    11.9 MB/sec    1.00   1399.4±7.36µs    11.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    143.0±1.17µs    20.6 MB/sec    1.00    143.7±3.45µs    20.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.02ms     8.4 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-08-10 06:01_

Interesting to see this change in action. 

Our formatter goal is to have (close) Black parity for already formatted code, but we intentionally allow deviation for unformatted code (using Black and Ruff in the same project isn't a use case we want to support). I don't expect this change to help increase parity for already Black formatted code because Black already have moved all comments to the end of the line. That's why I'm surprised that it affects the similarity index at all (the overall change seems neutral). We'll have to manually investigate the changes before moving forward. I suspect these are accidental changes where we have a different comment placement, and the lines now happen to be short enough to place the comment in the same position as Black.

The main question to me is whether the fewest vertical lines by collapsing comments improve readability and express the author's intentions. In my view, this isn't the case because comments are moved away from where I intentionally placed them:

### Pragma comments

```python
def test(
	a, #  type-ignore
	b
):
	pass
```

This now gets formatted as 

```python
def test(a, b): #  type-ignore
	pass
```

which not only suppresses typing errors for `a`, but also `b`. I now have to go back, expand the parameters and add a trailing comma after `b` if I want to keep the suppression specific to `a` (what I initially intended by adding the comment at the end of line `a`). This is a lot of work.

The workaround with adding trailing commas only works in positions where they are supported. For example, the following doesn't work.

```python
(
    call(has_type_error) # type-ignore
    and another_call()
)
```

gets formatted as 

```python
(call(has_type_error) and another_call())  # type-ignore
```

which suppresses typing errors in `another_call` too. I doubt anyone would bother enough (until they have a bug because of the missed type checker error) to undo Black's change and add suppression comments:

```python
# This doesn't work, probably because we have collapsed comments
(
    call(has_type_error) # fmt: skip # type-ignore 
    and another_call()
)

(
    # fmt: off
    call(has_type_error)  # type-ignore
    # fmt: on
    and another_call()
)
```

Ruff won't be able to provide this escape hatch because it doesn't support suppression comments on the expression level (you would need to suppress the whole statement). And suppression comments are a net negative in my view. 


### Comment context

This is related to pragma comments. Moving comments too far means that the comments loos their context. 

```python
call(
	[], #  TODO fill in values
	b,
	[]
)
```

gets formatted to

```python
call([], b, [])  #  TODO fill in values
```

It is now unclear if I have to fill in the values in the first or last list. Again, I can work around this by adding a trailing comma, but that's a lot of work to make the formatter respect my comment placement.

Similar

```python
call(
	[], # Empty because of X
	c,
	[], # Empty because of Y
)
```

gets formatted as 

```python
call([], c, [])  # Empty because of X  # Empty because of Y
```

Again, we have the context problem, but I also dislike collapsed comments `# comment A # comment B` because they look strange. It also comes with problems where pragma comments stop working if tools don't support nested comments, as we've seen with Black's `fmt: skip` pragma comment. 





---

_@konstin approved on 2023-08-10 10:06_

i'm not really opinionated either way, both styles have their advantages and disadvantages to me

---

_Comment by @charliermarsh on 2023-08-11 15:58_

We're not making this change, for now at least.

---

_Closed by @charliermarsh on 2023-08-11 15:58_

---
