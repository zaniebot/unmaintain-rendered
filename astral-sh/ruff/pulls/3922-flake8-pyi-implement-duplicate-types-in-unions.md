```yaml
number: 3922
title: "[`flake8-pyi`] Implement duplicate types in unions (`PYI016`)"
type: pull_request
state: merged
author: USER-5
labels: []
assignees: []
merged: true
base: main
head: pyi016-duplicates-in-union
created_at: 2023-04-09T04:01:20Z
updated_at: 2023-04-12T04:24:30Z
url: https://github.com/astral-sh/ruff/pull/3922
synced_at: 2026-01-12T04:28:19Z
```

# [`flake8-pyi`] Implement duplicate types in unions (`PYI016`)

---

_Pull request opened by @USER-5 on 2023-04-09 04:01_

Addresses Y016 in #848:

>  Unions shouldn't contain duplicates, e.g. str | str is not allowed.

I hope I'm correct in assuming that it's always fixable - and that a suitable fix is to remove the last duplicates.

---

_Comment by @github-actions[bot] on 2023-04-09 04:17_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     14.5±0.51ms     2.8 MB/sec    1.03     15.0±0.60ms     2.7 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.03      3.9±0.15ms     4.3 MB/sec    1.00      3.8±0.15ms     4.4 MB/sec
linter/all-rules/numpy/globals.py          1.02   503.9±22.32µs     5.9 MB/sec    1.00   495.6±28.52µs     6.0 MB/sec
linter/all-rules/pydantic/types.py         1.03      6.5±0.31ms     3.9 MB/sec    1.00      6.3±0.28ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.01      7.7±0.31ms     5.3 MB/sec    1.00      7.6±0.27ms     5.4 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.03  1746.5±88.77µs     9.5 MB/sec    1.00  1689.2±68.35µs     9.9 MB/sec
linter/default-rules/numpy/globals.py      1.05   198.0±10.30µs    14.9 MB/sec    1.00   188.4±12.23µs    15.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.5±0.16ms     7.4 MB/sec    1.02      3.5±0.17ms     7.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.01     15.9±0.08ms     2.6 MB/sec    1.00     15.7±0.09ms     2.6 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.01      4.1±0.08ms     4.1 MB/sec    1.00      4.1±0.04ms     4.1 MB/sec
linter/all-rules/numpy/globals.py          1.02    507.2±9.71µs     5.8 MB/sec    1.00    496.0±6.24µs     5.9 MB/sec
linter/all-rules/pydantic/types.py         1.01      6.8±0.08ms     3.8 MB/sec    1.00      6.7±0.06ms     3.8 MB/sec
linter/default-rules/large/dataset.py      1.02      8.3±0.20ms     4.9 MB/sec    1.00      8.1±0.09ms     5.0 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.01  1858.5±46.64µs     9.0 MB/sec    1.00  1833.2±38.63µs     9.1 MB/sec
linter/default-rules/numpy/globals.py      1.04    203.0±8.77µs    14.5 MB/sec    1.00    195.7±3.98µs    15.1 MB/sec
linter/default-rules/pydantic/types.py     1.04      4.0±0.11ms     6.4 MB/sec    1.00      3.8±0.13ms     6.7 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @USER-5 on `crates/ruff/src/rules/flake8_pyi/rules/duplicates_in_union.rs`:33 on 2023-04-09 06:24_

I was using a HashSet here, but it looks like ExprKind doesn't implement HashCode & Equals

---

_@USER-5 reviewed on 2023-04-09 06:34_

---

_Comment by @bluetech on 2023-04-09 15:50_

The `field6 = 1 | 0 | 1` case seems like a false-positive to me. I think this lint should only trigger when the `|` occurs in a typing context. I haven't checked, but I bet Ruff is already able to recognize typing contexts because there's a lot of lints which depend on it.

(Would also be nice to emit this lint for `Union[int, int]` and `x: "int | int"` but that's extra)

---

_@bluetech reviewed on 2023-04-09 15:54_

---

_Review comment by @bluetech on `crates/ruff/src/rules/flake8_pyi/rules/duplicates_in_union.rs`:33 on 2023-04-09 15:54_

While large unions are not common, I'd definitely say they're possible. So an O(n^2) algorithm does seems problematic.

---

_Comment by @charliermarsh on 2023-04-09 22:29_

> ...but I bet Ruff is already able to recognize typing contexts because there's a lot of lints which depend on it.

Haven't reviewed fully yet but yes: we do have this ability. You probably want to check on `checker.ctx.in_type_definition`.

---

_@charliermarsh reviewed on 2023-04-09 22:44_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicates_in_union.rs`:33 on 2023-04-09 22:44_

We have a thing to help with this -- check out `ComparableExpr`. You can convert from `Expr` to `ComparableExpr` to get location-agnostic hash and equality comparisons.

---

_@charliermarsh reviewed on 2023-04-09 22:45_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicates_in_union.rs`:33 on 2023-04-09 22:45_

We use this (e.g.) for the "repeated keys in dictionary" checks (`repeated_keys`).

---

_Comment by @USER-5 on 2023-04-09 23:32_

Is this a type-only lint? I originally had it that way but changed it because the description didn't mention types.

---

_@USER-5 reviewed on 2023-04-10 08:29_

---

_Review comment by @USER-5 on `crates/ruff/src/rules/flake8_pyi/rules/duplicates_in_union.rs`:33 on 2023-04-10 08:29_

Turns out I'd over-complicated things - I've reduced this part down to use the ComparableExpr and FxHashSet (I assume we're using that over the regular HashSet for speed in this project?).

The violation code is recursive now, which I hope is ok.

---

_Comment by @bluetech on 2023-04-10 12:25_

> Is this a type-only lint? I originally had it that way but changed it because the description didn't mention types.

IMO this should definitely be a typing lint. The `|` operator means something else in non-typing contexts, and this lint can be more useful if it's specific to typing (e.g. also handling `Union` and string annotations).

Relatedly, I believe flake8-pyi only runs on pyi files, but I don't see a reason to restrict this particular lint in Ruff only to pyi files.

---

_Comment by @USER-5 on 2023-04-10 12:32_

> IMO this should definitely be a typing lint. The | operator means something else in non-typing contexts, and this lint can be more useful if it's specific to typing (e.g. also handling Union and string annotations).

AFAIK in non-typing contexts it's a bitwise-or, which is equally pointless to have duplicates in (unless there are other uses of it), but I'm happy to change this to type-definitions only.

> Relatedly, I believe flake8-pyi only runs on pyi files, but I don't see a reason to restrict this particular lint in Ruff only to pyi files.

Yeah, I believe there have been a few comments about enabling it for `.py` files. I didn't add anything in this branch to restrict it to only pyi files, so I assume that's being done somewhere else in the code.

---

_Review comment by @bluetech on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_types_in_union.rs`:29 on 2023-04-10 12:54_

You are passing and returning `FxHashSet` by value, which means it's (theoretically) copied each time you pass and return it. Consider instead taking a `&mut FxHashSet<ComparableExpr<'a>>` and returning nothing. This will eliminate the copies.

---

_@bluetech reviewed on 2023-04-10 12:54_

---

_@USER-5 reviewed on 2023-04-10 13:17_

---

_Review comment by @USER-5 on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_types_in_union.rs`:29 on 2023-04-10 13:17_

I had some issues with the recursive calls when I did this thanks to the borrow checker. But I'll give it another crack now that I've got lifetime annotations here.

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:30 on 2023-04-11 04:12_

I tweaked this to use an inner helper, so that the caller doesn't have to worry or know about (e.g.) creating a hash set for internal use.

---

_@charliermarsh reviewed on 2023-04-11 04:12_

---

_@charliermarsh reviewed on 2023-04-11 04:12_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:21 on 2023-04-11 04:12_

Changed this to use the same message as `flake8-pyi`, I think.

---

_@charliermarsh reviewed on 2023-04-11 04:13_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:64 on 2023-04-11 04:13_

Changed this to use `unparse_expr`. E.g., if we have `str | str`, it'll now say `str` instead of `name`.

---

_@charliermarsh reviewed on 2023-04-11 04:15_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:88 on 2023-04-11 04:15_

I think this can lead to syntax errors in the presence of parentheses. E.g., if you try to autofix this, we throw a syntax error:

```py
field2: (str | int) | str  # PYI016 Duplicate name in union
```

I'm guessing the issue is that the previous `end_location` won't include the parenthesis -- so we change to:

```py
field2: (str | int  # PYI016 Duplicate name in union
```

(I'm guessing here, I haven't looked at the erroneous source code.)

As an alternative strategy, we could use `unparse_expr`, and re-create the source `expr`, but with the duplicate expression removed. (That function takes an AST node and generates source code from the AST.)  It'll be guaranteed to be correct and valid syntax, but we will lose trivia (comments, optional user-added parentheses) -- that seems ok here, since comments and such are pretty rare within these unions?


---

_Comment by @charliermarsh on 2023-04-11 04:15_

Great work! Should be good to merge once we correct that one nuance in the autofix.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:3274 on 2023-04-11 04:16_

Oh sorry -- I also changed this to `.pyi` only for consistency with the other `flake8-pyi` rules. I want to enable these for non-`.pyi` files, but I want to do it all at once for the plugin.

---

_@charliermarsh reviewed on 2023-04-11 04:16_

---

_Review comment by @USER-5 on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:64 on 2023-04-11 12:09_

Oh that's neat! I was using `id` before but it didn't work for all node types.

---

_@USER-5 reviewed on 2023-04-11 12:09_

---

_Review comment by @USER-5 on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:88 on 2023-04-11 12:31_

Hm, and there is also:

```python
field: str | (str | int)
```

Which would presumably get corrected to
```python
field: str | int)
```

In the above case, we would either want to:
* delete the `|` character _after_ the duplicate
* reconstruct from unparse - removing all brackets in the process
* delete as we do currently, then re-introduce a bracket at the correct location (this might be the easiest?)

~~I'm not sure what other "fix" tools I have at my disposal to help achieve this.~~

I just saw your edit about `unparse_expr` so I think I have a way forward.

---

_@USER-5 reviewed on 2023-04-11 12:31_

---

_Review comment by @USER-5 on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:88 on 2023-04-11 13:08_

I've hopefully fixed this issue, and I've added a couple test cases to the fixture for parentheses cases, which work as I'd expect:

```python
field9: int | (int | str)  # PYI016 Duplicate union member `int`
field10: (str | int) | str  # PYI016 Duplicate union member `str`
# Gets auto fixed to
field9: int | (str)  # PYI016 Duplicate union member `int`
field10: str | int  # PYI016 Duplicate union member `str`
```

---

_@USER-5 reviewed on 2023-04-11 13:08_

---

_Review comment by @USER-5 on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:79 on 2023-04-11 13:10_

I don't know if there's a better way to do this unwrapping.

---

_@USER-5 reviewed on 2023-04-11 13:10_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:88 on 2023-04-12 03:47_

Cool, this looks good to me!

---

_@charliermarsh reviewed on 2023-04-12 03:47_

---

_Renamed from "PYI016 - Duplicates in union" to "[`flake8-pyi`] Implement duplicate types in unions (`PYI016`)" by @charliermarsh on 2023-04-12 03:47_

---

_@charliermarsh reviewed on 2023-04-12 03:48_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/flake8_pyi/rules/duplicate_union_member.rs`:88 on 2023-04-12 03:48_

Yeah, the approach here makes sense. I guess I was suggesting taking the top-level expression (the thing we pass into `duplicate_union_member`) and recursively filtering to find the duplicated child, but what you settled on here seems better.

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:3284 on 2023-04-12 04:03_

@USER-5 - I loosened this check a bit, I think we just want to avoid flagging members that already would've been handled by the recursive check.

---

_@charliermarsh reviewed on 2023-04-12 04:03_

---

_@charliermarsh reviewed on 2023-04-12 04:03_

---

_Review comment by @charliermarsh on `crates/ruff/src/checkers/ast/mod.rs`:3284 on 2023-04-12 04:03_

As-is, we weren't flagging cases like `dict[str | str, int]`.

---

_Merged by @charliermarsh on 2023-04-12 04:06_

---

_Closed by @charliermarsh on 2023-04-12 04:06_

---
