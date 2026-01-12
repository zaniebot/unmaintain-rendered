```yaml
number: 1835
title: warning is output to stdout
type: issue
state: closed
author: Yggdroot
labels:
  - enhancement
  - question
  - wontfix
assignees: []
created_at: 2021-03-25T06:07:46Z
updated_at: 2023-11-24T20:21:03Z
url: https://github.com/BurntSushi/ripgrep/issues/1835
synced_at: 2026-01-12T16:13:24Z
```

# warning is output to stdout

---

_@Yggdroot_


#### What operating system are you using ripgrep on?

Linux

#### Describe your bug.

`WARNING: stopped searching binary file src/security-scanner-frontend/dist/fonts/ionicons.a558ac78.eot after match (found "\u{0}" byte around offset 3)` is output to stdout rather than stderr.

#### What are the steps to reproduce the behavior?

`rg "" 2> /dev/null  | grep WARNING`, the warning is displayed.

#### What is the actual behavior?


#### What is the expected behavior?
Warning is output to stderr.

What do you think ripgrep should have done?


---

_Label `enhancement` added by @BurntSushi on 2021-03-25 08:54_

---

_Label `question` added by @BurntSushi on 2021-03-25 08:54_

---

_Comment by @skuzzymiglet on 2021-04-19 16:59_

@Yggdroot Can you help us reproduce your bug by providing some files in your directory that caused the warning?

---

_Comment by @BurntSushi on 2021-04-19 17:05_

ripgrep's test suite reproduces this behavior as it is intended: https://github.com/BurntSushi/ripgrep/blob/92286ad4d2a9c8b054de8aaea05c23a7220d20b3/tests/binary.rs#L37-L46

I think the question here is whether we should change this. IIRC, GNU grep recently changed its behavior to emit these messages to stderr as well. (But I could be mis-remembering.) I can see arguments in favor of both ways personally. I currently tend to lean towards retaining existing behavior personally.

---

_Comment by @BurntSushi on 2023-11-24 20:20_

Going to stick with existing behavior for now.

---

_Closed by @BurntSushi on 2023-11-24 20:20_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 20:21_

---
