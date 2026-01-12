```yaml
number: 840
title: Man page missing on Ubuntu
type: issue
state: closed
author: rperce
labels:
  - wontfix
assignees: []
created_at: 2018-02-28T16:44:42Z
updated_at: 2018-03-08T19:31:46Z
url: https://github.com/BurntSushi/ripgrep/issues/840
synced_at: 2026-01-12T16:13:22Z
```

# Man page missing on Ubuntu

---

_@rperce_

#### What version of ripgrep are you using?

Through snap:
ripgrep 0.7.1
-AVX -SIMD

Through cargo:
ripgrep 0.8.1
-SIMD -AVX

#### What operating system are you using ripgrep on?

Ubuntu 16.04

#### Describe your question, feature request, or bug.

After installing ripgrep either through cargo or snap on Ubuntu 16.04 doesn't seem to install the man page. In either case, I receive the following output after installation:

```
╔═(robertp@robertp)════[505]════(~/puppet)
╚══╡λ man rg
No manual entry for rg
See 'man 7 undocumented' for help when manual pages are not available.
╔═(robertp@robertp)════[506]════(~/puppet)
╚══╡λ man ripgrep
No manual entry for ripgrep
```

#### If this is a bug, what are the steps to reproduce the behavior?

1. On Ubuntu 16.04, run either `cargo install ripgrep` or `sudo snap install rg`.
2. Run `man rg`.

#### If this is a bug, what is the actual behavior?

Man outputs 
```
No manual entry for rg
See 'man 7 undocumented' for help when manual pages are not available.
```

#### If this is a bug, what is the expected behavior?

Man outputs ripgrep's man page


---

_Label `wontfix` added by @BurntSushi on 2018-02-28 16:49_

---

_Comment by @BurntSushi on 2018-02-28 16:50_

I don't maintain the `rg` snap. Sorry.

Also, `cargo install` will never install the man page.

---

_Closed by @BurntSushi on 2018-02-28 16:50_

---

_Comment by @hackel on 2018-03-08 19:31_

This is being worked on by the snapd team, and will probably be fixed soon:
https://bugs.launchpad.net/snapd/+bug/1575593

---
