```yaml
number: 2351
title: "Use `rustls-tls` with manual `SSL_CERT_FILE` implementation"
type: pull_request
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
base: main
head: charlie/ssl
created_at: 2024-03-11T02:17:54Z
updated_at: 2024-03-11T20:02:12Z
url: https://github.com/astral-sh/uv/pull/2351
synced_at: 2026-01-10T14:49:08Z
```

# Use `rustls-tls` with manual `SSL_CERT_FILE` implementation

---

_Pull request opened by @charliermarsh on 2024-03-11 02:17_

## Summary

This is one solution to resolving https://github.com/astral-sh/uv/issues/2346: use `rustls-tls`, but continue to allow `SSL_CERT_FILE`.

On my machine, it cuts simple commands dramatically.

On main:

```
❯ echo "requests" | ./target/release/uv pip compile -
Resolved 5 packages in 123ms
```

On this branch:

```
❯ echo "requests" | ./target/release/uv pip compile -
Resolved 5 packages in 4ms
```

I'm sure there are other considerations here but it's an option.

---

_Review requested from @zanieb by @charliermarsh on 2024-03-11 02:17_

---

_Label `performance` added by @charliermarsh on 2024-03-11 02:18_

---

_Comment by @zanieb on 2024-03-11 04:08_

Hm, that's a bummer. Can we toggle using system certificates with a flag instead and have it be off by default? I'm hesitant to require `SSL_CERT_FILE` and just using the system trust store is superior in a lot of cases. I'd maybe even suggest it be on by default... but the performance regression is pretty large.

---

_Comment by @charliermarsh on 2024-03-11 12:41_

I can try that, yeah.

---

_Comment by @charliermarsh on 2024-03-11 20:02_

Closing in favor of https://github.com/astral-sh/uv/pull/2362.

---

_Closed by @charliermarsh on 2024-03-11 20:02_

---
