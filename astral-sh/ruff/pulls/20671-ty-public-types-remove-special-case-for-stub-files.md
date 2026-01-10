```yaml
number: 20671
title: "[ty] Public types: remove special-case for stub files"
type: pull_request
state: closed
author: sharkdp
labels:
  - ty
assignees: []
draft: true
base: david/no-unknown-union-no-promotion
head: david/remove-stub-special-case
created_at: 2025-10-01T14:29:11Z
updated_at: 2025-10-01T14:37:14Z
url: https://github.com/astral-sh/ruff/pull/20671
synced_at: 2026-01-10T17:40:28Z
```

# [ty] Public types: remove special-case for stub files

---

_Pull request opened by @sharkdp on 2025-10-01 14:29_

<!--
Thank you for contributing to Ruff/ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title? (Please prefix with `[ty]` for ty pull
  requests.)
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan

<!-- How was it tested? -->


---

_Label `ty` added by @sharkdp on 2025-10-01 14:29_

---

_Comment by @github-actions[bot] on 2025-10-01 14:31_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_Comment by @github-actions[bot] on 2025-10-01 14:32_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
<details>
<summary>Changes were detected when running on open source projects</summary>

```diff
discord.py (https://github.com/Rapptz/discord.py)
- discord/ext/commands/parameters.py:98:9: error[invalid-parameter-default] Default value of type `<class '_empty'>` is not assignable to annotated parameter type `str`
+ discord/ext/commands/parameters.py:98:9: error[invalid-parameter-default] Default value of type `Unknown | <class '_empty'>` is not assignable to annotated parameter type `str`
- discord/ext/commands/parameters.py:99:9: error[invalid-parameter-default] Default value of type `<class '_empty'>` is not assignable to annotated parameter type `str`
+ discord/ext/commands/parameters.py:99:9: error[invalid-parameter-default] Default value of type `Unknown | <class '_empty'>` is not assignable to annotated parameter type `str`
- discord/ext/commands/parameters.py:100:9: error[invalid-parameter-default] Default value of type `<class '_empty'>` is not assignable to annotated parameter type `str`
+ discord/ext/commands/parameters.py:100:9: error[invalid-parameter-default] Default value of type `Unknown | <class '_empty'>` is not assignable to annotated parameter type `str`
- discord/ext/commands/parameters.py:219:5: error[invalid-parameter-default] Default value of type `<class '_empty'>` is not assignable to annotated parameter type `str`
+ discord/ext/commands/parameters.py:219:5: error[invalid-parameter-default] Default value of type `Unknown | <class '_empty'>` is not assignable to annotated parameter type `str`
- discord/ext/commands/parameters.py:220:5: error[invalid-parameter-default] Default value of type `<class '_empty'>` is not assignable to annotated parameter type `str`
+ discord/ext/commands/parameters.py:220:5: error[invalid-parameter-default] Default value of type `Unknown | <class '_empty'>` is not assignable to annotated parameter type `str`
- discord/ext/commands/parameters.py:221:5: error[invalid-parameter-default] Default value of type `<class '_empty'>` is not assignable to annotated parameter type `str`
+ discord/ext/commands/parameters.py:221:5: error[invalid-parameter-default] Default value of type `Unknown | <class '_empty'>` is not assignable to annotated parameter type `str`
- discord/ext/commands/parameters.py:278:9: error[invalid-parameter-default] Default value of type `<class '_empty'>` is not assignable to annotated parameter type `str`
+ discord/ext/commands/parameters.py:278:9: error[invalid-parameter-default] Default value of type `Unknown | <class '_empty'>` is not assignable to annotated parameter type `str`
- discord/ext/commands/parameters.py:279:9: error[invalid-parameter-default] Default value of type `<class '_empty'>` is not assignable to annotated parameter type `str`
+ discord/ext/commands/parameters.py:279:9: error[invalid-parameter-default] Default value of type `Unknown | <class '_empty'>` is not assignable to annotated parameter type `str`
- discord/ext/commands/parameters.py:280:9: error[invalid-parameter-default] Default value of type `<class '_empty'>` is not assignable to annotated parameter type `str`
+ discord/ext/commands/parameters.py:280:9: error[invalid-parameter-default] Default value of type `Unknown | <class '_empty'>` is not assignable to annotated parameter type `str`

pandas-stubs (https://github.com/pandas-dev/pandas-stubs)
+ tests/indexes/test_indexes.py:1536:9: error[type-assertion-failure] Argument does not have asserted type `Categorical`
+ tests/indexes/test_indexes.py:1544:9: error[type-assertion-failure] Argument does not have asserted type `IntervalArray`
+ tests/indexes/test_indexes.py:1555:11: error[type-assertion-failure] Argument does not have asserted type `DatetimeArray`
+ tests/indexes/test_indexes.py:1558:9: error[type-assertion-failure] Argument does not have asserted type `TimedeltaArray`
+ tests/series/arithmetic/test_truediv.py:125:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:126:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:127:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:128:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:135:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:136:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:137:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:138:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:167:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:168:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:169:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:170:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:171:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:179:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:180:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:181:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:182:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:183:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:212:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:213:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:214:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:215:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:216:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:224:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:225:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:226:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:227:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/arithmetic/test_truediv.py:228:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/test_properties.py:39:9: error[type-assertion-failure] Argument does not have asserted type `PeriodProperties`
+ tests/series/test_series.py:1056:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/test_series.py:1058:9: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/series/test_series.py:1076:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/series/test_series.py:1209:9: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/test_series.py:1222:13: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/series/test_series.py:1295:13: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_frame.py:1631:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1707:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1709:9: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1712:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1722:15: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_frame.py:1724:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1727:15: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1729:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1733:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1743:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1746:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1748:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1751:9: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1765:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_frame.py:1901:9: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1910:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1914:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1918:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1922:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1927:9: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1931:9: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:1935:9: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:3987:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_frame.py:4078:15: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_frame.py:4080:13: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_groupby.py:528:15: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_groupby.py:651:15: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_groupby.py:723:15: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_groupby.py:850:15: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_groupby.py:918:19: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_groupby.py:1024:19: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_groupby.py:1140:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_groupby.py:1141:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_plotting.py:385:11: error[type-assertion-failure] Argument does not have asserted type `Axes`
+ tests/test_resampler.py:101:15: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_resampler.py:102:15: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_windowing.py:119:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_windowing.py:153:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_windowing.py:219:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_windowing.py:278:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_windowing.py:332:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
+ tests/test_windowing.py:363:11: error[type-assertion-failure] Argument does not have asserted type `DataFrame`
+ tests/test_windowing.py:394:11: error[type-assertion-failure] Argument does not have asserted type `Series[Any]`
- Found 4899 diagnostics
+ Found 4981 diagnostics

hydpy (https://github.com/hydpy-dev/hydpy)
- hydpy/core/seriestools.py:339:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Series[Any]`
+ hydpy/core/seriestools.py:339:12: error[invalid-return-type] Return type does not match returned value: expected `DataFrame`, found `Unknown | Series[Any]`

```
</details>
No memory usage changes detected ✅


---

_Comment by @sharkdp on 2025-10-01 14:36_

Ok, looks like we shouldn't do this, due to things like:

https://github.com/python/typeshed/blob/6547ec10b881acac28ed000a7c2108f8de1c6a1e/stdlib/inspect.pyi#L343

or this

https://github.com/pandas-dev/pandas-stubs/blob/a9f0bd321ef67cecd4ce0d5aeecd1a87b90ea1a2/pandas-stubs/core/series.pyi#L4007

---

_Closed by @sharkdp on 2025-10-01 14:37_

---
