```yaml
number: 405
title: "--glob excluding a directory from the root"
type: issue
state: closed
author: roblourens
labels: []
assignees: []
created_at: 2017-03-12T19:12:39Z
updated_at: 2017-03-12T19:51:21Z
url: https://github.com/BurntSushi/ripgrep/issues/405
synced_at: 2026-01-12T16:13:21Z
```

# --glob excluding a directory from the root

---

_@roblourens_

Say I have two files in my workspace, `./foo/bar/file1.txt`, and `./bar/foo/file2.txt`. There seems to be an inconsistency in how the `--glob` option works:

- `rg -g 'foo/**'` searches both, also expected
- `rg -g '/foo/**'` searches file1.txt, which is expected
- `rg -g '!foo/**'` searches neither, which is expected
- `rg -g '!/foo/**'` searches neither, whereas I'd expect it to only exclude the file1 path.

The .gitignore works as I'd expect:
- `foo/` excludes both
- `/foo/` only excludes the file1 path

Am I using the CLI option wrong?

---

_Closed by @BurntSushi on 2017-03-12 19:51_

---
