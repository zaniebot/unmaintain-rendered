---
number: 15318
title: "Flake in test `malo_malicious_zipinzip` due to `peer closed connection without sending TLS close_notify`"
type: issue
state: closed
author: zanieb
labels:
  - testing
assignees: []
created_at: 2025-08-15T21:06:57Z
updated_at: 2025-08-15T23:31:23Z
url: https://github.com/astral-sh/uv/issues/15318
synced_at: 2026-01-07T13:12:19-06:00
---

# Flake in test `malo_malicious_zipinzip` due to `peer closed connection without sending TLS close_notify`

---

_Issue opened by @zanieb on 2025-08-15 21:06_

```

    thread 'extract::malo_malicious_zipinzip' panicked at crates/uv/tests/it/extract.rs:7:44:
    called `Result::unwrap()` on an `Err` value: reqwest::Error { kind: Request, url: "https://pub-c6f28d316acd406eae43501e51ad30fa.r2.dev/0723f54ceb33a4fdc7f2eddc19635cd704d61c84/malicious/zipinzip.zip", source: hyper_util::client::legacy::Error(SendRequest, hyper::Error(Io, Custom { kind: UnexpectedEof, error: "peer closed connection without sending TLS close_notify: https://docs.rs/rustls/latest/rustls/manual/_03_howto/index.html#unexpected-eof" })) }
```

---

_Label `testing` added by @zanieb on 2025-08-15 21:07_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-08-15 22:55_

---

_Referenced in [astral-sh/uv#15322](../../astral-sh/uv/pulls/15322.md) on 2025-08-15 23:08_

---

_Closed by @charliermarsh on 2025-08-15 23:31_

---
