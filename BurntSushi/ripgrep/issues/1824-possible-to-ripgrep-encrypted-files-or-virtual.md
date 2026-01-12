```yaml
number: 1824
title: possible to ripgrep encrypted files, or virtual filesystem?
type: issue
state: closed
author: bionicles
labels: []
assignees: []
created_at: 2021-03-17T13:48:08Z
updated_at: 2021-03-17T13:51:56Z
url: https://github.com/BurntSushi/ripgrep/issues/1824
synced_at: 2026-01-12T16:13:24Z
```

# possible to ripgrep encrypted files, or virtual filesystem?

---

_@bionicles_

#### Describe your feature request

To comply with certain regulations around security and privacy, I'd like to enable users to search encryped data. 

Is it possible to either,

A) store files encrypted and streaming-decrypt like https://docs.rs/cryptostream/0.3.1/cryptostream/ them right before ripgrep?
or
B) store nothing, and just have a in-memory virtual filesystem like https://docs.rs/vfs/0.5.1/vfs/, and ripgrep over that, then delete the virtual filesystem?
or
C) all of the above, streaming-decrypt an encrypted virtual filesystem and pipe it into ripgrep

This would be pretty rad


---

_Closed by @BurntSushi on 2021-03-17 13:51_

---

_Locked by @ghost on 2021-03-17 13:51_

---
