```yaml
number: 1474
title: How to run uv behind a corporate proxy?
type: issue
state: closed
author: NiklasRosenstein
labels:
  - question
  - network
assignees: []
created_at: 2024-02-16T10:20:58Z
updated_at: 2025-11-21T11:17:09Z
url: https://github.com/astral-sh/uv/issues/1474
synced_at: 2026-01-10T03:23:52Z
```

# How to run uv behind a corporate proxy?

---

_Issue opened by @NiklasRosenstein on 2024-02-16 10:20_

Our corporate proxy inspects traffic and thus inserts its own certificate that has to be configured to trust across applications on the system. I do this by setting `REQUESTS_CA_BUNDLE` and `SSL_CERT_FILE`, etc. It seems that `uv` doesn't respect either of those. Is there another environment variable that it takes into account?

```
error: error sending request for url (https://pypi.org/simple/protobuf/): error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: invalid peer certificate: UnknownIssuer
```


---

_Comment by @nevoodoo on 2024-02-16 13:28_

Run into the same problem here, both those variables are not respected by `uv` but works fine with `pip`, `pipenv`

EDIT: I see the discussion is already open in #1339 

---

_Comment by @kvelicka on 2024-02-16 15:44_

#1339 seems like a near-duplicate but I think it's important to distinguish "treat this host as trusted" and "verify the trustworthiness of this host, but using a user-specified certificate". For a corporate environment the latter option is much prefereable as we'd rather not compromise on security. Having said that, resolving #1339 would allow corporate users to at least _try_ `uv` in earnest so it would be some progress!

---

_Comment by @j-baker on 2024-02-16 15:56_

This might have a much simpler solution - basically, if you changed your `reqwest` feature flag from `rustls-tls` (which means `rustls-tls-webpki-roots`) to `rustls-tls-native-roots`, it'd use the operating system's truststore (which corporate certs are typically added to), as well as using `SSL_CERT_FILE` and other standard config mechanisms.

https://github.com/seanmonstar/reqwest/blob/master/Cargo.toml#L44
https://github.com/rustls/rustls-native-certs


---

_Comment by @zanieb on 2024-02-16 16:38_

I actually made this exact change in https://github.com/astral-sh/uv/pull/609 but it didn't get merged, we'll reconsider.

---

_Comment by @c3pmark on 2024-02-16 16:50_

This would be very useful for me too.  We don't have a proxy that intercepts HTTPS, but we do use an internal index with a certificate signed by our internal CA.  Having to specify `REQUESTS_CA_BUNDLE` everywhere is a huge pain point with `pip`, so if the system trust store could be used that would save a ton of hassle.

---

_Closed by @BurntSushi on 2024-02-16 19:07_

---

_Comment by @carlosjourdan on 2024-02-27 16:05_

I'm still facing the same issue on version 0.1.11

Corporate network with ssl inspection firewall, custom ca on every site. Root certificate is trusted by windows, and environment variables REQUESTS_CA_BUNDLE and SSL_CERT_FILE are setup. Python requests work fine. Pip install as well.  uv fails with error below.

```
error: error sending request for url (https://pypi.org/simple/zeep/): error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: error trying to connect: invalid peer certificate: UnknownIssuer
  Caused by: invalid peer certificate: UnknownIssuer
```



---

_Label `network` added by @zanieb on 2024-02-28 18:28_

---

_Label `question` added by @zanieb on 2024-02-28 18:28_

---

_Comment by @dorschw on 2024-08-27 13:52_

in WSL, running [windows-certs-2-wsl](https://github.com/bayaro/windows-certs-2-wsl), `update-ca-certificates`, then 
```bash
export SSL_CERT_DIR=/etc/ssl/certs
export REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
```
did the trick. 

---

_Comment by @a087861 on 2024-09-06 12:52_

Following as I am in a similar boat :-/

On mac with apple silicon behind proxy.  Tried running `uv venv -p 3.8` which resulted in an 'invalid peer certificate' error.  I think this is due to the same thing that affects others in this discussion but certs aren't my forte so I may be wrong.

---

_Comment by @zanieb on 2024-09-06 13:11_

@a087861 try using the `--native-tls` flag

---

_Comment by @a087861 on 2024-09-06 13:51_

@zanieb that worked like a charm - thanks!

---

_Comment by @Gr3at on 2024-12-05 08:59_

> [@a087861](https://github.com/a087861) try using the `--native-tls` flag

### Concern
this might work for linux based systems (including wsl) but in Windows it greatly depends on several preconditions (such as the selected shell).

### How i bypassed the issue
In my case i was able to install packages using the following on Windows 10 using git-bash as my shell of choice.

```bash
export HTTPS_PROXY=<company-proxy-here>
uv add --allow-insecure-host pypi.org --allow-insecure-host files.pythonhosted.org <package_name>
```

### Suggested Permanent Solution
IMO a great flag addition similar to the `--native-tls` would be to introduce a `--custom-ca-file`.
This would allow to specifically define what certs needs to be utilized to make any `uv` requests.

Not sure how difficult this is to add.
I would like to be able to do something like the following:

```bash
export HTTPS_PROXY=<company-proxy-here>
uv add --custom-ca-file <path to company internal root ca cert or bundle of certs> <package_name>
```


## Update

As @zanieb mentioned there is already an env variable in place to provide the desired functionality.
So now the following works like a charm

```bash
export HTTPS_PROXY=<company-proxy-here>
export SSL_CERT_FILE=<path to company internal root ca cert or bundle of certs>
uv add <package_name>
```
Thanks @zanieb for pointing this out.

---

_Comment by @zanieb on 2024-12-05 15:11_

You can use the standard [`SSL_CERT_FILE` variable](https://docs.astral.sh/uv/configuration/environment/#ssl_cert_file)

---

_Comment by @duygu-sahin on 2025-08-07 04:48_

For those on Windows, here’s the equivalent workaround that worked for me: 

set HTTPS_PROXY=<company-proxy-here>
uv pip install --allow-insecure-host pypi.org --allow-insecure-host files.pythonhosted.org <package_name>


---

_Comment by @oguz-hanoglu on 2025-10-24 11:05_

For github related downloads like new python installs
`export UV_INSECURE_HOST="github.com raw.githubusercontent.com objects.githubusercontent.com"`

---

_Comment by @oguz-hanoglu on 2025-10-24 11:59_

export UV_NATIVE_TLS=true

---

_Comment by @cloudquestor on 2025-11-21 11:17_

> For github related downloads like new python installs `export UV_INSECURE_HOST="github.com raw.githubusercontent.com objects.githubusercontent.com"`

This worked beautifully.

---
