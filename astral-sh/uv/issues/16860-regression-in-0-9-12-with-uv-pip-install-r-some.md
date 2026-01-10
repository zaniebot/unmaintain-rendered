---
number: 16860
title: "Regression in 0.9.12 with `uv pip install -r <(some command)`"
type: issue
state: closed
author: alex
labels:
  - bug
assignees: []
created_at: 2025-11-26T13:44:17Z
updated_at: 2025-11-26T16:33:51Z
url: https://github.com/astral-sh/uv/issues/16860
synced_at: 2026-01-10T01:26:10Z
---

# Regression in 0.9.12 with `uv pip install -r <(some command)`

---

_Issue opened by @alex on 2025-11-26 13:44_

### Summary

This is done within a checkout of https://github.com/mitmproxy/mitmproxy/

Failing:

```
bash-3.2$ uv --version
uv 0.9.12 (Homebrew 2025-11-24)
bash-3.2$ uv pip install -r <(uv export --locked)
Resolved 118 packages in 7ms
warning: Requirements file `/dev/fd/63` does not contain any dependencies
Audited in 0.67ms
```

Working:

```
bash-3.2$ .venv/bin/uv --version
uv 0.9.11 (8d8aabb88 2025-11-20)
bash-3.2$ .venv/bin/uv pip install -r <(uv export --locked)
Resolved 118 packages in 6ms
Resolved 87 packages in 355ms
      Built mitmproxy @ file:///private/tmp/mitmproxy
Prepared 86 packages in 5.44s
Installed 86 packages in 99ms
 + aioquic==1.2.0
 + altgraph==0.17.5
 + argon2-cffi==25.1.0
 + argon2-cffi-bindings==25.1.0
 + asgiref==3.10.0
 + attrs==25.4.0
 + bcrypt==5.0.0
 + blinker==1.9.0
 + brotli==1.2.0
 + build==1.3.0
 + cachetools==6.2.2
 + certifi==2025.11.12
 + cffi==2.0.0
 + chardet==5.2.0
 + charset-normalizer==3.4.4
 + click==8.3.0
 + colorama==0.4.6
 + coverage==7.12.0
 + cryptography==46.0.3
 + distlib==0.4.0
 + execnet==2.1.2
 + filelock==3.20.0
 + flask==3.1.2
 + h11==0.16.0
 + h2==4.3.0
 + hpack==4.1.0
 + hyperframe==6.1.0
 + hypothesis==6.130.6
 + idna==3.11
 + iniconfig==2.3.0
 + itsdangerous==2.2.0
 + jinja2==3.1.6
 + kaitaistruct==0.11
 + ldap3==2.9.1
 + macholib==1.16.4
 + markdown2==2.5.4
 + markupsafe==3.0.3
 + maturin==1.9.6
 + mitmproxy==13.0.0.dev0 (from file:///private/tmp/mitmproxy)
 + mitmproxy-macos==0.12.8
 + mitmproxy-rs==0.12.8
 + msgpack==1.1.2
 + mypy==1.18.2
 + mypy-extensions==1.1.0
 + packaging==25.0
 + pathspec==0.12.1
 + pdoc==16.0.0
 + platformdirs==4.5.0
 + pluggy==1.6.0
 + publicsuffix2==2.20191221
 + pyasn1==0.6.1
 + pyasn1-modules==0.4.2
 + pycparser==2.23
 + pygments==2.19.2
 + pyinstaller==6.16.0
 + pyinstaller-hooks-contrib==2025.9
 + pylsqpack==0.3.23
 + pyopenssl==25.3.0
 + pyparsing==3.2.5
 + pyperclip==1.11.0
 + pyproject-api==1.10.0
 + pyproject-hooks==1.2.0
 + pytest==8.4.2
 + pytest-asyncio==1.2.0
 + pytest-cov==7.0.0
 + pytest-timeout==2.4.0
 + pytest-xdist==3.8.0
 + requests==2.32.5
 + ruamel-yaml==0.18.16
 + ruff==0.14.3
 + service-identity==24.2.0
 + setuptools==80.9.0
 + sortedcontainers==2.4.0
 + tornado==6.5.2
 + tox==4.32.0
 + tox-uv==1.29.0
 + types-requests==2.32.4.20250913
 + typing-extensions==4.14.0
 + urllib3==2.5.0
 + urwid==3.0.3
 + virtualenv==20.35.4
 + wcwidth==0.2.14
 + werkzeug==3.1.3
 + wheel==0.45.1
 + wsproto==1.2.0
 + zstandard==0.25.0
```

### Platform

Darwin 25.1.0 arm64, also seen on Github Actions (Linux x86-64)

### Version

uv 0.9.12

### Python version

_No response_

---

_Label `bug` added by @alex on 2025-11-26 13:44_

---

_Referenced in [pyca/cryptography#13925](../../pyca/cryptography/pulls/13925.md) on 2025-11-26 13:44_

---

_Comment by @alex on 2025-11-26 13:45_

This is maybe already resolved on main by https://github.com/astral-sh/uv/pull/16855

---

_Comment by @zanieb on 2025-11-26 13:46_

https://github.com/astral-sh/uv/pull/16857 we'll release a fix soon

---

_Comment by @alex on 2025-11-26 13:48_

Ah great, so fixed but by a different commit than I thought :-)

---

_Comment by @zanieb on 2025-11-26 14:48_

We've been playing whack-a-mole with the various issues so #16861 will just revert the cause entirely.

---

_Closed by @zanieb on 2025-11-26 16:31_

---

_Comment by @zanieb on 2025-11-26 16:33_

A fix is out in 0.9.13

---
