---
number: 2020
title: Still not able to run uv on corporate network with custom root certificates (uv 0.0.11)
type: issue
state: closed
author: carlosjourdan
labels:
  - question
  - network
assignees: []
created_at: 2024-02-27T16:13:19Z
updated_at: 2024-09-02T09:35:45Z
url: https://github.com/astral-sh/uv/issues/2020
synced_at: 2026-01-10T01:23:10Z
---

# Still not able to run uv on corporate network with custom root certificates (uv 0.0.11)

---

_Issue opened by @carlosjourdan on 2024-02-27 16:13_

https://github.com/astral-sh/uv/issues/1474 was not fixed by https://github.com/astral-sh/uv/pull/1512 for me on v 0.1.11

Corporate network with ssl inspection firewall, custom ca on every site. Root certificate is trusted by windows, and environment variables REQUESTS_CA_BUNDLE and SSL_CERT_FILE are setup. Python requests work fine. Pip install as well.  uv fails with error below.

```
error: error sending request for url (https://pypi.org/simple/zeep/): error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: invalid peer certificate: UnknownIssuer
```

---

_Comment by @zanieb on 2024-02-28 02:50_

We do not respect the `REQUESTS_CA_BUNDLE` variable, what do you set that to? We do respect `SSL_CERT_FILE` but if your certificate are registered with the Windows certificate store then that should not be necessary.

It may be worth seeing if there are any upstream [`rustls-native-certs`](https://github.com/rustls/rustls-native-certs) issues that apply to your situation and confirm that your certificates are available through the [`schannel`](https://docs.rs/schannel/0.1.23/schannel/cert_store/struct.CertStore.html) API.

All of the certificate loading and validation is done outside of `uv`, generally these issues are a problem with the certificate configuration or an upstream bug.

---

_Label `question` added by @zanieb on 2024-02-28 02:50_

---

_Comment by @carlosjourdan on 2024-02-28 11:41_

Thanks for the info @zanieb 

I'm unable to replicate the issue now, it was probably something transient within the host environment

---

_Closed by @carlosjourdan on 2024-02-28 11:41_

---

_Label `network` added by @zanieb on 2024-02-28 18:28_

---

_Referenced in [astral-sh/uv#4077](../../astral-sh/uv/issues/4077.md) on 2024-06-07 03:00_

---

_Comment by @LordFckHelmchen on 2024-09-02 09:35_

For me, the `--native-tls` option did the trick

---
