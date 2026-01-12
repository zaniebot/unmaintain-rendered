```yaml
number: 6836
title: Add support for PatternMatchMapping formatting
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/mapping
created_at: 2023-08-24T04:36:51Z
updated_at: 2023-08-25T04:56:54Z
url: https://github.com/astral-sh/ruff/pull/6836
synced_at: 2026-01-12T02:45:38Z
```

# Add support for PatternMatchMapping formatting

---

_Pull request opened by @charliermarsh on 2023-08-24 04:36_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

Adds support for `PatternMatchMapping` -- i.e., cases like:

```python
match foo:
    case {"a": 1, "b": 2, **rest}:
        pass
```

Unfortunately, this node has _three_ kinds of dangling comments:

```python
{  # "open parenthesis comment"
   key: pattern,
   **  # end-of-line "double star comment"
   # own-line "double star comment"
   rest  # end-of-line "after rest comment"
   # own-line "after rest comment"
}
```

Some of the complexity comes from the fact that in `**rest`, `rest` is an _identifier_, not a node, so we have to handle comments _after_ it as dangling on the enclosing node, rather than trailing on `**rest`. (We could change the AST to use `PatternMatchAs` there, which would be more permissive than the grammar but not totally crazy -- `PatternMatchAs` is used elsewhere to mean "a single identifier".)

Closes https://github.com/astral-sh/ruff/issues/6644.

## Test Plan

`cargo test`


---

_Label `formatter` added by @charliermarsh on 2023-08-24 04:43_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-24 04:47_

---

_Review requested from @dhruvmanila by @charliermarsh on 2023-08-24 04:47_

---

_Comment by @charliermarsh on 2023-08-24 04:48_

We may want to instead change the `rest` in `**rest` to a `Pattern`, to simplify the comment handling -- I've put that change up here (https://github.com/astral-sh/ruff/pull/6838). If that seems like a reasonable idea, I'll merge that first, then rework this PR.

---

_Comment by @github-actions[bot] on 2023-08-24 05:18_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.5±0.13ms     9.1 MB/sec    1.00      4.5±0.11ms     9.1 MB/sec
formatter/numpy/ctypeslib.py               1.00   947.3±28.18µs    17.6 MB/sec    1.00   946.5±43.83µs    17.6 MB/sec
formatter/numpy/globals.py                 1.00     87.4±3.65µs    33.8 MB/sec    1.02     89.1±3.84µs    33.1 MB/sec
formatter/pydantic/types.py                1.00  1852.6±57.87µs    13.8 MB/sec    1.02  1885.8±97.98µs    13.5 MB/sec
linter/all-rules/large/dataset.py          1.04     12.5±0.27ms     3.2 MB/sec    1.00     12.1±0.22ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.3±0.07ms     5.0 MB/sec    1.00      3.2±0.07ms     5.2 MB/sec
linter/all-rules/numpy/globals.py          1.02   472.4±18.36µs     6.2 MB/sec    1.00   462.7±15.09µs     6.4 MB/sec
linter/all-rules/pydantic/types.py         1.04      6.5±0.14ms     3.9 MB/sec    1.00      6.3±0.10ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.05      6.7±0.14ms     6.0 MB/sec    1.00      6.4±0.12ms     6.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1440.8±27.79µs    11.6 MB/sec    1.01  1454.9±97.40µs    11.4 MB/sec
linter/default-rules/numpy/globals.py      1.05    177.8±5.98µs    16.6 MB/sec    1.00    169.6±3.12µs    17.4 MB/sec
linter/default-rules/pydantic/types.py     1.02      3.0±0.08ms     8.4 MB/sec    1.00      3.0±0.06ms     8.6 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.7±0.06ms     8.6 MB/sec    1.02      4.8±0.07ms     8.5 MB/sec
formatter/numpy/ctypeslib.py               1.00   970.0±18.87µs    17.2 MB/sec    1.01   980.3±19.30µs    17.0 MB/sec
formatter/numpy/globals.py                 1.00     92.9±1.92µs    31.7 MB/sec    1.04     96.3±4.72µs    30.6 MB/sec
formatter/pydantic/types.py                1.00  1937.4±25.48µs    13.2 MB/sec    1.01  1960.0±36.18µs    13.0 MB/sec
linter/all-rules/large/dataset.py          1.00     13.0±0.15ms     3.1 MB/sec    1.00     13.0±0.16ms     3.1 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.5±0.04ms     4.7 MB/sec    1.01      3.5±0.04ms     4.7 MB/sec
linter/all-rules/numpy/globals.py          1.00    436.9±5.45µs     6.8 MB/sec    1.00    438.1±6.38µs     6.7 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.6±0.09ms     3.9 MB/sec    1.01      6.7±0.10ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.00      7.0±0.07ms     5.8 MB/sec    1.01      7.1±0.09ms     5.7 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1508.7±23.86µs    11.0 MB/sec    1.01  1524.4±17.70µs    10.9 MB/sec
linter/default-rules/numpy/globals.py      1.00    175.7±2.63µs    16.8 MB/sec    1.00    176.2±3.73µs    16.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.2±0.04ms     7.9 MB/sec    1.00      3.2±0.04ms     7.9 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_mapping.rs`:32 on 2023-08-24 07:22_

Lol, Our DSL sometimes is too powerful. I was like, whoot, since when does `&[SourceComment]` implement `Format` but it's `empty_parenthesized` that accepts comments.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_mapping.rs`:69 on 2023-08-24 07:24_

Nit: I prefer to use `if_else` to reduce the nesting (or else, but our pedantic rules don't allow me to)

```suggestion
        let (open_parenthesis_comments, double_star_comments, after_rest_comments) = if let Some((double_star, rest)) = 
            find_double_star(item, f.context().source()) {
            	let (open_parenthesis_comments, dangling) =
                        dangling.split_at(dangling.partition_point(|comment| {
                            comment.line_position().is_end_of_line()
                                && comment.start() < double_star.start()
                        }));
                    let (double_star_comments, after_rest_comments) = dangling.split_at(
                        dangling.partition_point(|comment| comment.start() < rest.start()),
                    );
                    (
                        open_parenthesis_comments,
                        double_star_comments,
                        after_rest_comments,
                    )
            } else {
            	(&[], &[], &[])
          	};
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_mapping.rs`:53 on 2023-08-24 07:25_

Nit: This confused me a bit. Like, why repeat the same comments three times. Using empty slices may be less confusing 

```suggestion
                (&[], &[] &[])
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_mapping.rs`:91 on 2023-08-24 07:26_

NIt
```suggestion
            trailing_comments(after_rest_comments).fmt(f)
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_mapping.rs`:176 on 2023-08-24 07:27_

You can tell which methods you or I wrote by the name of `contents`/`source` :laughing: (or `type_` vs `type_expression`)

---

_@MichaReiser approved on 2023-08-24 07:28_

I love it how you came in and are pushing match formatting over the line!

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1229 on 2023-08-24 07:32_

imo we can skip this since we pass the pattern in.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/placement.rs`:1298 on 2023-08-24 09:46_

When would we hit this case? I tried
```python
match x:
    case {1: (2 # a
    # b
    ), **rest}:
        pass
```
but i only get trailing comments on the 2

---

_@konstin approved on 2023-08-24 09:49_

---

_@charliermarsh reviewed on 2023-08-24 13:23_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/pattern/pattern_match_mapping.rs`:53 on 2023-08-24 13:23_

I totally agree but I can't get `(dangling, &[], &[])` to compile :(

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/pattern/pattern_match_mapping.rs`:53 on 2023-08-24 13:30_

`([].as_slice(), [].as_slice(), [].as_slice()),` works 

---

_@MichaReiser reviewed on 2023-08-24 13:30_

---

_@charliermarsh reviewed on 2023-08-25 04:15_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/pattern/pattern_match_mapping.rs`:91 on 2023-08-25 04:15_

I find that I tend to write it this way because it reduces diffs. E.g., it's odd that the _last_ format call omits `?;` "just because" it's the last, unlike the others (reminds me of trailing commas). But I will change :)

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/pattern/pattern_match_mapping.rs`:176 on 2023-08-25 04:15_

I'm trying to do `source` now everywhere!

---

_@charliermarsh reviewed on 2023-08-25 04:15_

---

_@charliermarsh reviewed on 2023-08-25 04:24_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/placement.rs`:1298 on 2023-08-25 04:24_

Yes you're right -- thanks.

---

_Merged by @charliermarsh on 2023-08-25 04:33_

---

_Closed by @charliermarsh on 2023-08-25 04:33_

---

_Branch deleted on 2023-08-25 04:33_

---
