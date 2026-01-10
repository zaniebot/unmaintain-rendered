---
number: 4391
title: "`uv pip install something.whl` does not install the new version"
type: issue
state: closed
author: ThiefMaster
labels:
  - bug
assignees: []
created_at: 2024-06-18T17:40:53Z
updated_at: 2024-06-19T14:50:14Z
url: https://github.com/astral-sh/uv/issues/4391
synced_at: 2026-01-10T01:23:37Z
---

# `uv pip install something.whl` does not install the new version

---

_Issue opened by @ThiefMaster on 2024-06-18 17:40_

When installing from a local wheel file, a newer (or generally different, since it should not matter if it's newer or older) version does not get installed since uv thinks it's already there.

Here are two simple wheels (just containing a package with an empty `__init__.py`) to reproduce it:

- [mypkg-3.3.3-py3-none-any.whl.zip](https://github.com/user-attachments/files/15890401/mypkg-3.3.3-py3-none-any.whl.zip)
- [mypkg-3.3.4-py3-none-any.whl.zip](https://github.com/user-attachments/files/15890402/mypkg-3.3.4-py3-none-any.whl.zip)

```
[adrian@claptrap:/tmp/uv-bug-test/test]> uv version
uv 0.2.13

[adrian@claptrap:/tmp/uv-bug-test/test]> ll ../*/dist/*.whl
-rw-r--r-- 1 adrian users 1066 Jun 18 19:34 ../v1/dist/mypkg-3.3.3-py3-none-any.whl
-rw-r--r-- 1 adrian users 1071 Jun 18 19:34 ../v2/dist/mypkg-3.3.4-py3-none-any.whl

[adrian@claptrap:/tmp/uv-bug-test/test]> uv venv
Using Python 3.12.3 interpreter at: /home/adrian/.pyenv/versions/3.12.3/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate

[adrian@claptrap:/tmp/uv-bug-test/test]> source .venv/bin/activate

[adrian@claptrap:/tmp/uv-bug-test/test]> uv pip install --no-cache ../v1/dist/*.whl
Resolved 1 package in 1ms
Downloaded 1 package in 0.62ms
Installed 1 package in 0.28ms
 + mypkg==3.3.3 (from file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl)

[adrian@claptrap:/tmp/uv-bug-test/test]> uv pip install --no-cache ../v2/dist/*.whl
Resolved 1 package in 1ms
Audited 1 package in 0.04ms

[adrian@claptrap:/tmp/uv-bug-test/test]> uv pip freeze
mypkg @ file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl
```

Verbose `uv pip install` output:

<details>

```
[adrian@claptrap:/tmp/uv-bug-test/test]> uv pip install --no-cache --verbose ../v1/dist/*.whl
DEBUG uv 0.2.13
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found cpython 3.12.3 at `/tmp/uv-bug-test/test/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: mypkg*
DEBUG Searching for a compatible version of mypkg @ file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl (*)
DEBUG Tried 1 versions: mypkg 1
Resolved 1 package in 1ms
DEBUG Identified uncached requirement: mypkg @ file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl
Downloaded 1 package in 0.63ms
Installed 1 package in 0.22ms
 + mypkg==3.3.3 (from file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl)

[adrian@claptrap:/tmp/uv-bug-test/test]> uv pip install --no-cache --verbose ../v2/dist/*.whl
DEBUG uv 0.2.13
DEBUG Searching for Python interpreter in virtual environments
DEBUG Found cpython 3.12.3 at `/tmp/uv-bug-test/test/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: file:///tmp/uv-bug-test/v2/dist/mypkg-3.3.4-py3-none-any.whl
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: mypkg*
DEBUG Searching for a compatible version of mypkg @ file:///tmp/uv-bug-test/v2/dist/mypkg-3.3.4-py3-none-any.whl (*)
DEBUG Tried 1 versions: mypkg 1
Resolved 1 package in 1ms
DEBUG Requirement already installed: mypkg==3.3.3 (from file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl)
Audited 1 package in 0.03ms
```
</details>

---

I don't think it's relevant, but in my real application the newer wheel also contained an updated dependency and that got installed, but the updated package itself from that wheel didn't.

<details>

```
[indico@indico-sandbox:~]> indico --version
Indico v3.3.3-dev+202406171640.3e1360a9ad

[indico@indico-sandbox:~]> uv pip install wheels/*.whl
Resolved 138 packages in 221ms
Downloaded 1 package in 31ms
Uninstalled 1 package in 2ms
Installed 1 package in 1ms
 - urllib3==1.26.18
 + urllib3==1.26.19

[indico@indico-sandbox:~]> ll wheels/*.whl
-rw-r--r--. 1 indico indico 38328151 Jun 18 16:54 wheels/indico-3.3.3.dev0+202406181653.57d770a2a9-py3-none-any.whl

[indico@indico-sandbox:~]> pip freeze | grep 'indico '
indico @ file:///opt/indico/wheels/indico-3.3.3.dev0%2B202406171640.3e1360a9ad-py3-none-any.whl#sha256=8c7fe138d56f54bb6906f2bdc8de4a929c2a91709b375a801572486b34b1ab80
```

</details>

---

_Renamed from "`uv pip install something.whl` does not install newer version" to "`uv pip install something.whl` does not install the new version" by @ThiefMaster on 2024-06-18 17:41_

---

_Comment by @ThiefMaster on 2024-06-18 17:44_

Also unrelated, but "Downloaded 1 package in 0.62ms" is misleading since there is no download in case of a local wheel file.

---

_Comment by @zanieb on 2024-06-18 17:50_

See https://github.com/astral-sh/uv/issues/4011 for the download / build time.

---

_Comment by @ThiefMaster on 2024-06-18 17:52_

```
[adrian@claptrap:/tmp/uv-bug-test/test]> RUST_LOG=uv=trace uv pip install --no-cache --verbose ../v1/dist/*.whl
DEBUG uv 0.2.13
DEBUG Searching for Python interpreter in virtual environments
TRACE Querying interpreter executable at /tmp/uv-bug-test/test/.venv/bin/python3
DEBUG Found cpython 3.12.3 at `/tmp/uv-bug-test/test/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
TRACE Checking lock for `.venv`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl
DEBUG Using request timeout of 30s
TRACE Performing lookahead for mypkg @ file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: mypkg*
DEBUG Searching for a compatible version of mypkg @ file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl (*)
DEBUG Tried 1 versions: mypkg 1
Resolved 1 package in 1ms
DEBUG Identified uncached requirement: mypkg @ file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl
Downloaded 1 package in 0.60ms
Installed 1 package in 0.26ms
 + mypkg==3.3.3 (from file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl)

[adrian@claptrap:/tmp/uv-bug-test/test]> RUST_LOG=uv=trace uv pip install --no-cache --verbose ../v2/dist/*.whl
DEBUG uv 0.2.13
DEBUG Searching for Python interpreter in virtual environments
TRACE Querying interpreter executable at /tmp/uv-bug-test/test/.venv/bin/python3
DEBUG Found cpython 3.12.3 at `/tmp/uv-bug-test/test/.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.3 environment at .venv/bin/python3
TRACE Checking lock for `.venv`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: file:///tmp/uv-bug-test/v2/dist/mypkg-3.3.4-py3-none-any.whl
DEBUG Using request timeout of 30s
TRACE Performing lookahead for mypkg @ file:///tmp/uv-bug-test/v2/dist/mypkg-3.3.4-py3-none-any.whl
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: mypkg*
DEBUG Searching for a compatible version of mypkg @ file:///tmp/uv-bug-test/v2/dist/mypkg-3.3.4-py3-none-any.whl (*)
DEBUG Tried 1 versions: mypkg 1
Resolved 1 package in 1ms
TRACE Comparing installed with source: Url(InstalledDirectUrlDist { name: PackageName("mypkg"), version: "3.3.3", direct_url: ArchiveUrl { url: "file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl", archive_info: ArchiveInfo { hash: None, hashes: None }, subdirectory: None }, url: Url { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl", query: None, fragment: None }, editable: false, path: "/tmp/uv-bug-test/test/.venv/lib/python3.12/site-packages/mypkg-3.3.3.dist-info" }) Path { install_path: "/tmp/uv-bug-test/v2/dist/mypkg-3.3.4-py3-none-any.whl", lock_path: "/tmp/uv-bug-test/v2/dist/mypkg-3.3.4-py3-none-any.whl", url: VerbatimUrl { url: Url { scheme: "file", cannot_be_a_base: false, username: "", password: None, host: None, port: None, path: "/tmp/uv-bug-test/v2/dist/mypkg-3.3.4-py3-none-any.whl", query: None, fragment: None }, given: Some("../v2/dist/mypkg-3.3.4-py3-none-any.whl") } }
TRACE Path mismatch: "/tmp/uv-bug-test/v2/dist/mypkg-3.3.4-py3-none-any.whl" vs. "/tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl"
DEBUG Requirement already installed: mypkg==3.3.3 (from file:///tmp/uv-bug-test/v1/dist/mypkg-3.3.3-py3-none-any.whl)
Audited 1 package in 0.07ms
```

---

_Comment by @zanieb on 2024-06-18 17:54_

Uh interesting so at https://github.com/astral-sh/uv/blob/d8f1de61347b5dc18bfde977dc1b3c34f8a3de01/crates/uv-installer/src/satisfies.rs#L176-L185 we're detecting that these are "different" but we're treating that as satisfied.

---

_Comment by @zanieb on 2024-06-18 17:56_

cc @konstin / @charliermarsh looks like this was last touched in https://github.com/astral-sh/uv/pull/3864?

---

_Referenced in [astral-sh/uv#4393](../../astral-sh/uv/pulls/4393.md) on 2024-06-18 18:06_

---

_Label `bug` added by @zanieb on 2024-06-18 18:07_

---

_Comment by @zanieb on 2024-06-18 18:07_

This looks wrong. I resolved your MRE with https://github.com/astral-sh/uv/pull/4393 — we'll see if there's something else that was relying on this weird behavior.

---

_Referenced in [astral-sh/uv#4396](../../astral-sh/uv/pulls/4396.md) on 2024-06-18 18:36_

---

_Closed by @zanieb on 2024-06-19 14:50_

---

_Closed by @zanieb on 2024-06-19 14:50_

---
