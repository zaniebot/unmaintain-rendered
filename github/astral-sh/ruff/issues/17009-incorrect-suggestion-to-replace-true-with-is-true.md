---
number: 17009
title: Incorrect suggestion to replace == True with is True for pandas dataframe
type: issue
state: closed
author: unknown-eps
labels: []
assignees: []
created_at: 2025-03-27T08:31:18Z
updated_at: 2025-03-27T10:56:41Z
url: https://github.com/astral-sh/ruff/issues/17009
synced_at: 2026-01-07T13:12:16-06:00
---

# Incorrect suggestion to replace == True with is True for pandas dataframe

---

_Issue opened by @unknown-eps on 2025-03-27 08:31_

### Summary

https://play.ruff.rs/c4303e07-49f1-456d-b48c-6a6a9b9bfcd5

import pandas as pd

temp =pd.DataFrame({'a':[1,2,3],'b':['True','True','False']})

filter_temp = temp[temp['b'] == True] #Correct

ruff_suggested = temp[temp['b'] is True] # Will give error on running

Ruff suggests the second one

### Version

_No response_

---

_Comment by @Daverball on 2025-03-27 09:14_

This requires type inference and multi-file analysis, both of which are a little ways off still.

This boils down to, that the rule should not trigger when `x == True` returns something other than `bool`.

I tend to use `.eq(True)` instead of `== True` in pandas, it reads better and won't trigger this rule. Usually you don't even really need that though and can just directly use `temp['b']` as the filter, since `eq(True)` just gives you back the same series, if all the values are already boolean, so you only need it when you have mixed values.

---

_Comment by @dylwil3 on 2025-03-27 10:56_

Sorry you ran into this! This is a known issue, and it's why the rule auto-fix is marked as unsafe (see the [documentation for the rule](https://docs.astral.sh/ruff/rules/true-false-comparison/#fix-safety))

---

_Closed by @dylwil3 on 2025-03-27 10:56_

---
