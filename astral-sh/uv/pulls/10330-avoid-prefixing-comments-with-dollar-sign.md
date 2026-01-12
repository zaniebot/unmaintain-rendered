```yaml
number: 10330
title: Avoid prefixing comments with dollar sign
type: pull_request
state: closed
author: charliermarsh
labels:
  - documentation
assignees: []
base: main
head: charlie/dollar
created_at: 2025-01-06T18:16:09Z
updated_at: 2025-01-06T18:38:44Z
url: https://github.com/astral-sh/uv/pull/10330
synced_at: 2026-01-12T16:09:14Z
```

# Avoid prefixing comments with dollar sign

---

_@charliermarsh_

## Summary

We aren't consistent about this, but it seems like it makes sense to omit the $?


---

_Review requested from @zanieb by @charliermarsh on 2025-01-06 18:16_

---

_Label `documentation` added by @charliermarsh on 2025-01-06 18:16_

---

_Comment by @zanieb on 2025-01-06 18:21_

I think this was required for some reason... did you test the copy/paste?

---

_@zanieb reviewed on 2025-01-06 18:23_

---

_Review comment by @zanieb on `docs/guides/integration/alternative-indexes.md`:63 on 2025-01-06 18:23_

Copy-pasting this gets me

```
Pre-install keyring and the Artifacts plugin from the public PyPI
uv tool install keyring --with artifacts-keyring

Enable keyring authentication
export UV_KEYRING_PROVIDER=subprocess

Configure the index URL with the username
export UV_EXTRA_INDEX_URL=https://VssSessionToken@pkgs.dev.azure.com/{organisation}/{project}/_packaging/{feedName}/pypi/simple/
```

---

_@zanieb requested changes on 2025-01-06 18:24_

We'll need to either fix the copy-paste script or leave these as-is, unfortunately.

---

_Comment by @charliermarsh on 2025-01-06 18:24_

No not at all. I can though. Where is the code that handles this?

---

_Comment by @charliermarsh on 2025-01-06 18:25_

Do you agree that you'd rather omit the dollar if we can?

---

_Comment by @zanieb on 2025-01-06 18:27_

https://github.com/astral-sh/uv/blob/78df14e7a8ff3345af3f58dfae53b07cacd8decb/docs/js/extra.js#L1-L17

I think it's nice to omit it, yeah. I'm sure it's feasible.

---

_Comment by @zanieb on 2025-01-06 18:29_

This also breaks syntax highlighting of comments though

<img width="1783" alt="Screenshot 2025-01-06 at 12 28 08 PM" src="https://github.com/user-attachments/assets/99845e7e-89a9-47eb-8fbf-0f69e7c374d0" />
<img width="1783" alt="Screenshot 2025-01-06 at 12 27 53 PM" src="https://github.com/user-attachments/assets/05a6fde5-541b-4d7b-bbe1-424b1db705a8" />


---

_Comment by @charliermarsh on 2025-01-06 18:37_

Alright, I fixed the copy-paste, but I guess we leave it then? That's a bummer.

---

_Closed by @charliermarsh on 2025-01-06 18:38_

---
