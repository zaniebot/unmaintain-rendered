```yaml
number: 440
title: Searching with a glob pattern that matches a single file - file name is not printed
type: issue
state: closed
author: roblourens
labels: []
assignees: []
created_at: 2017-04-05T21:17:03Z
updated_at: 2017-04-05T22:37:56Z
url: https://github.com/BurntSushi/ripgrep/issues/440
synced_at: 2026-01-12T16:13:22Z
```

# Searching with a glob pattern that matches a single file - file name is not printed

---

_@roblourens_

Have an empty directory

```
echo "foo" > a.txt
rg foo     # prints filename header
rg foo ./  # also prints filename
rg foo *   # does not print filename
rg foo ./* # does not print filename
rg foo ./a.txt  # does not print filename
```

If another file is in the directory, then the filenames are printed. I think this makes sense in the `rg a.txt` case but in the other cases, a user may be searching with a complex glob in a large directory, so doesn't it make sense to also print the filename?

---

_Comment by @BurntSushi on 2017-04-05 22:37_

Globs are resolved by the shell on Unix, so when you use `rg foo *`, ripgrep never actually sees the `*`. Instead, it just sees the expansion, which in this case, is simply `rg foo a.txt`. In other words, in a directory with a single file `a.txt`, every single one of your commands is indistinguishable from the other from ripgrep's perspective.

If you always want to display the file path, then you can use the `-H/--with-filename` flag (which is also found in GNU grep).

---

_Closed by @BurntSushi on 2017-04-05 22:37_

---
