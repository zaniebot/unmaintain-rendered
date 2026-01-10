---
number: 1714
title: Secrets in URLs are displayed in plaintext but should be redacted
type: issue
state: closed
author: zanieb
labels:
  - security
assignees: []
created_at: 2024-02-19T19:49:36Z
updated_at: 2025-05-12T16:58:26Z
url: https://github.com/astral-sh/uv/issues/1714
synced_at: 2026-01-10T01:23:08Z
---

# Secrets in URLs are displayed in plaintext but should be redacted

---

_Issue opened by @zanieb on 2024-02-19 19:49_

e.g.

```
uv pip install "uv-private-pypackage@git+https://$GITHUB_TOKEN@github.com/astral-test/uv-private-pypackage"
Updating https://github_pat_blaslgmaskgnsaiugbasufbasbfsahufbs/ (github.com/astra
⠙ Resolving dependencies...      
```

---

_Comment by @zanieb on 2024-02-19 19:50_

`pip` has some inconsistent behavior here

```
Collecting uv-private-pypackage@ git+https://github_pat_blaslgmaskgnsaiugbasufbasbfsahufbs@github.com/astral-test/uv-private-pypackage
  Cloning https://****@github.com/astral-test/uv-private-pypackage to /private/var/folders/bc/qlsk3t6x7c9fhhbvvcg68k9c0000gp/T/pip-install-ch2lbg94/uv-private-pypackage_4d0cd18db04c42f0bd628080e4bf5722
  Running command git clone --filter=blob:none --quiet 'https://****@github.com/astral-test/uv-private-pypackage' /private/var/folders/bc/qlsk3t6x7c9fhhbvvcg68k9c0000gp/T/pip-install-ch2lbg94/uv-private-pypackage_4d0cd18db04c42f0bd628080e4bf5722
  ```

---

_Label `security` added by @zanieb on 2024-02-19 19:50_

---

_Comment by @charliermarsh on 2024-02-19 19:52_

Is it fair to assume that we should always redact the segment preceding the `@`?

---

_Comment by @zanieb on 2024-02-19 19:54_

Probably!

---

_Referenced in [astral-sh/uv#1717](../../astral-sh/uv/pulls/1717.md) on 2024-02-19 20:30_

---

_Referenced in [astral-sh/uv#2950](../../astral-sh/uv/issues/2950.md) on 2024-04-10 01:58_

---

_Referenced in [astral-sh/uv#3106](../../astral-sh/uv/issues/3106.md) on 2024-04-17 20:39_

---

_Referenced in [astral-sh/uv#5119](../../astral-sh/uv/issues/5119.md) on 2024-07-16 18:19_

---

_Referenced in [astral-sh/uv#10554](../../astral-sh/uv/issues/10554.md) on 2025-01-13 15:35_

---

_Referenced in [astral-sh/uv#12944](../../astral-sh/uv/pulls/12944.md) on 2025-04-17 13:47_

---

_Referenced in [astral-sh/uv#13325](../../astral-sh/uv/issues/13325.md) on 2025-05-07 14:29_

---

_Referenced in [astral-sh/uv#13357](../../astral-sh/uv/issues/13357.md) on 2025-05-09 07:21_

---

_Referenced in [astral-sh/uv#13333](../../astral-sh/uv/pulls/13333.md) on 2025-05-09 07:21_

---

_Closed by @jtfmumm on 2025-05-12 16:58_

---

_Closed by @jtfmumm on 2025-05-12 16:58_

---
