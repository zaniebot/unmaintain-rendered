```yaml
number: 1439
title: Doc suggestion for stdin and .gitingore behavior
type: issue
state: closed
author: JohnC32
labels:
  - doc
  - rollup
assignees: []
created_at: 2019-12-04T20:07:56Z
updated_at: 2020-03-15T17:19:17Z
url: https://github.com/BurntSushi/ripgrep/issues/1439
synced_at: 2026-01-12T16:13:23Z
```

# Doc suggestion for stdin and .gitingore behavior

---

_@JohnC32_

Using ripgrep 11.0.2 I ran into a hang using ripgrep from GNU Emacs via helm-rg on Windows. Window creates a stdin and ripgrep detects that stdin exists and waits for data, making it look like ripgrep is hung. I was able to easily work around the issue once I saw that I could explicitly specify the directory to search. Having this in the doc may save others confusion. Also having the note about .gitignore's are only respected if .git exists.

The man page intro currently contains:

       ripgrep (rg) recursively searches your current directory for a regex pattern. By default,
       ripgrep will respect your .gitignore and automatically skip hidden files/directories and
       binary files.

Could it be updated to:

       ripgrep (rg) recursively searches your current directory for a regex pattern. By default,
       ripgrep will respect your .gitignore files when a .git directory exists and automatically skip
       hidden files/directories and binary files.

       ripgrep will automatically detect if stdin exists and search stdin for a regex pattern, e.g.
       "ls | rg foo". In some environments, stdin may exist when it shouldn't. To turn off stdin
       detection explicitly specify the directory to search, e.g. "rg foo ./".

It would also be nice to mention why you need a .git directory to have .gitignores but that may be lost in time?

Thanks

---

_Label `doc` added by @BurntSushi on 2020-03-15 13:47_

---

_Label `rollup` added by @BurntSushi on 2020-03-15 13:55_

---

_Closed by @BurntSushi on 2020-03-15 17:19_

---
