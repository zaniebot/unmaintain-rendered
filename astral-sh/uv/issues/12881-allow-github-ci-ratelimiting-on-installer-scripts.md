```yaml
number: 12881
title: allow github ci ratelimiting on installer scripts to be managed with UV_GITHUB_TOKEN
type: issue
state: closed
author: JeremieDoctrine
labels:
  - enhancement
assignees: []
created_at: 2025-04-14T14:26:37Z
updated_at: 2025-04-17T23:32:57Z
url: https://github.com/astral-sh/uv/issues/12881
synced_at: 2026-01-12T16:01:14Z
```

# allow github ci ratelimiting on installer scripts to be managed with UV_GITHUB_TOKEN

---

_@JeremieDoctrine_

### Summary

```
curl -LsSf https://astral.sh/uv/0.6.14/install.sh | sh
downloading uv 0.6.14 x86_64-unknown-linux-gnu
curl: (22) The requested URL returned error: 403
failed to download https://github.com/astral-sh/uv/releases/download/0.6.14/uv-x86_64-unknown-linux-gnu.tar.gz
this may be a standard network error, but it may also indicate
that uv's release process is not working. When in doubt
please feel free to open an issue!
make: *** [Makefile:12: install-uv] Error 1

Exited with code exit status 2
```

Some of our CI's are encoutering the following error from time to time.

I'm not sure but i guess we are being rate limited by Github

Is there a way to authenticate the installation call ?

### Platform

Ubuntu 22.04

### Version

uv 0.6.14

### Python version

python 3.11

---

_Label `bug` added by @JeremieDoctrine on 2025-04-14 14:26_

---

_Comment by @konstin on 2025-04-14 14:50_

CC @Gankra 

---

_Comment by @Gankra on 2025-04-14 15:04_

https://docs.astral.sh/uv/reference/cli/#uv-self-update

You can pass `--token` to `uv self update` or set `UV_GITHUB_TOKEN`.

---

_Comment by @Gankra on 2025-04-14 15:05_

Oh wait this is the bootstrap script, apologies, one moment while I pull up those settings.

---

_Comment by @Gankra on 2025-04-14 15:16_

I'm afraid we don't read these values in the installer scripts right now -- I've filed an issue to do that 

* https://github.com/astral-sh/cargo-dist/issues/23

---

_Assigned to @Gankra by @Gankra on 2025-04-14 15:17_

---

_Comment by @JeremieDoctrine on 2025-04-14 15:17_

Yes thanks ! 

---

_Label `bug` removed by @Gankra on 2025-04-14 15:17_

---

_Label `enhancement` added by @Gankra on 2025-04-14 15:17_

---

_Renamed from "403 on uv installation" to "allow github ci ratelimiting to be managed with UV_GITHUB_TOKEN" by @Gankra on 2025-04-14 15:18_

---

_Renamed from "allow github ci ratelimiting to be managed with UV_GITHUB_TOKEN" to "allow github ci ratelimiting on installer scripts to be managed with UV_GITHUB_TOKEN" by @Gankra on 2025-04-14 15:18_

---

_Comment by @Gankra on 2025-04-14 15:24_

That said I can't guarantee that setting this token would definitely fix the flakiness. In my experience 403 is what github likes to throw when it has any internal issues.

---

_Comment by @JeremieDoctrine on 2025-04-14 15:32_

We had the same issue with TFlint.

But when we started (the 10th of February) to use the option : https://github.com/terraform-linters/tflint/blob/master/install_linux.sh#L55 we stopped having the issue.

![Image](https://github.com/user-attachments/assets/2c0900ad-caa3-4dde-8bc5-7f8e8b46da56)

So I'm quite confident.

---

_Closed by @Gankra on 2025-04-17 23:32_

---

_Closed by @Gankra on 2025-04-17 23:32_

---
