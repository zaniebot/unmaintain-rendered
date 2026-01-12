```yaml
number: 2924
title: Update readme/installation instructions to remove broken URL
type: issue
state: closed
author: wackget
labels: []
assignees: []
created_at: 2024-11-07T16:25:12Z
updated_at: 2025-09-24T12:59:25Z
url: https://github.com/BurntSushi/ripgrep/issues/2924
synced_at: 2026-01-12T16:13:25Z
```

# Update readme/installation instructions to remove broken URL

---

_@wackget_

The installation section of the readme contains a broken link for RHEL/CentOS installation:
https://copr.fedorainfracloud.org/coprs/carlwgeorge/ripgrep/repo/epel-7/carlwgeorge-ripgrep-epel-7.repo

Please kindly remove this and/or update the readme with working instructions for RHEL.

Thanks

---

_Comment by @wackget on 2025-01-30 17:04_

For those still waiting, this'll install ripgrep on RHEL/CentOS/Rocky Linux 9:

```
sudo dnf config-manager --set-enabled crb
sudo dnf install -y \
    https://dl.fedoraproject.org/pub/epel/epel-release-latest-9.noarch.rpm \
    https://dl.fedoraproject.org/pub/epel/epel-next-release-latest-9.noarch.rpm
sudo dnf install -y ripgrep
```

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---

_Comment by @carlwgeorge on 2025-09-24 03:50_

> For those still waiting, this'll install ripgrep on RHEL/CentOS/Rocky Linux 9:

These instructions are wrong.  Installing epel-next-release on RHEL or Rocky will result in broken systems.    I provided the correct instructions in #3124, which rely on linking to the official EPEL setup instructions to avoid duplication and mistakes (such as the one being made here).  That should have been merged, but instead it seems an amended version #2981 was merged as c03731005020a4fef8016e46c1ba9fdab8d7df4c.

@BurntSushi This issue is not resolve and should be reopened.  Please either correct the instructions (use #3124) or remove them to avoid breaking people's systems.

---

_Comment by @BurntSushi on 2025-09-24 12:59_

See https://github.com/BurntSushi/ripgrep/pull/3159

---
