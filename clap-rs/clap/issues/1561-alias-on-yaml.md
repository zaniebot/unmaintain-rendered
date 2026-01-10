---
number: 1561
title: Alias on yaml
type: issue
state: closed
author: GrayJack
labels: []
assignees: []
created_at: 2019-10-03T04:21:12Z
updated_at: 2019-10-03T04:32:30Z
url: https://github.com/clap-rs/clap/issues/1561
synced_at: 2026-01-10T01:26:57Z
---

# Alias on yaml

---

_Issue opened by @GrayJack on 2019-10-03 04:21_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

* 1.38.0

### Affected Version of clap

* 2.33.0

### Expected Behavior Summary
I expected that if I use alias on yaml, it would get the alias correctly


### Actual Behavior Summary
It panics with 'Unknown Arg setting 'alias' in YAML file for arg 'N'


### Steps to Reproduce the issue
Put a 
```
alias: a
```
on any args of the yaml config

### Sample Code or Link to Sample Code


### Debug output

Cannot be generated due to panic

---

_Comment by @GrayJack on 2019-10-03 04:32_

Oh, looks like it expects `aliases`

But also, there is a way to create visible aliases from yaml?

What about alias for short args?

---

_Closed by @GrayJack on 2019-10-03 04:32_

---
