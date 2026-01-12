```yaml
number: 2913
title: "[modular] Decouple help generation from clap"
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-builder
  - E-medium
assignees: []
created_at: 2021-10-19T15:38:04Z
updated_at: 2021-12-13T22:06:44Z
url: https://github.com/clap-rs/clap/issues/2913
synced_at: 2026-01-12T16:14:14Z
```

# [modular] Decouple help generation from clap

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

master

### Describe your use case

We are looking to modular clap
- We can look for opportunities to shrink clap
- We can share logic with other argument parsers

One aspect of this will be to have a general reflection API. A good stepping stone test case is for help generation to not depend on `clap` but a `clap_reflect`.  This will allow us to test the reflection API out before making it public.

### Describe the solution you'd like

1. Create an internal reflection API in clap
2. Port the help generation to it
3. Work to move these out into a `clap_reflect` and `clap_help` crates

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `T: new feature` added by @epage on 2021-10-19 15:38_

---

_Assigned to @epage by @epage on 2021-10-19 15:38_

---

_Referenced in [clap-rs/clap#2914](../../clap-rs/clap/issues/2914.md) on 2021-10-19 15:40_

---

_Referenced in [clap-rs/clap#1013](../../clap-rs/clap/issues/1013.md) on 2021-10-27 18:05_

---

_Referenced in [clap-rs/clap#2963](../../clap-rs/clap/issues/2963.md) on 2021-10-29 19:58_

---

_Referenced in [epage/clapng#75](../../epage/clapng/issues/75.md) on 2021-12-06 16:34_

---

_Referenced in [epage/clapng#226](../../epage/clapng/issues/226.md) on 2021-12-06 22:21_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:08_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:08_

---

_Referenced in [clap-rs/clap#1334](../../clap-rs/clap/issues/1334.md) on 2021-12-10 16:22_

---

_Label `A-builder` added by @epage on 2021-12-13 22:06_

---

_Label `E-medium` added by @epage on 2021-12-13 22:06_

---

_Referenced in [clap-rs/clap#1407](../../clap-rs/clap/issues/1407.md) on 2022-01-11 17:31_

---

_Referenced in [clap-rs/clap#3380](../../clap-rs/clap/pulls/3380.md) on 2022-02-01 17:30_

---

_Referenced in [clap-rs/clap#3480](../../clap-rs/clap/issues/3480.md) on 2022-02-17 20:08_

---

_Referenced in [clap-rs/clap#4082](../../clap-rs/clap/pulls/4082.md) on 2022-08-16 14:16_

---

_Referenced in [clap-rs/clap#6140](../../clap-rs/clap/issues/6140.md) on 2025-09-25 16:15_

---
