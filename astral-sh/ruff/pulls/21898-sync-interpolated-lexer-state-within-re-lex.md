```yaml
number: 21898
title: "Sync interpolated lexer state within `re_lex_logical_token`"
type: pull_request
state: closed
author: MichaReiser
labels:
  - parser
assignees: []
base: main
head: micha/relex-interpolated-state
created_at: 2025-12-10T16:18:20Z
updated_at: 2025-12-26T11:49:35Z
url: https://github.com/astral-sh/ruff/pull/21898
synced_at: 2026-01-12T15:57:36Z
```

# Sync interpolated lexer state within `re_lex_logical_token`

---

_@MichaReiser_

## Summary

I don't think this is the proper fix but it seems more correct than what we have today? 

This PR ensures that we also update the interpolated string state when decrementing the nesting level
to avoid that it has pending interpolations that are more deeply nested than the lexer itself.

I'm pretty neutral on whether we should land this change.

I wrote up a more detailed summary here https://github.com/astral-sh/ruff/issues/21896


## Test Plan

Updated test


---

_Review requested from @dhruvmanila by @MichaReiser on 2025-12-10 16:18_

---

_Label `parser` added by @MichaReiser on 2025-12-10 16:18_

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:1547 on 2025-12-10 16:26_

```suggestion
```

---

_@MichaReiser reviewed on 2025-12-10 16:26_

---

_Comment by @astral-sh-bot[bot] on 2025-12-10 16:26_


<!-- generated-comment ecosystem -->


## `ruff-ecosystem` results

### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.





---

_Comment by @codspeed-hq[bot] on 2025-12-10 16:45_

<!-- __CODSPEED_PERFORMANCE_REPORT_COMMENT__ -->
## [CodSpeed Performance Report](https://codspeed.io/astral-sh/ruff/branches/micha%2Frelex-interpolated-state?utm_source=github&utm_medium=comment&utm_content=header)

### Merging #21898 will **improve performances by 11%**

<sub>Comparing <code>micha/relex-interpolated-state</code> (224d6df) with <code>micha/parser-trivia-relex</code> (7d47f71)</sub>



### Summary

`⚡ 1` improvement  
`✅ 51` untouched  



### Benchmarks breakdown

|     | Mode | Benchmark | `BASE` | `HEAD` | Change |
| --- | ---- | --------- | ------ | ------ | ------ |
| ⚡ | WallTime | [`` medium[colour-science] ``](https://codspeed.io/astral-sh/ruff/branches/micha%2Frelex-interpolated-state?uri=crates%2Fruff_benchmark%2Fbenches%2Fty_walltime.rs%3A%3Amedium%5Bcolour-science%5D&runnerMode=WallTime&utm_source=github&utm_medium=comment&utm_content=benchmark) | 108.9 s | 98.1 s | +11% |


---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:1493 on 2025-12-11 06:03_

I'm a bit worried about whether we need to restore the interpolated string state in this branch based on some condition. I'm not sure, I haven't looked deep into it.

---

_@dhruvmanila approved on 2025-12-11 06:03_

I only have one worry but otherwise this change makes sense.

---

_Closed by @MichaReiser on 2025-12-26 11:49_

---
