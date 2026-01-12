```yaml
number: 1196
title: context/before/after options use different separator ahead of lines
type: issue
state: closed
author: Porkepix
labels:
  - question
assignees: []
created_at: 2019-02-11T09:27:40Z
updated_at: 2024-11-22T22:04:41Z
url: https://github.com/BurntSushi/ripgrep/issues/1196
synced_at: 2026-01-12T16:13:23Z
```

# context/before/after options use different separator ahead of lines

---

_@Porkepix_

#### What version of ripgrep are you using?

`ripgrep 0.10.0`

#### How did you install ripgrep?

ArchLinux's AUR

#### What operating system are you using ripgrep on?
ArchLinux

#### Describe your question, feature request, or bug.

Separator between file name/line number and matching line is different from the one for context/before/after options, for example with `sudo rg bar -C 5 --color=always -n | less -R`

```
foobar-3-foo
foobar-4-foo
foobar-5-foo
foobar-6-foo
foobar-7-foo
foobar:8:bar
foobar:9:bar
foobar:10:bar
foobar-11-foo
foobar-12-foo
foobar-13-foo
foobar-14-foo
foobar-15-foo
```
For matched lines, separator is `:`, for context lines, separator is`-`. Is it an intended behavior?

#### If this is a bug, what is the expected behavior?

What do you think ripgrep should have done?


---

_Comment by @BurntSushi on 2019-02-11 12:15_

> For matched lines, separator is `:`, for context lines, separator is `-`. Is it an intended behavior?

Yes.

---

_Closed by @BurntSushi on 2019-02-11 12:15_

---

_Label `question` added by @BurntSushi on 2019-02-11 12:15_

---
