```yaml
number: 2043
title: "uv pip install error: No such host is known. (os error 11001)"
type: issue
state: closed
author: contang0
labels:
  - question
  - network
assignees: []
created_at: 2024-02-28T13:30:44Z
updated_at: 2024-03-12T02:41:23Z
url: https://github.com/astral-sh/uv/issues/2043
synced_at: 2026-01-10T05:40:32Z
```

# uv pip install error: No such host is known. (os error 11001)

---

_Issue opened by @contang0 on 2024-02-28 13:30_

I am trying to use uv in a corporate environment and unfortunately packages fail to install. I get the following error:

```
error: error sending request for url (https://pypi.org/simple/networkx/): error trying to connect: dns error: No such host is known. (os error 11001)
  Caused by: error trying to connect: dns error: No such host is known. (os error 11001)
  Caused by: dns error: No such host is known. (os error 11001)
```

* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.

uv pip install networkx

* The current uv platform.

Windows 11

* The current uv version (`uv --version`).

uv 0.1.11 (32e5cacdd 2024-02-26)




---

_Comment by @konstin on 2024-02-28 15:09_

Do you know some details about your proxy setup?

---

_Comment by @contang0 on 2024-02-28 15:18_

Hardly.. we're a big organisation. I know that we're using certificates to bypass a proxy, but not much more.

---

_Label `question` added by @zanieb on 2024-02-28 18:27_

---

_Label `network` added by @zanieb on 2024-02-28 18:27_

---

_Comment by @samypr100 on 2024-02-28 23:43_

> I know that we're using certificates to bypass a proxy

What's you pip.conf file like?

Do you configure a custom CA certificate like `pip config set global.cert /path/to/your/ca-bundle.crt`
or do you set some environment variables like `REQUESTS_CA_BUNDLE` or anything like described in https://pip.pypa.io/en/stable/topics/https-certificates/#using-a-specific-certificate-store

---

_Comment by @samypr100 on 2024-02-28 23:51_

@konstin we can probably add support to allow users to specify their own CA bundles for `uv-client` like pip allows?

Maybe `UV_CERT` or `UV_CA_BUNDLE` to be similar to of `PIP_CERT`? Which would mean it reads it in and adds it to the [reqwest client](https://docs.rs/reqwest/latest/reqwest/struct.ClientBuilder.html#method.add_root_certificate).

Also worth considering supporting pip.conf eventually.

---

_Comment by @zanieb on 2024-02-29 02:37_

You can use `SSL_CERT_FILE` since we're using [`rustls-native-certs`](https://github.com/rustls/rustls-native-certs)

There's a `pip.conf` issue at #1404 

---

_Comment by @contang0 on 2024-02-29 10:25_

I had a look at my pip.ini (which is, I believe, the Windows alternative of pip.conf). It actually doesn't have any configuration for certs, just URLs to our Artifactory server that I think hosts a copy of pypi. We use our own copy for security reasons.

So I believe I would need a way to point uv to our local artifactory server rather than looking up pypi.

```
[global]
# pypi-remote is a local mirror of https://files.pythonhosted.org
index-url = https://artifactory.url/api/pypi/pypi-remote/simple
# pypi-local is a repository for packages developed at our organisation
extra-index-url = https:///artifactory.url/artifactory/api/pypi/pypi-local/simple

[search]
index = https://artifactory.url/artifactory/api/pypi/pypi-remote/
```



---

_Comment by @zanieb on 2024-02-29 16:25_

You can set the `UV_INDEX_URL` and `UV_EXTRA_INDEX_URL` environment variables or use the corresponding CLI flags.

---

_Comment by @charliermarsh on 2024-03-12 02:41_

Gonna tentatively close this out, since the thread suggests this is solved by the env vars.

---

_Closed by @charliermarsh on 2024-03-12 02:41_

---
