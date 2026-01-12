```yaml
number: 1701
title: Failed to build HTTP client, could not load PEM file
type: issue
state: closed
author: mattmess1221
labels:
  - bug
  - external
  - network
assignees: []
created_at: 2024-02-19T15:54:28Z
updated_at: 2024-06-07T02:57:41Z
url: https://github.com/astral-sh/uv/issues/1701
synced_at: 2026-01-12T15:58:31Z
```

# Failed to build HTTP client, could not load PEM file

---

_@mattmess1221_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
All commands that connect to the internet are failing when trying to load the local CAFILE. I do have some private certs installed, so it may be related.

Command: `uv venv --seed`
Platform: Debian 10
UV Version: 0.1.5
[ca-certificates.crt](https://gist.github.com/killjoy1221/b7406351afb186b9842f765e1ee7bcf7)

The cafile loaded fine in uv 0.1.2, so this is a regression.

Error output:
```
$ uv --version
uv 0.1.5
$ RUST_BACKTRACE=full uv venv --seed 
Using Python 3.11.0 interpreter at /usr/software/pkgs/Python-3.11.0/bin/python3.11
Creating virtualenv at: .venv
thread 'main' panicked at crates/uv-client/src/registry_client.rs:87:33:
Failed to build HTTP client.: reqwest::Error { kind: Builder, source: Custom { kind: InvalidData, error: "Could not load PEM file \"/usr/lib/ssl/certs/ca-certificates.crt\": Invalid byte 32, offset 0." } }
stack backtrace:
   0:     0x55ed8874602c - <unknown>
   1:     0x55ed88779150 - <unknown>
   2:     0x55ed88741f4f - <unknown>
   3:     0x55ed88745e14 - <unknown>
   4:     0x55ed88747c77 - <unknown>
   5:     0x55ed887479df - <unknown>
   6:     0x55ed887480f8 - <unknown>
   7:     0x55ed88747fde - <unknown>
   8:     0x55ed887464f6 - <unknown>
   9:     0x55ed88747d42 - <unknown>
  10:     0x55ed87792cf5 - <unknown>
  11:     0x55ed87793233 - <unknown>
  12:     0x55ed87e42a26 - <unknown>
  13:     0x55ed87af85ec - <unknown>
  14:     0x55ed87afda1d - <unknown>
  15:     0x55ed87adac96 - <unknown>
  16:     0x55ed879e7776 - <unknown>
  17:     0x55ed87b23831 - <unknown>
  18:     0x55ed878436e3 - <unknown>
  19:     0x55ed87ac5499 - <unknown>
  20:     0x55ed88737937 - <unknown>
  21:     0x55ed87ac548e - <unknown>
  22:     0x7f20f290ad0a - __libc_start_main
  23:     0x55ed87793549 - <unknown>
  24:                0x0 - <unknown>
```


---

_Comment by @mattmess1221 on 2024-02-19 16:19_

Likely introduced by #1512 

---

_Label `bug` added by @zanieb on 2024-02-19 17:11_

---

_Comment by @zanieb on 2024-02-19 17:12_

Presumably the certificate was not being loaded at all prior to #1512, is it a valid certificate? This might be an upstream bug.

I wonder if we can fallback to using bundled certificates if we fail to load the system store like this, cc @BurntSushi

---

_Comment by @mattmess1221 on 2024-02-19 17:36_

curl/openssl is able to load it fine, so I would assume so.

---

_Comment by @zanieb on 2024-02-22 22:46_

I'm not sure what to do about this, unfortunately. You may need to open this as an issue upstream or do some more debugging yourself. We're not in control of certificate validation.

---

_Label `needs-mre` added by @zanieb on 2024-02-22 22:46_

---

_Comment by @mattmess1221 on 2024-02-26 19:58_

I figured out the issue was caused by there being indentation in the pem file (for some reason). I've created an issue upstream at rustls/pemfile#40

---

_Comment by @zanieb on 2024-02-26 23:06_

Thank you!

---

_Label `needs-mre` removed by @zanieb on 2024-02-26 23:07_

---

_Label `upstream` added by @zanieb on 2024-02-26 23:07_

---

_Label `network` added by @zanieb on 2024-02-28 18:28_

---

_Comment by @mattmess1221 on 2024-03-04 20:01_

The bug was fixed in `rustls-pemfile` v2.1.1, however `reqwest` is still using v1.0.

Likely blocked by seanmonstar/reqwest#2136

---

_Comment by @zanieb on 2024-06-07 02:57_

Thank you for checking back in @killjoy1221!

We're on rustls-pemfile v2.1.2 now.

---

_Closed by @zanieb on 2024-06-07 02:57_

---
