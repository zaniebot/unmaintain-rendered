```yaml
number: 1545
title: arg.take_value conflicts with arg.default_value
type: issue
state: closed
author: maicallist
labels:
  - C-enhancement
  - E-easy
assignees: []
created_at: 2019-09-10T08:54:15Z
updated_at: 2020-10-14T21:20:36Z
url: https://github.com/clap-rs/clap/issues/1545
synced_at: 2026-01-12T16:14:11Z
```

# arg.take_value conflicts with arg.default_value

---

_@maicallist_

<!--
Please use the following template to assist with creating an issue and to ensure a speedy resolution. If an area is not applicable, feel free to delete the area or mark with `N/A`
-->

### Rust Version

1.37.0

### Affected Version of clap

2.33.0

### Bug or Feature Request Summary

`arg` default_value conflicts with takes_value(true)

### Expected Behavior Summary
when options are specified but no values are given, clap should print an error message.

### Actual Behavior Summary
when options are specified but no values are given, clap prints nothing and use the default value as input.


---

_Comment by @CreepySkeleton on 2020-02-01 12:23_

> when options are specified but no values are given, clap prints nothing and use the default value as input.

I believe this is how it should work ¯\_(ツ)_/¯ I'm not sure I get it right so feel free to reopen.

---

_Closed by @CreepySkeleton on 2020-02-01 12:23_

---

_Comment by @pksunkara on 2020-02-01 20:00_

Yup, if you want to print an error, then you shouldn't give a default value.

---

_Comment by @Dylan-DPC-zz on 2020-02-02 01:08_

I think we need to set up some precedence here for conflicting options. Re-opening 

---

_Reopened by @Dylan-DPC-zz on 2020-02-02 01:08_

---

_Label `C: docs` added by @CreepySkeleton on 2020-02-02 01:50_

---

_Comment by @pksunkara on 2020-02-02 05:58_

Do you mean describe the precedence in docs? Or in error messages when building?

---

_Comment by @Dylan-DPC-zz on 2020-02-02 19:41_

No I mean the way clap handles it 

---

_Added to milestone `3.0` by @pksunkara on 2020-02-06 19:24_

---

_Label `C: asserts` added by @pksunkara on 2020-04-12 21:49_

---

_Label `T: enhancement` added by @pksunkara on 2020-04-12 21:49_

---

_Label `Z: good first issue` added by @pksunkara on 2020-04-12 21:49_

---

_Referenced in [clap-rs/clap#984](../../clap-rs/clap/issues/984.md) on 2020-04-12 21:56_

---

_Label `C: asserts` removed by @pksunkara on 2020-04-12 21:56_

---

_Comment by @pksunkara on 2020-10-14 05:49_

With the `default_missing_value` support we added recently, I agree with the proposed behaviour. 

---

_Label `C: docs` removed by @pksunkara on 2020-10-14 05:50_

---

_Label `C: args` added by @pksunkara on 2020-10-14 05:50_

---

_Closed by @bors[bot] on 2020-10-14 21:20_

---
