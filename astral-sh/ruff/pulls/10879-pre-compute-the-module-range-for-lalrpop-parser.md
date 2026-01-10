```yaml
number: 10879
title: Pre-compute the module range for LALRPOP parser
type: pull_request
state: merged
author: dhruvmanila
labels:
  - internal
assignees: []
merged: true
base: dhruv/parser
head: dhruv/mod-range-lalrpop
created_at: 2024-04-11T13:20:51Z
updated_at: 2024-04-12T06:18:19Z
url: https://github.com/astral-sh/ruff/pull/10879
synced_at: 2026-01-10T22:37:01Z
```

# Pre-compute the module range for LALRPOP parser

---

_Pull request opened by @dhruvmanila on 2024-04-11 13:20_

## Summary

This PR updates the LALRPOP to use the complete source range for module by looking at the tokens before the parsing begins. This means the module range now corresponds to the actual source even if it only contains trivia tokens. This is mainly so that the fuzzer doesn't crash because the AST doesn't match.


---

_Label `internal` added by @dhruvmanila on 2024-04-11 13:20_

---

_Review requested from @MichaReiser by @dhruvmanila on 2024-04-11 13:20_

---

_@MichaReiser approved on 2024-04-11 16:29_

---

_Comment by @MichaReiser on 2024-04-11 16:31_

Oh no, the formatter fails now. I think we need to fix

https://github.com/astral-sh/ruff/blob/e2ec42539bfedc13270a2164faebe61e3c2e51dd/crates/ruff_python_formatter/src/module/mod_module.rs#L19-L23

to count the lines after `range.start()` instead of after `range.end()`?

---

_Comment by @MichaReiser on 2024-04-11 16:35_

I went ahead and pushed the fix. I hope that's okay with you

---

_Comment by @dhruvmanila on 2024-04-11 16:36_

Thank you!

---

_Merged by @dhruvmanila on 2024-04-11 16:45_

---

_Closed by @dhruvmanila on 2024-04-11 16:45_

---

_Branch deleted on 2024-04-11 16:45_

---

_Comment by @codspeed-hq[bot] on 2024-04-11 16:50_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/dhruv/mod-range-lalrpop)

### Merging #10879 will **degrade performances by 5.27%**

<sub>Comparing <code>dhruv/mod-range-lalrpop</code> (d2d4b3e) with <code>dhruv/parser</code> (b069358)</sub>



### Summary

`⚡ 2` improvements
`❌ 1` regressions
`✅ 27` untouched benchmarks



> :warning: _Please fix the performance issues or [acknowledge them on CodSpeed](https://codspeed.io/astral-sh/ruff/branches/dhruv/mod-range-lalrpop)._

### Benchmarks breakdown

|     | Benchmark | `dhruv/parser` | `dhruv/mod-range-lalrpop` | Change |
| --- | --------- | ----------------------- | ------------------- | ------ |
| ⚡ | `parser[large/dataset.py]` | 28.8 ms | 26.7 ms | +8.16% |
| ⚡ | `parser[pydantic/types.py]` | 11.8 ms | 10.9 ms | +8.77% |
| ❌ | `parser[unicode/pypinyin.py]` | 1.6 ms | 1.7 ms | -5.27% |


---

_Comment by @dhruvmanila on 2024-04-12 06:18_

Ugh, I force pushed the branch and removed your change, I'll fix this.

---
