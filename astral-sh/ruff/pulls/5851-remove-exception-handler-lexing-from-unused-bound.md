```yaml
number: 5851
title: "Remove exception-handler lexing from `unused-bound-exception` fix"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/except
created_at: 2023-07-18T01:45:03Z
updated_at: 2023-07-18T18:55:21Z
url: https://github.com/astral-sh/ruff/pull/5851
synced_at: 2026-01-12T15:55:19Z
```

# Remove exception-handler lexing from `unused-bound-exception` fix

---

_@charliermarsh_

## Summary

The motivation here is that it will make this rule easier to rewrite as a deferred check. Right now, we can't run this rule in the deferred phase, because it depends on the `except_handler` to power its autofix. Instead of lexing the `except_handler`, we can use the `SimpleTokenizer` from the formatter, and just lex forwards and backwards.

For context, this rule detects the unused `e` in:

```python
try:
  pass
except ValueError as e:
  pass
```

---

_Review requested from @MichaReiser by @charliermarsh on 2023-07-18 01:45_

---

_@charliermarsh reviewed on 2023-07-18 01:45_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/trivia.rs`:138 on 2023-07-18 01:45_

I decided to move this to the shared `ruff_python_whitespace` crate, whose name is getting less and less appropriate.

---

_Comment by @github-actions[bot] on 2023-07-18 02:34_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      9.3±0.02ms     4.4 MB/sec    1.00      9.3±0.03ms     4.4 MB/sec
formatter/numpy/ctypeslib.py               1.00   1855.4±1.46µs     9.0 MB/sec    1.00   1859.9±3.14µs     9.0 MB/sec
formatter/numpy/globals.py                 1.00    207.8±3.01µs    14.2 MB/sec    1.01    209.1±1.56µs    14.1 MB/sec
formatter/pydantic/types.py                1.00      4.0±0.01ms     6.4 MB/sec    1.00      4.0±0.01ms     6.4 MB/sec
linter/all-rules/large/dataset.py          1.00     12.8±0.02ms     3.2 MB/sec    1.00     12.8±0.11ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.3±0.01ms     5.0 MB/sec    1.00      3.3±0.00ms     5.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    435.2±0.67µs     6.8 MB/sec    1.00    435.1±0.65µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.02      6.0±0.01ms     4.3 MB/sec    1.00      5.8±0.01ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.00      6.6±0.02ms     6.2 MB/sec    1.00      6.5±0.01ms     6.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1409.1±1.46µs    11.8 MB/sec    1.00   1409.4±3.74µs    11.8 MB/sec
linter/default-rules/numpy/globals.py      1.00    155.3±0.24µs    19.0 MB/sec    1.00    155.3±0.33µs    19.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.01ms     8.6 MB/sec    1.00      3.0±0.01ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00     15.0±0.41ms     2.7 MB/sec    1.01     15.2±0.36ms     2.7 MB/sec
formatter/numpy/ctypeslib.py               1.01      2.9±0.08ms     5.7 MB/sec    1.00      2.9±0.09ms     5.8 MB/sec
formatter/numpy/globals.py                 1.00   333.2±16.41µs     8.9 MB/sec    1.02   340.4±17.51µs     8.7 MB/sec
formatter/pydantic/types.py                1.00      6.3±0.21ms     4.1 MB/sec    1.02      6.4±0.24ms     4.0 MB/sec
linter/all-rules/large/dataset.py          1.00     21.5±0.51ms  1938.7 KB/sec    1.01     21.7±0.48ms  1922.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      5.7±0.20ms     2.9 MB/sec    1.00      5.6±0.13ms     3.0 MB/sec
linter/all-rules/numpy/globals.py          1.01   671.1±30.63µs     4.4 MB/sec    1.00   666.6±26.80µs     4.4 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.8±0.27ms     2.6 MB/sec    1.00      9.8±0.28ms     2.6 MB/sec
linter/default-rules/large/dataset.py      1.00     11.8±0.31ms     3.5 MB/sec    1.01     11.9±0.28ms     3.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.4±0.07ms     7.0 MB/sec    1.00      2.4±0.07ms     7.0 MB/sec
linter/default-rules/numpy/globals.py      1.01   280.2±12.73µs    10.5 MB/sec    1.00   278.5±11.92µs    10.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.1±0.15ms     5.0 MB/sec    1.00      5.1±0.14ms     5.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/fixes.rs`:93 on 2023-07-18 06:12_

Nit: Can we give this range a more specific name: E.g. `except_handler_range`? I think it's also fine to pass in the except handler even if we only use its range. It acts as "parameter documentation"

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/fixes.rs`:97 on 2023-07-18 06:14_

#5825 adds a new `up_to_no_back_comments` constructor function that is significantly faster when we know, that the end position's line contains no comments. 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pyflakes/fixes.rs`:109 on 2023-07-18 06:16_

What are the valid tokens for the exception value? The simple tokenizer (intentionally) only supports a very limited set of tokens. E.g. strings no strings or identifiers. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:4 on 2023-07-18 06:16_

Hmm. Why does git not recognize that this file was moved? It will make merging my PRs rather painful.

---

_@MichaReiser approved on 2023-07-18 06:18_

Can you take a look at why git doesn't recognize the move of the `tokenizer`. 

`ruff_python_trivia` might be a better name now. But renaming is probably worth its own PR.

One thing that now might be confusing to users is that we have two conflicting `TokenKind` definitions.

---

_@charliermarsh reviewed on 2023-07-18 15:32_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/trivia.rs`:4 on 2023-07-18 15:32_

Will resolve, but also happy to just wait for you to merge and handle the rebase on my end.

---

_@charliermarsh reviewed on 2023-07-18 15:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/fixes.rs`:109 on 2023-07-18 15:33_

We just want _anything_ that isn't a continuation.

---

_@charliermarsh reviewed on 2023-07-18 15:33_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pyflakes/fixes.rs`:109 on 2023-07-18 15:33_

It's basically `X as Y`, and we're starting at `Y`. The only thing you can have between `X` and `as`, and between `as` and `Y`, is whitespace and continuation (backslash).

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/trivia.rs`:4 on 2023-07-18 15:53_

Getting git to recognize the move would also be good to preserve the history. I otherwise don't mind merging.

---

_@MichaReiser reviewed on 2023-07-18 15:53_

---

_@charliermarsh reviewed on 2023-07-18 15:56_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/trivia.rs`:4 on 2023-07-18 15:56_

The problem could be that I didn't move the whole file, I left behind some of the utility methods... But I can just move them all.

---

_Merged by @charliermarsh on 2023-07-18 18:27_

---

_Closed by @charliermarsh on 2023-07-18 18:27_

---

_Branch deleted on 2023-07-18 18:27_

---
