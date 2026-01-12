```yaml
number: 4513
title: Replace token iteration with Indexer/Locator lookups for relevant rules
type: pull_request
state: merged
author: evanrittenhouse
labels:
  - internal
assignees: []
merged: true
base: main
head: indexer_in_token_rules
created_at: 2023-05-19T02:20:01Z
updated_at: 2023-05-22T15:35:26Z
url: https://github.com/astral-sh/ruff/pull/4513
synced_at: 2026-01-12T03:50:03Z
```

# Replace token iteration with Indexer/Locator lookups for relevant rules

---

_Pull request opened by @evanrittenhouse on 2023-05-19 02:20_

[This comment](https://github.com/charliermarsh/ruff/pull/3921#discussion_r1181461746) raised the idea of using `Indexer`/`Locator` for comment-related rules based on token checks. I went through the rules examined in `checkers/tokens.rs` and changed it out for the ones I identified.

---

_@evanrittenhouse reviewed on 2023-05-19 02:22_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules.rs`:409 on 2023-05-19 02:22_

Not directly related to this PR, but I didn't feel like it was worth raising a new one entirely.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:308 on 2023-05-19 06:42_

Nit:

```suggestion
            let next_comment = locator.slice(*next_comment_range);
```

I think this should work because `TextRange` is copy or you might need to use `**next_comment_range` in case it is a `&&TextRange`

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:327 on 2023-05-19 06:47_

I think we still need some handling that tests whether the two comments are next to each other (or have only trivia in between). We can do this by testing whether the range between `range.end()` and `next_comment_range.start()` only contains whitespace. 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:308 on 2023-05-19 06:50_

Unrelated to this PR. Do we need multipeek here or could we rewrite this as 

```
while let Some(next_comment_range) = iter.peek() {
    let next_comment = locator.slice(*next_comment_range);
    if detect_tag(...).is_some() {
        break;
    }

    if ISSUE_LINK_REGEX_SET.is_matcch(next_comment) {
        has_issue_link = true;
        // Should we call `next()` here too because it is guaranteed that this comment has no tag
        // so it will fail the check on line 299
        break;
    }

    // skip over that comment because it neither has a tag nor link. It's not worth testing in the next iteration if it has a tag.
    iter.next();
}
```

---

_@MichaReiser reviewed on 2023-05-19 06:50_

---

_Comment by @MichaReiser on 2023-05-19 06:51_

Nice, thanks for working on this. Will be interesting to see by how much this improves performance.

---

_Label `internal` added by @MichaReiser on 2023-05-19 06:51_

---

_@evanrittenhouse reviewed on 2023-05-19 13:00_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules.rs`:308 on 2023-05-19 13:00_

I'm curious, is double deref a best practice in this situation? It is indeed a &&TextRange but thought that double deref was weird, which is why I said `to_owned()`. Though I guess that results in unnecessary heap allocation

---

_@evanrittenhouse reviewed on 2023-05-19 13:01_

---

_Review comment by @evanrittenhouse on `crates/ruff/src/rules/flake8_todos/rules.rs`:327 on 2023-05-19 13:01_

RE this comment and below

I just wanted to change the arguments to the respective checkers and get the PR up - still need to remove tokens from use. Should have made it a draft!

Sorry though, what's trivia?

---

_Comment by @github-actions[bot] on 2023-05-19 16:45_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     17.2±0.17ms     2.4 MB/sec    1.00     16.9±0.09ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.01ms     4.0 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.01    507.9±2.40µs     5.8 MB/sec    1.00    502.6±2.23µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.0±0.05ms     3.6 MB/sec    1.00      7.0±0.04ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.01      8.1±0.03ms     5.0 MB/sec    1.00      8.0±0.02ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1756.5±9.27µs     9.5 MB/sec    1.00   1736.9±7.33µs     9.6 MB/sec
linter/default-rules/numpy/globals.py      1.01    195.4±1.65µs    15.1 MB/sec    1.00    193.5±1.29µs    15.2 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.7±0.02ms     7.0 MB/sec    1.00      3.6±0.01ms     7.0 MB/sec
parser/large/dataset.py                    1.00      6.5±0.06ms     6.3 MB/sec    1.02      6.6±0.02ms     6.2 MB/sec
parser/numpy/ctypeslib.py                  1.00   1268.7±3.83µs    13.1 MB/sec    1.01   1279.7±5.09µs    13.0 MB/sec
parser/numpy/globals.py                    1.00    130.0±0.62µs    22.7 MB/sec    1.01    131.8±0.70µs    22.4 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.3 MB/sec    1.01      2.8±0.03ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     17.3±0.39ms     2.4 MB/sec    1.00     16.6±0.26ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.02      4.3±0.10ms     3.8 MB/sec    1.00      4.2±0.07ms     3.9 MB/sec
linter/all-rules/numpy/globals.py          1.02    505.7±9.47µs     5.8 MB/sec    1.00   497.1±12.88µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.03      7.2±0.14ms     3.5 MB/sec    1.00      7.0±0.12ms     3.7 MB/sec
linter/default-rules/large/dataset.py      1.03      8.5±0.20ms     4.8 MB/sec    1.00      8.3±0.10ms     4.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1768.3±28.70µs     9.4 MB/sec    1.00  1774.0±36.38µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.5±3.85µs    14.9 MB/sec    1.03    202.7±9.80µs    14.6 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.8±0.05ms     6.8 MB/sec    1.00      3.8±0.09ms     6.8 MB/sec
parser/large/dataset.py                    1.01      6.8±0.05ms     6.0 MB/sec    1.00      6.7±0.05ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01  1289.7±15.11µs    12.9 MB/sec    1.00  1277.5±17.31µs    13.0 MB/sec
parser/numpy/globals.py                    1.02    134.3±4.21µs    22.0 MB/sec    1.00    131.7±2.39µs    22.4 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.03ms     8.8 MB/sec    1.00      2.8±0.03ms     9.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @evanrittenhouse on 2023-05-20 19:05_

@MichaReiser ready for final review!

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_todos/rules.rs`:327 on 2023-05-22 07:54_

> Sorry though, what's trivia?

Sorry for the late reply. Trivia are tokens that have no semantic meaning, e.g. whitespace, newlines (Python is a bit awkward because some newlines have semantic meaning), comments etc. I think in this context it is sufficient to test if there's any content between the two comments. 

---

_@MichaReiser approved on 2023-05-22 07:55_

---

_Comment by @MichaReiser on 2023-05-22 07:56_

Awesome! Looks great and it seems to improve performance by a percent!

---

_Merged by @MichaReiser on 2023-05-22 07:56_

---

_Closed by @MichaReiser on 2023-05-22 07:56_

---

_Branch deleted on 2023-05-22 15:35_

---
