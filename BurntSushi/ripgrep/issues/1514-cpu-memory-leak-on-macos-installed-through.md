```yaml
number: 1514
title: CPU/Memory leak on MacOS installed through homebrew
type: issue
state: closed
author: naefl
labels:
  - invalid
assignees: []
created_at: 2020-03-14T03:41:31Z
updated_at: 2020-03-14T15:57:11Z
url: https://github.com/BurntSushi/ripgrep/issues/1514
synced_at: 2026-01-12T16:13:23Z
```

# CPU/Memory leak on MacOS installed through homebrew

---

_@naefl_

#### What version of ripgrep are you using?

ripgrep 11.0.2
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

homebrew

#### What operating system are you using ripgrep on?

macos
#### Describe your question, feature request, or bug.

rg CPU/memory leak once started. E.g. start ripgrep from within vim, find things. stop ripgrep. After stopping CPU usage keeps rising (100% load on 8-core)

![image](https://user-images.githubusercontent.com/26996526/76674274-eaa4a000-6583-11ea-9647-0575b449165e.png)
![image](https://user-images.githubusercontent.com/26996526/76674280-ef695400-6583-11ea-88e0-e4faae85498a.png)


If a bug, please see below.

#### If this is a bug, what are the steps to reproduce the behavior?

Start ripgrep from vim or command line. Close ripgrep. Watch CPU keep rising.


---

_Comment by @BurntSushi on 2020-03-14 11:05_

This ticket is not actionable. Please provide a complete reproducible example. Please include all inputs (including corpora), expected output, actual output and the exact command used.

Your ticket says that you "stopped" ripgrep but that it is still consuming CPU. That doesn't make any sense. Sounds like you have a bad editor plugin.

---

_Comment by @naefl on 2020-03-14 15:48_

@BurntSushi  thanks - Indeed seems to be caused by `fzf.vim`. Will open issue there.

---

_Closed by @naefl on 2020-03-14 15:48_

---

_Label `invalid` added by @BurntSushi on 2020-03-14 15:57_

---
