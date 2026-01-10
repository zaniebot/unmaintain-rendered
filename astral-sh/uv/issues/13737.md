```yaml
number: 13737
title: "`uv pip install` not respecting `index.authenticate` on followup requests"
type: issue
state: closed
author: JoshuaHassler
labels:
  - bug
assignees: []
created_at: 2025-05-30T17:49:07Z
updated_at: 2025-06-02T00:20:57Z
url: https://github.com/astral-sh/uv/issues/13737
synced_at: 2026-01-10T03:41:47Z
```

# `uv pip install` not respecting `index.authenticate` on followup requests

---

_Issue opened by @JoshuaHassler on 2025-05-30 17:49_

### Summary

[PR #12470 ](https://github.com/astral-sh/uv/pull/12470) updated the pip commands to respect the `index.authenticate` settings, however, this seems to only apply to the initial request. Once the package is found the subsequent request to download the package is first made unauthenticated. I can verify this both in the uv logs and by looking at the logs of our Python index. Due to the specifics of our index setup, this causes the download to fail.

**UV logs**
Initial good request:
```
DEBUG No cache entry for: https://<REPO_URL>/root/pypi/+simple/<PACKAGE>/
TRACE Sending fresh GET request for https://<REPO_URL>/root/pypi/+simple/<PACKAGE>/
TRACE Handling request for https://<REPO_URL>/root/pypi/+simple/<PACKAGE>/ with authentication policy always
```

Followup bad request:
```
DEBUG No cache entry for: https://<REPO_URL>/root/pypi/+<HASH>/<PACKAGE>-py3-none-any.whl
TRACE Sending fresh HEAD request for https://<REPO_URL>/root/pypi/+<HASH>/<PACKAGE>-py3-none-any.whl
TRACE Handling request for https://<REPO_URL>/root/pypi/+<HASH>/<PACKAGE>-py3-none-any.whl with authentication policy auto
```

### Platform

Fedora Linux 6.14.6-300.fc42.x86_64 x86_64 GNU/Linux

### Version

uv 0.7.8

### Python version

Python 3.13.3

---

_Label `bug` added by @JoshuaHassler on 2025-05-30 17:49_

---

_Comment by @zanieb on 2025-05-30 17:50_

We'll need complete logs, example commands, and your configuration.

---

_Comment by @JoshuaHassler on 2025-05-30 18:35_

Sorry, it took me a minute to redact the logs. I have attached the requested information.

To give some more insight the index is being run with Devpi using the devpi-tokens plugin. I have it set up so that if the token is present (in this case as the password in the basic auth header) it uses that. If the token is not present it will redirect the user to a login page.

With this setup it does require `index.authenticate` to be set to always. This allows UV to hit the index page without having its requests redirected. During install when it tries to actually download the whl file it does the default of first attempting to download with no authentication. It receives a 304 followed by a 200 OK where it gets "served" the login page. It then tries to use the login page as a whl leading to the error `WARN cryptography has an invalid package format: Failed to read from zip file`

I only consider this a bug in UV since it seems like `index.authenticate` should affect all requests to the index. Additionally, vanilla pip includes the auth header in all requests and I have verified it works with this setup.

Lastly, this might be related to [Issue 11636](https://github.com/astral-sh/uv/issues/11636). This seems like something that would pop up for repos that use OIDC/Oauth2 for the web interface and auth tokens for CLI apps.

Thanks, and let me know if you would like any more information or testing!

example command:
```
uv pip install -vv --no-cache --force-reinstall cryptography==45.0.3
```

uv config:
```
[[index]]
name = "local-pypi"
url = "https://username:password@python.localhost/root/awepy/+simple"
default = true
authenticate = "always"
```

complete logs:
```
DEBUG uv 0.7.8
DEBUG Searching for default Python interpreter in virtual environments
TRACE Querying interpreter executable at /home/<USER>/<PACKAGE>/.venv/bin/python3
DEBUG Found `cpython-3.13.3-linux-x86_64-gnu` at `/home/<USER>/<PACKAGE>/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.13.3 environment at: .venv
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
TRACE Caching credentials for https://username:password@python.localhost/root/pypi/+simple
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.13.3
DEBUG Solving with target Python version: >=3.13.3
TRACE Assigned packages:
TRACE Chose package for decision: root. remaining choices:
DEBUG Adding direct dependency: cryptography>=45.0.3, <45.0.3+
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: cryptography. remaining choices:
TRACE Fetching metadata for cryptography from https://python.localhost/root/pypi/+simple/cryptography/
TRACE No cache entry exists for /tmp/.tmpiJ8Mht/simple-v16/index/7647f7ee1191f67b/cryptography.rkyv
DEBUG No cache entry for: https://python.localhost/root/pypi/+simple/cryptography/
TRACE Sending fresh GET request for https://python.localhost/root/pypi/+simple/cryptography/
TRACE Handling request for https://username:****@python.localhost/root/pypi/+simple/cryptography/ with authentication policy always
TRACE Request for https://username:****@python.localhost/root/pypi/+simple/cryptography/ already contains username and password
TRACE Updating cached credentials for https://python.localhost/root/pypi/+simple/cryptography/ to Basic { username: Username(Some("username")), password: Some(****) }
TRACE Cached request https://python.localhost/root/pypi/+simple/cryptography/ is not storable because its response has a 'no-store' cache-control directive
TRACE Received package metadata for: cryptography
TRACE Selecting candidate for cryptography with range >=45.0.3, <45.0.3+ with 141 remote versions
TRACE Found candidate for package cryptography with range >=45.0.3, <45.0.3+ after 1 steps: 45.0.3 version
TRACE Returning candidate for package cryptography with range >=45.0.3, <45.0.3+ after 1 steps
DEBUG Searching for a compatible version of cryptography (>=45.0.3, <45.0.3+)
TRACE Selecting candidate for cryptography with range >=45.0.3, <45.0.3+ with 141 remote versions
TRACE Found candidate for package cryptography with range >=45.0.3, <45.0.3+ after 1 steps: 45.0.3 version
TRACE Returning candidate for package cryptography with range >=45.0.3, <45.0.3+ after 1 steps
DEBUG Selecting: cryptography==45.0.3 [compatible] (cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl)
TRACE No cache entry exists for /tmp/.tmpiJ8Mht/wheels-v5/index/7647f7ee1191f67b/cryptography/45.0.3-cp311-abi3-manylinux_2_34_x86_64.msgpack
DEBUG No cache entry for: https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl
TRACE Sending fresh HEAD request for https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl
TRACE Handling request for https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl with authentication policy auto
TRACE Request for https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl
TRACE Attempting unauthenticated request for https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl
TRACE Cached request https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl is storable because its response has a heuristically cacheable status code 200
TRACE Considering retry of error: Error { kind: AsyncHttpRangeReader(Url { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("python.localhost")), port: None, path: "/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl", query: None, fragment: None }, HttpRangeRequestUnsupported) }
TRACE Cannot retry error: not an IO error
WARN Range requests not supported for cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl; streaming wheel
TRACE No cache entry exists for /tmp/.tmpiJ8Mht/wheels-v5/index/7647f7ee1191f67b/cryptography/45.0.3-cp311-abi3-manylinux_2_34_x86_64.msgpack
DEBUG No cache entry for: https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl
TRACE Sending fresh GET request for https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl
TRACE Handling request for https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl with authentication policy auto
TRACE Request for https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl
TRACE Attempting unauthenticated request for https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl
TRACE Cached request https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl is storable because its response has a heuristically cacheable status code 200
TRACE Considering retry of error: Metadata("https://python.localhost/root/pypi/+f/c82/4c9281cb62801/cryptography-45.0.3-cp311-abi3-manylinux_2_34_x86_64.whl", AsyncZip(UnexpectedHeaderError(1329865020, 67324752)))
TRACE Cannot retry error: not an IO error
TRACE Received built distribution metadata for: cryptography==45.0.3
WARN cryptography==45.0.3 has an invalid package format: Failed to read from zip file
WARN cryptography has an invalid package format: Failed to read from zip file
TRACE Assigned packages: root==0a0.dev0
TRACE Chose package for decision: cryptography. remaining choices:
DEBUG Searching for a compatible version of cryptography (>45.0.3, <45.0.3+)
TRACE Selecting candidate for cryptography with range >45.0.3, <45.0.3+ with 141 remote versions
TRACE Exhausted all candidates for package cryptography with range >45.0.3, <45.0.3+ after 0 steps
DEBUG No compatible version found for: cryptography
TRACE Selecting candidate for cryptography with range >45.0.3, <45.0.3+ with 141 remote versions
TRACE Exhausted all candidates for package cryptography with range >45.0.3, <45.0.3+ after 0 steps
TRACE Resolver derivation tree before reduction
term root==0a0.dev0
  root==0a0.dev0 depends on cryptography==45.0.3
  cryptography==45.0.3 an invalid package format
TRACE Resolver derivation tree after reduction
term root==0a0.dev0
  root==0a0.dev0 depends on cryptography==45.0.3
  cryptography==45.0.3 an invalid package format
  × No solution found when resolving dependencies:
  ╰─▶ Because cryptography==45.0.3 has an invalid package format and you require cryptography==45.0.3, we can conclude that
      your requirements are unsatisfiable.

      hint: The structure of `cryptography` (v45.0.3) was invalid:
        Failed to read from zip file
DEBUG Released lock at `/home/<USER>/<PACKAGE>/.venv/.lock`
```

---

_Comment by @zanieb on 2025-05-30 20:49_

This could be because we don't propagate credentials to URLs with a different root unless the index URL ends in exactly `/simple`

https://github.com/astral-sh/uv/blob/c19a294a48351ca84f18149fd6bcbf17d9e20bd9/crates/uv-distribution-types/src/index_url.rs#L70-L89

`https://python.localhost/root/pypi/+simple/cryptography/ ` authenticates because it's a child of your given index URL `https://python.localhost/root/pypi/+simple`, but ` https://python.localhost/root/pypi/+f/c82/4c9281cb62801/` does not because it's on a different path.

---

_Comment by @zanieb on 2025-05-30 20:53_

Could you try https://github.com/astral-sh/uv/pull/13743 please? There will be a uv binary attached to the CI job summary when it completes, otherwise, you can build from source.



---

_Comment by @JoshuaHassler on 2025-05-30 21:23_

That build fixes the issue! Thanks for the quick turnaround.

---

_Comment by @zanieb on 2025-05-30 21:27_

Wonderful, we'll get that released.

---

_Closed by @charliermarsh on 2025-06-02 00:20_

---
