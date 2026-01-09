---
number: 1458
title: Bad help for arg with multiple occurrences but only 1 value per occurrence
type: issue
state: closed
author: rvolgers
labels: []
assignees: []
created_at: 2019-04-26T11:38:08Z
updated_at: 2020-02-01T14:24:47Z
url: https://github.com/clap-rs/clap/issues/1458
synced_at: 2026-01-07T13:12:19-06:00
---

# Bad help for arg with multiple occurrences but only 1 value per occurrence

---

_Issue opened by @rvolgers on 2019-04-26 11:38_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

N/A

### Affected Version of clap

2.33.0 (with structopt 0.2.15)

### Bug or Feature Request Summary

I have an argument with `multiple(true)` and `number_of_values(1)`.

The idea is that it should accept `mytool -c foo -c bar -c baz`.

### Expected Behavior Summary

I want help for such an argument  to show up as:

```
OPTIONS:
    -c, --config <config>    Specify a config. This option can be specified multiple times.
```

### Actual Behavior Summary

This is what clap produces:

```
OPTIONS:
    -c, --config <configs>...    Specify a config
```

This is both confusing and ugly. The help message strongly implies that the user should use the (incorrect) syntax `mytool -c foo bar baz` instead of  `mytool -c foo -c bar -c baz`.

### Steps to Reproduce the issue

I can make a reproducer if needed, but I might just be missing something so I wanted to ask first.

### Sample Code or Link to Sample Code


### Debug output



---

_Comment by @rvolgers on 2019-04-26 12:02_

Basically I think these three lines are wrong and should be removed:
https://github.com/clap-rs/clap/blob/6acc8b6a621a765cbf513450188000d943676a30/src/app/help.rs#L354

Also the `arg.num_vals() == None` case does not appear to be handled, and it seems to me that in *that* case in fact you would want "..." to be printed.

I can probably submit a patch if all of this sounds reasonable?

---

_Comment by @CreepySkeleton on 2020-02-01 14:24_

Duplicate of #1571

---

_Closed by @CreepySkeleton on 2020-02-01 14:24_

---

_Label `T: duplicate` added by @CreepySkeleton on 2020-02-01 14:24_

---
