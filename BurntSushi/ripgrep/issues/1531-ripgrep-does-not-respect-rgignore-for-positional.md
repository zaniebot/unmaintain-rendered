```yaml
number: 1531
title: "RipGrep does not respect `.rgignore` for positional arguments"
type: issue
state: closed
author: zachriggle
labels:
  - invalid
assignees: []
created_at: 2020-03-27T18:52:47Z
updated_at: 2020-03-27T19:03:14Z
url: https://github.com/BurntSushi/ripgrep/issues/1531
synced_at: 2026-01-12T16:13:23Z
```

# RipGrep does not respect `.rgignore` for positional arguments

---

_@zachriggle_

It's not clear that this is intended behavior -- RipGrep does not respect `.rgignore` when a file is specified on the command line.

```
~/ripgrep-issue ❯❯❯ echo hello > myfile
~/ripgrep-issue ❯❯❯ echo myfile > .rgignore
~/ripgrep-issue ❯❯❯ rg hello
~/ripgrep-issue ❯❯❯ rg hello -- myfile
1:hello
```

---

_Comment by @okdana on 2020-03-27 19:00_

The manual says

```
POSITIONAL ARGUMENTS
       PATTERN
           A regular expression used for searching. To match a pattern
           beginning with a dash, use the -e/--regexp option.

       PATH
           A file or directory to search. Directories are searched
           recursively. Paths specified explicitly on the command line
           override glob and ignore rules.
```

---

_Label `invalid` added by @BurntSushi on 2020-03-27 19:03_

---

_Closed by @BurntSushi on 2020-03-27 19:03_

---
