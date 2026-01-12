```yaml
number: 1460
title: "ripgrep doesn't find string while grep does"
type: issue
state: closed
author: eatthoselemons
labels:
  - invalid
assignees: []
created_at: 2020-01-17T17:40:49Z
updated_at: 2020-01-17T17:56:25Z
url: https://github.com/BurntSushi/ripgrep/issues/1460
synced_at: 2026-01-12T16:13:23Z
```

# ripgrep doesn't find string while grep does

---

_@eatthoselemons_

#### What version of ripgrep are you using?

11.0.2

#### How did you install ripgrep?

Arch package manager


#### What operating system are you using ripgrep on?

Arch 5.4.11 kernel

#### Describe your question, feature request, or bug.

I used ripgrep to search a section of files for a string `rg -i newhardwarestatussvc` and ripgrep showed only 1 result it was a correct result however it missed several cases that grep found.

Even going to the directory with one of the files containing the string, ripgrep doesn't find the string. Ripgrep is searching golang files.

Unfortunately the code is proprietary so cannot give the maintainers the ability to reproduce it however I am willing to run what commands you give me and see if we can fix it.


---

_Comment by @BurntSushi on 2020-01-17 17:47_

This issue doesn't have sufficient information. In particular, `rg -i newhardwarestatussvc` is not the same as `grep -ri newhardwarestatussvc` because the former does smart filtering while the latter does not. As the simplest possible next step, please consider trying `rg -uuu -i newhardwarestatussvc`.

If that still does not show the result where as grep does, then the next step is to reproduce this using a single file search. e.g., `rg -i newhardwarestatussvc the-file` vs `grep -i newhardwarestatussvc the-file`. Once you have that, you'll need to find some way to censor that file and send it to me in a way that lets you share it.

---

_Label `question` added by @BurntSushi on 2020-01-17 17:48_

---

_Comment by @eatthoselemons on 2020-01-17 17:51_

The `-uuu` command did find the same results as `grep -ir` does `-uuu` disable smart filtering? and what does smart filtering do? Why does it remove valid results?

---

_Closed by @eatthoselemons on 2020-01-17 17:52_

---

_Comment by @BurntSushi on 2020-01-17 17:54_

@eatthoselemons The first two sentences of the README should clear up your confusion:

> ripgrep is a line-oriented search tool that recursively searches your current directory for a regex pattern. By default, ripgrep will respect your .gitignore and automatically skip hidden files/directories and binary files.

It's also mentioned in the first couple lines of the description in ripgrep's man page.

You might consider reading the [user guide](https://github.com/BurntSushi/ripgrep/blob/master/GUIDE.md) to get more insight on ripgrep's filtering options and behavior.

---

_Label `question` removed by @BurntSushi on 2020-01-17 17:55_

---

_Label `invalid` added by @BurntSushi on 2020-01-17 17:55_

---

_Comment by @eatthoselemons on 2020-01-17 17:56_

Ah I see, thank you for your help BurntSushi!

I will read further, sorry for the invalid question!

---
