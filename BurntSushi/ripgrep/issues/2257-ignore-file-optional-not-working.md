```yaml
number: 2257
title: "--ignore-file optional not working"
type: issue
state: closed
author: jzhao2007
labels: []
assignees: []
created_at: 2022-07-14T13:18:41Z
updated_at: 2022-07-14T13:33:49Z
url: https://github.com/BurntSushi/ripgrep/issues/2257
synced_at: 2026-01-12T16:13:24Z
```

# --ignore-file optional not working

---

_@jzhao2007_

#### What version of ripgrep are you using?

ripgrep 13.0.0 (rev af6b6c543b)

#### How did you install ripgrep?
unzip

#### What operating system are you using ripgrep on?
Windows 10

#### Describe your bug.
--ignore-file optional not working

#### What are the steps to reproduce the behavior?
echo aaa >> test.txt
echo aaa >> test2.txt
rg --ignore-file test.txt aaa

#### What is the actual behavior?


```
test2.txt
1:aaa

test.txt
1:aaa

```

#### What is the expected behavior?
-- test.txt should be excluded from the result

test2.txt
1:aaa




---

_Locked by @ghost on 2022-07-14 13:33_

---
