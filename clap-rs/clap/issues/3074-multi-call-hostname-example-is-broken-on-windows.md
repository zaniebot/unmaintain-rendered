---
number: 3074
title: multi-call hostname example is broken on Windows
type: issue
state: closed
author: epage
labels:
  - C-bug
  - A-parsing
assignees: []
created_at: 2021-12-07T21:53:06Z
updated_at: 2021-12-13T15:22:01Z
url: https://github.com/clap-rs/clap/issues/3074
synced_at: 2026-01-10T01:27:31Z
---

# multi-call hostname example is broken on Windows

---

_Issue opened by @epage on 2021-12-07 21:53_

### 

- [x] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [x] I have searched the existing issues

### Description

#3015 is verifying the examples more thoroughly but broke on Windows, requiring me to disable the example test.

The original failure:
```
Testing examples\24a_multicall_busybox.md:8 ... failed
Expected 0, was 2

--- stdout	expected
+++ stdout	actual
@@ -0,0 +1,6 @@
+error: Found argument 'busybox[EXE]' which wasn't expected, or isn't valid in this context
+
+USAGE:
+     [OPTIONS] [SUBCOMMAND]
+
+For more information try --help

stderr:
```

### Version

_No response_

### Minimal reproducible code

See the example

### Actual Behaviour

_No response_

### Expected Behaviour

_No response_

### Debug Output

_No response_

---

_Referenced in [clap-rs/clap#2861](../../clap-rs/clap/issues/2861.md) on 2021-12-07 21:53_

---

_Referenced in [clap-rs/clap#3041](../../clap-rs/clap/pulls/3041.md) on 2021-12-08 00:40_

---

_Label `A-parsing` added by @epage on 2021-12-09 00:37_

---

_Label `C-bug` added by @epage on 2021-12-09 00:37_

---

_Closed by @epage on 2021-12-13 15:22_

---
