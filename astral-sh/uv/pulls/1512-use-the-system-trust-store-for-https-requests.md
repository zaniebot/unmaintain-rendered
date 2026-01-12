```yaml
number: 1512
title: Use the system trust store for HTTPS requests
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
  - registry
assignees: []
merged: true
base: main
head: zb/certs
created_at: 2024-02-16T16:40:01Z
updated_at: 2024-02-27T16:11:04Z
url: https://github.com/astral-sh/uv/pull/1512
synced_at: 2026-01-12T16:04:38Z
```

# Use the system trust store for HTTPS requests

---

_@zanieb_

Closes #1474 

Using the `rustls-tls-native-roots` feature

> `rustls-tls`: Enables TLS functionality provided by rustls. Equivalent to rustls-tls-webpki-roots.
>
> `rustls-tls-webpki-roots`: Enables TLS functionality provided by rustls, while using root certificates from the webpki-roots crate.
>
> `rustls-tls-native-roots`: Enables TLS functionality provided by rustls, while using root certificates from the rustls-native-certs crate.

Additional context:

- https://github.com/seanmonstar/reqwest/issues/1554
- https://github.com/encode/httpx/issues/302
- [Should I use the native certs or webpki-roots?](https://github.com/rustls/rustls-native-certs#should-i-use-this-or-webpki-roots)

Prior discussion at https://github.com/astral-sh/uv/pull/609

---

_Label `enhancement` added by @zanieb on 2024-02-16 16:45_

---

_Label `index` added by @zanieb on 2024-02-16 16:45_

---

_Marked ready for review by @zanieb on 2024-02-16 16:52_

---

_Comment by @zanieb on 2024-02-16 16:54_

The major downsides are:

> - The OS update system may, in fact, be quite poor at keeping the root certificates up-to-date if it is disabled or out-of-support.
> - The quality of the ca-certificates package on debian-based Linux distributions is poor. At the time of writing, this ships many certificates not included in the Mozilla set, either because they [failed an audit and were withdrawn](https://bugzilla.mozilla.org/show_bug.cgi?id=1448506) or [were removed for mississuance](https://bugzilla.mozilla.org/show_bug.cgi?id=1552374).

---

_Review requested from @charliermarsh by @zanieb on 2024-02-16 17:04_

---

_Review requested from @BurntSushi by @zanieb on 2024-02-16 17:04_

---

_@BurntSushi approved on 2024-02-16 19:07_

Seems okay to me. In particular, it seems like this is the recommended approach from [rustls](https://github.com/rustls).

---

_Merged by @BurntSushi on 2024-02-16 19:07_

---

_Closed by @BurntSushi on 2024-02-16 19:07_

---

_Branch deleted on 2024-02-16 19:07_

---

_Comment by @notatallshaw on 2024-02-16 20:08_

FYI, Pip is moving to this model via [truststore](https://github.com/sethmlarson/truststore): https://pip.pypa.io/en/stable/topics/https-certificates/

My experience is it is massively helpful in large organization environments.

---

_Comment by @carlosjourdan on 2024-02-27 16:05_

Corporate network with ssl inspection firewall, custom ca on every site. Root certificate is trusted by windows, and environment variables REQUESTS_CA_BUNDLE and SSL_CERT_FILE are setup. Python requests work fine. Pip install as well.  uv fails with error below.

```
error: error sending request for url (https://pypi.org/simple/zeep/): error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: invalid peer certificate: UnknownIssuer
```

---

_Comment by @zanieb on 2024-02-27 16:10_

Hi @carlosjourdan would you mind opening a new issue?


---

_Comment by @carlosjourdan on 2024-02-27 16:10_

Just did

---
