```yaml
number: 4162
title: Keyring stderr is not displayed
type: issue
state: closed
author: Rogdham
labels:
  - help wanted
  - tracing
assignees: []
created_at: 2024-06-08T15:27:12Z
updated_at: 2024-06-17T19:03:23Z
url: https://github.com/astral-sh/uv/issues/4162
synced_at: 2026-01-10T05:31:37Z
```

# Keyring stderr is not displayed

---

_Issue opened by @Rogdham on 2024-06-08 15:27_

When calling `keyring` as a subprocess, its `stderr` is hidden when using `uv` version 0.2.9 on Linux while `pip` version 24.0 displays it to the user.

Showing the `stderr` output seems important as it allows `keyring` to tell what's wrong in case of error or to ask user to perform some actions (e.g. clicking on an URL for authentication via a web browser).

### Preparation

I have modified my `$PATH` variable so that the `keyring` binary is the following:
```bash
#!/bin/bash
echo "  ~~~> KEYRING: called with $@" >&2
exit 1
```

Also I have a web server on `localhost:8000` that returns a 401 error no matter what.

### UV

```
$ uv pip install --keyring-provider subprocess --index-url=http://user@localhost:8000/simple/ custompkg==0.0.1
  × No solution found when resolving dependencies:
  ╰─▶ Because custompkg was not found in the package registry and you require custompkg==0.0.1, we can conclude that the requirements are unsatisfiable.
```

<details><summary>With trace info</summary>
<p>

```
$ RUST_LOG=uv=trace uv pip install --keyring-provider subprocess --index-url=http://user@localhost:8000/simple/ custompkg==0.0.1 --verbose
DEBUG Searching for Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.12.3, skipping probing: uv/bin/python3
DEBUG Found CPython 3.12.3 at `/redacted/bin/python3` (active virtual environment)
DEBUG Using Python 3.12.3 environment at uv/bin/python3
TRACE Checking lock for `uv`
DEBUG Acquired lock for `uv`
DEBUG At least one requirement is not satisfied: custompkg==0.0.1
TRACE Caching credentials for http://user@localhost:8000/simple/
DEBUG Using registry request timeout of 30s
DEBUG Solving with installed Python version: 3.12.3
DEBUG Adding direct dependency: custompkg==0.0.1
TRACE Fetching metadata for custompkg from http://user@localhost:8000/simple/custompkg/
TRACE No cache entry exists for /home/redacted/.cache/uv/simple-v8/7fc49dbfbd1b2ee9/custompkg.rkyv
DEBUG No cache entry for: http://localhost:8000/simple/custompkg/
TRACE Sending fresh GET request for http://localhost:8000/simple/custompkg/
TRACE Handling request for http://user@localhost:8000/simple/custompkg/
TRACE Request for http://user@localhost:8000/simple/custompkg/ is missing a password, looking for credentials
TRACE No password in cache for realm user@http://localhost:8000
DEBUG Checking netrc for credentials for http://localhost:8000/simple/custompkg/
DEBUG Checking keyring for credentials for user@http://localhost:8000/simple/custompkg/
TRACE Checking keyring for URL http://localhost:8000/simple/custompkg/
TRACE Checking keyring for host localhost
TRACE Received package metadata for: custompkg
DEBUG Searching for a compatible version of custompkg (==0.0.1)
TRACE selecting candidate for package custompkg with range Range { segments: [(Included("0.0.1"), Included("0.0.1"))] } with 0 remote versions
DEBUG No compatible version found for: custompkg
  × No solution found when resolving dependencies:
  ╰─▶ Because custompkg was not found in the package registry and you require custompkg==0.0.1, we can conclude that the requirements are unsatisfiable.
```

</p>
</details> 


### Pip

```
$ PIP_FORCE_KEYRING=1 PIP_KEYRING_PROVIDER=subprocess pip install --index-url http://user@localhost:8000/simple/ custompkg==0.0.1
Looking in indexes: http://****@localhost:8000/simple/
  ~~~> KEYRING: called with get http://localhost:8000/simple/ user
  ~~~> KEYRING: called with get localhost:8000 user
WARNING: 401 Error, Credentials not correct for http://localhost:8000/simple/custompkg/
ERROR: Could not find a version that satisfies the requirement custompkg==0.0.1 (from versions: none)
ERROR: No matching distribution found for custompkg==0.0.1
```

---

_Comment by @zanieb on 2024-06-08 17:01_

Thanks for raising this issue.

If someone wants to look into forwarding the output, it should be relatively simple. I'm not sure if we should do it always or just on failure?

---

_Label `help wanted` added by @zanieb on 2024-06-08 17:01_

---

_Label `tracing` added by @zanieb on 2024-06-08 17:01_

---

_Comment by @Rogdham on 2024-06-08 18:00_

> If someone wants to look into forwarding the output, it should be relatively simple. I'm not sure if we should do it always or just on failure?

I suggest to always display `keyring`'s `stderr`.

First, this is what `pip` is doing.

And foremost because it allows the keyring plugin to do some kind of user interaction. A typical example would to ask the user to open a browser to finalize authentication, displaying a message like the following:

```
Visit https://auth.example.com/?nonce=ABCDEF to authenticate
```

_This would be the case of `artifacts-keyring` plugin for Azure Devops Artifact, except for https://github.com/microsoft/artifacts-keyring/pull/73. In fact this ticket might be needed for #3260._

As a result, please don't wait for the `keyring` command to terminate before displaying its `stderr` to the user!

---

_Comment by @zanieb on 2024-06-08 18:20_

I don't think we support interactive keyring prompts at all right now, we might need to consider what that looks like more holistically. 

---

_Comment by @Rogdham on 2024-06-09 06:17_

> interactive keyring prompts

Just to be clear on that, in my last message I was speaking about displaying `keyring`'s `stderr` to the end user as soon as it is available (without waiting for the `keyring` process to terminate). I was not speaking at all about sending `uv`'s `stdin` to `keyring`'s `stdin`.

So maybe there are 3 levels of implementation:
1. Display `keyring`'s `stderr` to the user when the `keyring` process is terminated
2. Display `keyring`'s `stderr` to the user as soon as it is available (streaming)
3. Last point plus allow the user to pass input to `keyring`'s `stdin`

As far as I'm concerned, level 2 would be good enough for me (this is [what `pip` does](https://github.com/pypa/pip/blob/main/src/pip/_internal/network/auth.py#L137-L142)), and level 1 would be a good step forward. Of course you decide on what you want to support!

---

_Closed by @zanieb on 2024-06-17 18:29_

---

_Comment by @Rogdham on 2024-06-17 19:03_

I'm confirming #4343 fixes this issue by displaying `keyring`'s `stderr` to the user as soon as it is available :tada: 

Thanks @samypr100  @zanieb :hugs: 

---
