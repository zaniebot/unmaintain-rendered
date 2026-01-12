```yaml
number: 1846
title: Running rg in vscode terminal sometimes makes vscode freeze.
type: issue
state: closed
author: lala7573
labels:
  - invalid
assignees: []
created_at: 2021-04-13T04:09:33Z
updated_at: 2021-04-26T10:54:29Z
url: https://github.com/BurntSushi/ripgrep/issues/1846
synced_at: 2026-01-12T16:13:24Z
```

# Running rg in vscode terminal sometimes makes vscode freeze.

---

_@lala7573_

#### What version of ripgrep are you using?

ripgrep 12.0.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

brew install rg

#### What operating system are you using ripgrep on?

OS: Darwin x64 19.6.0

#### Describe your bug.

I'm not sure if it's correct to post here. If not, please tell me where do this bug report.
When I try to print a lot of data using 'rg' in the vscode terminal (actually that much output is not intended), vscode hangs and I have to restart it.
I doubt it's a buffer problem, maybe. Because other tools like grep works well with printing that much data.

#### What are the steps to reproduce the behavior?

Just open vscode terminal on directory includes lots of data like home directory, then use rg 'a'

#### What is the actual behavior?
hang vscode

#### What is the expected behavior?
Not to hang vscode

What do you think ripgrep should have done?


---

_Comment by @BurntSushi on 2021-04-13 11:05_

I think this world be more appropriate to file against the terminal itself. ripgrep is just printing bytes. One thing you might try is disabling colors.

---

_Comment by @lala7573 on 2021-04-26 05:11_

Thank you for your answer :)
I upgraded rg using brew and the problem has been solved. vscode doesn't hang anymore even if there're lots of data on terminal.
```
ripgrep 12.1.1
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)
```

---

_Closed by @lala7573 on 2021-04-26 05:11_

---

_Label `invalid` added by @BurntSushi on 2021-04-26 10:54_

---
