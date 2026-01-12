```yaml
number: 1925
title: How can I execute it with other commands at the same time?
type: issue
state: closed
author: ghost
labels: []
assignees: []
created_at: 2021-07-04T11:11:48Z
updated_at: 2023-11-23T04:33:33Z
url: https://github.com/BurntSushi/ripgrep/issues/1925
synced_at: 2026-01-12T16:13:24Z
```

# How can I execute it with other commands at the same time?

---

_@ghost_

When i execute
**apt list | rg 'apk'**
Its execution result is the direct output result of rg
How should I use *|rg ？
thank you ！

---

_Comment by @BurntSushi on 2021-07-04 11:39_

I don't understand the question. Why not include the actual output and the expected output? Please also consider using standard commands. I don't have `apt` on my system.

---

_Comment by @ghost on 2021-07-05 01:01_

> 我不明白这个问题。为什么不包括实际输出和预期输出？还请考虑使用标准命令。`apt`我的系统上没有。

Can **rg** not be used together with  **|**  ?

https://user-images.githubusercontent.com/83487691/124404922-8c4e9280-dd6f-11eb-8057-d4f70bcfbd40.mp4





---

_Comment by @BurntSushi on 2021-07-05 01:06_

Please provide a reproduction. I don't want a video. I want commands I can run. Of course ripgrep works with `|`.

What version of ripgrep are you using?

Why did you delete the issue template that requested this basic information? I didn't put it there for fun.

---

_Comment by @jplatte on 2023-10-17 07:26_

The account of the user who asked this question has since been deleted, time to close this? 

---

_Closed by @BurntSushi on 2023-11-23 04:33_

---
