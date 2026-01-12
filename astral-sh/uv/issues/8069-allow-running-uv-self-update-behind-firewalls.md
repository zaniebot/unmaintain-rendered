```yaml
number: 8069
title: Allow running uv self update behind firewalls
type: issue
state: closed
author: gaborbernat
labels:
  - releases
assignees: []
created_at: 2024-10-10T04:59:36Z
updated_at: 2025-02-12T17:09:13Z
url: https://github.com/astral-sh/uv/issues/8069
synced_at: 2026-01-12T15:59:19Z
```

# Allow running uv self update behind firewalls

---

_@gaborbernat_

So there's an implicit feature within the installation script that allows moving the standalone installer behind the firewall too, `ARTIFACT_DOWNLOAD_URL` can be configured via INSTALLER_DOWNLOAD_URL environment variable, so something like:

```bash
curl -LsSf https://magic.com/uv/uv-installer.sh | env INSTALLER_DOWNLOAD_URL=https://magic.com/uv sh
```
This will work assuming you mirror the GitHub release to `https://magic.com/uv`. This is great!

Now the problem is that self update is not so smart, would it be appetite to change https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/self_update.rs. So when the `INSTALLER_DOWNLOAD_URL` environment is set, instead of reaching out to GitHub, it would download the update from within that HTTP host. We can come up with some kind of version file there so that it can pull the latest version from there too.

If we agree, this is something I can contribute. 😊

---

_Comment by @gaborbernat on 2024-10-10 05:00_

We should also codify that `INSTALLER_DOWNLOAD_URL` is a supported feature and not just an implementation detail, 😊 while we're at it.

---

_Label `releases` added by @zanieb on 2024-10-10 17:15_

---

_Comment by @gaborbernat on 2024-10-24 14:52_

Contributed this feature upstream, https://github.com/axodotdev/axoupdater/pull/199. Should be available within their next release.

---

_Closed by @gaborbernat on 2025-02-12 17:09_

---

_Comment by @gaborbernat on 2025-02-12 17:09_

This is now possible and done.

---
