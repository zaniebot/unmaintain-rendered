---
number: 11595
title: "Consider using the system SSL library, i.e., OpenSSL instead of `rusttls` / `ring`"
type: issue
state: open
author: marco-cloudflare
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2025-02-18T12:02:19Z
updated_at: 2025-02-18T15:32:56Z
url: https://github.com/astral-sh/uv/issues/11595
synced_at: 2026-01-10T01:25:07Z
---

# Consider using the system SSL library, i.e., OpenSSL instead of `rusttls` / `ring`

---

_Issue opened by @marco-cloudflare on 2025-02-18 12:02_

### Summary

I thought that the native-tls flag would enable the native-ssl client but after close inspection, which is openssl in Linux, but, counter-intuitively, it actually just enables loading native certs using rustls and by extension ring, which has limitations e.g.: https://github.com/briansmith/ring/pull/1631

See https://github.com/astral-sh/uv/blob/929e7c3ad96ff6b14aeb60527e6a4526ed24ec43/crates/uv-client/src/base_client.rs#L286C13-L286C59

This might the reason for problems like #9243 

It seems that the reasoning behind this is for musl static compilation: #234 so I imagine that dynamic linking openssl is out of question. Another option could be enabling native-tls-vendored from reqwest which will static link openssl (it could be enabled only on musl builds, for others use native-tls) and have the uv's native-tls flag switch between reqwest use_rustls_tls() and use_native_tls()


### Platform

Linux, but should affect any

### Version

master branch

### Python version

_No response_

---

_Label `bug` added by @marco-cloudflare on 2025-02-18 12:02_

---

_Comment by @zanieb on 2025-02-18 14:25_

I'm a little hesitant to call this a bug. It looks like more of a request to dynamically link OpenSSL to work around unimplemented features in Ring?

---

_Comment by @marco-cloudflare on 2025-02-18 15:29_

> I'm a little hesitant to call this a bug. It looks like more of a request to dynamically link OpenSSL to work around unimplemented features in Ring?

I guess you're right, the category and title could be better. Now looking at the documentation, it does say that "Whether to load TLS certificates from the platform's native certificate store" with no mention that it's/it's not the system's native ssl lib.


---

_Label `bug` removed by @zanieb on 2025-02-18 15:32_

---

_Label `enhancement` added by @zanieb on 2025-02-18 15:32_

---

_Renamed from "Native-tls support is limited" to "Consider using the system SSL library, i.e., OpenSSL instead of `rusttls` / `ring`" by @zanieb on 2025-02-18 15:32_

---

_Label `compatibility` added by @zanieb on 2025-02-18 15:32_

---
