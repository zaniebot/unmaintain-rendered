---
number: 528
title: YAML default_value doesn’t work
type: issue
state: closed
author: calebmer
labels:
  - C-bug
assignees: []
created_at: 2016-06-11T12:48:23Z
updated_at: 2018-08-02T03:29:50Z
url: https://github.com/clap-rs/clap/issues/528
synced_at: 2026-01-10T01:26:30Z
---

# YAML default_value doesn’t work

---

_Issue opened by @calebmer on 2016-06-11 12:48_

First off, thanks for the amazing library! By far the best CLI helper tool I’ve seen across all the ecosystems I’ve worked in.

Currently I’m using a YAML configuration file to define my arguments/subcommands. I tried to set the `default_value` using this method and it panicked with the following error:

```
thread '<main>' panicked at 'Unknown Arg setting 'default_value' in YAML file for arg 'n'', /Users/calebmer/.multirust/toolchains/stable/cargo/registry/src/github.com-88ac128001ac3a9a/clap-2.5.2/src/args/arg.rs:226
Process didn't exit successfully: `target/debug/accelerate` (exit code: 101)
```


---

_Comment by @kbknapp on 2016-06-12 19:30_

Whoops! Forgot to include that. It'll be fixed on the next release! Thanks for pointing this out :+1: 


---

_Label `T: bug` added by @kbknapp on 2016-06-12 19:31_

---

_Label `P1: urgent` added by @kbknapp on 2016-06-12 19:31_

---

_Label `C: args` added by @kbknapp on 2016-06-12 19:31_

---

_Label `D: easy` added by @kbknapp on 2016-06-12 19:31_

---

_Label `W: 2.x` added by @kbknapp on 2016-06-12 19:31_

---

_Added to milestone `2.6.0` by @kbknapp on 2016-06-12 19:37_

---

_Comment by @kbknapp on 2016-06-13 02:12_

This is fixed with #530 


---

_Closed by @homu on 2016-06-13 04:39_

---
