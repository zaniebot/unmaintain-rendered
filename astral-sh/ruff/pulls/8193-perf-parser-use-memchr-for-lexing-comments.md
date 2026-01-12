```yaml
number: 8193
title: "perf(parser): use memchr for lexing comments"
type: pull_request
state: merged
author: sno2
labels:
  - parser
assignees: []
merged: true
base: main
head: main
created_at: 2023-10-25T01:40:48Z
updated_at: 2023-10-27T01:07:43Z
url: https://github.com/astral-sh/ruff/pull/8193
synced_at: 2026-01-12T02:32:42Z
```

# perf(parser): use memchr for lexing comments

---

_Pull request opened by @sno2 on 2023-10-25 01:40_

## Summary

This might improve the performance of lexing. Not sure, though.

## Test Plan

This was tested using the existing tests.


---

_Comment by @github-actions[bot] on 2023-10-25 01:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_Renamed from "perf(parser): use memchr to improve parsing" to "perf(parser): use memchr for lexing comments" by @sno2 on 2023-10-25 02:01_

---

_Comment by @sno2 on 2023-10-25 02:01_

Improvements seem neglible: https://codspeed.io/astral-sh/ruff/branches/sno2:main

---

_Closed by @sno2 on 2023-10-25 02:01_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:414 on 2023-10-25 02:04_

I think `memchr` handl this automatically and not having the configuration ensures that newly supported platforms automatically profit from a faster `memchr` routine.

---

_@MichaReiser reviewed on 2023-10-25 02:05_

It gives us a small speed up for the lexer. I didn't expect parsing to improve, because it performs way too many allocations. However, this could speed up things once we improve our parser.

---

_@sno2 reviewed on 2023-10-25 02:07_

---

_Review comment by @sno2 on `crates/ruff_python_parser/src/lexer.rs`:414 on 2023-10-25 02:07_

True, will look into this more.

---

_Reopened by @sno2 on 2023-10-25 02:08_

---

_Comment by @codspeed-hq[bot] on 2023-10-25 04:17_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/sno2:main)

### Merging #8193 will **not alter performance**

<sub>Comparing <code>sno2:main</code> (0d3717f) with <code>main</code> (6f31e9c)</sub>



### Summary

`✅ 25` untouched benchmarks






---

_Marked ready for review by @sno2 on 2023-10-25 15:17_

---

_Comment by @sno2 on 2023-10-25 15:18_

I've given up on trying to optimize the String creation so this is ready for review :^) It seems that the only way we could get a performance boost from comment string allocation in the lexer is to not do it at all and use lifetimes and `&str`s.

---

_Comment by @MichaReiser on 2023-10-27 01:06_

> I've given up on trying to optimize the String creation so this is ready for review :^) It seems that the only way we could get a performance boost from comment string allocation in the lexer is to not do it at all and use lifetimes and `&str`s.

I once had a pr that used `SmolStr` instead of `String` in the lexer to speed up things, but it didn't improve performance. I suspected that it was mainly because our string parsing is so slow (and allocates all over the place). It might be worth to give this another try after the string parsing rework lands

---

_Label `parser` added by @MichaReiser on 2023-10-27 01:06_

---

_@MichaReiser approved on 2023-10-27 01:07_

Thank you :)

---

_Merged by @MichaReiser on 2023-10-27 01:07_

---

_Closed by @MichaReiser on 2023-10-27 01:07_

---
