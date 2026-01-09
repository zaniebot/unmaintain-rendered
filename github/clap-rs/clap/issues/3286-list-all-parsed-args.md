---
number: 3286
title: List all parsed args
type: issue
state: closed
author: antonshkurenko
labels:
  - C-enhancement
  - A-parsing
  - S-duplicate
assignees: []
created_at: 2022-01-12T09:08:25Z
updated_at: 2022-01-12T15:34:16Z
url: https://github.com/clap-rs/clap/issues/3286
synced_at: 2026-01-07T13:12:19-06:00
---

# List all parsed args

---

_Issue opened by @antonshkurenko on 2022-01-12 09:08_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

3.0.6

### Describe your use case

In clap v2 we used `ArgMatches::args` field for inner purpose, but now, in v3 there is no visible possibility to iterate through all parsed arguments, we'd like to update. Please, can you add that functionality?

### Describe the solution you'd like

As I understand, making `args` field public is not an option, but maybe there is a possibility to provide copy of iterator?

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `C-enhancement` added by @antonshkurenko on 2022-01-12 09:08_

---

_Label `A-parsing` added by @epage on 2022-01-12 15:34_

---

_Label `S-duplicate` added by @epage on 2022-01-12 15:34_

---

_Comment by @epage on 2022-01-12 15:34_

Our plan is to morph `ArgMatches` to be more container like.  We have https://github.com/clap-rs/clap/issues/1206 for providing an iterator.

Can you comment on that issue on your use case so we can keep it in mind when designing it and to understand priority (e.g. if it blocks you, exploring alternatives).

---

_Closed by @epage on 2022-01-12 15:34_

---

_Referenced in [clap-rs/clap#1206](../../clap-rs/clap/issues/1206.md) on 2022-01-18 11:59_

---
