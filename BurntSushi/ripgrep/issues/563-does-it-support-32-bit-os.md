```yaml
number: 563
title: Does it support 32 bit OS?
type: issue
state: closed
author: btstw
labels: []
assignees: []
created_at: 2017-07-21T09:11:23Z
updated_at: 2017-07-21T10:34:56Z
url: https://github.com/BurntSushi/ripgrep/issues/563
synced_at: 2026-01-12T16:13:22Z
```

# Does it support 32 bit OS?

---

_@btstw_

I am using fedora_24 _i386.
When I try to install this tool, it shows error message .
"Failed to synchronize cache for repo 'carlwgeorge-ripgrep', disabling."


---

_Comment by @okdana on 2017-07-21 09:18_

Doesn't seem like it, the only RPMs here are for `x86_64`:

https://copr-be.cloud.fedoraproject.org/results/carlwgeorge/ripgrep/

Pretty sure that repo is maintained by a third party tho so you might have to talk to whoever that is

In the mean time, though, the first-party builds on GitHub include a 32-bit version:

https://github.com/BurntSushi/ripgrep/releases/tag/0.5.2

---

_Comment by @btstw on 2017-07-21 10:34_

I was able to use first-party builds, thanks.


---

_Closed by @btstw on 2017-07-21 10:34_

---
