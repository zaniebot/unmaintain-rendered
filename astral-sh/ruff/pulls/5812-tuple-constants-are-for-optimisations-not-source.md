```yaml
number: 5812
title: tuple constants are for optimisations, not source
type: pull_request
state: closed
author: davidszotten
labels: []
assignees: []
base: main
head: format-no-tuple-constant
created_at: 2023-07-16T20:58:34Z
updated_at: 2023-07-18T06:58:37Z
url: https://github.com/astral-sh/ruff/pull/5812
synced_at: 2026-01-12T15:55:19Z
```

# tuple constants are for optimisations, not source

---

_@davidszotten_

my reading of https://docs.python.org/3/library/ast.html#ast.unparse and
https://discuss.python.org/t/ast-constant-value-tuple-s-and-frozenset-s/22578 is that tuple constants cannot come from parsing python source, they are only for optimised bytecode


---

_Comment by @charliermarsh on 2023-07-16 21:03_

Oh that’s so interesting, I’ve always wondered how these were “created”.

---

_Comment by @github-actions[bot] on 2023-07-16 21:10_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.4±0.04ms     4.3 MB/sec    1.01      9.5±0.02ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   1871.3±1.78µs     8.9 MB/sec    1.01   1886.2±6.39µs     8.8 MB/sec
formatter/numpy/globals.py                 1.00    210.2±0.47µs    14.0 MB/sec    1.00    211.1±1.27µs    14.0 MB/sec
formatter/pydantic/types.py                1.01      4.1±0.01ms     6.2 MB/sec    1.00      4.0±0.01ms     6.3 MB/sec
linter/all-rules/large/dataset.py          1.01     13.2±0.06ms     3.1 MB/sec    1.00     13.1±0.04ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    451.9±0.65µs     6.5 MB/sec    1.00    451.7±1.98µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.0±0.01ms     4.3 MB/sec    1.00      6.0±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1476.5±1.60µs    11.3 MB/sec    1.01   1486.3±2.21µs    11.2 MB/sec
linter/default-rules/numpy/globals.py      1.01    169.5±0.23µs    17.4 MB/sec    1.00    168.6±0.30µs    17.5 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.4 MB/sec    1.00      3.0±0.01ms     8.4 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.07     11.7±0.11ms     3.5 MB/sec    1.00     10.9±0.11ms     3.7 MB/sec
formatter/numpy/ctypeslib.py               1.05      2.3±0.04ms     7.4 MB/sec    1.00      2.2±0.04ms     7.7 MB/sec
formatter/numpy/globals.py                 1.01    251.0±4.54µs    11.8 MB/sec    1.00   249.5±11.53µs    11.8 MB/sec
formatter/pydantic/types.py                1.05      5.0±0.08ms     5.2 MB/sec    1.00      4.7±0.08ms     5.4 MB/sec
linter/all-rules/large/dataset.py          1.01     15.8±0.17ms     2.6 MB/sec    1.00     15.6±0.25ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.1±0.06ms     4.0 MB/sec    1.00      4.2±0.06ms     4.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    501.3±7.04µs     5.9 MB/sec    1.00   503.4±12.90µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.09ms     3.6 MB/sec    1.00      7.0±0.11ms     3.6 MB/sec
linter/default-rules/large/dataset.py      1.00      8.2±0.08ms     5.0 MB/sec    1.00      8.1±0.10ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1737.3±19.87µs     9.6 MB/sec    1.00  1728.0±21.17µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    202.9±3.51µs    14.5 MB/sec    1.00    202.9±2.68µs    14.5 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.04ms     7.0 MB/sec    1.00      3.6±0.03ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-07-16 21:18_

@MichaReiser - any objection to me just removing this node from the AST altogether?

---

_Comment by @MichaReiser on 2023-07-17 07:35_

> @MichaReiser - any objection to me just removing this node from the AST altogether?

Nope. I would actually prefer this. Less generated code and less confusion. 

---

_Comment by @davidszotten on 2023-07-17 08:18_

> > @MichaReiser - any objection to me just removing this node from the AST altogether?
> 
> Nope. I would actually prefer this. Less generated code and less confusion.

👍  where does that live?

---

_Comment by @MichaReiser on 2023-07-17 08:28_

You can find the parser code in this repository. https://github.com/astral-sh/RustPython-Parser

We would need to:
* create a PR to the parser repository that removes the node
* tag the new parser version
* Update the Ruff's workspace `Cargo.toml` to use the new commit (same as the tag) and delete all code that no longer compiles. You can already start working on this PR before the parser PR is merged by changing the rustpython version to use your repo & commit. 

---

_Comment by @davidszotten on 2023-07-18 06:58_

fixed by #5840 

---

_Closed by @davidszotten on 2023-07-18 06:58_

---
