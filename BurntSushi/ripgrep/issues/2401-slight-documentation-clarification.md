```yaml
number: 2401
title: Slight documentation clarification
type: issue
state: closed
author: nyurik
labels:
  - doc
  - rollup
assignees: []
created_at: 2023-01-23T20:26:16Z
updated_at: 2023-11-25T20:03:58Z
url: https://github.com/BurntSushi/ripgrep/issues/2401
synced_at: 2026-01-12T16:13:24Z
```

# Slight documentation clarification

---

_@nyurik_

I think the first documentation line should clarify that `ripgrep` searches the **content** of the files, as oppose to the filenames that match regex (like `fd`).

Current description:

> `ripgrep` is a line-oriented search tool that recursively searches the current
directory for a regex pattern.

Some ideas (not sure about the wording yet):

> `ripgrep` recursively searches for text files whose content matches a regular expression pattern.

---

_Comment by @BurntSushi on 2023-11-22 21:54_

I settled on this:

```
ripgrep (rg) recursively searches the current directory for lines matching
a regex pattern. By default, ripgrep will respect gitignore rules and
automatically skip hidden files/directories and binary files.
```

---

_Label `doc` added by @BurntSushi on 2023-11-22 21:54_

---

_Label `rollup` added by @BurntSushi on 2023-11-22 21:54_

---

_Comment by @nyurik on 2023-11-22 21:57_

Do you think `the current directory` is needed in the description?  Seems like `recursively searches for lines ...` is descriptive enough.

---

_Comment by @nyurik on 2023-11-22 22:01_

P.S. tbh `lines` is also a bit of a minor confusion - usually novice users think of files in terms of content or text.  The fact that its a line search tool is more of an implementation detail (that might even be overridable with various multiline regexes?). So perhaps something like this? (sorry for bikeshading):

```
ripgrep (rg) recursively searches files for content matching a regex pattern.
By default, ripgrep will respect gitignore rules and automatically skip
hidden files/directories and binary files.
```

---

_Closed by @BurntSushi on 2023-11-25 20:03_

---
