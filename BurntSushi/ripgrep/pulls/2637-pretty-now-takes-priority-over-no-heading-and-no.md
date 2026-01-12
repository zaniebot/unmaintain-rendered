```yaml
number: 2637
title: "`--pretty` now takes priority over `--no-heading` and `--no-line-number`"
type: pull_request
state: closed
author: propbreakerfpv
labels:
  - rollup
assignees: []
base: master
head: master
created_at: 2023-10-22T04:37:44Z
updated_at: 2023-11-21T23:39:36Z
url: https://github.com/BurntSushi/ripgrep/pull/2637
synced_at: 2026-01-12T18:23:14Z
```

# `--pretty` now takes priority over `--no-heading` and `--no-line-number`

---

_@propbreakerfpv_

Related Issue: #2381
# Problem
if `--no-line-number` or `--no-heading` are in config file or passed as arguments and the `--pretty` argument is also given the `--no` arguments will take priority. 
# Fix
line-number and heading now look for pretty before `--no-line-number` or `--no-heading` causing `--pretty` to take priority.

---

_Renamed from "changed heading and line-number to check for pretty before there respective --no flags" to "`--pretty` takes priority over `--no-heading` and `--no-line-number`" by @propbreakerfpv on 2023-10-22 04:39_

---

_Renamed from "`--pretty` takes priority over `--no-heading` and `--no-line-number`" to "`--pretty` now takes priority over `--no-heading` and `--no-line-number`" by @propbreakerfpv on 2023-10-22 04:40_

---

_Label `rollup` added by @BurntSushi on 2023-11-21 23:04_

---

_Closed by @BurntSushi on 2023-11-21 23:39_

---
