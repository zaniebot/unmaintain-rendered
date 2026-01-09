---
number: 5448
title: UV 0.2.29 Permission error
type: issue
state: open
author: lskbr
labels:
  - bug
assignees: []
created_at: 2024-07-25T15:25:42Z
updated_at: 2024-08-01T13:00:00Z
url: https://github.com/astral-sh/uv/issues/5448
synced_at: 2026-01-07T13:12:17-06:00
---

# UV 0.2.29 Permission error

---

_Issue opened by @lskbr on 2024-07-25 15:25_

I'm using uv with tox to build multiple versions of the same packet on Ubuntu Linux 22.04 LTS.

The tox.ini works well with uv 0.2.28, but starts to fail with  0.2.29.
Changing back to uv 0.2.28 fixes the problem:

The command that causes the error:
any uv pip install that tries to download a new file.

The error happens when uv is called inside tox and also in the command line with the virtual environment active.

The error happens when UV decides to download a package. I have the same error when the environment is empty or when it already has some installed packages.

Environment
```
$ uv --version
uv 0.2.29
$ python --version
Python 3.11.7
$ uname -a
Linux nilodev 5.15.0-116-generic #126-Ubuntu SMP Mon Jul 1 10:14:24 UTC 2024 x86_64 x86_64 x86_64 GNU/Linux
```

```
$ export RUST_BACKTRACE=full
$ uv pip install rich
thread 'main' panicked at crates/uv-client/src/base_client.rs:178:33:
Failed to build HTTP client: reqwest::Error { kind: Builder, source: Os { code: 13, kind: PermissionDenied, message: "Permission denied" } }
stack backtrace:
   0:     0x55c400517c45 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h1e1a1972118942ad
   1:     0x55c4005483eb - core::fmt::write::hc090a2ffd6b28c4a
   2:     0x55c40051358f - std::io::Write::write_fmt::h8898bac6ff039a23
   3:     0x55c400517a1e - std::sys_common::backtrace::print::ha96650907276675e
   4:     0x55c400519519 - std::panicking::default_hook::{{closure}}::h215c2a0a8346e0e0
   5:     0x55c40051925d - std::panicking::default_hook::h207342be97478370
   6:     0x55c4005199b3 - std::panicking::rust_panic_with_hook::hac8bdceee1e4fe2c
   7:     0x55c400519894 - std::panicking::begin_panic_handler::{{closure}}::h00d785e82757ce3c
   8:     0x55c400518109 - std::sys_common::backtrace::__rust_end_short_backtrace::h1628d957bcd06996
   9:     0x55c4005195c7 - rust_begin_unwind
  10:     0x55c3ff1a8783 - core::panicking::panic_fmt::hdc63834ffaaefae5
  11:     0x55c3ff1a8c36 - core::result::unwrap_failed::h82b551e0ff2b2176
  12:     0x55c3fffc2c76 - uv_client::base_client::BaseClientBuilder::build::hc4066069826b73b6
  13:     0x55c3fffd2a15 - uv_client::registry_client::RegistryClientBuilder::build::h09b56654473e14c4
  14:     0x55c3ff389682 - uv::commands::pip::install::pip_install::{{closure}}::hffe6d854e0c66324
  15:     0x55c3ff366092 - uv::run::{{closure}}::{{closure}}::hbd05a1e2e322c758
  16:     0x55c3ff40d509 - <core::pin::Pin<P> as core::future::future::Future>::poll::ha12a028225440d46
  17:     0x55c3ff576985 - tokio::runtime::scheduler::current_thread::Context::enter::h61c98b15c073ae39
  18:     0x55c3ff76d19e - tokio::runtime::context::set_scheduler::hadeae89fb860aac6
  19:     0x55c3ff576366 - tokio::runtime::runtime::Runtime::block_on::h6bb9c06934456063
  20:     0x55c3ff522a3b - uv::main::habe90a2d9b756a52
  21:     0x55c3ff74c199 - uv::main::h6766109f7db7d026
  22:     0x55c3ff5240b3 - std::sys_common::backtrace::__rust_begin_short_backtrace::h3c30aaf928253bef
  23:     0x55c3ff6ba379 - std::rt::lang_start::{{closure}}::h9bf9c3026104f20c
  24:     0x55c400508860 - std::rt::lang_start_internal::h3ed4fe7b2f419135
  25:     0x55c3ff74c1c5 - main
  26:     0x7f690c69cd90 - <unknown>
  27:     0x7f690c69ce40 - __libc_start_main
  28:     0x55c3ff1a8ee9 - <unknown>
  29:                0x0 - <unknown>
```
The package name makes no difference, having the same errors with numpy and pandas.



---

_Label `bug` added by @zanieb on 2024-07-25 15:31_

---

_Comment by @zanieb on 2024-07-25 15:32_

Thanks for the report! We will investigate. Are you using native TLS?

---

_Comment by @charliermarsh on 2024-07-25 15:39_

Can this be reproduced in a Docker image?

---

_Comment by @lskbr on 2024-07-25 15:53_

> Thanks for the report! We will investigate. Are you using native TLS?

Yes, I'm using native TLS, set in the env var.

---

_Comment by @lskbr on 2024-07-25 15:53_

> Can this be reproduced in a Docker image?
I can try and paste the DOCKERFILE here tomorrow.

---

_Comment by @zanieb on 2024-07-25 15:57_

This is probably an error reading the certificates from your system, presumably caused by a change in one of our dependencies. You may want to try `RUST_LOG=trace uv pip install rich -vvv`, it might show what cert is being read?

---

_Comment by @charliermarsh on 2024-07-25 16:29_

> You may want to try RUST_LOG=trace uv pip install rich -vvv, it might show what cert is being read?

This would be super useful, especially if we need to file an issue upstream.


---

_Comment by @charliermarsh on 2024-07-25 21:28_

This is probably https://github.com/rustls/rustls-native-certs/issues/116.

---

_Comment by @charliermarsh on 2024-07-25 21:29_

Apparently this is a sign of real problem though. The subsequent PR just surfaces more context in the error, so I suspect there is an actual permissions mismatch at play in your setup: https://github.com/rustls/rustls-native-certs/pull/117

---

_Comment by @lskbr on 2024-07-26 09:23_

Here you have the RUST_LOG=trace output:

```
 $ RUST_LOG=trace uv pip install rich -vvv
    0.000022s DEBUG uv uv 0.2.29
 uv_requirements::specification::from_source source=rich
    0.010689s DEBUG uv_python::discovery Searching for Python interpreter in system path
    0.010866s TRACE uv_python::interpreter Cached interpreter info for Python 3.11.7, skipping probing: venv/bin/python3
    0.010893s DEBUG uv_python::discovery Found `cpython-3.11.7-linux-x86_64-gnu` at `/home/nilo/projects/xxxxxx/git/data-types/venv/bin/python3` (active virtual environment)
    0.010908s DEBUG uv::commands::pip::install Using Python 3.11.7 environment at venv/bin/python3
    0.010967s TRACE uv_fs Checking lock for `venv`
    0.010981s DEBUG uv_fs Acquired lock for `venv`
    0.011330s DEBUG uv::commands::pip::install At least one requirement is not satisfied: rich
 uv_client::linehaul::linehaul
    0.012051s DEBUG uv_client::base_client Using request timeout of 30s
thread 'main' panicked at crates/uv-client/src/base_client.rs:178:33:
Failed to build HTTP client: reqwest::Error { kind: Builder, source: Os { code: 13, kind: PermissionDenied, message: "Permission denied" } }
stack backtrace:
   0:     0x564885bbac45 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h1e1a1972118942ad
   1:     0x564885beb3eb - core::fmt::write::hc090a2ffd6b28c4a
   2:     0x564885bb658f - std::io::Write::write_fmt::h8898bac6ff039a23
   3:     0x564885bbaa1e - std::sys_common::backtrace::print::ha96650907276675e
   4:     0x564885bbc519 - std::panicking::default_hook::{{closure}}::h215c2a0a8346e0e0
   5:     0x564885bbc25d - std::panicking::default_hook::h207342be97478370
   6:     0x564885bbc9b3 - std::panicking::rust_panic_with_hook::hac8bdceee1e4fe2c
   7:     0x564885bbc894 - std::panicking::begin_panic_handler::{{closure}}::h00d785e82757ce3c
   8:     0x564885bbb109 - std::sys_common::backtrace::__rust_end_short_backtrace::h1628d957bcd06996
   9:     0x564885bbc5c7 - rust_begin_unwind
  10:     0x56488484b783 - core::panicking::panic_fmt::hdc63834ffaaefae5
  11:     0x56488484bc36 - core::result::unwrap_failed::h82b551e0ff2b2176
  12:     0x564885665c76 - uv_client::base_client::BaseClientBuilder::build::hc4066069826b73b6
  13:     0x564885675a15 - uv_client::registry_client::RegistryClientBuilder::build::h09b56654473e14c4
  14:     0x564884a2c682 - uv::commands::pip::install::pip_install::{{closure}}::hffe6d854e0c66324
  15:     0x564884a09092 - uv::run::{{closure}}::{{closure}}::hbd05a1e2e322c758
  16:     0x564884ab0509 - <core::pin::Pin<P> as core::future::future::Future>::poll::ha12a028225440d46
  17:     0x564884c19985 - tokio::runtime::scheduler::current_thread::Context::enter::h61c98b15c073ae39
  18:     0x564884e1019e - tokio::runtime::context::set_scheduler::hadeae89fb860aac6
  19:     0x564884c19366 - tokio::runtime::runtime::Runtime::block_on::h6bb9c06934456063
  20:     0x564884bc5a3b - uv::main::habe90a2d9b756a52
  21:     0x564884def199 - uv::main::h6766109f7db7d026
  22:     0x564884bc70b3 - std::sys_common::backtrace::__rust_begin_short_backtrace::h3c30aaf928253bef
  23:     0x564884d5d379 - std::rt::lang_start::{{closure}}::h9bf9c3026104f20c
  24:     0x564885bab860 - std::rt::lang_start_internal::h3ed4fe7b2f419135
  25:     0x564884def1c5 - main
  26:     0x7fd9ad7d1d90 - <unknown>
  27:     0x7fd9ad7d1e40 - __libc_start_main
  28:     0x56488484bee9 - <unknown>
  29:                0x0 - <unknown>
````

---

_Comment by @lskbr on 2024-07-26 09:29_

I'm attaching the same output with uv 0.2.28 (same machine and env, with version specified). It still works.
[
[uv0.2.28.txt.zip](https://github.com/user-attachments/files/16389717/uv0.2.28.txt.zip)
](URL)
I'm using the same certificate on both, which I installed on Linux some time ago. No problems with 0.2.28.

---

_Comment by @lskbr on 2024-07-26 13:27_

The DOCKERFILE:
```
FROM ubuntu:22.04
RUN apt update -y
RUN apt install -y ca-certificates
COPY ROOT_CRT.crt /usr/local/share/ca-certificates/ROOT_CRT.crt
RUN /usr/sbin/update-ca-certificates
RUN apt install python3.11 python3.11-venv wget curl -y
RUN useradd -ms /bin/bash vuser
USER vuser
WORKDIR /home/vuser
ENV REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
ENV UV_NATIVE_TLS=1
ENV RUST_LOG=trace
ENV RUST_BACKTRACE=full
RUN python3.11 -m venv venv
RUN venv/bin/python -m pip install uv==0.2.29
```

The error:
```
$ docker build -t uvlinux .
$ docker run -t -i --rm uvlinux
vuser@3b85bfc1510b:~$ source venv/bin/activate
(venv) vuser@3b85bfc1510b:~$ uv pip install rich
DEBUG uv 0.2.29
DEBUG Searching for Python interpreter in system path
TRACE Querying interpreter executable at /home/vuser/venv/bin/python3
DEBUG Found `cpython-3.11.0-linux-x86_64-gnu` at `/home/vuser/venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.11.0rc1 environment at venv/bin/python3
TRACE Checking lock for `venv`
DEBUG Acquired lock for `venv`
DEBUG At least one requirement is not satisfied: rich
DEBUG Using request timeout of 30s
thread 'main' panicked at crates/uv-client/src/base_client.rs:178:33:
Failed to build HTTP client: reqwest::Error { kind: Builder, source: Os { code: 13, kind: PermissionDenied, message: "Permission denied" } }
stack backtrace:
   0:     0x55f82b5bdc45 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h1e1a1972118942ad
   1:     0x55f82b5ee3eb - core::fmt::write::hc090a2ffd6b28c4a
   2:     0x55f82b5b958f - std::io::Write::write_fmt::h8898bac6ff039a23
   3:     0x55f82b5bda1e - std::sys_common::backtrace::print::ha96650907276675e
   4:     0x55f82b5bf519 - std::panicking::default_hook::{{closure}}::h215c2a0a8346e0e0
   5:     0x55f82b5bf25d - std::panicking::default_hook::h207342be97478370
   6:     0x55f82b5bf9b3 - std::panicking::rust_panic_with_hook::hac8bdceee1e4fe2c
   7:     0x55f82b5bf894 - std::panicking::begin_panic_handler::{{closure}}::h00d785e82757ce3c
   8:     0x55f82b5be109 - std::sys_common::backtrace::__rust_end_short_backtrace::h1628d957bcd06996
   9:     0x55f82b5bf5c7 - rust_begin_unwind
  10:     0x55f82a24e783 - core::panicking::panic_fmt::hdc63834ffaaefae5
  11:     0x55f82a24ec36 - core::result::unwrap_failed::h82b551e0ff2b2176
  12:     0x55f82b068c76 - uv_client::base_client::BaseClientBuilder::build::hc4066069826b73b6
  13:     0x55f82b078a15 - uv_client::registry_client::RegistryClientBuilder::build::h09b56654473e14c4
  14:     0x55f82a42f682 - uv::commands::pip::install::pip_install::{{closure}}::hffe6d854e0c66324
  15:     0x55f82a40c092 - uv::run::{{closure}}::{{closure}}::hbd05a1e2e322c758
  16:     0x55f82a4b3509 - <core::pin::Pin<P> as core::future::future::Future>::poll::ha12a028225440d46
  17:     0x55f82a61c985 - tokio::runtime::scheduler::current_thread::Context::enter::h61c98b15c073ae39
  18:     0x55f82a81319e - tokio::runtime::context::set_scheduler::hadeae89fb860aac6
  19:     0x55f82a61c366 - tokio::runtime::runtime::Runtime::block_on::h6bb9c06934456063
  20:     0x55f82a5c8a3b - uv::main::habe90a2d9b756a52
  21:     0x55f82a7f2199 - uv::main::h6766109f7db7d026
  22:     0x55f82a5ca0b3 - std::sys_common::backtrace::__rust_begin_short_backtrace::h3c30aaf928253bef
  23:     0x55f82a760379 - std::rt::lang_start::{{closure}}::h9bf9c3026104f20c
  24:     0x55f82b5ae860 - std::rt::lang_start_internal::h3ed4fe7b2f419135
  25:     0x55f82a7f21c5 - main
  26:     0x7fcd050e9d90 - <unknown>
  27:     0x7fcd050e9e40 - __libc_start_main
  28:     0x55f82a24eee9 - <unknown>
  29:                0x0 - <unknown>
```

To confirm the certificate was well installed:

```
(venv) vuser@3b85bfc1510b:~$ curl https://google.com
<HTML><HEAD><meta http-equiv="content-type" content="text/html;charset=utf-8">
<TITLE>301 Moved</TITLE></HEAD><BODY>
<H1>301 Moved</H1>
The document has moved
<A HREF="https://www.google.com/">here</A>.
</BODY></HTML>
```

In another run, I removed the custom certificate (rebuild the image without the COPY), but curl fails of course:
```
vuser@d1598194b6a1:~$ curl https://google.com
curl: (60) SSL certificate problem: self-signed certificate in certificate chain
More details here: https://curl.se/docs/sslcerts.html
curl failed to verify the legitimacy of the server and therefore could not
establish a secure connection to it. To learn more about this situation and
how to fix it, please visit the web page mentioned above.
```

But uv installs rich fine:
(too big, attached as a file)


[uv0.2.29-install-with-non-trusted-certificate.txt](https://github.com/user-attachments/files/16392277/uv0.2.29-install-with-non-trusted-certificate.txt)

To confirm company CA is being added by the proxy, I got the certificate chain with `openssl s_client -connect google.com` (cropped):

```
$ openssl s_client -connect google.com:443
 Certificate chain
 0 s:CN = *.google.com
   i:C = EU, ST = Hauts de Seine, L = Paris, O = SZ Issuing CA, OU = SZ, CN = SZ Issuing CA Zscaler 01
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jul  1 06:35:43 2024 GMT; NotAfter: Sep 23 06:35:42 2024 GMT
 1 s:C = EU, ST = Hauts de Seine, L = Paris, O = SZ Issuing CA, OU = SZ, CN = SZ Issuing CA Zscaler 01
   i:C = EU, O = SZ, CN = SZ Intermediate CA 1
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: May 30 14:02:51 2017 GMT; NotAfter: May 30 14:12:51 2027 GMT
 2 s:C = EU, O = ISINFRA, CN = ISINFRA ROOT CA
   i:C = EU, O = ISINFRA, CN = ISINFRA ROOT CA
   a:PKEY: rsaEncryption, 2048 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jul 12 07:32:18 2016 GMT; NotAfter: Jul 12 07:42:18 2056 GMT
 3 s:C = EU, O = SZ, CN = SZ Intermediate CA 1
   i:C = EU, O = ISINFRA, CN = ISINFRA ROOT CA
   a:PKEY: rsaEncryption, 4096 (bit); sigalg: RSA-SHA256
   v:NotBefore: Jul 12 07:45:00 2016 GMT; NotAfter: Jul 12 07:55:00 2036 GMT
```
where ISINFRA is my self-signed certificate.

In the original Dockerfile, if I swap uv with version 0.2.28, it works fine.

0.2.28 also installs when the certificate is not added to the chain (accepts a self-signed certificate, even if not added to CA trust), but I think this is another issue.
The env variables I used are all in the Dockerfile.

---

_Comment by @charliermarsh on 2024-07-26 14:50_

Hmm, could you share the permissions on `ROOT_CRT.crt`? Like `ls -l`? I'm trying to reproduce myself.

---

_Comment by @lskbr on 2024-07-26 15:47_

Only root can read and write, no permission to all other users. The certificate is concatenated in a larger certificates file by ubuntu… I don’t think applications should have access to it directly, I maybe wrong.Envoyé de mon iPhoneLe 26 juil. 2024 à 16:50, Charlie Marsh ***@***.***> a écrit :﻿
Hmm, could you share the permissions on ROOT_CRT.crt? Like ls -l? I'm trying to reproduce myself.

—Reply to this email directly, view it on GitHub, or unsubscribe.You are receiving this because you authored the thread.Message ID: ***@***.***>

---

_Comment by @charliermarsh on 2024-07-26 15:49_

The `.crt` file is just a public key though, right?

---

_Comment by @lskbr on 2024-07-26 15:52_

Yes, just to add the ca to the chain.Envoyé de mon iPhoneLe 26 juil. 2024 à 17:49, Charlie Marsh ***@***.***> a écrit :﻿
The .crt file is just a public key though, right?

—Reply to this email directly, view it on GitHub, or unsubscribe.You are receiving this because you authored the thread.Message ID: ***@***.***>

---

_Comment by @samypr100 on 2024-07-27 00:00_

What's the actual linux permissions of the file?

Output should look like this
```
$ ls -l /path/to/ROOT_CRT.crt
-rw-r--r--  ...other metadata... ROOT_CRT.crt
```

Are the permissions `-rw-r--r--` or `-rw-------`?

Mainly for clarification, when you say `only root can read and write` do you mean its `-rw-------  root:root`?


---

_Comment by @samypr100 on 2024-07-27 00:37_

For what it's worth, I just tried it with a `-rw------- root root` CA bundle file under a different user and got the same backtrace. I get it on 0.2.28 as well as 0.2.30. Maybe it's a docker caching issue?

<details><summary>Trace Info</summary>
<p>

```
DEBUG uv 0.2.30
DEBUG Searching for Python interpreter in system path
TRACE Querying interpreter executable at /home/uv/.venv/bin/python3
DEBUG Found `cpython-3.12.4-linux-x86_64-gnu` at `/home/uv/.venv/bin/python3` (virtual environment)
DEBUG Using Python 3.12.4 environment at .venv/bin/python3
TRACE Checking lock for `.venv`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: flask
DEBUG Using request timeout of 30s
thread 'main' panicked at crates/uv-client/src/base_client.rs:178:33:
Failed to build HTTP client: reqwest::Error { kind: Builder, source: Os { code: 13, kind: PermissionDenied, message: "Permission denied" } }
stack backtrace:
   0:     0x556a84e726e5 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h1e1a1972118942ad
   1:     0x556a84ea2e8b - core::fmt::write::hc090a2ffd6b28c4a
   2:     0x556a84e6e02f - std::io::Write::write_fmt::h8898bac6ff039a23
   3:     0x556a84e724be - std::sys_common::backtrace::print::ha96650907276675e
   4:     0x556a84e73fb9 - std::panicking::default_hook::{{closure}}::h215c2a0a8346e0e0
   5:     0x556a84e73cfd - std::panicking::default_hook::h207342be97478370
   6:     0x556a84e74453 - std::panicking::rust_panic_with_hook::hac8bdceee1e4fe2c
   7:     0x556a84e74334 - std::panicking::begin_panic_handler::{{closure}}::h00d785e82757ce3c
   8:     0x556a84e72ba9 - std::sys_common::backtrace::__rust_end_short_backtrace::h1628d957bcd06996
   9:     0x556a84e74067 - rust_begin_unwind
  10:     0x556a83afa603 - core::panicking::panic_fmt::hdc63834ffaaefae5
  11:     0x556a83afaab6 - core::result::unwrap_failed::h82b551e0ff2b2176
  12:     0x556a8491bbe6 - uv_client::base_client::BaseClientBuilder::build::hf9452ebf93eb5c9c
  13:     0x556a8492b985 - uv_client::registry_client::RegistryClientBuilder::build::hc78f5b25152dc124
  14:     0x556a83cdd652 - uv::commands::pip::install::pip_install::{{closure}}::h351b868011709273
  15:     0x556a83cba532 - uv::run::{{closure}}::{{closure}}::hccd2d4da1bad7d73
  16:     0x556a83d5fcb9 - <core::pin::Pin<P> as core::future::future::Future>::poll::he22f133412905bcb
  17:     0x556a83ecad56 - tokio::runtime::runtime::Runtime::block_on::h3dfc693dcf47d351
  18:     0x556a83e7ccc3 - uv::main::h0e39d6aa198686e0
  19:     0x556a83f3b439 - uv::main::hfdc0b65da80c1fb9
  20:     0x556a83e7fc73 - std::sys_common::backtrace::__rust_begin_short_backtrace::h4c344b210e6c1d1d
  21:     0x556a84212d49 - std::rt::lang_start::{{closure}}::h5d4ffc2485b0fefc
  22:     0x556a84e63300 - std::rt::lang_start_internal::h3ed4fe7b2f419135
  23:     0x556a83f3b465 - main
  24:     0x7f25b4c72d90 - <unknown>
  25:     0x7f25b4c72e40 - __libc_start_main
  26:     0x556a83afad69 - <unknown>
  27:                0x0 - <unknown>
```

</p>
</details> 


Note, assuming `/usr/local/share/ca-certificates/ROOT_CRT.crt` is just a chain of public certficates (no private keys), it is okay to be readable by everyone the same way `/etc/ssl/certs/ca-certificates.crt` normally is.

Changing the dockerfile a bit might help

```Dockerfile
FROM ubuntu:22.04
RUN apt update -y
RUN apt install -y ca-certificates
COPY ROOT_CRT.crt /usr/local/share/ca-certificates/ROOT_CRT.crt
RUN chmod +r /usr/local/share/ca-certificates/ROOT_CRT.crt ######## DOES ADDING THIS LINE HELP?
RUN /usr/sbin/update-ca-certificates
RUN apt install python3.11 python3.11-venv wget curl -y
RUN useradd -ms /bin/bash vuser
USER vuser
WORKDIR /home/vuser
ENV REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
ENV UV_NATIVE_TLS=1
ENV RUST_LOG=trace
ENV RUST_BACKTRACE=full
RUN python3.11 -m venv venv
RUN venv/bin/python -m pip install uv==0.2.29
```


---

_Comment by @lskbr on 2024-07-27 08:12_

Thx. I’m on vacations and I won’t have access to the machine until Thursday. I will retest with the new permission.Any reason why uv is the only software to complain? and why it was working until 0.2.28?Regarding the other problem, the self signed certificate that is accepted with the ca installed. Should I open another ticket?Envoyé de mon iPhoneLe 27 juil. 2024 à 02:37, samypr100 ***@***.***> a écrit :﻿
For what it's worth, I just tried it with a -rw------- root root CA file under a different user and got the same backtrace. I get it on 0.2.28 as well as 0.2.30. Maybe it's a docker caching issue?
Trace Info

DEBUG uv 0.2.30
DEBUG Searching for Python interpreter in system path
TRACE Querying interpreter executable at /home/uv/.venv/bin/python3
DEBUG Found cpython-3.12.4-linux-x86_64-gnu at /home/uv/.venv/bin/python3 (virtual environment)
DEBUG Using Python 3.12.4 environment at .venv/bin/python3
TRACE Checking lock for .venv
DEBUG Acquired lock for .venv
DEBUG At least one requirement is not satisfied: flask
DEBUG Using request timeout of 30s
thread 'main' panicked at crates/uv-client/src/base_client.rs:178:33:
Failed to build HTTP client: reqwest::Error { kind: Builder, source: Os { code: 13, kind: PermissionDenied, message: "Permission denied" } }
stack backtrace:
0:     0x556a84e726e5 - <std::sys_common::backtrace::_print::DisplayBacktrace as core::fmt::Display>::fmt::h1e1a1972118942ad
1:     0x556a84ea2e8b - core::fmt::write::hc090a2ffd6b28c4a
2:     0x556a84e6e02f - std::io::Write::write_fmt::h8898bac6ff039a23
3:     0x556a84e724be - std::sys_common::backtrace::print::ha96650907276675e
4:     0x556a84e73fb9 - std::panicking::default_hook::{{closure}}::h215c2a0a8346e0e0
5:     0x556a84e73cfd - std::panicking::default_hook::h207342be97478370
6:     0x556a84e74453 - std::panicking::rust_panic_with_hook::hac8bdceee1e4fe2c
7:     0x556a84e74334 - std::panicking::begin_panic_handler::{{closure}}::h00d785e82757ce3c
8:     0x556a84e72ba9 - std::sys_common::backtrace::__rust_end_short_backtrace::h1628d957bcd06996
9:     0x556a84e74067 - rust_begin_unwind
10:     0x556a83afa603 - core::panicking::panic_fmt::hdc63834ffaaefae5
11:     0x556a83afaab6 - core::result::unwrap_failed::h82b551e0ff2b2176
12:     0x556a8491bbe6 - uv_client::base_client::BaseClientBuilder::build::hf9452ebf93eb5c9c
13:     0x556a8492b985 - uv_client::registry_client::RegistryClientBuilder::build::hc78f5b25152dc124
14:     0x556a83cdd652 - uv::commands::pip::install::pip_install::{{closure}}::h351b868011709273
15:     0x556a83cba532 - uv::run::{{closure}}::{{closure}}::hccd2d4da1bad7d73
16:     0x556a83d5fcb9 - <core::pin::Pin as core::future::future::Future>::poll::he22f133412905bcb
17:     0x556a83ecad56 - tokio::runtime::runtime::Runtime::block_on::h3dfc693dcf47d351
18:     0x556a83e7ccc3 - uv::main::h0e39d6aa198686e0
19:     0x556a83f3b439 - uv::main::hfdc0b65da80c1fb9
20:     0x556a83e7fc73 - std::sys_common::backtrace::__rust_begin_short_backtrace::h4c344b210e6c1d1d
21:     0x556a84212d49 - std::rt::lang_start::{{closure}}::h5d4ffc2485b0fefc
22:     0x556a84e63300 - std::rt::lang_start_internal::h3ed4fe7b2f419135
23:     0x556a83f3b465 - main
24:     0x7f25b4c72d90 - 
25:     0x7f25b4c72e40 - __libc_start_main
26:     0x556a83afad69 - 
27:                0x0 - 

 
Note, assuming /usr/local/share/ca-certificates/ROOT_CRT.crt is just a chain of public certficates (no private keys), it is okay to be readable by everyone the same way /etc/ssl/certs/ca-certificates.crt normally is.
Changing the dockerfile a bit might help
FROM ubuntu:22.04
RUN apt update -y
RUN apt install -y ca-certificates
COPY ROOT_CRT.crt /usr/local/share/ca-certificates/ROOT_CRT.crt
RUN chmod +r /usr/local/share/ca-certificates/ROOT_CRT.crt ######## DOES ADDING THIS LINE HELP?
RUN /usr/sbin/update-ca-certificates
RUN apt install python3.11 python3.11-venv wget curl -y
RUN useradd -ms /bin/bash vuser
USER vuser
WORKDIR /home/vuser
ENV REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
ENV UV_NATIVE_TLS=1
ENV RUST_LOG=trace
ENV RUST_BACKTRACE=full
RUN python3.11 -m venv venv
RUN venv/bin/python -m pip install uv==0.2.29

—Reply to this email directly, view it on GitHub, or unsubscribe.You are receiving this because you authored the thread.Message ID: ***@***.***>

---

_Comment by @charliermarsh on 2024-07-27 12:24_

> and why it was working until 0.2.28

Yes, as mentioned above there was a change in rustls-native-certs: https://github.com/rustls/rustls-native-certs/issues/116. The issue suggests that the library is correct to raise the error in this case though I'm not familiar enough with TLS to know if that's the case. I'll \cc @djc for visibility.

---

_Comment by @charliermarsh on 2024-07-27 17:29_

Somehow I'm not able to reproduce with that Dockerfile.

---

_Comment by @samypr100 on 2024-07-27 19:39_

@charliermarsh here's a self-contained reproduceable Dockefile

```python
FROM ubuntu:24.04
RUN apt update -y
RUN apt install -y ca-certificates curl
# Add dummy cert (to simulate permission issue)
RUN curl -JLO "https://dl.filippo.io/mkcert/latest?for=linux/amd64"
RUN chmod +x mkcert-v*-linux-amd64
RUN cp mkcert-v*-linux-amd64 /usr/local/bin/mkcert
RUN mkcert example.org
RUN chmod 600 example.org.pem && mv example.org.pem ROOT_CRT.crt
# Previous dockerfile
RUN cp ROOT_CRT.crt /usr/local/share/ca-certificates/ROOT_CRT.crt
RUN /usr/sbin/update-ca-certificates
RUN apt install python3.12 python3.12-venv wget curl -y
RUN useradd -ms /bin/bash vuser
USER vuser
WORKDIR /home/vuser
ENV REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
ENV UV_NATIVE_TLS=1
ENV RUST_LOG=trace
ENV RUST_BACKTRACE=full
RUN python3.12 -m venv venv
RUN venv/bin/python -m pip install uv==0.2.30
RUN RUST_LOG=trace venv/bin/uv venv
# This below will panic, comment it out if you'd like to succesfully build it
RUN RUST_LOG=trace venv/bin/uv pip install rich -vvv
```

---

_Comment by @eth3lbert on 2024-07-29 06:33_

> @charliermarsh here's a self-contained reproduceable Dockefile
> 
> ```python
> FROM ubuntu:24.04
> RUN apt update -y
> RUN apt install -y ca-certificates curl
> # Add dummy cert (to simulate permission issue)
> RUN curl -JLO "https://dl.filippo.io/mkcert/latest?for=linux/amd64"
> RUN chmod +x mkcert-v*-linux-amd64
> RUN cp mkcert-v*-linux-amd64 /usr/local/bin/mkcert
> RUN mkcert example.org
> RUN chmod 600 example.org.pem && mv example.org.pem ROOT_CRT.crt
> # Previous dockerfile
> RUN cp ROOT_CRT.crt /usr/local/share/ca-certificates/ROOT_CRT.crt
> RUN /usr/sbin/update-ca-certificates
> RUN apt install python3.12 python3.12-venv wget curl -y
> RUN useradd -ms /bin/bash vuser
> USER vuser
> WORKDIR /home/vuser
> ENV REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
> ENV UV_NATIVE_TLS=1
> ENV RUST_LOG=trace
> ENV RUST_BACKTRACE=full
> RUN python3.12 -m venv venv
> RUN venv/bin/python -m pip install uv==0.2.30
> RUN RUST_LOG=trace venv/bin/uv venv
> # This below will panic, comment it out if you'd like to succesfully build it
> RUN RUST_LOG=trace venv/bin/uv pip install rich -vvv
> ```

Since this uses `rust-tls-native-certs`, I believe it should work with the `ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt`.

---

_Comment by @samypr100 on 2024-07-29 22:33_

> > @charliermarsh here's a self-contained reproduceable Dockefile
> > ```python
> > FROM ubuntu:24.04
> > RUN apt update -y
> > RUN apt install -y ca-certificates curl
> > # Add dummy cert (to simulate permission issue)
> > RUN curl -JLO "https://dl.filippo.io/mkcert/latest?for=linux/amd64"
> > RUN chmod +x mkcert-v*-linux-amd64
> > RUN cp mkcert-v*-linux-amd64 /usr/local/bin/mkcert
> > RUN mkcert example.org
> > RUN chmod 600 example.org.pem && mv example.org.pem ROOT_CRT.crt
> > # Previous dockerfile
> > RUN cp ROOT_CRT.crt /usr/local/share/ca-certificates/ROOT_CRT.crt
> > RUN /usr/sbin/update-ca-certificates
> > RUN apt install python3.12 python3.12-venv wget curl -y
> > RUN useradd -ms /bin/bash vuser
> > USER vuser
> > WORKDIR /home/vuser
> > ENV REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
> > ENV UV_NATIVE_TLS=1
> > ENV RUST_LOG=trace
> > ENV RUST_BACKTRACE=full
> > RUN python3.12 -m venv venv
> > RUN venv/bin/python -m pip install uv==0.2.30
> > RUN RUST_LOG=trace venv/bin/uv venv
> > # This below will panic, comment it out if you'd like to succesfully build it
> > RUN RUST_LOG=trace venv/bin/uv pip install rich -vvv
> > ```
> 
> Since this uses `rust-tls-native-certs`, I believe it should work with the `ENV SSL_CERT_FILE=/etc/ssl/certs/ca-certificates.crt`.

Yes, I believe the issue is that there's still a symlink to `ROOT_CRT.crt` in `/etc/ssl/certs` which `UV_NATIVE_TLS` will try to load regardless and fail. It will fail is there's even one cert in `/etc/ssl/certs` with locked down permissions, which is undesireable in cases where you don't control the permissions of the existing certs being problematic.

You can arguably get around the issue with not setting `UV_NATIVE_TLS` and only setting `SSL_CERT_FILE`, but I'm not sure if that's acceptable in all cases.

Otherwise, generally another easy way to sort-off reproduce is generating a dummy cert, locking it down, and running a command that uses it like below:
* `mkcert example.org`
* `chmod 600 example.org.pem`
* `sudo chown root:root example.org.pem`
* `uv venv`
* `SSL_CERT_FILE=./example.org.pem uv pip install flask`

But this example doesn't capture the nuance that's ocurring with `UV_NATIVE_TLS` 


---

_Comment by @eth3lbert on 2024-07-30 06:14_

Indeed, thank you for the write-up and the MRE!

---

_Comment by @lskbr on 2024-07-31 07:58_

I confirm the problem is fixed if I chmod 644 the certificate file.
Adding RUN chmod og=r /usr/local/share/ca-certificates/ROOT_CRT.crt to my original Dockerfile fixes the problem.

Regarding the other issue:
I also confirm that the company certificate is accepted by UV without any error when the custom CA is not added, which is wrong. It is correctly refused by wget and curl. To reproduce it, just use a custom certificate that is not in the CA chain. I commented out the COPY line in the original Dockerfile. You need to use a company proxy that requires a custom certificate like Z-Scaler.

Tested with uv 0.2.29 and uv 0.2.30:

```
FROM ubuntu:22.04
RUN apt update -y
RUN apt install -y ca-certificates
#COPY ROOT_CRT.crt /usr/local/share/ca-certificates/ROOT_CRT.crt
RUN /usr/sbin/update-ca-certificates
RUN apt install python3.11 python3.11-venv wget curl -y
RUN useradd -ms /bin/bash vuser
USER vuser
WORKDIR /home/vuser
ENV REQUESTS_CA_BUNDLE=/etc/ssl/certs/ca-certificates.crt
ENV UV_NATIVE_TLS=1
ENV RUST_LOG=trace
ENV RUST_BACKTRACE=full
RUN python3.11 -m venv venv
RUN venv/bin/python -m pip install uv==0.2.29
```

Log attached

[uv-no-cert.txt](https://github.com/user-attachments/files/16438090/uv-no-cert.txt)

Expected result:
uv refuses the connection, as it is tampered with an SSL certificate not in the trust chain.
Current:
uv download the files using the untrusted source.


---

_Comment by @djc on 2024-07-31 10:12_

@charliermarsh thanks for tagging me on the issue. We changed rustls-native-certs in 0.7.1 to make it inspect more potential locations for roots to include, so it is finding more places and, in order to read the certificate from those places, obviously the current user needs to have read privileges on those certificate files.

I'm still a little confused on the use of `UV_NATIVE_TLS` here, I would naively assume this means that the native-tls crates are used for TLS (in this case OpenSSL on Linux), presumably via (something like) reqwest? I was not aware that rustls-native-certs is used with native-tls (instead of with rustls), although I can't think of anything wrong with such usage off the top of my head.

(Currently on vacation so a little slower to respond than usual.)

---

_Comment by @charliermarsh on 2024-07-31 12:51_

Thanks @djc. If `UV_NATIVE_TLS` is set, we enable `tls_built_in_native_certs` on the `reqwest::ClientBuilder`:

```rust
// Configure TLS.
let client_core = if self.native_tls || ssl_cert_file_exists {
    client_core.tls_built_in_native_certs(true)
} else {
    client_core.tls_built_in_webpki_certs(true)
};
```

Which, IIUC, is based on the `rustls-tls-native-roots` feature.

---

_Comment by @djc on 2024-08-01 12:59_

Right. IMO the environment variable name is a little unfortunate since it confuses the TLS implementation crate (native-tls is still used by many in the Rust world) with the source of trust roots (the crate name is rustls-native-certs and reqwest chose to name the feature `-native-roots`).

Not sure what else I can contribute here, sounds like the uv team has things under control? In the future, could also consider using the rustls-platform-verifier crate which might be more robust on platforms like Windows, macOS (and derivatives) and Android, but that will do the same thing here. This is currently not natively supported within reqwest, but you can pass it a preconfigured TLS config to make it work.

---

_Referenced in [rustls/rustls-native-certs#124](../../rustls/rustls-native-certs/issues/124.md) on 2024-08-19 10:56_

---

_Referenced in [astral-sh/uv#1339](../../astral-sh/uv/issues/1339.md) on 2024-08-20 12:37_

---

_Referenced in [astral-sh/rye#1130](../../astral-sh/rye/issues/1130.md) on 2024-08-30 13:48_

---
