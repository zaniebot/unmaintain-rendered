```yaml
number: 3626
title: mTLS Support?
type: issue
state: closed
author: SystemCoder99
labels:
  - enhancement
  - help wanted
  - configuration
assignees: []
created_at: 2024-05-16T15:08:55Z
updated_at: 2024-06-12T13:15:22Z
url: https://github.com/astral-sh/uv/issues/3626
synced_at: 2026-01-12T15:58:45Z
```

# mTLS Support?

---

_@SystemCoder99_

Now that uv.toml is implemented and we can set the native-tls flag, will there be support for mTLS? 
Will we be able to pass a client_cert file path through the uv.toml so our developers and their package repos can authenticate each other?
We rely heavily on this feature in pip, it would be great to see UV have some kind of implementation too!

---

_Comment by @zanieb on 2024-05-17 02:01_

Can you say more about how you're using this in pip? Perhaps link to an example or the relevant part of the pip implementation?

It sounds reasonable to configure certificates via the persistent config file. We do support `SSL_CERT_FILE` already.

---

_Comment by @SystemCoder99 on 2024-05-17 12:10_

So for pip, we use the "cert" and "client_cert" items in our pip.conf, "cert" to pass the path to our trust store, and "client_cert" to pass the path to the file containing both our client certificate and it's key. [Pip's cert and client-cert options](https://pip.pypa.io/en/stable/cli/pip/#cmdoption-client-cert). Our pip.conf currently looks something like this:

> [global]
> index-url=https://package.index.one.url
> extra-index-url=https://package.index.two.url
> cert=/path/to/ca-chain.crt
> client_cert = /path/to/client_cert/bundle.pem

When a developer only needs to use TLS, we only pass the "cert" item, and it authenticates our jfrog index - which is what I see happens when we use UV with native-tls. But when we need to use mTLS, the "client_cert" item is ALSO added into the pip.conf and passed in the index request. 

My understanding from the UV docs is SSL_CERT_FILE is what is used to point directly to our trust store/what we pass in the "cert" item, INSTEAD of native-tls, right? We need something where we can use the native-tls, while also passing the client-cert bundle as well.
[Pip's _build_session()](https://github.com/pypa/pip/blob/612515d2e0a6ff8676c139c096a45bc28b3456f4/src/pip/_internal/cli/index_command.py#L78) and [Pip's TLS handshake()](https://github.com/pypa/pip/blob/612515d2e0a6ff8676c139c096a45bc28b3456f4/src/pip/_vendor/urllib3/contrib/securetransport.py#L544) both implement the cert and client_cert options, if that helps at all

---

_Label `enhancement` added by @zanieb on 2024-05-17 13:18_

---

_Label `configuration` added by @zanieb on 2024-05-17 13:18_

---

_Comment by @zanieb on 2024-05-17 13:20_

Thanks for the additional details. This seems reasonable but I'd have to start working on an implementation to understand if it makes sense.

The `SSL_CERT_FILE` loading comes from https://github.com/rustls/rustls-native-certs but I _think_ that's their behavior.

---

_Comment by @alkatar21 on 2024-06-04 16:51_

We also need the `cert` option. I think our problem is simmilar. Currently this option is used for pip to access our index and I have not found an alternative way with `uv` to access our index.

---

_Comment by @zanieb on 2024-06-04 18:22_

I'm guessing this looks something like https://github.com/camelop/rust-mtls-example/blob/1379379eb08c63f22f9b4eae080c9380e29ffd44/src/main.rs#L22-L71

If anyone is interested in putting up a pull request I'm happy to review

---

_Label `help wanted` added by @zanieb on 2024-06-04 18:22_

---

_Closed by @zanieb on 2024-06-11 01:11_

---

_Comment by @zanieb on 2024-06-12 13:15_

@SystemCoder99 this should be available in the latest release.

---
