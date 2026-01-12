```yaml
number: 1216
title: add fossil support for ignored files
type: issue
state: closed
author: minusf
labels:
  - wontfix
assignees: []
created_at: 2019-03-10T23:15:20Z
updated_at: 2019-03-11T09:40:18Z
url: https://github.com/BurntSushi/ripgrep/issues/1216
synced_at: 2026-01-12T16:13:23Z
```

# add fossil support for ignored files

---

_@minusf_

#### What version of ripgrep are you using?

`ripgrep 0.10.0`

#### How did you install ripgrep?

brew

#### What operating system are you using ripgrep on?

macOS

#### Describe your question, feature request, or bug.

It would be great if ripgrep could extend it's ignored files support for fossil scm.
Fossil ignore file is: `.fossil-settings/ignore-glob`

The glob patterns are not the same though.  One difference I have noticed
for example was not being able to use `/venv` -> `venv/`.

---

_Label `wontfix` added by @BurntSushi on 2019-03-11 00:12_

---

_Comment by @BurntSushi on 2019-03-11 00:14_

While in principle I agree it would be nice, in practice, I don't think this is worth doing. Getting the ignore rules correct is one of the biggest sources of bugs in ripgrep, and that's considering that we're only trying to handle one source control system. If I were to devote resources to supporting another source control system, it wouldn't be Fossil, it would probably be Mercurial, just based on my perception of how many people use it.

My suggestion would be to just translate your Fossil rules to an `.ignore` file.

---

_Closed by @BurntSushi on 2019-03-11 00:14_

---

_Comment by @minusf on 2019-03-11 09:40_

thank you for your explanation.  i just realised that there is a very good document comparing
git's ignore with fossil's in the fossil wiki, so for the sake of completeness i'll just put it here:
https://www.fossil-scm.org/xfer/doc/trunk/www/globs.md

---
