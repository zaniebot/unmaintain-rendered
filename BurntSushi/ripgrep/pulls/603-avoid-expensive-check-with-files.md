```yaml
number: 603
title: Avoid expensive check with --files
type: pull_request
state: merged
author: chrmarti
labels: []
assignees: []
merged: true
base: master
head: chrmarti/600
created_at: 2017-09-08T23:05:10Z
updated_at: 2017-09-18T15:54:50Z
url: https://github.com/BurntSushi/ripgrep/pull/603
synced_at: 2026-01-12T18:23:13Z
```

# Avoid expensive check with --files

---

_@chrmarti_

Fixes #600

Stacktraces show that this check is the culprit. Listing all files in my test folder with 235k files on Windows now takes about 4.2 seconds. It didn't finish before I canceled before that when MsMpEng.exe kicked in.

The stacktrace:
![image](https://user-images.githubusercontent.com/9205389/30234157-59e06de4-94af-11e7-8118-2acd48e53999.png)

/cc @BurntSushi @roblourens

---

_Comment by @roblourens on 2017-09-08 23:20_

Excellent detective work @chrmarti! A little background @BurntSushi, with this change we will be able to use ripgrep to power quick open in VS Code as well.

---

_Comment by @chrmarti on 2017-09-11 15:33_

The Mac CI build failed with an empty log.

---

_Comment by @BurntSushi on 2017-09-18 12:56_

@chrmarti Nice detective work! I think my key issue here is that this only fixes the problem when using the `--files` flag, but based on your observations, it sounds like searching the files will result in a tremendous bottleneck.

So I'm not quite sure what to do. It seems like we should try to fix the root issue, but maybe it would be a good idea to just take this in for now?

---

_Comment by @roblourens on 2017-09-18 15:48_

I think Defender is doing the "right thing" here, and in text search, by scanning files when they are opened. It does cache results when possible, so this won't happen every time, but it will happen often. That was basically the result of our discussion with the Defender team. So I think the best thing we can do for now is to remove an unnecessary file open when using `--files`.

---

_Comment by @BurntSushi on 2017-09-18 15:54_

@roblourens Hmm all right, that's good enough for me!

---

_Merged by @BurntSushi on 2017-09-18 15:54_

---

_Closed by @BurntSushi on 2017-09-18 15:54_

---
