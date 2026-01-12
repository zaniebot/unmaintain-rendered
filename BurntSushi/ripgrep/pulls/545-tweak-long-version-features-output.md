```yaml
number: 545
title: Tweak long_version features output
type: pull_request
state: merged
author: g2p
labels: []
assignees: []
merged: true
base: master
head: long-version
created_at: 2017-07-07T15:48:03Z
updated_at: 2017-07-07T16:18:45Z
url: https://github.com/BurntSushi/ripgrep/pull/545
synced_at: 2026-01-12T18:23:13Z
```

# Tweak long_version features output

---

_@g2p_

This reuses the systemd convention of putting flags on a separate line.
All credit to okdana for the implementation.  Addresses #524.

---

_Comment by @BurntSushi on 2017-07-07 15:51_

@okdana Thoughts?

---

_Comment by @okdana on 2017-07-07 16:10_

🤔

Well, it does certainly match `systemd`.

The only potential concern i can think of is that making the 'flags' in the list different from the actual feature names means that there's no longer a 1:1 mapping between the list and the crate/build stuff. Not sure if that would ever be an issue (and i think you said you eventually want to make this all unnecessary anyway?), but that was kind of my thinking with the way i wrote it i guess.

I'm cool with it if that doesn't bother you tho

---

_Comment by @BurntSushi on 2017-07-07 16:18_

Yeah, it's a little iffy, but I think the mapping is still unambiguous, and yeah, hopefully we won't need these some day.

---

_Merged by @BurntSushi on 2017-07-07 16:18_

---

_Closed by @BurntSushi on 2017-07-07 16:18_

---
