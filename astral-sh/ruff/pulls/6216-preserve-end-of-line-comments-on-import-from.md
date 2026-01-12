```yaml
number: 6216
title: Preserve end-of-line comments on import-from statements
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/comment-in-import
created_at: 2023-08-01T01:08:46Z
updated_at: 2023-08-01T19:15:05Z
url: https://github.com/astral-sh/ruff/pull/6216
synced_at: 2026-01-12T15:55:20Z
```

# Preserve end-of-line comments on import-from statements

---

_@charliermarsh_

## Summary

Ensures that we keep comments at the end-of-line in cases like:

```python
from foo import (  # comment
  bar,
)
```

Closes https://github.com/astral-sh/ruff/issues/6067.


---

_Review requested from @konstin by @charliermarsh on 2023-08-01 01:08_

---

_Comment by @charliermarsh on 2023-08-01 01:19_

We can probably generalize some of this for other container types (lists, sets, etc).

---

_Comment by @charliermarsh on 2023-08-01 01:30_

(I'm looking into it.)

---

_Comment by @github-actions[bot] on 2023-08-01 01:40_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      8.3±0.07ms     4.9 MB/sec    1.00      8.2±0.13ms     5.0 MB/sec
formatter/numpy/ctypeslib.py               1.00  1644.0±36.70µs    10.1 MB/sec    1.00  1645.4±45.03µs    10.1 MB/sec
formatter/numpy/globals.py                 1.00    184.7±4.79µs    16.0 MB/sec    1.00    184.1±5.58µs    16.0 MB/sec
formatter/pydantic/types.py                1.02      3.6±0.10ms     7.1 MB/sec    1.00      3.5±0.13ms     7.3 MB/sec
linter/all-rules/large/dataset.py          1.02     11.4±0.20ms     3.6 MB/sec    1.00     11.1±0.10ms     3.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      2.8±0.04ms     5.9 MB/sec    1.00      2.8±0.03ms     5.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    383.6±0.87µs     7.7 MB/sec    1.00    384.0±0.51µs     7.7 MB/sec
linter/all-rules/pydantic/types.py         1.02      5.1±0.10ms     5.0 MB/sec    1.00      5.0±0.07ms     5.1 MB/sec
linter/default-rules/large/dataset.py      1.05      6.1±0.08ms     6.7 MB/sec    1.00      5.8±0.06ms     7.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1228.9±21.87µs    13.5 MB/sec    1.00  1212.1±17.84µs    13.7 MB/sec
linter/default-rules/numpy/globals.py      1.00    134.1±0.98µs    22.0 MB/sec    1.00    133.9±0.82µs    22.0 MB/sec
linter/default-rules/pydantic/types.py     1.03      2.6±0.05ms     9.6 MB/sec    1.00      2.6±0.06ms     9.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.8±0.10ms     4.1 MB/sec    1.02     10.0±0.14ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1934.6±63.54µs     8.6 MB/sec    1.00  1935.5±29.37µs     8.6 MB/sec
formatter/numpy/globals.py                 1.00   216.0±10.87µs    13.7 MB/sec    1.00    215.1±6.97µs    13.7 MB/sec
formatter/pydantic/types.py                1.00      4.2±0.06ms     6.0 MB/sec    1.03      4.3±0.09ms     5.9 MB/sec
linter/all-rules/large/dataset.py          1.00     13.4±0.14ms     3.0 MB/sec    1.03     13.8±0.29ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.05ms     4.7 MB/sec    1.01      3.6±0.07ms     4.6 MB/sec
linter/all-rules/numpy/globals.py          1.00    433.1±5.26µs     6.8 MB/sec    1.00    435.1±7.13µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.07ms     4.2 MB/sec    1.01      6.1±0.09ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      7.3±0.11ms     5.5 MB/sec    1.04      7.6±0.13ms     5.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1489.5±25.78µs    11.2 MB/sec    1.03  1529.8±38.35µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    167.2±3.07µs    17.6 MB/sec    1.01    169.4±4.16µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     8.0 MB/sec    1.04      3.3±0.07ms     7.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Label `formatter` added by @charliermarsh on 2023-08-01 04:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1138 on 2023-08-01 06:05_

This comment confuses me. It says that the comment must be on the same line, but before the first member. But the snipped shows a comment that is after the second member. Which one is it?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1147 on 2023-08-01 06:05_

Nit:

```suggestion
        && import_from
            .names
            .first()
            .is_some_and(|name| comment.end() < name.start())
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1151 on 2023-08-01 06:11_

I'm a bit surprised that this isn't working by default. The default comment placement associates the comment with the most outer node:

```
# comment
a + b
```

The default placement associates the comment with the `ExpressionStatement` and not the `Expr::Name`, because that would expand the binary expression. The same should be true for trailing comments. 

I pasted the given example and the default placement logic does place the comment correctly:

```
[crates/ruff_python_formatter/src/lib.rs:316] formatted.context().comments().debug(formatted.context().source_code()) = {
    Node {
        kind: StmtImportFrom,
        range: 1..20,
        source: `from foo import bar`,
    }: {
        "leading": [],
        "dangling": [],
        "trailing": [
            SourceComment {
                text: "# comment",
                position: EndOfLine,
                formatted: true,
            },
        ],
    },
}
```
Could you update the example or remove the placement logic if it is unnecessary

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/stmt_import_from.rs`:42 on 2023-08-01 06:17_

I would have expected the comments to be written right before the `names` to match the logic in `placement.rs`: The dangling comments are dangling comments after any possible opening parentheses, meaning they should come after the `*`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1143 on 2023-08-01 06:18_

Does this work with line continuation? 

```python
from a import \
	( # comment
		b,
		c
	)
```

It could make sense to search for any opening `(` parentheses instead and assert that the comment starts after the opening but before the start member. This may even be faster because the tokenizer only needs to eat over a few whitespace tokens. 

---

_@MichaReiser requested changes on 2023-08-01 06:20_

---

_Review comment by @konstin on `crates/ruff_python_formatter/tests/snapshots/format@statement__import_from.py.snap`:80 on 2023-08-01 07:30_

For me, the current style (same as black's style) matches my expectations better:
```python
from a import b, dkasjhfaslhdflkjashdlfjkashldfkjahlsdkjfha, sdkfjhlasdfkjlhasdfkjhasdflkjhasd # c
from a import b, dkasjhfaslhdflkjashdlfjkashldfkjahlsdkjfha, sdkfjhlasdfkjlhasdfkjhasdflkjhasd # c

def f():
    pass
```
into
```python
from a import (
    b,
    dkasjhfaslhdflkjashdlfjkashldfkjahlsdkjfha,
    sdkfjhlasdfkjlhasdfkjhasdflkjhasd,
)  # c
from a import (
    b,
    dkasjhfaslhdflkjashdlfjkashldfkjahlsdkjfha,
    sdkfjhlasdfkjlhasdfkjhasdflkjhasd,
)  # c


def f():
    pass
```

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1151 on 2023-08-01 07:56_

nit: 
```suggestion
        CommentPlacement::dangling(import_from.into(), comment)
```
or without `.into()` after #6231.

---

_@konstin reviewed on 2023-08-01 07:56_

---

_@charliermarsh reviewed on 2023-08-01 12:55_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1138 on 2023-08-01 12:55_

The comment is accurate. In order for this path to trigger, `# comment` must be on the same line as the `import`, but before the first member. We want to trigger on:

```python
from foo import ( # bar
 baz,
)
```

But not:

```python
from foo import ( baz, # bar
)
```

---

_@charliermarsh reviewed on 2023-08-01 12:58_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1151 on 2023-08-01 12:58_

The default logic does not work here. Given `from foo import bar  # comment`, we treat `# comment` as a trailing comment on `from foo import bar`. However, that doesn't do the right thing.

Given:
```python
from foo import bar, bar, bar, bar, bar, bar, bar, bar, bar, bar, bar, bar, bar, bar, bar, bar  # comment
```

_Without_ this code, we format this as:
```python
from foo import (
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
)  # comment
```

Which is incorrect. We need to keep the `# comment` attached to the `import`.

Instead, we want to do:

```python
from foo import (  # comment
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
    bar,
)
```

So we treat these comments as dangling and handle the placement in the statement formatter.

---

_@MichaReiser reviewed on 2023-08-01 13:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1138 on 2023-08-01 13:00_

I'm still confused... Doesn't the comment show an example where it isn't the first member?

---

_@charliermarsh reviewed on 2023-08-01 13:00_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__import_from.py.snap`:80 on 2023-08-01 13:00_

I'm so confused, I swear I checked this last night and Black was putting the comment after the `from a import (`, but the playground is showing me otherwise now? This seems odd to me though, it means Black is erroneously moving the `# noqa`, but I will revert it.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1151 on 2023-08-01 13:02_

It looks like Black gets this wrong too so I will revert this: https://github.com/astral-sh/ruff/pull/6216#discussion_r1280603568. I thought I tested it yesterday but I must've been mixing up all the different cases.

---

_@charliermarsh reviewed on 2023-08-01 13:02_

---

_@MichaReiser reviewed on 2023-08-01 13:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1151 on 2023-08-01 13:02_

Can we update the method comment? Having an example that doesn't require any special handling seems misleading. 

---

_@charliermarsh reviewed on 2023-08-01 13:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__import_from.py.snap`:80 on 2023-08-01 13:02_

Candidly I think what Black is doing is wrong (the comment is on the import, not the member).

---

_@charliermarsh reviewed on 2023-08-01 13:02_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__import_from.py.snap`:80 on 2023-08-01 13:02_

Candidly I think what Black is doing is wrong (the comment is on the import, not the member).

---

_@charliermarsh reviewed on 2023-08-01 13:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1138 on 2023-08-01 13:04_

(I will clarify of course.)

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__import_from.py.snap`:80 on 2023-08-01 13:05_

For example, given:

```python
from foo import (  # comment
  bar,
  baz
)
```

Black will then format as:

```python
from foo import bar, baz  # comment
```

Under Black's logic, if that import then breaks again by getting too long, it will move the comment:

```python
from foo import (
  bar,
  baz,
)  # comment
```

---

_@charliermarsh reviewed on 2023-08-01 13:05_

---

_@charliermarsh reviewed on 2023-08-01 13:06_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__import_from.py.snap`:80 on 2023-08-01 13:06_

I'm actually inclined to keep what we have here, but I'm definitely open to disagreement. What do you think?

---

_@charliermarsh reviewed on 2023-08-01 13:25_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/snapshots/format@statement__import_from.py.snap`:80 on 2023-08-01 13:25_

Eh, I will just revert it for now for simplicity.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1147 on 2023-08-01 14:54_

[i like that](https://github.com/astral-sh/ruff/pull/6244)

---

_@konstin reviewed on 2023-08-01 14:54_

---

_@charliermarsh reviewed on 2023-08-01 15:17_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1138 on 2023-08-01 15:17_

Yes, the comment shows the unformatted code for which we _don't_ want to trigger the `if` condition.

---

_@charliermarsh reviewed on 2023-08-01 15:18_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1138 on 2023-08-01 15:18_

`to triggering on` should say `to avoid triggering on`.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1151 on 2023-08-01 15:18_

I'm just gonna revert for now, for simplicity, even though I think our behavior is slightly better.

---

_@charliermarsh reviewed on 2023-08-01 15:18_

---

_@charliermarsh reviewed on 2023-08-01 15:24_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1138 on 2023-08-01 15:24_

It should be much clearer now?

---

_@charliermarsh reviewed on 2023-08-01 15:33_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1143 on 2023-08-01 15:33_

Good suggestion, changed.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-01 15:33_

---

_Review requested from @konstin by @charliermarsh on 2023-08-01 15:33_

---

_Comment by @charliermarsh on 2023-08-01 15:33_

Okay, hopefully good to go.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/placement.rs`:1143 on 2023-08-01 16:39_

Nit: I would love to remove the backward lexing if possible in the future. It kind of works, but I'm worried that we'll run into issues sooner or later (e.g. magic commands are interesting...) . Could we instead search forward from the end of the identifier (or the start of the statement if absent)? 



---

_@MichaReiser approved on 2023-08-01 16:39_

---

_@charliermarsh reviewed on 2023-08-01 18:51_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1143 on 2023-08-01 18:51_

I think I figured out a solution that doesn't require lexing. If the comment is end-of-line, and _between_ the start of the statement and the first member, it _has_ to be immediately following the parenthesis.

---

_Merged by @charliermarsh on 2023-08-01 18:58_

---

_Closed by @charliermarsh on 2023-08-01 18:58_

---

_Branch deleted on 2023-08-01 18:58_

---
