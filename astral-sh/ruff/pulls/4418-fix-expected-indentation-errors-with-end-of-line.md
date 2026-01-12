```yaml
number: 4418
title: Fix expected-indentation errors with end-of-line comments
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: charlie/pycodestyle
head: charlie/indentation
created_at: 2023-05-13T17:17:12Z
updated_at: 2023-05-15T13:58:06Z
url: https://github.com/astral-sh/ruff/pull/4418
synced_at: 2026-01-12T15:55:15Z
```

# Fix expected-indentation errors with end-of-line comments

---

_@charliermarsh_

## Summary

Given code like:

```py
if False:  #
    print()
```

The token stream will contain a comment, followed by a newline. At present, we end up creating a separate logical line for _just_ the newline, which throws off our logic around expected-indentation rules, which needs to look back at the previous newline to determine the appropriate indentation.

I've looked at the lexer to better understand the logic around when to expect a newline token (following a comment). My understanding is that there should _always_ be a newline unless the comment appears at the start of a (logical?) line, since the only codepath that consumes but doesn't generate a newline is via `eat_indentation`.

Closes #4417.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-13 17:17_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:119 on 2023-05-13 17:23_

Alternatively, we may be able to get away with checking if this is the first token in the line? In theory that should also work. It's arguably more reliant on details of the lexer, but it's also making the expectation more explicit.

---

_@charliermarsh reviewed on 2023-05-13 17:23_

---

_Comment by @github-actions[bot] on 2023-05-13 18:08_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     13.8±0.05ms     3.0 MB/sec    1.00     13.8±0.06ms     3.0 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.01ms     5.0 MB/sec
linter/all-rules/numpy/globals.py          1.00    412.3±0.71µs     7.2 MB/sec    1.00    414.2±1.29µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.03ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.01ms     6.1 MB/sec    1.00      6.7±0.02ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01   1442.9±6.81µs    11.5 MB/sec    1.00   1432.9±1.87µs    11.6 MB/sec
linter/default-rules/numpy/globals.py      1.00    158.5±0.85µs    18.6 MB/sec    1.00    158.0±0.17µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.01      3.0±0.03ms     8.4 MB/sec    1.00      3.0±0.00ms     8.5 MB/sec
parser/large/dataset.py                    1.04      5.7±0.00ms     7.1 MB/sec    1.00      5.5±0.02ms     7.4 MB/sec
parser/numpy/ctypeslib.py                  1.03   1097.9±2.13µs    15.2 MB/sec    1.00   1064.1±0.75µs    15.6 MB/sec
parser/numpy/globals.py                    1.02    109.9±0.10µs    26.8 MB/sec    1.00    108.3±0.65µs    27.3 MB/sec
parser/pydantic/types.py                   1.03      2.4±0.00ms    10.6 MB/sec    1.00      2.3±0.00ms    10.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     16.4±0.46ms     2.5 MB/sec    1.25     20.6±0.69ms  2025.6 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.29ms     3.9 MB/sec    1.22      5.2±0.25ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.00   486.4±20.42µs     6.1 MB/sec    1.30   634.0±40.27µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.1±0.32ms     3.6 MB/sec    1.23      8.7±0.68ms     2.9 MB/sec
linter/default-rules/large/dataset.py      1.00      8.3±0.32ms     4.9 MB/sec    1.21     10.0±0.45ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1744.4±92.48µs     9.5 MB/sec    1.21      2.1±0.10ms     7.9 MB/sec
linter/default-rules/numpy/globals.py      1.00   207.3±14.40µs    14.2 MB/sec    1.31   271.2±37.04µs    10.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.7±0.14ms     6.9 MB/sec    1.27      4.7±0.30ms     5.4 MB/sec
parser/large/dataset.py                    1.00      6.6±0.21ms     6.1 MB/sec    1.27      8.4±0.23ms     4.8 MB/sec
parser/numpy/ctypeslib.py                  1.00  1313.7±51.98µs    12.7 MB/sec    1.26  1649.0±67.66µs    10.1 MB/sec
parser/numpy/globals.py                    1.00    132.7±6.45µs    22.2 MB/sec    1.26    166.6±9.61µs    17.7 MB/sec
parser/pydantic/types.py                   1.00      2.9±0.09ms     8.9 MB/sec    1.27      3.6±0.15ms     7.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @charliermarsh on 2023-05-14 17:13_

I'm guessing that might be a real performance regression? Although is it comparing against `main` or `charlie/pycodestyle`? The latter includes the logical line rules, the former does not. I'll have to benchmark manually.

---

_@MichaReiser reviewed on 2023-05-14 21:17_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:119 on 2023-05-14 21:17_

Should we fix this in the lexer instead? It feels odd that the newline is conditional on the comment placement. I would expect that a comment is always followed by a newline.

Nit. You can match directly on the Tok. Using TokenKind has some overhead that is only worth it when there are many reads. 

---

_@charliermarsh reviewed on 2023-05-14 22:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:119 on 2023-05-14 22:29_

Honestly not sure. \cc @youknowone - do you know, if it's intentional that we don't emit a newline when a comment is at start-of-line?

---

_@charliermarsh reviewed on 2023-05-14 22:41_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:119 on 2023-05-14 22:41_

It may not actually be a bug, I'd have to compare to CPython.

---

_@charliermarsh reviewed on 2023-05-14 22:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:119 on 2023-05-14 22:45_

It looks like CPython _does_ emit a non-logical newline, even for entirely empty lines, unlike RustPython.

RustPython also has this test:

```rust
#[test]
fn test_logical_newline_line_comment() {
    let source = "#Hello\n#World";
    let tokens = lex_source(source);
    assert_eq!(
        tokens,
        vec![
            Tok::Comment("#Hello".to_owned()),
            // tokenize.py does put an NL here...
            Tok::Comment("#World".to_owned()),
            // ... and here, but doesn't seem very useful.
        ]
    );
}
```

@youknowone - Do you know if this is intentional, or should I fix it?

---

_@charliermarsh reviewed on 2023-05-15 03:09_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:119 on 2023-05-15 03:09_

Put up https://github.com/RustPython/Parser/pull/27. (We'll still need code changes here to fix this bug, once that merges.)

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:119 on 2023-05-15 06:53_

Thank you! Makes sense. Is the change to remove the explicit `Comment` handling and only rely on the presence of `Newline` and `NonLogicalNewline`?

---

_@MichaReiser approved on 2023-05-15 06:53_

---

_Comment by @MichaReiser on 2023-05-15 06:53_

> I'm guessing that might be a real performance regression? Although is it comparing against `main` or `charlie/pycodestyle`? The latter includes the logical line rules, the former does not. I'll have to benchmark manually.

I think it compares against `main`.

---

_Review comment by @youknowone on `crates/ruff/src/rules/pycodestyle/rules/logical_lines/mod.rs`:119 on 2023-05-15 07:25_

Thank you! I don't think RustPython intended to have different behavior to CPython.
I will look in the patch.

---

_@youknowone reviewed on 2023-05-15 07:25_

---

_Merged by @charliermarsh on 2023-05-15 13:22_

---

_Closed by @charliermarsh on 2023-05-15 13:22_

---

_Branch deleted on 2023-05-15 13:22_

---

_Comment by @charliermarsh on 2023-05-15 13:22_

Ugh this got confused.

---

_Comment by @charliermarsh on 2023-05-15 13:22_

Because I tried to swap the upstreams between this and `charlie/pycodestyle`.

---

_Comment by @charliermarsh on 2023-05-15 13:32_

Re-opened as #4438.

---
