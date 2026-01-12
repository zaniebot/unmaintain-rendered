```yaml
number: 6337
title: "Formatter: Show preceding, following and enclosing nodes of comments"
type: pull_request
state: closed
author: konstin
labels:
  - formatter
assignees: []
base: main
head: collect_decorated_comments
created_at: 2023-08-04T10:42:14Z
updated_at: 2023-08-23T12:00:39Z
url: https://github.com/astral-sh/ruff/pull/6337
synced_at: 2026-01-12T02:45:38Z
```

# Formatter: Show preceding, following and enclosing nodes of comments

---

_Pull request opened by @konstin on 2023-08-04 10:42_

**Summary** I used to always add `dbg!` for preceding, following and enclosing. With this change `--print-comments` can do this instead.
```python
import re  # import

def to_camel_case(node: str) -> str:
    """Converts PascalCase to camel_case"""
    return re.sub("([A-Z])", r"_\1", node).lower().lstrip("_")

# a
if True:
    pass  # b
else:
    print()
```
Debug output:
```
11..19 Some((StmtImport, 0..9)) Some((StmtFunctionDef, 22..165)) (ModModule, 0..213) "# import"
168..171 Some((StmtFunctionDef, 22..165)) Some((StmtIf, 172..212)) (ModModule, 0..213) "# a"
191..194 Some((StmtPass, 185..189)) Some((ElifElseClause, 195..212)) (StmtIf, 172..212) "# b"
```

**Test Plan** n/a

---

_Comment by @konstin on 2023-08-04 10:42_

Not what i wanted to do, but it's bothering me everytime i have to debug comment placement

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/visitor.rs`:342 on 2023-08-04 10:43_



I've been applying pub(crate) liberally after rust complained about private types in public interfaces


---

_@konstin reviewed on 2023-08-04 10:43_

---

_Comment by @github-actions[bot] on 2023-08-04 11:11_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.9±0.05ms     4.1 MB/sec    1.01      9.9±0.05ms     4.1 MB/sec
formatter/numpy/ctypeslib.py               1.00  1943.0±20.34µs     8.6 MB/sec    1.00  1948.3±14.55µs     8.5 MB/sec
formatter/numpy/globals.py                 1.00    218.2±8.10µs    13.5 MB/sec    1.00    219.1±2.35µs    13.5 MB/sec
formatter/pydantic/types.py                1.00      4.1±0.07ms     6.2 MB/sec    1.01      4.2±0.02ms     6.1 MB/sec
linter/all-rules/large/dataset.py          1.00     13.3±0.05ms     3.1 MB/sec    1.00     13.4±0.07ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.02ms     5.0 MB/sec    1.01      3.4±0.02ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    459.8±1.04µs     6.4 MB/sec    1.00    460.3±1.53µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.1±0.04ms     4.2 MB/sec    1.01      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.00      6.3±0.02ms     6.5 MB/sec    1.01      6.4±0.04ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1345.7±7.30µs    12.4 MB/sec    1.01   1353.2±4.37µs    12.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    150.8±2.59µs    19.6 MB/sec    1.00    150.3±0.99µs    19.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.8±0.01ms     9.0 MB/sec    1.01      2.9±0.01ms     8.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.06     10.1±0.08ms     4.0 MB/sec    1.00      9.6±0.27ms     4.2 MB/sec
formatter/numpy/ctypeslib.py               1.02  1929.6±18.64µs     8.6 MB/sec    1.00  1900.6±169.22µs     8.8 MB/sec
formatter/numpy/globals.py                 1.05    200.4±3.81µs    14.7 MB/sec    1.00    191.8±3.09µs    15.4 MB/sec
formatter/pydantic/types.py                1.05      4.3±0.11ms     6.0 MB/sec    1.00      4.1±0.04ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.02     13.4±0.11ms     3.0 MB/sec    1.00     13.2±0.08ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      3.6±0.02ms     4.7 MB/sec    1.00      3.5±0.03ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    360.4±7.30µs     8.2 MB/sec    1.01    362.6±4.70µs     8.1 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.1±0.08ms     4.2 MB/sec    1.00      6.1±0.04ms     4.2 MB/sec
linter/default-rules/large/dataset.py      1.01      6.6±0.03ms     6.1 MB/sec    1.00      6.6±0.03ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1348.5±13.67µs    12.3 MB/sec    1.01  1355.7±20.65µs    12.3 MB/sec
linter/default-rules/numpy/globals.py      1.00    137.9±1.24µs    21.4 MB/sec    1.03   141.6±14.73µs    20.8 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.6 MB/sec    1.01      3.0±0.04ms     8.5 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:700 on 2023-08-04 11:47_

Hmm, this kind of changes the responsibilities I had in mind for the `visitor` and builder. The idea is that the builder is relatively dumb and simply collects the comments. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:26 on 2023-08-04 11:50_

We can make `CommentsVisitor` `pub(super)` instead of increasing the visibility of everything else.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:342 on 2023-08-04 11:54_

I can see how this is needed but it breaks with the intended encapsulation of the module. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:158 on 2023-08-04 11:54_

Adding a generic type here means that Rust monomorphizes the whole visitor implementation (with all walk methods) for each collector. That's rather a lot of code, causing longer compile time and an increase in binary size (for something that's only needed in dev builds).

---

_@MichaReiser reviewed on 2023-08-04 11:59_

I can see how this is useful, but I am not convinced that it is worth 

* exposing internals that intentionally were module scoped
* Monomorphizing the whole visitor twice (even in production builds)



---

_@konstin reviewed on 2023-08-04 13:57_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/visitor.rs`:158 on 2023-08-04 13:57_

I can also pass a `Box<dyn Fn(...) -> ...>`

---

_Label `formatter` added by @konstin on 2023-08-04 14:16_

---

_@MichaReiser reviewed on 2023-08-04 14:42_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:158 on 2023-08-04 14:42_

Yeah. I'm not sure what the best approach is. Maybe we use `Box<dyn Fn(...)>` and/or hide it behind a feature flag (like a `debug` or `dev` flag).

---

_Comment by @konstin on 2023-08-23 12:00_

Superseded by #6813

---

_Closed by @konstin on 2023-08-23 12:00_

---
