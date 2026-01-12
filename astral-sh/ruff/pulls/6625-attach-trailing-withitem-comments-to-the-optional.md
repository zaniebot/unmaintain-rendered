```yaml
number: 6625
title: "Attach trailing `WithItem` comments to the optional variable"
type: pull_request
state: closed
author: charliermarsh
labels:
  - formatter
assignees: []
draft: true
base: main
head: charlie/with-item
created_at: 2023-08-16T17:33:27Z
updated_at: 2023-08-31T21:04:18Z
url: https://github.com/astral-sh/ruff/pull/6625
synced_at: 2026-01-12T15:55:22Z
```

# Attach trailing `WithItem` comments to the optional variable

---

_@charliermarsh_

## Summary

This PR modifies our comment handling of `WithItem` nodes by attaching trailing comments to the optional variable rather than the `WithItem` itself, if there are no clear delimiters between the comment and the optional variable.

As a concrete example, given:

```python
with (
    (a
    # trailing own line comment
    )
    as # trailing as same line comment
    b # trailing b same line comment
): ...
```

We now format as:

```python
with (
    a
    # trailing own line comment
) as (  # trailing as same line comment
    b  # trailing b same line comment
):
    ...
```

Prior to this PR, we formatted as:

```python
with (
    a
    # trailing own line comment
) as (  # trailing as same line comment
    b
):  # trailing b same line comment
    ...
```

## Test Plan

`cargo test`


---

_Label `formatter` added by @charliermarsh on 2023-08-16 17:39_

---

_Marked ready for review by @charliermarsh on 2023-08-16 17:41_

---

_Comment by @github-actions[bot] on 2023-08-16 18:02_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00      3.3±0.17ms    12.4 MB/sec     1.06      3.5±0.18ms    11.7 MB/sec
formatter/numpy/ctypeslib.py               1.00   655.2±29.05µs    25.4 MB/sec     1.14   744.5±48.64µs    22.4 MB/sec
formatter/numpy/globals.py                 1.00     69.2±4.72µs    42.6 MB/sec     1.10     76.1±5.82µs    38.8 MB/sec
formatter/pydantic/types.py                1.00  1303.1±101.72µs    19.6 MB/sec    1.11  1441.8±102.50µs    17.7 MB/sec
linter/all-rules/large/dataset.py          1.04     11.1±0.25ms     3.7 MB/sec     1.00     10.7±0.30ms     3.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.0±0.05ms     5.6 MB/sec     1.00      2.9±0.09ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   416.0±21.30µs     7.1 MB/sec     1.13   471.1±19.95µs     6.3 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.7±0.39ms     4.5 MB/sec     1.06      6.0±0.39ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.13ms     7.4 MB/sec     1.13      6.2±0.30ms     6.6 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1231.9±49.76µs    13.5 MB/sec     1.08  1333.6±57.93µs    12.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    152.8±8.21µs    19.3 MB/sec     1.08    165.7±7.63µs    17.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.09ms    10.0 MB/sec     1.10      2.8±0.11ms     9.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.7±0.02ms    11.1 MB/sec    1.00      3.7±0.02ms    11.1 MB/sec
formatter/numpy/ctypeslib.py               1.00    726.3±8.27µs    22.9 MB/sec    1.00    727.1±8.20µs    22.9 MB/sec
formatter/numpy/globals.py                 1.01     74.1±0.90µs    39.8 MB/sec    1.00     73.5±1.27µs    40.2 MB/sec
formatter/pydantic/types.py                1.00  1505.0±13.33µs    16.9 MB/sec    1.00  1504.2±16.96µs    17.0 MB/sec
linter/all-rules/large/dataset.py          1.00     12.5±0.07ms     3.2 MB/sec    1.00     12.6±0.10ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.5±0.02ms     4.7 MB/sec    1.00      3.5±0.01ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    371.6±8.67µs     7.9 MB/sec    1.00    373.4±4.38µs     7.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.07ms     3.9 MB/sec    1.00      6.6±0.07ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.05ms     5.8 MB/sec    1.01      7.1±0.04ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1472.4±11.00µs    11.3 MB/sec    1.00  1472.6±12.35µs    11.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    151.4±1.55µs    19.5 MB/sec    1.00    151.9±2.23µs    19.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.02ms     8.1 MB/sec    1.01      3.2±0.02ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-16 22:27_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1333 on 2023-08-17 06:35_

Nit: We could integrate this into the `if let Some(...)` below

```rust
if let Some(AnyNodeRef::WithItem(...) = comment.preceding_node() {
	
} else if let Some(following) = comment.following_node() {
	// First with item
	if comment.start() < with_item.start() {
		return CommentPlacement::dangling(comment.enclosing_node(), comment);
	}
}

CommentPlacement::Default(comment)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1314 on 2023-08-17 06:39_

Does this work as intended if you have

```python
with (
	a as 
	(((b))	 # comment
    )
	, c as d
): ...

```

---

_@MichaReiser reviewed on 2023-08-17 06:44_

---

_@charliermarsh reviewed on 2023-08-18 03:25_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1314 on 2023-08-18 03:25_

So, that _already_ gets associated as a trailing comment on `b`. I believe because the range of the `WithItem` starts at the `a` and goes until the parenthesis _after_ the `b`. The case we care about is that in which the comment trails the range of the `WithItem`, like:


```python
with (
    a
    as
    # comment
    b  # leading comment
): ...
```

Which makes me think: perhaps this condition should ignore `RParen`?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1333 on 2023-08-18 03:28_

Doesn't the following node need to be the first `WithItem` though?

---

_@charliermarsh reviewed on 2023-08-18 03:28_

---

_@MichaReiser reviewed on 2023-08-18 06:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1333 on 2023-08-18 06:05_

I thought that we could make use of the fact that the preceding node is `None`. It then should be the first item?

---

_@charliermarsh reviewed on 2023-08-18 15:24_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1333 on 2023-08-18 15:24_

Hmm... I think there are some issues with the ranges on these `WithItem`s. Need to look into it.

---

_Converted to draft by @charliermarsh on 2023-08-20 23:24_

---

_Comment by @charliermarsh on 2023-08-20 23:24_

Converting to draft until we figure out how to fix the `WithItem` ranges for the parenthesized, non-`as` case.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1333 on 2023-08-30 17:58_

Okay, this finally works :joy:

---

_@charliermarsh reviewed on 2023-08-30 17:58_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-30 17:58_

---

_Marked ready for review by @charliermarsh on 2023-08-30 17:58_

---

_@charliermarsh reviewed on 2023-08-30 17:59_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1314 on 2023-08-30 17:59_

This now gets formatted as:

```python
with (
    a as b,  # comment
    c as d,
):
    ...
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/statement/with.py`:263 on 2023-08-31 06:32_

Leading comment is confusing, aren't these all trailing comments?

---

_@MichaReiser reviewed on 2023-08-31 06:35_

What exact problem is this PR solving? Does it solve some instability? I think I prefer the current formatting 

```
with (
    a as b,  # leading comment
    c as d,
):
    ...


with (
    a as b,  # leading comment
    c as d,
):
    ...
```

Attaching the comments to the optional vars instead of the with item (if there are no tokens in between) also deviates from our practice to always attach comments to the outer most node.

---

_Comment by @charliermarsh on 2023-08-31 07:35_

I think the motivation here (per PR summary) was to ensure that the “# trailing b same line comment” wasn’t moved “off” of the with-item as in the demonstrated example (where it’s moved to the next line, after the colon, rather than remaining as a same-line comment the b). I don’t know that attaching as a trailing comment on the optional var is actually the right solution though. I’m tempted to just close this and move on. However I don’t fully understand your most recent comment. What is the code snippet you’d included there intended to communicate? Is that not the formatting we achieve before and after this PR?

---

_Comment by @MichaReiser on 2023-08-31 07:39_

Okay I see. That means this is specific for cases where the `as` is parenthesized. 

I would be okay changing the logic so that the comment becomes a trailing comment of the optional var if it is inside the parentheses. 

---

_Converted to draft by @charliermarsh on 2023-08-31 15:22_

---

_Comment by @charliermarsh on 2023-08-31 21:04_

I'm gonna close this for now. The proposed change is actually _less_ consistent in some cases -- for example, it treats the comments after `b` very differently in these two cases:

```python
with (
    a
    as
    # leading comment
    b  # trailing comment
): ...

with (
    a
    as
    # leading comment
    b,  # trailing comment
    c as d
): ...
```

We can revisit if it comes up in practice.

---

_Closed by @charliermarsh on 2023-08-31 21:04_

---
