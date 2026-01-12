```yaml
number: 1082
title: ripgrep warns about a .gitignore pattern accepted by git
type: issue
state: closed
author: hferreiro
labels:
  - duplicate
assignees: []
created_at: 2018-10-11T08:56:13Z
updated_at: 2018-10-11T10:10:45Z
url: https://github.com/BurntSushi/ripgrep/issues/1082
synced_at: 2026-01-12T16:13:22Z
```

# ripgrep warns about a .gitignore pattern accepted by git

---

_@hferreiro_

#### What version of ripgrep are you using?

0.8.1.

#### How did you install ripgrep?

Fedora repo package.

#### What operating system are you using ripgrep on?

Fedora 28.

#### Describe your question, feature request, or bug.

I'm getting the following warning:
```
.gitignore: line 7: error parsing glob '**.sw[po]': invalid use of **; must be one path component
```

By looking at the gitignore man page, I assume the correct way to write this would be `**/*.sw[po]`, but git seems to accept both versions.

---

_Comment by @okdana on 2018-10-11 09:25_

This one sure is popular

#1080
#945
#859
#507
#373

---

_Comment by @BurntSushi on 2018-10-11 10:09_

Yup. There are a number of issues for this, so closing as a duplicate.

---

_Closed by @BurntSushi on 2018-10-11 10:09_

---

_Label `duplicate` added by @BurntSushi on 2018-10-11 10:09_

---

_Comment by @BurntSushi on 2018-10-11 10:10_

FYI, if your don't want to see error messages from parsing ignore files, you can use `--no-ignore-messages`.

---
