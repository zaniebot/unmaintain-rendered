```yaml
number: 1360
title: single vs double quotes in glob
type: issue
state: closed
author: gvdeynde
labels:
  - invalid
assignees: []
created_at: 2019-08-29T14:03:36Z
updated_at: 2019-08-29T14:26:01Z
url: https://github.com/BurntSushi/ripgrep/issues/1360
synced_at: 2026-01-12T16:13:23Z
```

# single vs double quotes in glob

---

_@gvdeynde_

#### What version of ripgrep are you using?

ripgrep 11.0.2 (rev 3de31f7527)
-SIMD -AVX (compiled)
+SIMD +AVX (runtime)

#### How did you install ripgrep?

Github binary release for Windows 64bit (msvc)

#### What operating system are you using ripgrep on?

Windows 10

#### Describe your question, feature request, or bug.
using double quotes in the glob:

> rg -g "*mp4*" --files

gives me a list of mp4 files in the tree below my cwd, as expected.

using single quotes in the glob:

> rg -g '*mp4*' --files

gives me nothing. 

#### If this is a bug, what is the expected behavior?

I would assume double and single quotes would behave identical (as long as you don't mix them up). 


---

_Comment by @BurntSushi on 2019-08-29 14:11_

This isn't an issue with ripgrep, as far as I can tell. ripgrep doesn't care about quotes. This is almost certainly your shell. You'll need to consult the documentation for whatever shell you're using on Windows.

---

_Closed by @BurntSushi on 2019-08-29 14:11_

---

_Label `invalid` added by @BurntSushi on 2019-08-29 14:11_

---

_Comment by @gvdeynde on 2019-08-29 14:26_

Tried it with cmd.exe, powershell and bash (through conemu). The behaviour is linked to cmd.exe... [StackOverflow](https://stackoverflow.com/questions/24173825/what-does-single-quote-do-in-windows-batch-files) to the rescue. Apparently single quotes are simply ignored by cmd.exe except in a `for` statement. Of course, I should have know ;-)

Agreed this is not a bug in ripgrep. But it might be worth mentioning in the FAQ or docs...

---
