```yaml
number: 10537
title: "Fix `PD002` autofix for chained calls"
type: pull_request
state: closed
author: augustelalande
labels: []
assignees: []
base: main
head: pd002-fix-bug
created_at: 2024-03-23T17:50:37Z
updated_at: 2024-03-31T15:59:10Z
url: https://github.com/astral-sh/ruff/pull/10537
synced_at: 2026-01-10T22:47:02Z
```

# Fix `PD002` autofix for chained calls

---

_Pull request opened by @augustelalande on 2024-03-23 17:50_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This error was found browsing https://github.com/qarmin/Automated-Fuzzer/actions/runs/8396966850. Which failed when trying to autofix the PD002 violation in the following code:
```python
x.reorder_levels().sort_index(inplace=True)
```
the autofix transformed the code to
```python
x.reorder_levels() = x.reorder_levels().sort_index()
```
to address this I implemented a function to iterate down the list of chain calls and find the first attribute which is not a call and use that as the target. I'm guessing this is probably already implemented somewhere but I couldn't find it.

The new autofix is:
```python
x = x.reorder_levels().sort_index()
```

## Test Plan

Added misbehaving code to the test fixture.


---

_Comment by @codspeed-hq[bot] on 2024-03-23 18:02_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/augustelalande:pd002-fix-bug)

### Merging #10537 will **degrade performances by 5.29%**

<sub>Comparing <code>augustelalande:pd002-fix-bug</code> (8683a98) with <code>main</code> (895d9df)</sub>



### Summary

`❌ 2` regressions
`✅ 28` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/augustelalande:pd002-fix-bug)._

### Benchmarks breakdown

|     | Benchmark | `main` | `augustelalande:pd002-fix-bug` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ❌ | `linter/default-rules[large/dataset.py]` | 19.2 ms | 20.3 ms | -5.29% |
| ❌ | `linter/default-rules[unicode/pypinyin.py]` | 1.5 ms | 1.6 ms | -4.59% |


---

_Comment by @augustelalande on 2024-03-23 18:03_

Actually thinking about it now (after I wrote the fix lol). The example code doesn't make sense unless the chained calls are also inplace calls, since otherwise the result is not assigned to anything and gets lost. But then if it's a chain of inplace operations it also doesn't make sense since inplace calls return `None` and so don't allow chaining.

The only reason you would have a call chain ending in an inplace call, is if the other calls are also inplace, but also return the modified object, which I don't think exist in pandas.

---

_Closed by @augustelalande on 2024-03-23 18:03_

---

_Branch deleted on 2024-03-31 15:59_

---
