```yaml
number: 3026
title: Support alternative name/value separator
type: issue
state: open
author: epage
labels:
  - C-enhancement
  - A-parsing
  - S-waiting-on-mentor
assignees: []
created_at: 2021-11-15T14:52:51Z
updated_at: 2021-12-13T16:39:01Z
url: https://github.com/clap-rs/clap/issues/3026
synced_at: 2026-01-12T16:14:14Z
```

# Support alternative name/value separator

---

_@epage_

### Please complete the following tasks

- [X] I have searched the [discussions](https://github.com/clap-rs/clap/discussions)
- [X] I have searched the existing issues

### Clap Version

master

### Describe your use case

A part of supporting non-unix syntax (#2468) is supporting a custom name/value separator.

Example:
```
icacls <filename> [/grant[:r] <sid>:<perm>[...]] [/deny <sid>:<perm>[...]] [/remove[:g|:d]] <sid>[...]] [/t] [/c] [/l] [/q] [/setintegritylevel <Level>:<policy>[...]]
```

### Describe the solution you'd like

1. Come up with a name for this separator
2. Add a `use_<name>`, `requires_<name>`, and `<name>_delimiter` like `use_delimiter`, `requires_delimiter`, and  `value_delimiter`
3. Deprecated `requires_equals` in favor of `requires_<name>` (especially since it references a specific syntax)
4. Rename `use_delimiter` to `use_value_delimiter` and `requires_delimiter` to `requires_value_delimiter` to disambiguate

### Alternatives, if applicable

_No response_

### Additional Context

_No response_

---

_Label `T: new feature` added by @epage on 2021-11-15 14:52_

---

_Label `W: maybe` added by @epage on 2021-11-15 14:52_

---

_Label `C: parsing` added by @epage on 2021-11-15 14:52_

---

_Referenced in [clap-rs/clap#2468](../../clap-rs/clap/issues/2468.md) on 2021-11-15 14:53_

---

_Referenced in [clap-rs/clap#3030](../../clap-rs/clap/issues/3030.md) on 2021-11-15 20:50_

---

_Comment by @mkayaalp on 2021-11-19 06:20_

> 1. Come up with a name for this separator

I don't think we'll be able to come up with anything that does not include the word "separator" in it and calling it simply the `separator` would be no better than calling the other one simply the `delimiter`.  So, I think you got in the title. I vote `name_value_separator`. It does separate the `name` and the `value` from each other after all.

> 4. Rename `use_delimiter` to `use_value_delimiter` and `requires_delimiter` to `requires_value_delimiter` to disambiguate

Agreed. It parallels `value_terminator`.

---

_Referenced in [epage/clapng#239](../../epage/clapng/issues/239.md) on 2021-12-06 22:24_

---

_Label `C-enhancement` added by @epage on 2021-12-08 21:07_

---

_Label `T: new feature` removed by @epage on 2021-12-08 21:07_

---

_Label `S-waiting-on-decision` removed by @epage on 2021-12-13 16:39_

---

_Label `S-waiting-on-mentor` added by @epage on 2021-12-13 16:39_

---
