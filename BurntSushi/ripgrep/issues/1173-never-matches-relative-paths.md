```yaml
number: 1173
title: "** never matches relative paths"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2019-01-23T23:11:07Z
updated_at: 2019-01-23T23:26:50Z
url: https://github.com/BurntSushi/ripgrep/issues/1173
synced_at: 2026-01-12T16:13:23Z
```

# ** never matches relative paths

---

_@BurntSushi_

Originally reported by @okdana in https://github.com/BurntSushi/ripgrep/pull/1093#issuecomment-433691732

#### What version of ripgrep are you using?

master @ b48bbf52

#### How did you install ripgrep?

Compiled.

#### What operating system are you using ripgrep on?

Linux.

#### Describe your question, feature request, or bug.

The `**` pattern does not work in a `.gitignore` file.

#### If this is a bug, what are the steps to reproduce the behavior?

```
$ mkdir /tmp/rgbug
$ cd /tmp/rgbug
$ git init
$ echo '**' > .gitignore
$ echo test > foo
$ rg test
foo
1:test
```

#### If this is a bug, what is the actual behavior?

`foo` is shown as a search result, but it should be ignored.

#### If this is a bug, what is the expected behavior?

`foo` should have been ignored by the `**` pattern.


---

_Closed by @BurntSushi on 2019-01-23 23:13_

---

_Label `bug` added by @BurntSushi on 2019-01-23 23:26_

---
