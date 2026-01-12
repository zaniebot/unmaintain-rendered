```yaml
number: 67
title: .gitignore handling of overlapping patterns is inconsistent with git
type: issue
state: closed
author: bennofs
labels: []
assignees: []
created_at: 2016-09-24T18:41:42Z
updated_at: 2016-09-25T00:11:36Z
url: https://github.com/BurntSushi/ripgrep/issues/67
synced_at: 2026-01-12T18:23:11Z
```

# .gitignore handling of overlapping patterns is inconsistent with git

---

_@bennofs_

Git allows the following in a `.gitignore`:

```
/*
!/dir
```

which means: ignore every directory in the root of the repository except the `dir` directory. Git understands this, but `rg` will fail to find results in `/dir`. Note that `pt` handles this correctly. 


---

_Closed by @BurntSushi on 2016-09-25 00:10_

---

_Comment by @BurntSushi on 2016-09-25 00:11_

Thanks for the report! This wasn't actually a bug with overlapping patterns or anything. There was a bug in the gitignore parser that was triggered by the whitelist that was preventing `/dir` from being interpreted as an anchored path.


---
