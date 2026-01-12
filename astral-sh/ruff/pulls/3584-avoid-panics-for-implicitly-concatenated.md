```yaml
number: 3584
title: Avoid panics for implicitly-concatenated docstrings
type: pull_request
state: merged
author: charliermarsh
labels:
  - bug
  - docstring
assignees: []
merged: true
base: main
head: charlie/doc
created_at: 2023-03-17T20:26:07Z
updated_at: 2023-03-19T18:20:54Z
url: https://github.com/astral-sh/ruff/pull/3584
synced_at: 2026-01-12T15:55:13Z
```

# Avoid panics for implicitly-concatenated docstrings

---

_@charliermarsh_

## Summary

In the rare event that a docstring contains an implicit string concatenation, we currently have the potential to panic, because we assume that if a string starts with triple quotes, it _ends_ with triple quotes. But with implicit concatenation, that's not the case: a single `Expr` could start and end with different quote styles, because it can contain multiple string tokens.

Supporting these "properly" is pretty hard. In some cases it's hard to even know what the "right" behavior is. So for now, I'm just detecting and warning, which is better than a panic.

Closes #3543.

Closes #3585.


---

_Review requested from @MichaReiser by @charliermarsh on 2023-03-17 20:26_

---

_Label `bug` added by @charliermarsh on 2023-03-17 20:26_

---

_Label `docstring` added by @charliermarsh on 2023-03-17 20:26_

---

_@charliermarsh reviewed on 2023-03-17 20:26_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:5142 on 2023-03-17 20:26_

Ideally this would be a diagnostic but I'm not sure that it fits any of our existing diagnostics under the `pydocstyle` category.

---

_Comment by @github-actions[bot] on 2023-03-17 20:50_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     17.5±0.47ms     2.3 MB/sec    1.00     17.5±0.29ms     2.3 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      4.5±0.12ms     3.7 MB/sec    1.00      4.5±0.15ms     3.7 MB/sec
linter/all-rules/numpy/globals.py          1.00   614.7±18.48µs     4.8 MB/sec    1.00   617.0±22.13µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.7±0.17ms     3.3 MB/sec    1.00      7.7±0.17ms     3.3 MB/sec
linter/default-rules/large/dataset.py      1.00      9.1±0.21ms     4.5 MB/sec    1.00      9.1±0.16ms     4.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.0±0.06ms     8.2 MB/sec    1.01      2.0±0.07ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    247.2±7.05µs    11.9 MB/sec    1.00    245.9±8.53µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.11ms     5.9 MB/sec    1.01      4.3±0.17ms     5.9 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     15.3±0.09ms     2.7 MB/sec    1.00     14.7±0.10ms     2.8 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      4.1±0.04ms     4.0 MB/sec    1.00      4.0±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    521.1±4.52µs     5.7 MB/sec    1.00    520.2±4.53µs     5.7 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.8±0.05ms     3.8 MB/sec    1.00      6.5±0.08ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.07      8.6±0.05ms     4.7 MB/sec    1.00      8.0±0.04ms     5.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05  1863.1±17.89µs     8.9 MB/sec    1.00  1779.9±22.22µs     9.4 MB/sec
linter/default-rules/numpy/globals.py      1.03    203.7±4.58µs    14.5 MB/sec    1.00    197.7±2.64µs    14.9 MB/sec
linter/default-rules/pydantic/types.py     1.06      4.0±0.05ms     6.4 MB/sec    1.00      3.7±0.07ms     6.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/pydocstyle/helpers.rs`:66 on 2023-03-18 07:22_

Can we change its visibility to pub(crate) or pub(super)?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:80 on 2023-03-18 07:25_

This function only returns the correct result if the content is an expression. Would it be better to pass it the locator and the expression?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:78 on 2023-03-18 08:28_

This change regresses all our benchmarks by 3-5%. I assume that calling the lexer is the expensive operation. 

```
❯ cargo benchmark --baseline=main
    Finished bench [optimized] target(s) in 0.10s
     Running benches/linter.rs (target/release/deps/linter-b1b4f7dad7c3da18)
linter/default-rules/numpy/globals.py
                        time:   [114.58 µs 114.69 µs 114.81 µs]
                        thrpt:  [25.700 MiB/s 25.727 MiB/s 25.753 MiB/s]
                 change:
                        time:   [+3.4859% +3.6247% +3.7608%] (p = 0.00 < 0.05)
                        thrpt:  [-3.6245% -3.4979% -3.3685%]
                        Performance has regressed.
Found 4 outliers among 100 measurements (4.00%)
  4 (4.00%) high mild
linter/default-rules/pydantic/types.py
                        time:   [2.2319 ms 2.2410 ms 2.2508 ms]
                        thrpt:  [11.331 MiB/s 11.380 MiB/s 11.427 MiB/s]
                 change:
                        time:   [+2.6956% +2.9809% +3.2544%] (p = 0.00 < 0.05)
                        thrpt:  [-3.1519% -2.8946% -2.6248%]
                        Performance has regressed.
Found 20 outliers among 100 measurements (20.00%)
  15 (15.00%) low severe
  3 (3.00%) high mild
  2 (2.00%) high severe
linter/default-rules/numpy/ctypeslib.py
                        time:   [1.0648 ms 1.0652 ms 1.0658 ms]
                        thrpt:  [15.623 MiB/s 15.632 MiB/s 15.638 MiB/s]
                 change:
                        time:   [+3.7858% +3.8922% +4.0261%] (p = 0.00 < 0.05)
                        thrpt:  [-3.8703% -3.7464% -3.6477%]
                        Performance has regressed.
Found 1 outliers among 100 measurements (1.00%)
  1 (1.00%) high severe
linter/default-rules/large/dataset.py
                        time:   [5.0569 ms 5.0704 ms 5.0813 ms]
                        thrpt:  [8.0064 MiB/s 8.0236 MiB/s 8.0450 MiB/s]
                 change:
                        time:   [+4.4705% +4.9010% +5.3636%] (p = 0.00 < 0.05)
                        thrpt:  [-5.0906% -4.6720% -4.2792%]
                        Performance has regressed.

linter/all-rules/numpy/globals.py
                        time:   [266.26 µs 266.46 µs 266.64 µs]
                        thrpt:  [11.066 MiB/s 11.074 MiB/s 11.082 MiB/s]
                 change:
                        time:   [+5.0624% +5.4244% +5.7740%] (p = 0.00 < 0.05)
                        thrpt:  [-5.4588% -5.1453% -4.8185%]
                        Performance has regressed.
Found 7 outliers among 100 measurements (7.00%)
  3 (3.00%) high mild
  4 (4.00%) high severe
linter/all-rules/pydantic/types.py
                        time:   [3.6534 ms 3.6759 ms 3.6976 ms]
                        thrpt:  [6.8973 MiB/s 6.9380 MiB/s 6.9807 MiB/s]
                 change:
                        time:   [+0.0982% +0.5182% +0.9516%] (p = 0.02 < 0.05)
                        thrpt:  [-0.9427% -0.5156% -0.0981%]
                        Change within noise threshold.
Found 2 outliers among 100 measurements (2.00%)
  2 (2.00%) high mild
linter/all-rules/numpy/ctypeslib.py
                        time:   [2.2603 ms 2.2640 ms 2.2688 ms]
                        thrpt:  [7.3392 MiB/s 7.3548 MiB/s 7.3667 MiB/s]
                 change:
                        time:   [+4.0998% +4.3497% +4.6664%] (p = 0.00 < 0.05)
                        thrpt:  [-4.4583% -4.1684% -3.9383%]
                        Performance has regressed.
Found 13 outliers among 100 measurements (13.00%)
  4 (4.00%) high mild
  9 (9.00%) high severe
linter/all-rules/large/dataset.py
                        time:   [8.6941 ms 8.6980 ms 8.7021 ms]
                        thrpt:  [4.6751 MiB/s 4.6773 MiB/s 4.6793 MiB/s]
                 change:
                        time:   [+1.8525% +2.0888% +2.3387%] (p = 0.00 < 0.05)
                        thrpt:  [-2.2853% -2.0461% -1.8188%]
                        Performance has regressed.
Found 25 outliers among 100 measurements (25.00%)
  13 (13.00%) low severe
  5 (5.00%) low mild
  3 (3.00%) high mild
  4 (4.00%) high severe
```

Can we apply a heuristic to short-circuit the call into the lexer? Or implement the test explicitly by:

* Testing if the start and end quotes are identical: if not -> implicit concatenation 
* find the closing quote character (ignoring escaped quotes): If it is at the end of contents -> all good, if not -> concatenation. We should be able to use `find` only for docstrings, if my assumption that triple quotes can't be escaped is correct



---

_@MichaReiser reviewed on 2023-03-18 08:28_

The code looks good, but we should investigate the performance regression.

---

_@charliermarsh reviewed on 2023-03-18 17:12_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:78 on 2023-03-18 17:12_

Great call, horray for continuous benchmarking! Let me play around with this. I'm sure we can do better.

---

_@charliermarsh reviewed on 2023-03-18 20:08_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:78 on 2023-03-18 20:08_

Alright, let's see if this new version does any better...

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:80 on 2023-03-18 22:03_

I think I prefer this interface at-present since it's consistent with the rest of this module, which operates directly on strings (and thus has no dependencies, isn't coupled to the AST, etc.).

---

_@charliermarsh reviewed on 2023-03-18 22:03_

---

_@charliermarsh reviewed on 2023-03-18 22:04_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:78 on 2023-03-18 22:04_

This looks much _better_ but there's still a noticeable difference in the NumPy case.

---

_@charliermarsh reviewed on 2023-03-18 23:29_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:78 on 2023-03-18 23:29_

Actually... is this just noise? The default-rules benchmark shouldn't be affected by this change.

---

_@MichaReiser reviewed on 2023-03-19 12:42_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:78 on 2023-03-19 12:42_

1-2% changes are most often just noise. Especially if they are specific to a single test. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:80 on 2023-03-19 12:43_

Could you add a few examples (ideally, by using doctetsts)?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:73 on 2023-03-19 12:44_

Can we return `false` if the string does not have any quotes (avoids the potential panic)
```suggestion
    let Some(leading_quote_str) = leading_quote(content) else { return false; }
    let Some(trailing_quote_str) = trailing_quote(content) else { return false; }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:100 on 2023-03-19 12:48_

I understand that `trailing_quote` returns the quotes used inside the string. For example, it returns `""""` for `"""A docstring"""`. 

Shoulnd't `""" === trailing_quote(""")` always hold?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:81 on 2023-03-19 12:50_

Nit: You can use `let rest = &content[leading_quote_str.len()..]` to get the "rest" string without the leading quote so that you don't have to do as much offset calculations

You could even split the trailing_quote by using

```
let rest = &rest[..rest.len() - trailing_quote_str.len()];
```

That then eliminates the need for the unreachable because not finding a trailing quote means it's not implicitly concatenated. 


---

_@MichaReiser approved on 2023-03-19 12:52_

Neat! Thanks for taking the time to improve the performance.

---

_@charliermarsh reviewed on 2023-03-19 15:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:100 on 2023-03-19 15:37_

D'oh, this is supposed to be `trailing_quote(leading_quote_str)` on the right-hand side. Wasn't caught by tests since it's just a short-circuit...

To be clear, the trick is that the leading quote includes the string "kind" prefix.

So given a string like:

```py
u"""abc def"""
```

The leading quote would be `u"""`, and the trailing quote would be `"""`.

---

_@charliermarsh reviewed on 2023-03-19 15:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:81 on 2023-03-19 15:37_

Make :clap: invalid :clap: states :clap: unrepresentable

---

_@MichaReiser reviewed on 2023-03-19 16:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:100 on 2023-03-19 16:05_

I'm still somewhat confused. 

Will `trailing_quote(leading_quote_str)` not always return the `leading_quote_str` because the string only consists of the quotation characters?

I would expect it to be `leading_quote_str != trailing_quote_str`

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:81 on 2023-03-19 16:07_

Fixed, I had to use `let mut rest` and reassign as I went -- hopefully that matches your intention here.

---

_@charliermarsh reviewed on 2023-03-19 16:07_

---

_@charliermarsh reviewed on 2023-03-19 16:09_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:100 on 2023-03-19 16:09_

No, because `u"""` and `"""` is a valid leading-trailing quote pair.

What we want to know is that if the string has a leading quote of `u"""`, then its expected trailing quote is `"""` (which we can compute as `trailing_quote(u""")`). Here, we're checking if that expected trailing quote matches the _actual_ trailing quote.

For example, given:

```
u"""abc""" 'def'
```

The leading quote would be `u"""`. The trailing quote would be `'`. The _expected_ trailing quote would be `"""`. Since `'` does not equal `"""`, we'd return `false`.

---

_@charliermarsh reviewed on 2023-03-19 16:10_

---

_Review comment by @charliermarsh on `crates/ruff_python_ast/src/str.rs`:100 on 2023-03-19 16:10_

I'd like to make this clearer since it's evidently too confusing :joy: Any suggestions?

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/str.rs`:100 on 2023-03-19 16:17_

Uhh, python quotes are confusing. Why so many :confused: 

> I'd like to make this clearer since it's evidently too confusing joy Any suggestions?

Paste your explanation as a comment :) 

---

_@MichaReiser reviewed on 2023-03-19 16:17_

---

_Merged by @charliermarsh on 2023-03-19 18:16_

---

_Closed by @charliermarsh on 2023-03-19 18:16_

---

_Branch deleted on 2023-03-19 18:16_

---
