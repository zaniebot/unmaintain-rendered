```yaml
number: 43
title: "--glob doesn't work with directories"
type: issue
state: closed
author: BurntSushi
labels:
  - bug
assignees: []
created_at: 2016-09-24T03:23:34Z
updated_at: 2016-09-24T20:34:44Z
url: https://github.com/BurntSushi/ripgrep/issues/43
synced_at: 2026-01-12T18:23:11Z
```

# --glob doesn't work with directories

---

_@BurntSushi_

For whatever reason, it looks like I made `Overrides` completely ignore directories. This comment suggests a transcription error:

``` rust
        // File types don't apply to directories.
        if is_dir {
            return Match::None;
        }
```


---

_Label `bug` added by @BurntSushi on 2016-09-24 03:23_

---

_Closed by @BurntSushi on 2016-09-24 20:34_

---
