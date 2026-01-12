```yaml
number: 3125
title: "Please publish *-musl.deb for supported architectures"
type: issue
state: closed
author: pro-sumer
labels:
  - wontfix
assignees: []
created_at: 2025-08-14T20:21:09Z
updated_at: 2025-08-14T20:51:50Z
url: https://github.com/BurntSushi/ripgrep/issues/3125
synced_at: 2026-01-12T16:13:25Z
```

# Please publish *-musl.deb for supported architectures

---

_@pro-sumer_

#### Describe your feature request

Can you please build & publish `*-musl.deb` files for supported architectures?

Then https://apt.cli.rs/ can include your binaries and I can easier keep them up-to-date on my Raspberry Pi's and VPS.


---

_Comment by @BurntSushi on 2025-08-14 20:27_

It already is musl: https://github.com/BurntSushi/ripgrep/blob/119a58a400ea948c2d2b0cd4ec58361e74478641/.github/workflows/release.yml#L290

I do not want to publish more. You are welcome to set up another repo that does it, or just use the other artifacts and drop the binaries wherever.

---

_Closed by @BurntSushi on 2025-08-14 20:27_

---

_Label `wontfix` added by @BurntSushi on 2025-08-14 20:27_

---

_Comment by @pro-sumer on 2025-08-14 20:51_

Might be a nice learning experience for me; I'll keep it mind.

(And manually update for now)

---
