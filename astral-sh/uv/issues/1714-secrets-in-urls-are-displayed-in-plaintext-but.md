```yaml
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
synced_at: 2026-01-12T15:58:31Z
```

# Secrets in URLs are displayed in plaintext but should be redacted

---

_@zanieb_

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

_Closed by @jtfmumm on 2025-05-12 16:58_

---

_Closed by @jtfmumm on 2025-05-12 16:58_

---
