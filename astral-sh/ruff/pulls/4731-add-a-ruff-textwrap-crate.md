```yaml
number: 4731
title: "Add a `ruff_textwrap` crate"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/indent
created_at: 2023-05-30T17:10:41Z
updated_at: 2023-05-31T16:53:07Z
url: https://github.com/astral-sh/ruff/pull/4731
synced_at: 2026-01-12T03:50:03Z
```

# Add a `ruff_textwrap` crate

---

_Pull request opened by @charliermarsh on 2023-05-30 17:10_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

The `textwrap` crate doesn't respect the existing newlines present on a given string, instead always rejoining the lines with LF line endings (`\n`). This PR adds our own `textwrap` crate that uses our universal newline handler to preserve the existing line ends.

Closes #1510.


---

_@charliermarsh reviewed on 2023-05-30 17:11_

---

_Review comment by @charliermarsh on `crates/ruff_newlines/src/lib.rs`:267 on 2023-05-30 17:11_

Should we just add `LineEnding` to `Line`, and save it when we extract during the iterator?

---

_@charliermarsh reviewed on 2023-05-30 17:14_

---

_Review comment by @charliermarsh on `crates/ruff_textwrap/src/lib.rs`:120 on 2023-05-30 17:14_

Unlike the source crate, this doesn't between mixed whitespace styles. For example, the original crate has this test, which I removed:

```rust
#[test]
#[rustfmt::skip]
fn dedent_mixed_whitespace() {
    let x = [
        "\tfoo",
        "  bar",
    ].join("\n");
    let y = [
        "\tfoo",
        "  bar",
    ].join("\n");
    assert_eq!(dedent(&x), y);
}
```

It's trickier, because you have to compare each prefix, rather than tracking the prefix _length_ alone. But Python doesn't support mixed indentation anyway, so I biased towards leaving it alone.

---

_Comment by @charliermarsh on 2023-05-30 17:15_

(FYI: I need to use this functionality for the flake8-type-checking autofixes, and I don't want to propagate this newline issue any further.)

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-30 17:34_

---

_Comment by @github-actions[bot] on 2023-05-30 18:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.0±0.03ms     2.9 MB/sec    1.00     14.0±0.04ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.00ms     4.9 MB/sec    1.00      3.4±0.00ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    424.6±2.96µs     6.9 MB/sec    1.00    425.8±1.06µs     6.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.9±0.01ms     4.4 MB/sec    1.00      5.9±0.02ms     4.3 MB/sec
linter/default-rules/large/dataset.py      1.00      6.8±0.06ms     6.0 MB/sec    1.00      6.8±0.01ms     6.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1497.9±5.92µs    11.1 MB/sec    1.01   1507.5±3.55µs    11.0 MB/sec
linter/default-rules/numpy/globals.py      1.00    169.6±0.28µs    17.4 MB/sec    1.02    172.5±1.39µs    17.1 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.01ms     8.3 MB/sec    1.00      3.1±0.01ms     8.2 MB/sec
parser/large/dataset.py                    1.00      5.2±0.01ms     7.9 MB/sec    1.00      5.2±0.00ms     7.9 MB/sec
parser/numpy/ctypeslib.py                  1.01   1017.1±0.49µs    16.4 MB/sec    1.00   1006.3±0.83µs    16.5 MB/sec
parser/numpy/globals.py                    1.01    105.9±0.26µs    27.9 MB/sec    1.00    104.4±0.12µs    28.3 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.00      2.2±0.00ms    11.5 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.0±0.17ms     2.4 MB/sec    1.01     17.1±0.16ms     2.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.3±0.03ms     3.9 MB/sec    1.01      4.4±0.05ms     3.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    446.4±4.91µs     6.6 MB/sec    1.02    457.4±6.16µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.3±0.17ms     3.5 MB/sec    1.01      7.3±0.08ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.00      8.5±0.05ms     4.8 MB/sec    1.01      8.6±0.10ms     4.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1791.2±14.03µs     9.3 MB/sec    1.02  1824.8±13.56µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    197.7±1.40µs    14.9 MB/sec    1.02    201.0±4.87µs    14.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.9±0.04ms     6.5 MB/sec    1.00      3.9±0.04ms     6.5 MB/sec
parser/large/dataset.py                    1.00      6.6±0.05ms     6.1 MB/sec    1.00      6.6±0.03ms     6.1 MB/sec
parser/numpy/ctypeslib.py                  1.01   1257.2±8.85µs    13.2 MB/sec    1.00   1247.6±8.34µs    13.3 MB/sec
parser/numpy/globals.py                    1.01    131.3±0.90µs    22.5 MB/sec    1.00    130.4±1.40µs    22.6 MB/sec
parser/pydantic/types.py                   1.00      2.8±0.02ms     9.0 MB/sec    1.00      2.8±0.02ms     9.1 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_newlines/src/lib.rs`:279 on 2023-05-31 06:50_

Nit: We can change `as_str` to use this new `newline_str` method:

```rust
let newline_len = self.newline_str.len();
&self.text[..self.text.len() - newline_len]
```

---

_Review comment by @MichaReiser on `crates/ruff_newlines/src/lib.rs`:267 on 2023-05-31 06:53_

It depends on the frequency `newline_str` is called. Adding `LineEnding` increases the size of `Line` by 4 bytes (2 bytes for `LineEnding` + 2 bytes padding). Re-computing the `newline_str` is relatively cheap. It only requires looking at two characters in the worst case. 

I started to prefer re-computing information to storing it because it reduces memory usage and can often be faster. But again, this depends on the usage patterns.

But you can do some profiling or set-up a microbenchmark to see if it changes performance in a meaningful way

---

_Review comment by @MichaReiser on `crates/ruff_newlines/src/lib.rs`:267 on 2023-05-31 06:55_

This may not work because of dependencies, but should we rename the method to `line_ending` and change the return type to `Option<LineEnding>`. It would allow to match on the line ending more easily. 

---

_Review comment by @MichaReiser on `crates/ruff_textwrap/src/lib.rs`:1 on 2023-05-31 06:56_

Is this code, except for the usage of `universal_newlines`, identical to `textwrap` or did you make changes?

---

_Review comment by @MichaReiser on `crates/ruff_textwrap/src/lib.rs`:120 on 2023-05-31 07:03_

Python does support mixed indentation, rust python doesn't. 

> Indentation is rejected as inconsistent if a source file mixes tabs and spaces in a way that makes the meaning dependent on the worth of a tab in spaces; a [TabError](https://docs.python.org/3/library/exceptions.html#TabError) is raised in that case. [[source](https://docs.python.org/3/reference/lexical_analysis.html#indentation)]

It took me a long time to understand what that means. My understanding (from looking at the CPython source is that) the column (respecting the tab width) and the character (counting tabs as 1) of two indentations must be the same. But `\t ` and ` \t` are equal indents. 

I assume what you mean by mixed whitespace styles is to use `\t` and `  ` which would not be equal because it's 1 character vs 2. 







---

_Review comment by @MichaReiser on `crates/ruff_textwrap/src/lib.rs`:92 on 2023-05-31 07:04_

Nit: rename to `prefix_len`. It's not the prefix itself.

---

_Review comment by @MichaReiser on `crates/ruff_textwrap/src/lib.rs`:96 on 2023-05-31 07:06_

Nit: You can do

```
let leading_whitespace_len = line.len() - line.trim_start().len();
prefix_len = Some(cmp::min(prefix_len.unwrap_or(prefix_len, leading_whitespace_len));
```

But both works equally well

---

_Review comment by @MichaReiser on `crates/ruff_textwrap/src/lib.rs`:100 on 2023-05-31 07:08_

What do you think of initializing `prefix` with `usize::MAX` as marker for 'no prefix'. Any non-empty string will change the value to a lower value

---

_Review comment by @MichaReiser on `crates/ruff_textwrap/src/lib.rs`:59 on 2023-05-31 07:09_

Does the original crate use `with_capacity * 2`. Seems a very pessimistic capacity.

---

_@MichaReiser approved on 2023-05-31 07:09_

---

_@charliermarsh reviewed on 2023-05-31 16:01_

---

_Review comment by @charliermarsh on `crates/ruff_textwrap/src/lib.rs`:59 on 2023-05-31 16:01_

Yes lol

---

_@charliermarsh reviewed on 2023-05-31 16:23_

---

_Review comment by @charliermarsh on `crates/ruff_textwrap/src/lib.rs`:1 on 2023-05-31 16:23_

I made changes -- the API is the same (except we return `Cow` as an optimization), but the functions needed to be almost entirely rewritten to use our `universal_newlines` API. The test suite is the same though.

---

_@charliermarsh reviewed on 2023-05-31 16:24_

---

_Review comment by @charliermarsh on `crates/ruff_textwrap/src/lib.rs`:120 on 2023-05-31 16:24_

Woah, I did not realize this -- I thought it was always a `TabError`.

---

_Merged by @charliermarsh on 2023-05-31 16:35_

---

_Closed by @charliermarsh on 2023-05-31 16:35_

---

_Branch deleted on 2023-05-31 16:35_

---
