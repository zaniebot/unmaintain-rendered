---
number: 11747
title: Strange behavior with uv 0.5.24 and OpenSSL 3.2.2 in AlmaLinux 9.5
type: issue
state: closed
author: lskbr
labels:
  - bug
assignees: []
created_at: 2025-02-24T16:53:00Z
updated_at: 2025-02-25T15:37:11Z
url: https://github.com/astral-sh/uv/issues/11747
synced_at: 2026-01-07T13:12:18-06:00
---

# Strange behavior with uv 0.5.24 and OpenSSL 3.2.2 in AlmaLinux 9.5

---

_Issue opened by @lskbr on 2025-02-24 16:53_

### Summary

Hi, I spent my day banging my head with this issue :-D May be trivial.

I have a docker setup that runs fine without custom index urls when run from ADO (Microsoft Azure), but that didn't work when used internally with a custom CA.
The enterprise network uses a custom certificate authority (with ZScaler), so I'm used to install certificates on Ubuntu (docker files, etc).
Now, we are moving to build our packages using manylinux docker image that uses AlmaLinux 9.5. I followed the normal procedure to install the custom certificate in AlmaLinux:
```
# cp CUSTOM.crt /etc/pki/ca-trust/source/anchors/
# update-ca-trust
```
The certificate is installed and I can find it in the certificate chain.

PAT stands for Personal Access Token - it is used for authentication with Azure.
ENTERPRISE and project are the masked names of my ado repo.

You need a custom artifact repository URL :-(

Minimum example:
```
$ docker run --rm -t -i quay.io/pypa/manylinux_2_34_x86_64:latest
[root@ef8d6a7dfb2d /]# openssl version
OpenSSL 3.2.2 4 Jun 2024 (Library: OpenSSL 3.2.2 4 Jun 2024)
[root@ef8d6a7dfb2d /]# export UV_NATIVE_TLS=true
[root@ef8d6a7dfb2d /]# export PAT=PAT
[root@ef8d6a7dfb2d /]# export UV_EXTRA_INDEX_URL="https://${PAT}@pkgs.dev.azure.com/ENTERPRISE/_packaging/maths/pypi/simple/  https://${PAT}@pkgs.dev.azure.com/ENTERPRISE/project/_packaging/project/pypi/simple/"
uv pip install -n --reinstall colorconsole --system --allow-insecure-host *.core.windows.net
```
uv reports a certificate error (self-signed certificate in the chain). The traces are attached.
This error is a bit strange. For example, uv manages to connect to ADO pkgs, get the version of the package to install and its download location. It fails when connecting to the redirected server.

Using wget, I can download the file in the same docker image, so it is not blocked and the custom certificate seems to be working. The same URL with curl does not work, what makes me think it is a difference introduced by OpenSSL 3.2.2.

uv also works without the UV_EXTRA_INDEX_URL, it installs public packages without any problem. What I think is strange is that it manages to connect to ADO when I use the UV_EXTRA_INDEX_URL, but not to the Microsoft server (*.core.windows.net).

The allow-insecure-host also does not work with a wild card; I think this is for security purposes, right? The problem with ADO is that they have many servers with different names :-D

[uvlog.txt](https://github.com/user-attachments/files/18946767/uvlog.txt)

### Platform

Docker image with AlmaLinux 9.5 Manylinux 2.34

### Version

uv 0.5.24

### Python version

Python 3.12, Python 3.11, Python 3.9

---

_Label `bug` added by @lskbr on 2025-02-24 16:53_

---

_Comment by @samypr100 on 2025-02-25 02:02_

Does it work in older or newer uv versions?
Are you including the full certificate chain on your `CUSTOM.crt` (root cert& intermediaries certs)?

---

_Comment by @lskbr on 2025-02-25 11:00_

I think so; it is the same certificate chain I use on Ubuntu 22.04 and 24.04.
I did more tests this morning to retest the configuration. I checked the certificate regarding requirements of OpenSSL 3.3.2, and it is ok. It uses SHA256, it is 2048 bits.
uv works fine without the extra indexes. The problem seems to happen when it is redirected from the artifact repository to the download server. It is able to contact the artifact repository, get versions and dependencies. But it fails when trying to download.

I tested with uv 0.5.4 and 0.6.3.

I also tested with SSL_CERT_FILE pointing to the PEM, no success.

wget works fine without any extra configurations. curl works after using the same file as I passed to SSL_CERT_FILE in a --cacert option. So the connection itself is not blocked.

---

_Comment by @lskbr on 2025-02-25 15:36_

I found the problem. ManyLinux sets the SSL_CERT_FILE variable to /opt/_internal/certs.pem
The problem is solved by configuring the PAT + EXTRA_INDEX_URLS and unsettling SSL_CERT_FILE.
Another solution is to:
echo local_certificate.pem >> /opt/_internal/certs.pem

I was blindfolded by my script, paying attention to what I changed before checking the initial state of the docker image.
I got my error doing a strace in uv and its subprocess. The certificate store was never opened and the /opt/_internal/certs.pem showed up all the time.
Setting SSL_CERT_FILE to my certificate didn't solve the problem because only my certificate was there, not the other root CAs.

wget was working because it ignores the SSL_CERT_FILE var.

Why ManyLinux is important? Because it is used to build binary wheels. So openssl 3.2.2 is also innocent.

---

_Closed by @lskbr on 2025-02-25 15:37_

---
