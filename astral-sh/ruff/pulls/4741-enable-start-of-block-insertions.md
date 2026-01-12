```yaml
number: 4741
title: Enable start-of-block insertions
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/insertions
created_at: 2023-05-31T02:57:46Z
updated_at: 2023-05-31T17:27:15Z
url: https://github.com/astral-sh/ruff/pull/4741
synced_at: 2026-01-12T03:50:03Z
```

# Enable start-of-block insertions

---

_Pull request opened by @charliermarsh on 2023-05-31 02:57_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR adds a new `Insertion::start_of_block` mechanism, which will be used by the flake8-type-checking autofixes to add an import statement within an `if TYPE_CHECKING:` block.

Also necessary is the ability to differentiate between inline and own-line insertions. If we see a block like:

```py
import foo; x = 1
```

...and we try to add content between `import foo` and `x = 1`, we'll note that this is an inline insertion. Notably, we can't add multi-line statements (like `if` blocks with indented bodies) inline, so differentiating these positions in the API will enable us to avoid generating invalid edits for those rare cases.


---

_Review comment by @charliermarsh on `crates/ruff/src/importer/insertion.rs`:306 on 2023-05-31 02:59_

I'd like to change these to declarative snapshot tests, if I have time.

---

_@charliermarsh reviewed on 2023-05-31 02:59_

---

_Comment by @charliermarsh on 2023-05-31 03:09_

I'm not very happy with this API or concept, but I'm kind of just accepting it as "fine" for now. It already exists, and I just need to extend it for the purpose of supporting this new edit kind.

---

_Comment by @github-actions[bot] on 2023-05-31 03:32_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     18.0±0.49ms     2.3 MB/sec    1.02     18.3±0.60ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.2±0.11ms     3.9 MB/sec    1.03      4.4±0.15ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   539.3±22.05µs     5.5 MB/sec    1.00   536.9±12.33µs     5.5 MB/sec
linter/all-rules/pydantic/types.py         1.01      7.5±0.26ms     3.4 MB/sec    1.00      7.5±0.20ms     3.4 MB/sec
linter/default-rules/large/dataset.py      1.02      8.5±0.24ms     4.8 MB/sec    1.00      8.4±0.18ms     4.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1872.6±58.77µs     8.9 MB/sec    1.00  1858.0±57.76µs     9.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    220.8±7.23µs    13.4 MB/sec    1.00    220.7±6.51µs    13.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.8±0.10ms     6.6 MB/sec    1.00      3.8±0.08ms     6.7 MB/sec
parser/large/dataset.py                    1.00      6.4±0.11ms     6.3 MB/sec    1.00      6.4±0.11ms     6.3 MB/sec
parser/numpy/ctypeslib.py                  1.00  1282.2±34.15µs    13.0 MB/sec    1.00  1279.3±37.08µs    13.0 MB/sec
parser/numpy/globals.py                    1.02    130.9±4.44µs    22.5 MB/sec    1.00    128.3±4.18µs    23.0 MB/sec
parser/pydantic/types.py                   1.01      2.9±0.10ms     8.9 MB/sec    1.00      2.8±0.09ms     9.0 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     22.1±0.71ms  1886.8 KB/sec    1.00     22.0±0.75ms  1896.9 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.6±0.20ms     3.0 MB/sec    1.02      5.7±0.22ms     2.9 MB/sec
linter/all-rules/numpy/globals.py          1.00   652.6±34.36µs     4.5 MB/sec    1.00   653.6±29.28µs     4.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      9.3±0.35ms     2.7 MB/sec    1.02      9.5±0.34ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.00     10.8±0.32ms     3.8 MB/sec    1.00     10.8±0.34ms     3.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.3±0.07ms     7.2 MB/sec    1.02      2.4±0.09ms     7.1 MB/sec
linter/default-rules/numpy/globals.py      1.00   281.4±12.25µs    10.5 MB/sec    1.05   294.1±17.45µs    10.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      5.0±0.21ms     5.1 MB/sec    1.01      5.1±0.26ms     5.0 MB/sec
parser/large/dataset.py                    1.00      8.7±0.17ms     4.7 MB/sec    1.00      8.6±0.23ms     4.7 MB/sec
parser/numpy/ctypeslib.py                  1.00  1660.4±51.59µs    10.0 MB/sec    1.00  1658.1±62.61µs    10.0 MB/sec
parser/numpy/globals.py                    1.00   170.7±13.38µs    17.3 MB/sec    1.00    170.6±8.18µs    17.3 MB/sec
parser/pydantic/types.py                   1.00      3.7±0.24ms     6.8 MB/sec    1.00      3.7±0.11ms     6.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/insertion.rs`:2 on 2023-05-31 07:10_

Delete or add a TODO for remmoving it

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/insertion.rs`:64 on 2023-05-31 07:14_

Nit: Calling into the `Lexer` for this feels a bit overkill. What we want here is to find the next non-trivia character. We should be able to do this with a simple string search. We only need to track whether we are in a comment, and, if so, skip all content until we reach the end of the line. We can otherwise return the first token that we find. 


But I think this here is even simpler because you only want the next non whitespace character. So all you want is to `locator.slice(location.to_usize().._).trim_start_matches(is_python_whitespace).first() == Some(';')`

I actually don't know how expensive it is to call into the Lexer. So maybe this is totally fine. 

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/insertion.rs`:76 on 2023-05-31 07:15_

Shouldn't this also skip over `NonLogicalNewlines`?

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/insertion.rs`:77 on 2023-05-31 07:16_

Can we use `range.end()` directly when matching on `Tok::Newline | Tok::NonLogicalNewLines` and simply skipping comments?

Again, we may be able to implement this with a "trivial" text search. 

---

_Review comment by @MichaReiser on `crates/ruff/src/importer/insertion.rs`:142 on 2023-05-31 07:21_

I'm trying to figure out the right terminology for "indented" content. In most languages, it's called a block. I, thus far, called it Body in the formatter. 

The only term that I found in the python documentation is *grouping of statements* even though this is, strictly speaking, not correct, because it also applies to non-statements (e.g. match cases)

> Leading whitespace (spaces and tabs) at the beginning of a logical line is used to compute the indentation level of the line, which in turn is used to determine the grouping of statements.



---

_Review comment by @MichaReiser on `crates/ruff/src/importer/insertion.rs`:167 on 2023-05-31 07:21_

Nit: To avoid panicking if the parentheses are unbalanced. 

```suggestion
                        state = Awaiting::Colon(depth.saturdating_sub(1));
```

---

_@MichaReiser approved on 2023-05-31 07:23_

---

_Comment by @charliermarsh on 2023-05-31 17:01_

I'm going to follow-up on the lexing comments in a separate PR -- all of that code already existed in `Insertion`, it was just moved around a bit. I agree that we should change it, will do it today.

---

_Merged by @charliermarsh on 2023-05-31 17:08_

---

_Closed by @charliermarsh on 2023-05-31 17:08_

---

_Branch deleted on 2023-05-31 17:08_

---
