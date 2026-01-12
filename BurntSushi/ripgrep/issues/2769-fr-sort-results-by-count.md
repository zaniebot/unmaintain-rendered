```yaml
number: 2769
title: "FR: sort results by count"
type: issue
state: closed
author: etiennepellegrini
labels: []
assignees: []
created_at: 2024-03-28T02:42:20Z
updated_at: 2024-03-28T02:49:13Z
url: https://github.com/BurntSushi/ripgrep/issues/2769
synced_at: 2026-01-12T16:13:24Z
```

# FR: sort results by count

---

_@etiennepellegrini_

I'd love to see a `count` option to `--sort` and `--sortr`, to be able to sort files according to the number of matches.
Both `--sort=count` and `--sort=count-matches` would be ideal!

Thanks!

---

_Comment by @BurntSushi on 2024-03-28 02:49_

This is probably not going to happen, because it would require buffering _everything_ into memory (including all of the matches from all of the files), sorting it and then printing. Currently, the existing sort options only require buffering the file paths to search into memory. (Which is bad enough already.)

---

_Closed by @BurntSushi on 2024-03-28 02:49_

---
