---
number: 12982
title: "When using both `--native-tls` or `UV_NATIVE_TLS=true` AND using `SSL_CERT_FILE=...`, which one should take preference?"
type: issue
state: open
author: kdheepak
labels:
  - question
assignees: []
created_at: 2025-04-19T13:06:50Z
updated_at: 2025-04-19T13:06:50Z
url: https://github.com/astral-sh/uv/issues/12982
synced_at: 2026-01-10T01:25:27Z
---

# When using both `--native-tls` or `UV_NATIVE_TLS=true` AND using `SSL_CERT_FILE=...`, which one should take preference?

---

_Issue opened by @kdheepak on 2025-04-19 13:06_

### Question

Currently `SSL_CERT_FILE` takes preference. 

However, I'm running into a weird issue on one of my colleague's work computer where when `SSL_CERT_FILE` is specified `uv sync` runs into a proxy error (also same error when no environment variable is set). 

However, when `--native-tls` or `UV_NATIVE_TLS=true` is set AND `SSL_CERT_FILE` is unset it works fine.

But when `SSL_CERT_FILE` is set AND `--native-tls` or `UV_NATIVE_TLS=true` is used, we still run into the same proxy error.

Intuitively, I would expect any environment variables which `UV_` prefix to take precedence over a non prefixed environment variable, i.e. `UV_NATIVE_TLS=true` should take precedence over `SSL_CERT_FILE`. 

### Platform

Windows

### Version

uv --version uv 0.6.14 (a4cec56dc 2025-04-09)

---

_Label `question` added by @kdheepak on 2025-04-19 13:06_

---
