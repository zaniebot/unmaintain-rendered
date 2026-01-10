---
number: 13747
title: Behavior of the --offline flag along with cached packages from an internal pypi index seem to not work as expected
type: issue
state: closed
author: taranlu-houzz
labels:
  - question
assignees: []
created_at: 2025-05-30T23:54:44Z
updated_at: 2025-06-06T17:30:25Z
url: https://github.com/astral-sh/uv/issues/13747
synced_at: 2026-01-10T01:25:38Z
---

# Behavior of the --offline flag along with cached packages from an internal pypi index seem to not work as expected

---

_Issue opened by @taranlu-houzz on 2025-05-30 23:54_

### Summary

The below session illustrates the issue I am running into:
- Install an internal package from our internal pypi server via `uvx`
- When I try to use the tool in offline mode, it works, but only if the index url matches exactly.
- It does not seem to be possible to coax `uv` to pull the package from the cache without the correct index url, even in offline mode.
```
↓3 ~/.cache/uv
❯ eza -1 --group-directories-first --tree
.
├── interpreter-v4
│   └── a8e1654926643a11
│       └── 5ddcd655664a7e80.msgpack
├── sdists-v9
└── CACHEDIR.TAG

↓3 ~/.cache/uv
❯ uvx --python 3.11 --python-preference only-managed --index 'http://<internal pypi url>:8080' --prerelease allow --from <internal tool package> render --version
Installed 32 packages in 91ms
<internal tool package>: 1.0.0 (f9a69e9e6e43f917f3171631b2eebeef865673cb)

↓3 ~/.cache/uv took 22s
❯ fd "<internal tool package>"
archive-v0/Sd_1rmJ8VU52wP5IZsMpG/lib/python3.11/site-packages/<internal tool package>-1.0.0.dist-info/
archive-v0/nKnXCB5NKzyNdd92JBiQf/<internal tool package>-1.0.0.dist-info/

↓3 ~/.cache/uv
❯ uvx --offline --python 3.11 --python-preference only-managed --index 'http://<internal pypi url>:8080' --prerelease allow --from <internal tool package> render --version
<internal tool package>: 1.0.0 (f9a69e9e6e43f917f3171631b2eebeef865673cb)

↓3 ~/.cache/uv
❯ uvx --offline --python 3.11 --python-preference only-managed --prerelease allow --from <internal tool package> render --version
  × No solution found when resolving tool dependencies:
  ╰─▶ Because <internal tool package> was not found in the cache and you require
      <internal tool package>, we can conclude that your requirements are unsatisfiable.

      hint: Packages were unavailable because the network was disabled. When the network is disabled, registry
      packages may only be read from the cache.

↓3 ~/.cache/uv
❌ 1 ❯ uvx --offline --python 3.11 --python-preference only-managed --prerelease allow --from <internal tool package>==1.0.0 render --version
  × No solution found when resolving tool dependencies:
  ╰─▶ Because <internal tool package> was not found in the cache and you require
      <internal tool package>==1.0.0, we can conclude that your requirements are unsatisfiable.

      hint: Packages were unavailable because the network was disabled. When the network is disabled, registry
      packages may only be read from the cache.

↓3 ~/.cache/uv
❌ 1 ❯ uvx --offline --python 3.11 --python-preference only-managed --index 'http://bad-url.net:8080' --prerelease allow --from <internal tool package> render --version
  × No solution found when resolving tool dependencies:
  ╰─▶ Because <internal tool package> was not found in the cache and you require
      <internal tool package>, we can conclude that your requirements are unsatisfiable.

      hint: Packages were unavailable because the network was disabled. When the network is disabled, registry
      packages may only be read from the cache.

↓3 ~/.cache/uv
❌ 1 ❯ uvx --python 3.11 --python-preference only-managed --index 'http://bad-url.net:8080' --prerelease allow --from <internal tool package> render --version
⠇ Resolving dependencies...                                                                                           error: Failed to fetch: `http://bad-url.net:8080/<internal tool package>/`
  Caused by: Could not connect, are you offline?
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (http://bad-url.net:8080/<internal tool package>/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: nodename nor servname provided, or not known
  Caused by: failed to lookup address information: nodename nor servname provided, or not known

↓3 ~/.cache/uv took 3s
❌ 2 ❯
```

### Platform

macOS 14.7.5 (23H527)

### Version

uv 0.7.9 (13a86a23b 2025-05-30)

### Python version

3.11.11 (uv managed)

---

_Label `bug` added by @taranlu-houzz on 2025-05-30 23:54_

---

_Referenced in [astral-sh/uv#10380](../../astral-sh/uv/issues/10380.md) on 2025-05-30 23:56_

---

_Comment by @konstin on 2025-06-02 09:13_

This is intentional: For security and reproducibility reasons, uv considers packages from different indexes as totally different packages and uv will only use a different index if explicitly asked to.

You can use `uv tool install` to use a tool offline after installation.

---

_Label `bug` removed by @konstin on 2025-06-02 09:13_

---

_Label `question` added by @konstin on 2025-06-02 09:13_

---

_Closed by @taranlu-houzz on 2025-06-06 17:30_

---
