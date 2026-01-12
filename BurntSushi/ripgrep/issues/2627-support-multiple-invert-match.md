```yaml
number: 2627
title: support Multiple invert-match?
type: issue
state: closed
author: zz5678
labels: []
assignees: []
created_at: 2023-10-13T06:23:20Z
updated_at: 2023-10-13T13:29:54Z
url: https://github.com/BurntSushi/ripgrep/issues/2627
synced_at: 2026-01-12T16:13:24Z
```

# support Multiple invert-match?

---

_@zz5678_

For the following to be processed:
```
abc
efg
123
```
my command is:
```
rg -v ab -v 12
```

This command doesn't actually work, but the ChatGPT answer works like this.

Is there a feature like this? But I'm using it in wrong way?

![Screenshot_20231013142128](https://github.com/BurntSushi/ripgrep/assets/67672527/778597c0-6971-41b7-8927-90fa12123808)


---

_Comment by @s-p-turner on 2023-10-13 09:27_

Fwiw, two alternative ways to achieve what you want would be either:

rg -v ab "input-file" | rg -v 12

or (more efficient):

rg -v 'ab|12' "input-file"


---

_Comment by @zz5678 on 2023-10-13 11:15_

> Fwiw, two alternative ways to achieve what you want would be either:
> 
> rg -v ab "input-file" | rg -v 12
> 
> or (more efficient):
> 
> rg -v 'ab|12' "input-file"

Yes, the second method is more efficient, but it doesn't seem to appear in the man page of ripgrep

One question is, who told ChatGPT about the Way that doesn't exist????

---

_Locked by @ghost on 2023-10-13 13:29_

---
