```yaml
number: 3249
title: Support for symlinked .zst files
type: issue
state: closed
author: kenorb
labels: []
assignees: []
created_at: 2025-12-20T17:35:03Z
updated_at: 2025-12-20T17:47:41Z
url: https://github.com/BurntSushi/ripgrep/issues/3249
synced_at: 2026-01-12T16:13:25Z
```

# Support for symlinked .zst files

---

_@kenorb_

The following example of grep'ing .gz file works:

```
$ echo test | gzip > test.txt.gz
$ ln -vs test.txt.gz test.lnk.txt.gz
'test.lnk.txt.gz' -> 'test.txt.gz'
$ rg -zL . test.lnk.txt.gz 
1:test
$ zgrep test test.lnk.txt.gz
test
```

but when the file is in .zst format, it won't work:

```
$ echo test | zstd -o test.txt.zst
/*stdin*\            :360.00%   (     5 B =>     18 B, test.txt.zst)          
$ ln -vs test.txt.zst test.lnk.txt.zst
'test.lnk.txt.zst' -> 'test.txt.zst'
$ rg -Lz . test.lnk.txt.zst
rg: test.lnk.txt.zst: <stderr is empty>
$ zstdgrep test test.lnk.txt.zst
test
```

I'm not sure if this is related to GH-2811, but in general regular symlinked files in .gz format seems to work fine.

---

_Comment by @BurntSushi on 2025-12-20 17:40_

I don't think this is a problem with ripgrep. It looks like `zstd` doesn't behave as predictably as `gzip`:

```
$ zstd -q -d -c test.lnk.txt.zst
$ zstd -q -d -c test.txt.zst
test
```

I don't see anything relevant in `man zstd`.

---

_Comment by @kenorb on 2025-12-20 17:45_

Can `zstdcat` be used instead? It's part of `zstd` package.

---

_Comment by @BurntSushi on 2025-12-20 17:47_

I would rather see a bug filed against `zstd`. And if they won't fix it, then I'm willing to consider alternatives.

---

_Closed by @BurntSushi on 2025-12-20 17:47_

---
