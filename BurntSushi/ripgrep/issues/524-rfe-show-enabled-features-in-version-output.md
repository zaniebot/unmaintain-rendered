```yaml
number: 524
title: "RFE: show enabled features in --version output"
type: issue
state: closed
author: g2p
labels:
  - enhancement
assignees: []
created_at: 2017-06-20T21:41:53Z
updated_at: 2017-07-07T15:41:30Z
url: https://github.com/BurntSushi/ripgrep/issues/524
synced_at: 2026-01-12T16:13:22Z
```

# RFE: show enabled features in --version output

---

_@g2p_

Just to make sure that SSE and AVX are enabled.

See `systemd --version` for what the output could look like.

---

_Label `enhancement` added by @BurntSushi on 2017-06-21 00:13_

---

_Closed by @BurntSushi on 2017-07-07 15:31_

---

_Comment by @g2p on 2017-07-07 15:36_

Is it okay if I tweak the output to be more like systemd produces, before this is released?

---

_Comment by @BurntSushi on 2017-07-07 15:37_

@g2p I think I'm OK with that. Is systemd's output standard or common?

---

_Comment by @g2p on 2017-07-07 15:41_

It's machine parseable at least; I picked it as the best looking option I'm aware of.

---
