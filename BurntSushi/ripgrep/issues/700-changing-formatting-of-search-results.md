```yaml
number: 700
title: Changing formatting of search results
type: issue
state: closed
author: tpoliaw
labels:
  - question
assignees: []
created_at: 2017-11-30T17:31:29Z
updated_at: 2022-12-04T15:19:34Z
url: https://github.com/BurntSushi/ripgrep/issues/700
synced_at: 2026-01-12T16:13:22Z
```

# Changing formatting of search results

---

_@tpoliaw_

When using `--no-heading`, is there any way to add a space between the filename and the `:##:`?

At the moment it concatenates the file, the line number and the line with the search result meaning that double clicking to select a file selects the number as well so double-click → middle-click to type a file name isn't possible.

---

_Comment by @BurntSushi on 2017-11-30 17:38_

No, sorry. If you want this, I'd recommend writing a script that post-processes the output.

---

_Closed by @BurntSushi on 2017-11-30 17:38_

---

_Label `question` added by @BurntSushi on 2017-11-30 17:38_

---

_Comment by @tpoliaw on 2017-12-05 10:07_

For anyone else having a similar issue, you can change how double-click selects words. For gnome-terminal, `edit` → `General` → `Select-by-word-characters` → remove `:` from textbox.

---

_Comment by @sergeevabc on 2022-12-04 14:28_

Is there a native way (some switch) to concatenate, join, merge lines in the output?

Now:
Bat
Cat
Dog

Expected:
Bat Cat Dog

---

_Comment by @BurntSushi on 2022-12-04 15:19_

No. ripgrep isn't designed to perform arbitrary output manipulations. You'll need to find some other tool.

In the future, please don't necro post on old issues unless it's directly related. If you have a question, use Discussions.

---
