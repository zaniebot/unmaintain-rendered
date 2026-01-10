---
number: 13831
title: "uv 0.7.10: \"Connection reset by peer\" on macOS Apple Silicon"
type: issue
state: closed
author: lucarosa
labels:
  - bug
assignees: []
created_at: 2025-06-04T12:19:48Z
updated_at: 2025-06-13T09:51:00Z
url: https://github.com/astral-sh/uv/issues/13831
synced_at: 2026-01-10T01:25:39Z
---

# uv 0.7.10: "Connection reset by peer" on macOS Apple Silicon

---

_Issue opened by @lucarosa on 2025-06-04 12:19_

### Summary

Hi, 

After a fresh installation of uv 0.7.10 all PyPI connections fail with "Connection reset by peer (os error 54)" on macOS Apple Silicon. Same operations on uv 0.7.9 work perfectly.

### Minimal Reproduction

```
tmp git:(main) ✗ uv --version
uv 0.7.10 (1e5120e15 2025-06-03)
➜  tmp git:(main) ✗ uv add jupyter
⠇ tmp==0.1.0                                                                                                                                                error: Failed to fetch: `https://pypi.org/simple/jupyter/`
  Caused by: Request failed after 3 retries
  Caused by: error sending request
  Caused by: client error (Connect)
  Caused by: Connection reset by peer (os error 54)
➜  tmp git:(main) ✗ uv cache clean
Clearing cache at: /Users/lucarosa/.cache/uv
Removed 5 files (1.6KiB)
➜  tmp git:(main) ✗ rm ~/.local/bin/uv ~/.local/bin/uvx
➜  tmp git:(main) ✗ curl -LsSf https://astral.sh/uv/0.7.9/install.sh | sh
downloading uv 0.7.9 aarch64-apple-darwin
no checksums to verify
installing to /Users/lucarosa/.local/bin
  uv
  uvx
everything's installed!
➜  tmp git:(main) ✗ uv add jupyter
Resolved 102 packages in 468ms
Audited 98 packages in 0.10ms
```

This happen everytime regardless of the package. Here the error when run with `--verbose`

```
tmp git:(main) ✗ uv add numpy --verbose
DEBUG uv 0.7.10 (1e5120e15 2025-06-03)
DEBUG Found project root: `/Users/lucarosa/learndir/tmp`
DEBUG No workspace root found, using project root
DEBUG Acquired lock for `/Users/lucarosa/learndir/tmp`
DEBUG Reading Python requests from version file at `/Users/lucarosa/learndir/tmp/.python-version`
DEBUG Using Python request `3.13` from version file at `.python-version`
DEBUG Checking for Python environment at `.venv`
DEBUG The project environment's Python version satisfies the request: `Python 3.13`
DEBUG Released lock at `/var/folders/wz/ggwcpk2s1jq9hk80m_6t9vmr0000gn/T/uv-60938af87eca11c1.lock`
DEBUG Using request timeout of 30s
DEBUG Found static `pyproject.toml` for: tmp @ file:///Users/lucarosa/learndir/tmp
DEBUG No workspace root found, using project root
DEBUG Ignoring existing lockfile due to mismatched requirements for: `tmp==0.1.0`
  Requested: {Requirement { name: PackageName("jupyter"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "1.1.1" }]), index: None, conflict: None }, origin: None }, Requirement { name: PackageName("numpy"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([]), index: None, conflict: None }, origin: None }}
  Existing: {Requirement { name: PackageName("jupyter"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: GreaterThanEqual, version: "1.1.1" }]), index: None, conflict: None }, origin: None }}
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13
DEBUG Adding direct dependency: tmp*
DEBUG Searching for a compatible version of tmp @ file:///Users/lucarosa/learndir/tmp (*)
DEBUG Adding direct dependency: jupyter>=1.1.1
DEBUG Adding direct dependency: numpy*
DEBUG No cache entry for: https://pypi.org/simple/jupyter/
DEBUG No cache entry for: https://pypi.org/simple/numpy/
DEBUG Transient request failure for unknown URL, retrying: error sending request
  Caused by: client error (Connect)
  Caused by: Connection reset by peer (os error 54)
DEBUG Transient request failure for unknown URL, retrying: error sending request
  Caused by: client error (Connect)
  Caused by: Connection reset by peer (os error 54)
DEBUG Transient request failure for unknown URL, retrying: error sending request
  Caused by: client error (Connect)
  Caused by: Connection reset by peer (os error 54)
DEBUG Transient request failure for unknown URL, retrying: error sending request
  Caused by: client error (Connect)
  Caused by: Connection reset by peer (os error 54)
DEBUG Transient request failure for unknown URL, retrying: error sending request
  Caused by: client error (Connect)
  Caused by: Connection reset by peer (os error 54)
DEBUG Transient request failure for unknown URL, retrying: error sending request
  Caused by: client error (Connect)
  Caused by: Connection reset by peer (os error 54)
DEBUG Reverting changes to `pyproject.toml`
DEBUG Reverting changes to `uv.lock`
```

### Platform

mac OS 15.5 arm64

### Version

0.7.10

### Python version

Python 3.13.3

---

_Label `bug` added by @lucarosa on 2025-06-04 12:19_

---

_Renamed from "UV 0.7.10: "Connection reset by peer" on macOS Apple Silicon" to "uv 0.7.10: "Connection reset by peer" on macOS Apple Silicon" by @lucarosa on 2025-06-04 12:21_

---

_Comment by @konstin on 2025-06-04 13:35_

Are you using any specific settings, such as a proxy? Do you know if your connection is IPv4 or IPv6?

---

_Referenced in [astral-sh/uv#13835](../../astral-sh/uv/pulls/13835.md) on 2025-06-04 13:40_

---

_Comment by @konstin on 2025-06-04 13:40_

Could you try the branch at https://github.com/astral-sh/uv/pull/13835 to see if it resolves the problem?

---

_Comment by @zanieb on 2025-06-04 13:47_

cc @seanmonstar 

As a note, I can't reproduce — we'll need more information on the network setup. It seems reasonable to rollback the upgrade anyway though.

---

_Comment by @seanmonstar on 2025-06-04 13:59_

> Connection reset by peer (os error 54)

Hm, only thing I can think of that seems relevant in reqwest around connection establishment is some proxy refactors. Maybe a SOCKS proxy?

---

_Comment by @lucarosa on 2025-06-04 14:07_

> Are you using any specific settings, such as a proxy? Do you know if your connection is IPv4 or IPv6?

I'm on IPv4 with Google DNS server (8.8.8.8, 8.8.4.4), no proxy settings. My ISP often has issues with IPv6 but I've never investigated. Looks like that's what it is causing the issue maybe? 

```
➜  tmp git:(main) ✗ curl -6 https://pypi.org/
curl: (35) Recv failure: Connection reset by peer
```

it works with with IPv4. I can test the branch in a bit. 

---

_Comment by @konstin on 2025-06-04 14:08_

This might be caused by https://github.com/hyperium/hyper-util/pull/194 then

---

_Comment by @lucarosa on 2025-06-04 14:33_

It's still failing

```
 tmp git:(main) ✗ ../uv/target/release/uv --version
uv 0.7.10+6 (e863001f5 2025-06-04)
➜  tmp git:(main) ✗ ../uv/target/release/uv add numpy
⠇ tmp==0.1.0
error: Failed to fetch: `https://pypi.org/simple/numpy/`
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (https://pypi.org/simple/numpy/)
  Caused by: client error (Connect)
  Caused by: Connection reset by peer (os error 54)
```

---

_Comment by @seanmonstar on 2025-06-04 14:34_

That change "shouldn't" be an issue, since the connector will try all the IP addresses that the resolver returns, even if IPv6 ones fail. That change just adjusted the order of addresses that are attempted.

---

_Comment by @geofft on 2025-06-04 14:56_

If you're getting "connection reset by peer" that means the TCP connect succeeded but the other side immediately hung up on you after connecting, right? In that case, a change to the order of addresses will actually make a difference, in that Happy Eyeballs will see a successful connection and pass that onto the higher layer, and never try the later addresses.

---

_Comment by @konstin on 2025-06-04 15:15_

@lucarosa Sorry for the trial-and-error method, could you try #13835 again? (I've now downgraded the transitive hyper-util, too) 

---

_Comment by @lucarosa on 2025-06-04 16:04_

Not a problem! It's working now :) 

```
➜  tmp git:(main) ✗ uv --version
uv 0.7.10 (1e5120e15 2025-06-03)
➜  tmp git:(main) ✗ uv add numpy
⠼ tmp==0.1.0
error: Failed to fetch: `https://pypi.org/simple/numpy/`
  Caused by: Request failed after 3 retries
  Caused by: error sending request
  Caused by: client error (Connect)
  Caused by: Connection reset by peer (os error 54)
➜  tmp git:(main) ✗ ../uv/target/release/uv add numpy
Resolved 2 packages in 188ms
Audited 1 package in 0.30ms
➜  tmp git:(main) ✗ ../uv/target/release/uv --version
uv 0.7.10+7 (fe65c5aa2 2025-06-04)
```

---

_Comment by @konstin on 2025-06-04 16:27_

Thanks for testing! Downgrading hyper-util to 0.1.12 worked, so it really looks like https://github.com/hyperium/hyper-util/pull/194.

---

_Referenced in [hyperium/hyper#3900](../../hyperium/hyper/issues/3900.md) on 2025-06-04 16:33_

---

_Comment by @konstin on 2025-06-05 12:10_

The latest hyper-util release fixes the regression, so we can try to upgrade again and test if it works (https://github.com/hyperium/hyper-util/pull/200)

---

_Comment by @konstin on 2025-06-10 09:52_

HI @lucarosa, sorry to bother you again, would you be willing to test https://github.com/astral-sh/uv/pull/13912 to confirm that this version has fixed the regression?

---

_Comment by @lucarosa on 2025-06-11 16:49_

Hi @konstin, apologies for the late reply! I'll test it in the next couple of hours 

---

_Comment by @lucarosa on 2025-06-11 21:15_

I don't seem to be able to reproduce the original bug because now my IPv6 connection is working. When I disable IPv6, uv works fine. So it looks like the bug was triggered specifically by the "Connection reset by peer" error which I cannot longer reproduce. 

---

_Comment by @konstin on 2025-06-13 09:50_

Ok, thanks for trying!

Closing this as it seems fixed upstream, thanks @seanmonstar.

---

_Closed by @konstin on 2025-06-13 09:50_

---

_Referenced in [seanmonstar/reqwest#2743](../../seanmonstar/reqwest/pulls/2743.md) on 2025-06-24 13:13_

---

_Referenced in [astral-sh/uv#14243](../../astral-sh/uv/pulls/14243.md) on 2025-06-24 15:47_

---
