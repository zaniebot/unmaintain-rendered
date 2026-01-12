```yaml
number: 957
title: A path separator must be exactly one byte, but the given separator is 20 bytes
type: issue
state: closed
author: rofrol
labels:
  - duplicate
  - question
  - doc
assignees: []
created_at: 2018-06-20T13:24:27Z
updated_at: 2018-07-22T13:33:14Z
url: https://github.com/BurntSushi/ripgrep/issues/957
synced_at: 2026-01-12T16:13:22Z
```

# A path separator must be exactly one byte, but the given separator is 20 bytes

---

_@rofrol_

#### What version of ripgrep are you using?

```bash
$ rg --version
ripgrep 0.8.1
-SIMD -AVX
```

#### How did you install ripgrep?

`cargo install ripgrep`

#### What operating system are you using ripgrep on?

Windows 10 and git bash

#### Describe your question, feature request, or bug.

When in git bash, cannot provide path separator as just `/`.

#### If this is a bug, what are the steps to reproduce the behavior?

```bash
$ rg --files -telm --path-separator /
A path separator must be exactly one byte, but the given separator is 20 bytes.
```

#### If this is a bug, what is the actual behavior?

As above.

#### If this is a bug, what is the expected behavior?

ripgrep accepts `/` in git bash.


---

_Comment by @BurntSushi on 2018-06-20 14:04_

This looks like a duplicate of https://github.com/BurntSushi/ripgrep/issues/501#issuecomment-305797890. It looks like your shell is expanding `/` to something else. I don't use Windows, so you'll need to figure out how to disable or work around that behavior. From the linked issue, try `--path-separator '\x2F'` or `--path-separator //`.

---

_Label `duplicate` added by @BurntSushi on 2018-06-20 14:04_

---

_Label `question` added by @BurntSushi on 2018-06-20 14:04_

---

_Label `doc` added by @BurntSushi on 2018-07-22 13:12_

---

_Closed by @BurntSushi on 2018-07-22 13:33_

---
