```yaml
number: 1640
title: High memory consumption on MacOS from Visual Studio
type: issue
state: closed
author: madhavajay
labels:
  - invalid
assignees: []
created_at: 2020-07-08T23:25:33Z
updated_at: 2020-07-09T01:49:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1640
synced_at: 2026-01-12T16:13:24Z
```

# High memory consumption on MacOS from Visual Studio

---

_@madhavajay_

#### What version of ripgrep are you using?
```
./rg --version
ripgrep 11.0.1 (rev 973de50c9e)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

#### How did you install ripgrep?

This is part of a plugin in vscode im not sure if its bundled with the main app or a plugin.

#### What operating system are you using ripgrep on?
macOS Catalina 10.15.4 (19E266)

#### Describe your bug.
See images:
https://imgur.com/a/5nHcyoe

System ground to a halt when vscode was doing something.

#### What are the steps to reproduce the behavior?

I dont know, I was checking my installed extensions and looking at my settings.json as I am migrating to a new macbook.
I don't remember doing anything except maybe typing a search term in the Extensions input box.

#### What is the actual behavior?

All I can see is that the process was using 30gb of memory and my system nearly froze, its a 6-core 2018 MBP with 16gb of ram.

#### What is the expected behavior?

Is it possible to throttle / prevent ripgrep from consuming too much memory by limiting how much it takes in at a time?


---

_Comment by @BurntSushi on 2020-07-08 23:33_

This isn't actionable without a command line reproduction on a particular corpus that I can access. Sorry.

And your version of ripgrep is pretty old. Please upgrade.

---

_Closed by @BurntSushi on 2020-07-08 23:33_

---

_Label `invalid` added by @BurntSushi on 2020-07-08 23:33_

---

_Comment by @madhavajay on 2020-07-09 01:49_

I have linked this from the vscode-ripgrep repo:
https://github.com/microsoft/vscode-ripgrep/issues/11

---
