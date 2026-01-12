```yaml
number: 2695
title: Help me catch mismatches in number of values and value names
type: issue
state: closed
author: epage
labels:
  - C-enhancement
  - S-waiting-on-decision
  - A-builder
assignees: []
created_at: 2021-08-14T01:46:43Z
updated_at: 2022-05-06T19:15:48Z
url: https://github.com/clap-rs/clap/issues/2695
synced_at: 2026-01-12T16:14:13Z
```

# Help me catch mismatches in number of values and value names

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.0-beta.2

### Describe your use case

The two main use cases I have for value names with multiple_values are
- 1 value name repeated repeated for each value
- Value name per value

### Describe the solution you'd like

To catch mismatches in these, I'd like to have a debug assert so I know when I update one without the other

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `T: new feature` added by @epage on 2021-08-14 01:46_

---

_Comment by @pksunkara on 2021-08-14 01:50_

> 1 value name repeated repeated for each value

That is a case of N:KN where number of values is a multiple of value names. (This is not supported in help message except when N = 1)

---

_Label `C: asserts` added by @pksunkara on 2021-08-14 01:50_

---

_Label `C: help message` added by @pksunkara on 2021-08-14 01:50_

---

_Comment by @epage on 2021-08-14 01:55_

Good point about that generalization of the 1:N case.  Not sure if it ever shows up but if we have usage support for it, I'm assuming someone requested it at some point.

---

_Referenced in [epage/clapng#199](../../epage/clapng/issues/199.md) on 2021-12-06 21:20_

---

_Label `A-help` removed by @epage on 2021-12-08 19:58_

---

_Label `C: asserts` removed by @epage on 2021-12-08 19:58_

---

_Label `A-builder` added by @epage on 2021-12-08 19:58_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:09_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:09_

---

_Comment by @epage on 2021-12-13 17:12_

At minimum, we should have a debug assert for too many value names.

I am unsure how prevalent it is to have intentionally specify fewer value names.

---

_Assigned to @epage by @epage on 2021-12-13 17:12_

---

_Label `S-waiting-on-decision` added by @epage on 2021-12-13 22:43_

---

_Added to milestone `4.0` by @epage on 2022-02-08 23:56_

---

_Referenced in [clap-rs/clap#3700](../../clap-rs/clap/pulls/3700.md) on 2022-05-06 18:56_

---

_Closed by @epage on 2022-05-06 19:15_

---
