```yaml
number: 16414
title: Add support for SSL_CERT_DIR
type: issue
state: closed
author: samypr100
labels:
  - enhancement
assignees: []
created_at: 2025-10-23T03:28:34Z
updated_at: 2025-11-16T17:48:32Z
url: https://github.com/astral-sh/uv/issues/16414
synced_at: 2026-01-10T03:23:54Z
```

# Add support for SSL_CERT_DIR

---

_Issue opened by @samypr100 on 2025-10-23 03:28_

> Sidenote, `uv` doesn't support `SSL_CERT_DIR` directly but we should probably add explicit support so as its properly supported by `rustls-native-certs` as of a few months ago. 

 _Originally posted by @samypr100 in [#16412](https://github.com/astral-sh/uv/issues/16412#issuecomment-3434927201)_

For validation of ssl_cert_dir_exists, I'd use the approach from [rustls-native-certs](https://github.com/rustls/rustls-native-certs/blob/3ec7d86977f04fba8d369571595b47d16c8a60f2/src/lib.rs#L201-L208), validating each path and issuing a warning for paths that don't exists similar to ssl_cert_file_exists logic.



---

_Label `enhancement` added by @samypr100 on 2025-10-23 03:29_

---

_Closed by @zanieb on 2025-11-16 17:48_

---
