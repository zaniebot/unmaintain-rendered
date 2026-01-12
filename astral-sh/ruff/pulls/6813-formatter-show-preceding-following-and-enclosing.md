```yaml
number: 6813
title: " Formatter: Show preceding, following and enclosing nodes of comments, Attempt 2"
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: collect_decorated_comments2
created_at: 2023-08-23T11:54:15Z
updated_at: 2023-09-06T10:55:53Z
url: https://github.com/astral-sh/ruff/pull/6813
synced_at: 2026-01-12T02:45:38Z
```

#  Formatter: Show preceding, following and enclosing nodes of comments, Attempt 2

---

_Pull request opened by @konstin on 2023-08-23 11:54_

This is a new attempt after https://github.com/astral-sh/ruff/pull/6337.

**Summary** I used to always add `dbg!` for the preceding, following and enclosing node. With this change `--print-comments` can do this instead.

```python
match foo: # a
    # b
    case [1, 2, * # c
    rest]:
        pass
    # d
    case [1, 2, * #e
    _]:
        pass
    case [
        1,
        2,
        *rest,
    ]:
        pass
```
Additional `--print-comments` Output:
```
11..14 Some((ExprName, 6..9)) Some((MatchCase, 27..68)) (StmtMatch, 0..186) "# a"
19..22 Some((ExprName, 6..9)) Some((MatchCase, 27..68)) (StmtMatch, 0..186) "# b"
41..44 None None (PatternMatchStar, 39..53) "# c"
73..76 Some((MatchCase, 27..68)) Some((MatchCase, 81..118)) (StmtMatch, 0..186) "# d"
95..97 None None (PatternMatchStar, 93..103) "#e"
```

**Test Plan** n/a

---

_Label `internal` added by @konstin on 2023-08-23 11:54_

---

_Review requested from @MichaReiser by @konstin on 2023-08-23 11:54_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/visitor.rs`:546 on 2023-08-23 11:54_

i didn't find a better location for this

---

_@konstin reviewed on 2023-08-23 11:54_

---

_@konstin reviewed on 2023-08-23 11:55_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/visitor.rs`:46 on 2023-08-23 11:55_

side effect: the visitor is now private

---

_Comment by @github-actions[bot] on 2023-08-23 12:24_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      3.5±0.03ms    11.7 MB/sec    1.04      3.6±0.08ms    11.3 MB/sec
formatter/numpy/ctypeslib.py               1.00   709.8±12.54µs    23.5 MB/sec    1.02    724.8±6.08µs    23.0 MB/sec
formatter/numpy/globals.py                 1.00     73.2±0.35µs    40.3 MB/sec    1.02     75.0±0.31µs    39.3 MB/sec
formatter/pydantic/types.py                1.00  1411.3±12.13µs    18.1 MB/sec    1.02  1445.5±31.14µs    17.6 MB/sec
linter/all-rules/large/dataset.py          1.00     10.4±0.05ms     3.9 MB/sec    1.01     10.5±0.05ms     3.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      2.8±0.01ms     5.9 MB/sec    1.01      2.8±0.01ms     5.8 MB/sec
linter/all-rules/numpy/globals.py          1.00    316.4±2.06µs     9.3 MB/sec    1.01    319.9±1.74µs     9.2 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.4±0.01ms     4.7 MB/sec    1.01      5.4±0.03ms     4.7 MB/sec
linter/default-rules/large/dataset.py      1.00      5.5±0.01ms     7.4 MB/sec    1.01      5.6±0.03ms     7.3 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1178.4±5.08µs    14.1 MB/sec    1.00   1180.5±5.85µs    14.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    122.9±0.26µs    24.0 MB/sec    1.01    124.3±0.68µs    23.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      2.5±0.01ms    10.2 MB/sec    1.01      2.5±0.01ms    10.1 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.00      4.1±0.05ms     9.9 MB/sec    1.04      4.3±0.06ms     9.6 MB/sec
formatter/numpy/ctypeslib.py               1.00   820.5±11.22µs    20.3 MB/sec    1.04   850.1±13.68µs    19.6 MB/sec
formatter/numpy/globals.py                 1.00     84.5±2.55µs    34.9 MB/sec    1.06     89.5±2.10µs    33.0 MB/sec
formatter/pydantic/types.py                1.00  1654.3±23.05µs    15.4 MB/sec    1.04  1716.0±30.96µs    14.9 MB/sec
linter/all-rules/large/dataset.py          1.00     12.4±0.11ms     3.3 MB/sec    1.03     12.7±0.17ms     3.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.03ms     4.9 MB/sec    1.01      3.5±0.03ms     4.8 MB/sec
linter/all-rules/numpy/globals.py          1.00   431.5±14.23µs     6.8 MB/sec    1.00    432.2±6.59µs     6.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.5±0.12ms     3.9 MB/sec    1.01      6.6±0.08ms     3.9 MB/sec
linter/default-rules/large/dataset.py      1.00      6.9±0.06ms     5.9 MB/sec    1.02      7.0±0.06ms     5.8 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1474.9±18.64µs    11.3 MB/sec    1.01  1495.5±20.41µs    11.1 MB/sec
linter/default-rules/numpy/globals.py      1.00    172.5±2.74µs    17.1 MB/sec    1.01    175.0±2.65µs    16.9 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.1±0.03ms     8.2 MB/sec    1.02      3.2±0.04ms     8.0 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Comment by @MichaReiser on 2023-08-23 12:45_

Can you run some local benchmarks to confirm/deny the performance regression?

---

_Comment by @MichaReiser on 2023-08-26 16:07_

Should we just use `debug!` or something similar to print out the comments during debugging? We could even gate it with `cfg(debug_assertions)` to make sure it doesn't make it into production builds.

---

_Comment by @konstin on 2023-08-26 16:08_

you mean through tracing?

---

_Comment by @codspeed-hq[bot] on 2023-08-26 16:12_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/collect_decorated_comments2)

### Merging #6813 will **not alter performance**

<sub>Comparing <code>collect_decorated_comments2</code> (b153208) with <code>main</code> (15b73bd)</sub>



### Summary

`✅ 16` untouched benchmarks






---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/mod.rs`:346 on 2023-09-06 07:38_

This method feels misplaced because it doesn't involve `Comments` at all. Why not expose `collect_comments` directly?

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:29 on 2023-09-06 07:41_

Nit: I'm leaning towards inlining this function into `Comments::from_ast` because `get_comments_map` sounds like your getting a value rather than computing it. Maybe rename it to `build_comments_map`? IMO, `CommentsVisitor` and `CommentsBuilder` being visible to the comments module seems reasonable to me. 

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:532 on 2023-09-06 07:48_

It's uncommon to include the type-kind in the name, especially because only the `CommentsBuilder` is building `Comments`. 

A rusty name could be `AddComment`, or we use `Add<DecoratedComment>` and make `locator` a field on `CommentsBuilder`. 

Or another idea is to accept any type that implements `Extend`. The downside is that we'll have to pass `[comment]` because `extend_one` isn't stable. But I think that resembles the idea the closest. You get a comment and can append it to some data structure. Which also removes the need for `CommentsCollector` because you can simply pass a `&mut Vec`.

I think I'm then leaning to making `collect_comments` public and make it accept any `&dyn Extend<DecoratedComment<'a>>` and to make  `CommentsBuilder` pub(super)`. 

---

_@MichaReiser approved on 2023-09-06 07:52_

---

_@konstin reviewed on 2023-09-06 09:27_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/comments/visitor.rs`:532 on 2023-09-06 09:27_

I went with `PushComment::push_comment`, mirroring `Vec`. A possible improvement could be implementing `PushComment` directly on `CommentsMap<'a>` and `Vec<DecoratedComment<'a>>`. I think we do need a dedicated trait though.

---

_@MichaReiser reviewed on 2023-09-06 09:46_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/comments/visitor.rs`:532 on 2023-09-06 09:46_

I don't mind implementing it directly on `Vec<DecoratedComment>` but I think I would keep the builder around since we don't want adding the `Locator` to `CommentsMap`.

---

_Merged by @konstin on 2023-09-06 10:26_

---

_Closed by @konstin on 2023-09-06 10:26_

---

_Branch deleted on 2023-09-06 10:26_

---

_Comment by @MichaReiser on 2023-09-06 10:55_

One thing I forgot to mention before. I struggle reading the output of the new comment because it isn't clear to me which node is the enclosing, trailing, or leading node. I think it would help to have labels and/or use Rust's expanded struct display

---
