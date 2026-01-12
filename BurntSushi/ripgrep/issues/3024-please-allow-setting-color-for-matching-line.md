```yaml
number: 3024
title: Please allow setting color for matching line
type: issue
state: closed
author: depesz
labels:
  - rollup
assignees: []
created_at: 2025-04-08T16:26:24Z
updated_at: 2025-09-20T01:08:45Z
url: https://github.com/BurntSushi/ripgrep/issues/3024
synced_at: 2026-01-12T16:13:25Z
```

# Please allow setting color for matching line

---

_@depesz_

First of all - thank you for amazing tool. It's beyond great.

#### Describe your feature request

Sometimes I'm searching for short/small matches, and need to see context. While the match can be highlighted, it's not always obvious where it is.

For example, using gnu grep, I did set:

`export GREP_COLORS="sl=48;5;240:mt=38;5;226;48;5;124"`

and this lets me see matches like this:

![Image](https://github.com/user-attachments/assets/e9aa4be6-8f10-4d31-949d-618e144ff855)

As far as I understand `rg --help` - I can't have this configured like this in ripgrep.

I think it would be worthy addition.

---

_Comment by @BurntSushi on 2025-04-08 16:39_

I am unsure if this is worth adding. And it probably isn't something I'll work on. But I could probably review a patch that added it. Including updating all of the docs about color configuration.

---

_Label `rollup` added by @BurntSushi on 2025-08-20 01:42_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
