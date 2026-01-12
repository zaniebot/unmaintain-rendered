```yaml
number: 6900
title: Add UnusedConditionalExpressionResult rule
type: pull_request
state: closed
author: wookie184
labels: []
assignees: []
draft: true
base: main
head: unused-conditional-expression-result-rule
created_at: 2023-08-26T17:06:25Z
updated_at: 2023-12-12T21:44:19Z
url: https://github.com/astral-sh/ruff/pull/6900
synced_at: 2026-01-12T15:55:22Z
```

# Add UnusedConditionalExpressionResult rule

---

_@wookie184_

Closes #6850

I've just used RUF999 as the rule number temporarily.
<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This rule raises when a conditional expression (aka ternary) is used but the result of them is not used, e.g.
```python
print("hello") if cond else print("bye")
```
In this case a normal if-else statement should be used
```python
if cond:
    print("hello")
else:
    print("bye")
```


## Todo:
- [ ] Add tests
- [ ] Add docs
- [ ] Pick a rule number
- [ ] Add autofix

---

_Comment by @codspeed-hq[bot] on 2023-08-26 17:16_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/wookie184:unused-conditional-expression-result-rule)

### Merging #6900 will **not alter performance**

<sub>Comparing <code>wookie184:unused-conditional-expression-result-rule</code> (e3e04ef) with <code>main</code> (eae59cf)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_Comment by @github-actions[bot] on 2023-08-26 17:17_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+1, -0, 0 error(s))

<details><summary>ibis (+1, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/ibis-project/ibis/blob/f5da1a78916fd859b3785fa0ad6752d842b01649/ibis/backends/base/sql/compiler/query_builder.py#L338'>ibis/backends/base/sql/compiler/query_builder.py:338:17:</a> RUF999 Conditional expression with unused result; use an if-else statement instead
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF999 | 1 | 1 | 0 |
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @wookie184 on 2023-08-27 11:18_

> + [ibis/backends/base/sql/compiler/query_builder.py:338:17:](https://github.com/ibis-project/ibis/blob/f5da1a78916fd859b3785fa0ad6752d842b01649/ibis/backends/base/sql/compiler/query_builder.py#L338) RUF999 Conditional expression with unused result; use an if-else statement instead


Interestingly, the usage here is 
```python
buf.write(",\n       ") if i else buf.write("\n")
```
Which could be better written still using a ternary as 
```python
buf.write(",\n       " if i else "\n")
```
Though I think this might be quite hard to autofix generally so I think rewriting to a normal if-else would still be good.

---

_Comment by @Avasam on 2023-09-02 23:41_

I was thinking the same looking at your example here: if both results of a ternary are the same method call. Then it could be merged.

Maybe as a different rule ("prefer ternary for assignment" ?) that has priority over what you made here. It would also catch ternaries that are assigned, ie `result = int(a) if cond else int(b)`.

It would also allow the devs to choose which auto fix they prefer by enabling/disabling that rule.

---

_Comment by @charliermarsh on 2023-12-12 21:44_

I'm gonna close for now, but happy to reopen if someone is eager to make a case for this.

---

_Closed by @charliermarsh on 2023-12-12 21:44_

---
