```yaml
number: 6074
title: "fix: clap_man doesn't render optional values with [=<VALUE>]"
type: pull_request
state: merged
author: ericgumba
labels: []
assignees: []
merged: true
base: master
head: issue_3358
created_at: 2025-07-22T11:11:40Z
updated_at: 2025-07-29T00:36:00Z
url: https://github.com/clap-rs/clap/pull/6074
synced_at: 2026-01-12T16:14:17Z
```

# fix: clap_man doesn't render optional values with [=<VALUE>]

---

_@ericgumba_

In #3358 we discovered clapmangen wouldn’t render optional `required_equals` values with `[=<VALUE>]`. After further experimentation it was also found out that it didn’t appropriately render optional values `[VALUE]` and variadic values `<Val>…`  as well. This commit adds a check to the options render section where it will add the correct formatting. e.g instead of

`--option=VALUE` it will render `—option \<VALUE\>`, `—option [VALUE]`
`--option[=VALUE]` or option `<VALUE>…` and so on

Bringing it more in line with what clap shows when running --help. 

Fixes #3358

---
