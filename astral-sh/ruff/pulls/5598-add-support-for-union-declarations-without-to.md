```yaml
number: 5598
title: "Add support for `Union` declarations without `|` to PYI016"
type: pull_request
state: merged
author: zanieb
labels: []
assignees: []
merged: true
base: main
head: rule/pyi016
created_at: 2023-07-07T17:27:07Z
updated_at: 2023-07-10T17:33:06Z
url: https://github.com/astral-sh/ruff/pull/5598
synced_at: 2026-01-12T15:55:18Z
```

# Add support for `Union` declarations without `|` to PYI016

---

_@zanieb_

Previously, PYI016 only supported reporting violations for unions defined with `|`. Now, union declarations with `typing.Union` are supported.

This implementation reuses the union traversal logic from #5570.

PYI016 will not attempt to fix cases where `Union` is used. Unlike `|`, removing members from a `Union` can result in an invalid union definition. We may be able to support this in the future as in PYI030 the complexity seems high.

Tested with new snapshot cases `./target/debug/ruff --select PYI016 crates/ruff/resources/test/fixtures/flake8_pyi/PYI016.pyi --show-source`


---

_Review comment by @zanieb on `crates/ruff/resources/test/fixtures/flake8_pyi/PYI016.pyi`:50 on 2023-07-07 17:29_

I've added some more edge cases that are not relevant for this pull request but are good to cover

---

_@zanieb reviewed on 2023-07-07 17:29_

---

_Comment by @github-actions[bot] on 2023-07-07 17:38_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -0, 0 error(s))

<details><summary>bokeh (+3, -0)</summary>
<p>

<pre>
+ <a href='https://github.com/bokeh/bokeh/blob/branch-3.2/src/bokeh/core/property/factors.py#L51'>src/bokeh/core/property/factors.py:51:56:</a> PYI016 Duplicate union member `tuple[str, str]`
+ <a href='https://github.com/bokeh/bokeh/blob/branch-3.2/src/bokeh/core/property/factors.py#L52'>src/bokeh/core/property/factors.py:52:85:</a> PYI016 Duplicate union member `tp.Sequence[tuple[str, str]]`
+ <a href='https://github.com/bokeh/bokeh/blob/branch-3.2/src/bokeh/plotting/contour.py#L43'>src/bokeh/plotting/contour.py:43:88:</a> PYI016 Duplicate union member `ContourColor`
</pre>

</p>
</details>
Rules changed: 1

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| PYI016 | 3 | 3 | 0 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01      9.5±0.03ms     4.3 MB/sec    1.00      9.4±0.03ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.02      2.1±0.01ms     7.9 MB/sec    1.00      2.1±0.02ms     8.0 MB/sec
formatter/numpy/globals.py                 1.00    235.5±1.50µs    12.5 MB/sec    1.00    235.3±1.40µs    12.5 MB/sec
formatter/pydantic/types.py                1.03      4.7±0.03ms     5.5 MB/sec    1.00      4.5±0.01ms     5.6 MB/sec
linter/all-rules/large/dataset.py          1.03     16.9±0.10ms     2.4 MB/sec    1.00     16.3±0.11ms     2.5 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.2±0.01ms     4.0 MB/sec    1.00      4.1±0.02ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.00    526.0±2.37µs     5.6 MB/sec    1.00    524.5±3.25µs     5.6 MB/sec
linter/all-rules/pydantic/types.py         1.02      7.4±0.03ms     3.5 MB/sec    1.00      7.3±0.03ms     3.5 MB/sec
linter/default-rules/large/dataset.py      1.08      8.8±0.04ms     4.6 MB/sec    1.00      8.1±0.02ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.05   1879.3±4.13µs     8.9 MB/sec    1.00  1792.9±48.92µs     9.3 MB/sec
linter/default-rules/numpy/globals.py      1.03    211.8±1.06µs    13.9 MB/sec    1.00    204.7±0.72µs    14.4 MB/sec
linter/default-rules/pydantic/types.py     1.07      3.9±0.02ms     6.5 MB/sec    1.00      3.7±0.01ms     7.0 MB/sec
```

#### Windows
```
group                                      main                                    pr
-----                                      ----                                    --
formatter/large/dataset.py                 1.00     11.1±0.43ms     3.7 MB/sec     1.00     11.2±0.43ms     3.6 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.4±0.11ms     7.0 MB/sec     1.02      2.4±0.19ms     6.9 MB/sec
formatter/numpy/globals.py                 1.00   278.4±21.02µs    10.6 MB/sec     1.03   286.8±19.95µs    10.3 MB/sec
formatter/pydantic/types.py                1.04      5.4±0.28ms     4.7 MB/sec     1.00      5.2±0.25ms     4.9 MB/sec
linter/all-rules/large/dataset.py          1.00     17.6±0.88ms     2.3 MB/sec     1.07     18.9±0.53ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.9±0.36ms     3.4 MB/sec     1.00      4.9±0.15ms     3.4 MB/sec
linter/all-rules/numpy/globals.py          1.00   552.8±35.29µs     5.3 MB/sec     1.09   601.9±31.64µs     4.9 MB/sec
linter/all-rules/pydantic/types.py         1.00      7.8±0.31ms     3.3 MB/sec     1.09      8.5±0.40ms     3.0 MB/sec
linter/default-rules/large/dataset.py      1.00      8.9±0.54ms     4.6 MB/sec     1.10      9.7±0.34ms     4.2 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1993.0±115.12µs     8.4 MB/sec    1.02      2.0±0.08ms     8.2 MB/sec
linter/default-rules/numpy/globals.py      1.00    244.7±9.98µs    12.1 MB/sec     1.01   246.6±13.32µs    12.0 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.3±0.18ms     6.0 MB/sec     1.03      4.4±0.24ms     5.8 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @zanieb on 2023-07-07 17:43_

Example bokeh change https://github.com/bokeh/bokeh/blob/ee3f7f27fb744860763edd1d44fcac9b797e36b4/src/bokeh/core/property/factors.py#L51

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:2201 on 2023-07-08 19:20_

Nit: this part above can be rewritten with some optional chaining:

```rust
let check = self
    .semantic
    .expr_grandparent()
    .and_then(Expr::as_subscript_expr)
    .map_or(true, |parent| {
        !self.semantic.match_typing_expr(&parent.value, "Union")
    });
```

This is nice because you can avoid the `mut`, and all the logic is contained in one fluid expression. You could even inline this condition in the `if` and avoid creating `check` entirely.

---

_@charliermarsh reviewed on 2023-07-08 19:20_

---

_@charliermarsh reviewed on 2023-07-08 19:21_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/helpers.rs`:8 on 2023-07-08 19:21_

Nit: this could be `pub(super)`, since only modules in `flake8_pyi` need to have access to it.

---

_@charliermarsh reviewed on 2023-07-08 19:22_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:34 on 2023-07-08 19:22_

I'd probably recommend importing `HashSet` from `std::collections` so that you can just do: `let mut seen_nodes: HashSet<...> = ...`.

---

_@charliermarsh reviewed on 2023-07-08 19:23_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:55 on 2023-07-08 19:23_

I think this could be written as `if let Some(Expr::BinOp(...)) = parent` to avoid the `.expect` above. (I think this code already existed, but we generally prefer safe over unsafe even if the condition is _always_ expected to hold true -- it's debatable whether this is the right policy, here's a blog post that argues for the opposite: https://blog.burntsushi.net/unwrap/.)

---

_@charliermarsh reviewed on 2023-07-08 19:24_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:76 on 2023-07-08 19:24_

(It'd be nice to append these directly as we go rather than allocate an intermediary vector, but it's not critical. I assume that lifetimes and borrowing got in the way per Discord.)

---

_@charliermarsh approved on 2023-07-08 19:25_

This looks great! Love the additional test coverage too. Couple nits but nothing blocking.

---

_@dhruvmanila approved on 2023-07-09 08:38_

---

_Review comment by @MichaReiser on `crates/ruff/src/checkers/ast/mod.rs`:2201 on 2023-07-10 07:42_

Nit: I would prefer keeping the variable (I find such lengthy if's hard to read) but give it a name with more semantical meaning. Like, what are we testing here? `is_union` maybe?

This has the advantage that if there's a bug I have an easier time fixing it because:

* if `is_union` is `false` when the `expression` is a union -> Okay, the bug must be with the expression initializing `is_union`. The same if `is_union` is `true` when it should not.
* I don't need to spend time understanding this expression if the bug is with the diagnostic or fix generation but not identifying what a union is.

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/flake8_pyi/helpers.rs`:24 on 2023-07-10 07:42_

These comments are awesome. They make reviewing the code much easier and will certainly be useful when reading this code in the future.

---

_@MichaReiser approved on 2023-07-10 07:46_

---

_Review comment by @zanieb on `crates/ruff/src/checkers/ast/mod.rs`:2201 on 2023-07-10 15:07_

`is_union_but_grandparent_is_not` :) I'm not sure what a good name would be yet. I'll think on it.

edit: maybe `is_unchecked_union`

---

_@zanieb reviewed on 2023-07-10 15:07_

---

_@zanieb reviewed on 2023-07-10 15:13_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/helpers.rs`:24 on 2023-07-10 15:13_

These actually came from the previous contribution so I can't take credit :)

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:55 on 2023-07-10 15:21_

This is a little awkward actually we still need to have an `.unwrap()` when we call `range()` unless we want to write two `if let` statements? Perhaps I'm missing something. It is kind of nice to panic here because the traversal code is _broken_ if this invariant does not hold but 🤷‍♀️ I can see both sides.

---

_@zanieb reviewed on 2023-07-10 15:21_

---

_@zanieb reviewed on 2023-07-10 15:21_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:55 on 2023-07-10 15:21_

https://github.com/astral-sh/ruff/pull/5598/commits/cffdd346ad9fa37c3bc8c21de9ddd139a15160e6

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:76 on 2023-07-10 15:25_

Yeah I tried to do this a couple ways and it seems like a pain

---

_@zanieb reviewed on 2023-07-10 15:25_

---

_Review requested from @charliermarsh by @zanieb on 2023-07-10 16:52_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/mod.rs`:2 on 2023-07-10 16:59_

Can this just be `mod helpers`, since it doesn't need to accessed from outside of this module?

---

_@charliermarsh approved on 2023-07-10 17:00_

---

_@zanieb reviewed on 2023-07-10 17:02_

---

_Review comment by @zanieb on `crates/ruff/src/rules/flake8_pyi/mod.rs`:2 on 2023-07-10 17:02_

👍 thanks!

---

_Merged by @zanieb on 2023-07-10 17:11_

---

_Closed by @zanieb on 2023-07-10 17:11_

---
