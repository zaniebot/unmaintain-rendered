```yaml
number: 16879
title: "`debug` cli command is missing in `uv pip`"
type: issue
state: closed
author: o-murphy
labels:
  - compatibility
assignees: []
created_at: 2025-11-28T07:57:42Z
updated_at: 2025-12-03T17:50:56Z
url: https://github.com/astral-sh/uv/issues/16879
synced_at: 2026-01-12T16:02:39Z
```

# `debug` cli command is missing in `uv pip`

---

_@o-murphy_

### Summary

Common `pip` has `debug` command that gives some usefull info like environment info and compatibility tags

With `uv pip` I got
```bash
uv pip debug --verbose
error: unrecognized subcommand 'debug'
```

But with common pip it is successfull
```
uv run pip debug --verbose
WARNING: This command is only meant for debugging. Do not use this with automation for parsing and getting these details, since the output and options of this command may change without notice.
pip version: pip 25.2 from /home/murphy/.local/share/uv/python/cpython-3.14.0+freethreaded-linux-x86_64-gnu/lib/python3.14t/site-packages/pip (python 3.14)
sys.version: 3.14.0 free-threading build (main, Oct 14 2025, 21:27:26) [Clang 20.1.4 ]
sys.executable: /home/murphy/.local/share/uv/python/cpython-3.14.0+freethreaded-linux-x86_64-gnu/bin/python3.14t
sys.getdefaultencoding: utf-8
sys.getfilesystemencoding: utf-8
locale.getpreferredencoding: UTF-8
sys.platform: linux
sys.implementation:
  name: cpython
'cert' config value: global
REQUESTS_CA_BUNDLE: None
CURL_CA_BUNDLE: None
pip._vendor.certifi.where(): /home/murphy/.local/share/uv/python/cpython-3.14.0+freethreaded-linux-x86_64-gnu/lib/python3.14t/site-packages/pip/_vendor/certifi/cacert.pem
pip._vendor.DEBUNDLED: False
vendored library versions:
  CacheControl==0.14.3
  distlib==0.4.0
  distro==1.9.0
  msgpack==1.1.1
  packaging==25.0
  platformdirs==4.3.8
  pyproject-hooks==1.2.0
  requests==2.32.4
  certifi==2025.07.14
  idna==3.10
  urllib3==1.26.20
  rich==14.1.0 (Unable to locate actual module version, using vendor.txt specified version)
  pygments==2.19.2
  resolvelib==1.2.0
  setuptools==70.3.0 (Unable to locate actual module version, using vendor.txt specified version)
  tomli==2.2.1
  tomli-w==1.2.0
  truststore==0.10.1
  dependency-groups==1.3.1 (Unable to locate actual module version, using vendor.txt specified version)
Compatible tags: 773
  cp314-cp314t-manylinux_2_42_x86_64
  cp314-cp314t-manylinux_2_41_x86_64
  cp314-cp314t-manylinux_2_40_x86_64
  cp314-cp314t-manylinux_2_39_x86_64
  cp314-cp314t-manylinux_2_38_x86_64
  cp314-cp314t-manylinux_2_37_x86_64
  cp314-cp314t-manylinux_2_36_x86_64
  cp314-cp314t-manylinux_2_35_x86_64
  cp314-cp314t-manylinux_2_34_x86_64
  cp314-cp314t-manylinux_2_33_x86_64
  cp314-cp314t-manylinux_2_32_x86_64
  cp314-cp314t-manylinux_2_31_x86_64
  cp314-cp314t-manylinux_2_30_x86_64
  cp314-cp314t-manylinux_2_29_x86_64
  cp314-cp314t-manylinux_2_28_x86_64
  ...
```

### Platform

ubuntu 25.10

### Version

uv 0.9.3

### Python version

any

---

_Label `bug` added by @o-murphy on 2025-11-28 07:57_

---

_Label `bug` removed by @konstin on 2025-11-28 08:24_

---

_Label `compatibility` added by @konstin on 2025-11-28 08:24_

---

_Comment by @zanieb on 2025-11-30 15:06_

I don't think we can reasonably match the output of `pip debug`, e.g., "vendored library versions" doesn't apply to us. We could add a special-cased hidden command like we do for unsupported options that explains it's not present in `uv pip` though.

---

_Comment by @o-murphy on 2025-11-30 22:17_

> I don't think we can reasonably match the output of `pip debug`, e.g., "vendored library versions" doesn't apply to us. We could add a special-cased hidden command like we do for unsupported options that explains it's not present in `uv pip` though.

Or you can redirect it to pip installed to current venv for the commands that is not supported you can use this fallback

---

_Comment by @konstin on 2025-12-01 08:23_

The pip output can be different from the uv output, so `uv pip` shouldn't invoke pip.

---

_Closed by @EliteTK on 2025-12-01 18:00_

---

_Comment by @zanieb on 2025-12-01 18:05_

@EliteTK did you link the wrong issue? :)

---

_Reopened by @zanieb on 2025-12-01 18:05_

---

_Comment by @o-murphy on 2025-12-01 19:12_

> The pip output can be different from the uv output, so `uv pip` shouldn't invoke pip.

Possibly but then there no other ways to got true-pip output except as `uv run -m pip` or `python -m pip`.
But even then it creates problems:
```
└> Due to uv do not installs pip to generated venv 
     └> if pip does not installed system-wide 
          └> uv can't spawn pip  via `uv run pip`
```


---

_Comment by @zanieb on 2025-12-01 20:21_

I mean, you can do `uv run --with pip pip` or use `uv venv --seed` or add `pip` as a dependency in your project? We aren't going to transparently call into `pip` during `uv pip` invocations, it'd be way too confusing.

---

_Assigned to @EliteTK by @EliteTK on 2025-12-03 11:34_

---

_Comment by @EliteTK on 2025-12-03 14:16_

> We could add a special-cased hidden command like we do for unsupported options that explains it's not present in uv pip though.

I was wondering, should uv exit success or failure in that case? Technically given that `pip debug` output is unstable, a warning on stderr and an empty output is not strictly non-conforming with the format.

If someone was using `pip debug` in some automated way (despite the warning) and then wholesale swapped `pip` for `uv pip`, i can see it either way. On the one hand, exiting success may keep things working, on the other hand, if someone was running the command, whatever was depending on the output is now probably breaking, maybe silently.

---

_Comment by @zanieb on 2025-12-03 14:22_

It should exit with an error, imo.

---

_Comment by @zanieb on 2025-12-03 14:23_

(today it errors and we haven't really gotten complaints so that's a safer starting point)

---

_Comment by @EliteTK on 2025-12-03 16:13_

Not sure if it makes sense to close this now or not...

---

_Closed by @zanieb on 2025-12-03 17:50_

---
