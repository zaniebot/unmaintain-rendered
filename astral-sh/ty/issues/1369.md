```yaml
number: 1369
title: "Truncate `Literal` type display in some situations"
type: issue
state: closed
author: AlexWaygood
labels:
  - help wanted
  - diagnostics
assignees: []
created_at: 2025-10-16T14:09:17Z
updated_at: 2025-10-17T11:50:59Z
url: https://github.com/astral-sh/ty/issues/1369
synced_at: 2026-01-10T02:06:25Z
```

# Truncate `Literal` type display in some situations

---

_Issue opened by @AlexWaygood on 2025-10-16 14:09_

### Summary

In https://github.com/astral-sh/ruff/pull/20730, we started truncating union type displays in some situations. I think we need to consider doing similarly for `Literal` types. Here's an example diagnostic on https://github.com/astral-sh/ruff/pull/20107:

```
[error] invalid-argument-type - Argument to bound method `timeUnit` is incorrect: Expected `TimeUnitParams | Literal["yearquarter", "yearquartermonth", "yearmonth", "yearmonthdate", "yearmonthdatehours", "yearmonthdatehoursminutes", "yearmonthdatehoursminutesseconds", "yearweek", "yearweekday", "yearweekdayhours", "yearweekdayhoursminutes", "yearweekdayhoursminutesseconds", "yeardayofyear", "quartermonth", "monthdate", "monthdatehours", "monthdatehoursminutes", "monthdatehoursminutesseconds", "weekday", "weekdayhours", "weekdayhoursminutes", "weekdayhoursminutesseconds", "dayhours", "dayhoursminutes", "dayhoursminutesseconds", "hoursminutes", "hoursminutesseconds", "minutesseconds", "secondsmilliseconds", "utcyearquarter", "utcyearquartermonth", "utcyearmonth", "utcyearmonthdate", "utcyearmonthdatehours", "utcyearmonthdatehoursminutes", "utcyearmonthdatehoursminutesseconds", "utcyearweek", "utcyearweekday", "utcyearweekdayhours", "utcyearweekdayhoursminutes", "utcyearweekdayhoursminutesseconds", "utcyeardayofyear", "utcquartermonth", "utcmonthdate", "utcmonthdatehours", "utcmonthdatehoursminutes", "utcmonthdatehoursminutesseconds", "utcweekday", "utcweekdayhours", "utcweekdayhoursminutes", "utcweekdayhoursminutesseconds", "utcdayhours", "utcdayhoursminutes", "utcdayhoursminutesseconds", "utchoursminutes", "utchoursminutesseconds", "utcminutesseconds", "utcsecondsmilliseconds", "binnedyear", "binnedyearquarter", "binnedyearquartermonth", "binnedyearmonth", "binnedyearmonthdate", "binnedyearmonthdatehours", "binnedyearmonthdatehoursminutes", "binnedyearmonthdatehoursminutesseconds", "binnedyearweek", "binnedyearweekday", "binnedyearweekdayhours", "binnedyearweekdayhoursminutes", "binnedyearweekdayhoursminutesseconds", "binnedyeardayofyear", "binnedutcyear", "binnedutcyearquarter", "binnedutcyearquartermonth", "binnedutcyearmonth", "binnedutcyearmonthdate", "binnedutcyearmonthdatehours", "binnedutcyearmonthdatehoursminutes", "binnedutcyearmonthdatehoursminutesseconds", "binnedutcyearweek", "binnedutcyearweekday", "binnedutcyearweekdayhours", "binnedutcyearweekdayhoursminutes", "binnedutcyearweekdayhoursminutesseconds", "binnedutcyeardayofyear", "year", "quarter", "month", "week", "day", "dayofyear", "date", "hours", "minutes", "seconds", "milliseconds", "utcyear", "utcquarter", "utcmonth", "utcweek", "utcday", "utcdayofyear", "utcdate", "utchours", "utcminutes", "utcseconds", "utcmilliseconds"]`, found `Literal["invalid_value"]`
```

A better display for that long `Literal` type would probably be something like

```
Literal["yearquarter", "yearquartermonth", "yearmonth", "yearmonthdate", "yearmonthdatehours", ...omitted 103 elements]
```

@InvalidPathException, would you be interested in working on this, by any chance? Your last PR in this area was great :-)

### Version

_No response_

---

_Label `help wanted` added by @AlexWaygood on 2025-10-16 14:09_

---

_Label `diagnostics` added by @AlexWaygood on 2025-10-16 14:09_

---

_Comment by @InvalidPathException on 2025-10-16 14:12_

I am interested and will have a look tomorrow, thanks for letting me know!

---

_Comment by @AlexWaygood on 2025-10-16 14:12_

Awesome, thank you!!

---

_Assigned to @InvalidPathException by @AlexWaygood on 2025-10-16 14:13_

---

_Closed by @AlexWaygood on 2025-10-17 11:50_

---
