```yaml
number: 2017
title: Print header line
type: issue
state: closed
author: ljufa
labels:
  - wontfix
assignees: []
created_at: 2021-10-13T14:59:59Z
updated_at: 2023-11-24T20:14:01Z
url: https://github.com/BurntSushi/ripgrep/issues/2017
synced_at: 2026-01-12T16:13:24Z
```

# Print header line

---

_@ljufa_

Add option to print header line of tabular inputs 
i.e. for the input:

| Command | Description |
| --- | --- |
| git status | List all new or modified files |
| git diff | Show file differences that haven't been staged |

`ripgrep --header 1 staged`


Should print:

| Command | Description |
| --- | --- |
| git diff | Show file differences that haven't been staged |

Optionally, the header line number could be provided by the user (default to 1).
Let me know if this is an interesting feature. I will be happy to contribute.




---

_Comment by @BurntSushi on 2023-11-24 20:13_

I'm going to pass on this one. I think it's out of scope for ripgrep.

---

_Closed by @BurntSushi on 2023-11-24 20:13_

---

_Label `wontfix` added by @BurntSushi on 2023-11-24 20:14_

---
