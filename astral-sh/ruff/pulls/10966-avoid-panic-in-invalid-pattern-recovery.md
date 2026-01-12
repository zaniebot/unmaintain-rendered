```yaml
number: 10966
title: Avoid panic in invalid pattern recovery
type: pull_request
state: merged
author: dhruvmanila
labels:
  - bug
  - parser
  - fuzzer
assignees: []
merged: true
base: dhruv/parser
head: dhruv/avoid-pattern-recovery-panic
created_at: 2024-04-16T04:46:15Z
updated_at: 2024-04-16T10:40:28Z
url: https://github.com/astral-sh/ruff/pull/10966
synced_at: 2026-01-12T15:55:34Z
```

# Avoid panic in invalid pattern recovery

---

_@dhruvmanila_

## Summary

This PR avoids the panic when trying to recover from an invalid pattern.

The panic occurs because there's no way to represent `x as y` syntax as a Python expression. There were few cases which were not accounted for when working on this. 

The cases where it would panic are the ones where only a subset of pattern is valid in a position but all patterns are valid in a nested position of the mentioned top-level pattern. 

For example, consider a mapping pattern and the following points:
* Key position can only be a `MatchValue` and `MatchSingleton`
* `MatchClass` can contain a `MatchAs` as an argument
* `MatchAs` pattern with both pattern and name (`a as b`) cannot be converted to a valid expression

Considering this, if a mapping pattern has a class pattern with an `as` pattern as one of it's argument, the parser would panic.

```py
match subject:
	case {Foo(a as b): 1}: ...
#        ^^^^^^^^^^^^^^^^ mapping pattern
#         ^^^^^^^^^^^     mapping pattern key, class pattern
#             ^^^^^^      as pattern with both pattern and name
```

The same case is for complex literal patterns as well.

I'm not sure what the recovery should look like but for now it's better to avoid the panic and create an invalid expression node in it's place.

## Test Plan

Add new test cases and update the snapshots.

---

_Label `bug` added by @dhruvmanila on 2024-04-16 04:46_

---

_Label `parser` added by @dhruvmanila on 2024-04-16 04:46_

---

_Label `fuzzer` added by @dhruvmanila on 2024-04-16 04:46_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-16 04:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:176 on 2024-04-16 04:55_

This isn't required now that we don't panic.

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/parser/pattern.rs`:216 on 2024-04-16 04:56_

Same here, we don't panic so no need for the comment.

---

_@dhruvmanila reviewed on 2024-04-16 04:56_

---

_Comment by @github-actions[bot] on 2024-04-16 05:05_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+5 -52 violations, +0 -0 fixes in 3 projects; 41 projects unchanged)

<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/829b2a75c402d0d0abd7e37ff201fbdfd949d857/src/bokeh/events.py#L182'>src/bokeh/events.py:182:9:</a> PLW0642 Invalid assignment to `cls` argument in class method
</pre>

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+0 -51 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/categorical.py#L568'>pandas/core/arrays/categorical.py:568:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1083'>pandas/core/arrays/datetimelike.py:1083:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1098'>pandas/core/arrays/datetimelike.py:1098:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1099'>pandas/core/arrays/datetimelike.py:1099:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1127'>pandas/core/arrays/datetimelike.py:1127:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1136'>pandas/core/arrays/datetimelike.py:1136:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1147'>pandas/core/arrays/datetimelike.py:1147:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1149'>pandas/core/arrays/datetimelike.py:1149:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1154'>pandas/core/arrays/datetimelike.py:1154:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1203'>pandas/core/arrays/datetimelike.py:1203:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1205'>pandas/core/arrays/datetimelike.py:1205:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1221'>pandas/core/arrays/datetimelike.py:1221:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1278'>pandas/core/arrays/datetimelike.py:1278:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1292'>pandas/core/arrays/datetimelike.py:1292:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1498'>pandas/core/arrays/datetimelike.py:1498:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L1717'>pandas/core/arrays/datetimelike.py:1717:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L2144'>pandas/core/arrays/datetimelike.py:2144:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L2166'>pandas/core/arrays/datetimelike.py:2166:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L455'>pandas/core/arrays/datetimelike.py:455:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L806'>pandas/core/arrays/datetimelike.py:806:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/arrays/datetimelike.py#L997'>pandas/core/arrays/datetimelike.py:997:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/frame.py#L7715'>pandas/core/frame.py:7715:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/frame.py#L7728'>pandas/core/frame.py:7728:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/frame.py#L8071'>pandas/core/frame.py:8071:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/frame.py#L8083'>pandas/core/frame.py:8083:21:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/frame.py#L8123'>pandas/core/frame.py:8123:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/generic.py#L3407'>pandas/core/generic.py:3407:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/generic.py#L3568'>pandas/core/generic.py:3568:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/generic.py#L7902'>pandas/core/generic.py:7902:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/generic.py#L7905'>pandas/core/generic.py:7905:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/generic.py#L7908'>pandas/core/generic.py:7908:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/generic.py#L9165'>pandas/core/generic.py:9165:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/generic.py#L9172'>pandas/core/generic.py:9172:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/generic.py#L9175'>pandas/core/generic.py:9175:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/groupby/grouper.py#L249'>pandas/core/groupby/grouper.py:249:13:</a> PLW0642 Invalid assignment to `cls` argument in class method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/indexes/base.py#L1329'>pandas/core/indexes/base.py:1329:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/indexes/base.py#L2130'>pandas/core/indexes/base.py:2130:9:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/indexes/base.py#L2913'>pandas/core/indexes/base.py:2913:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/indexes/base.py#L3061'>pandas/core/indexes/base.py:3061:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/indexes/base.py#L3293'>pandas/core/indexes/base.py:3293:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/indexes/base.py#L4384'>pandas/core/indexes/base.py:4384:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/indexes/base.py#L5958'>pandas/core/indexes/base.py:5958:17:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/indexes/base.py#L5962'>pandas/core/indexes/base.py:5962:20:</a> PLW0642 Invalid assignment to `self` argument in instance method
- <a href='https://github.com/pandas-dev/pandas/blob/888b6bc1bb3fdf203dda64e50336db4eb9b1e778/pandas/core/internals/blocks.py#L1122'>pandas/core/internals/blocks.py:1122:13:</a> PLW0642 Invalid assignment to `self` argument in instance method
... 7 additional changes omitted for project
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+5 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/1abd356a910aa4a36a763f0a7834d2d5dddbd6fb/scripts/lib/zulip_tools.py#L601'>scripts/lib/zulip_tools.py:601:1:</a> E302 [*] Expected 2 blank lines, found 0
+ <a href='https://github.com/zulip/zulip/blob/1abd356a910aa4a36a763f0a7834d2d5dddbd6fb/zerver/decorator.py#L556'>zerver/decorator.py:556:1:</a> E302 [*] Expected 2 blank lines, found 0
+ <a href='https://github.com/zulip/zulip/blob/1abd356a910aa4a36a763f0a7834d2d5dddbd6fb/zerver/lib/validator.py#L268'>zerver/lib/validator.py:268:1:</a> E302 [*] Expected 2 blank lines, found 0
+ <a href='https://github.com/zulip/zulip/blob/1abd356a910aa4a36a763f0a7834d2d5dddbd6fb/zproject/config.py#L33'>zproject/config.py:33:1:</a> E302 [*] Expected 2 blank lines, found 0
+ <a href='https://github.com/zulip/zulip/blob/1abd356a910aa4a36a763f0a7834d2d5dddbd6fb/zproject/config.py#L56'>zproject/config.py:56:1:</a> E302 [*] Expected 2 blank lines, found 0
</pre>

</p>
</details>
<details><summary>Changes by rule (2 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLW0642 | 52 | 0 | 52 | 0 | 0 |
| E302 | 5 | 5 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_mapping_pattern.py.snap`:415 on 2024-04-16 06:13_

I'm surprised that the parsing both drops the `a` and `as b` parts. Is this intentional?

---

_@MichaReiser approved on 2024-04-16 06:22_

Would you mind opening an issue for reworking the pattern recovery? I'm okay removing the panic branch for the release but I think we need to rething the pattern recovery holistically and either recover less (e.g. return the left hand side if positioned at a `-` or `+` and the left isn't a number), or change the AST structure to allow for better recovery.

---

_@dhruvmanila reviewed on 2024-04-16 06:43_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/tests/snapshots/invalid_syntax@statements__match__invalid_mapping_pattern.py.snap`:415 on 2024-04-16 06:43_

Yes, we're replacing the `as` pattern with an invalid expression node when the conversion takes place in pattern error recovery. If not, the parser would panic.

---

_Merged by @dhruvmanila on 2024-04-16 10:40_

---

_Closed by @dhruvmanila on 2024-04-16 10:40_

---

_Branch deleted on 2024-04-16 10:40_

---
