```yaml
number: 609
title: Add tooling for recording and replaying PyPI interactions
type: pull_request
state: closed
author: zanieb
labels: []
assignees: []
base: main
head: zb/offlinepi
created_at: 2023-12-11T21:17:45Z
updated_at: 2024-01-18T17:51:12Z
url: https://github.com/astral-sh/uv/pull/609
synced_at: 2026-01-10T15:39:02Z
```

# Add tooling for recording and replaying PyPI interactions

---

_Pull request opened by @zanieb on 2023-12-11 21:17_

Uses mitmproxy to record and replay interactions with a PyPI server. For example:

```
./offlinepi record cargo test --features pypi -- --test-threads=1
```

Then run tests with replayed responses:

```
./offlinepi replay cargo test --features pypi
```

I tested this with my internet turned off, so it looks to be fully offline.

See the [README](https://github.com/astral-sh/puffin/blob/zb/offlinepi/scripts/offlinepi/README.md) for more details.

I want to figure out a good way to help people install the certificate as it's not trivial. I did:

```
sudo security add-trusted-cert -d -p ssl -p basic -k /Library/Keychains/System.keychain ~/.mitmproxy/mitmproxy-ca-cert.pem
```

See #615 for a more general solution.

I'm also not sure what the best way is to hook this into our test suite in practice. It'd be nice to have a command that recorded new responses automatically or just replayed if not needed, but it's unclear how that'd be done :) it seems best for others to play with this before we make it integral in any way.

---

_Review comment by @zanieb on `Cargo.toml`:63 on 2023-12-11 21:56_

I believe this is required because I registered the cert with my system.

---

_@zanieb reviewed on 2023-12-11 21:56_

---

_@zanieb reviewed on 2023-12-11 21:56_

---

_Review comment by @zanieb on `Cargo.toml`:63 on 2023-12-11 21:56_

Suggested in https://github.com/seanmonstar/reqwest/issues/1554

---

_@zanieb reviewed on 2023-12-11 22:03_

---

_Review comment by @zanieb on `Cargo.toml`:63 on 2023-12-11 22:03_

> rustls-tls: Enables TLS functionality provided by rustls. Equivalent to rustls-tls-webpki-roots.
> rustls-tls-webpki-roots: Enables TLS functionality provided by rustls, while using root certificates from the webpki-roots crate.
> rustls-tls-native-roots: Enables TLS functionality provided by rustls, while using root certificates from the rustls-native-certs crate.
[source](https://docs.rs/reqwest/latest/reqwest/#optional-features)

Hm...

---

_@zanieb reviewed on 2023-12-11 22:04_

---

_Review comment by @zanieb on `Cargo.toml`:63 on 2023-12-11 22:04_

Pros / cons at https://github.com/rustls/rustls-native-certs#should-i-use-this-or-webpki-roots

---

_Comment by @zanieb on 2023-12-12 05:17_

Oh also I needed to install from GitHub to get things working initially, I should verify and document what version we require.



---

_@zanieb reviewed on 2023-12-12 16:59_

---

_Review comment by @zanieb on `Cargo.toml`:63 on 2023-12-12 16:59_

Related https://github.com/encode/httpx/issues/302

---

_Comment by @zanieb on 2023-12-12 17:39_

A HAR file for our test suite is about 10 MB e.g. https://gist.github.com/zanieb/fb1d4f03176be020a2d0912476d19a1c

---

_Comment by @zanieb on 2023-12-14 18:52_

I'll revert my cert changes in favor of #615 if possible and document usage.

---

_Closed by @zanieb on 2024-01-18 17:51_

---
