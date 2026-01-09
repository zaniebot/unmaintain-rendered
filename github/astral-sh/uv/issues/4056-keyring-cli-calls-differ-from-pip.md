---
number: 4056
title: Keyring cli calls differ from pip
type: issue
state: open
author: Rogdham
labels:
  - compatibility
assignees: []
created_at: 2024-06-05T17:34:55Z
updated_at: 2025-04-03T14:09:37Z
url: https://github.com/astral-sh/uv/issues/4056
synced_at: 2026-01-07T13:12:17-06:00
---

# Keyring cli calls differ from pip

---

_Issue opened by @Rogdham on 2024-06-05 17:34_

I have noticed a difference in the calls to `keyring` between `pip` and `uv` version 0.2.6 on Linux.

The first call made to `keyring` specifies the package we want to install and not only the index URL. The second call uses the port while pip does not.

To get those results, I wrote a simple script that dumps the args it is called with, named it `keyring` and put it first in `$PATH`.

### Pip

```
$ PIP_FORCE_KEYRING=1 PIP_KEYRING_PROVIDER=subprocess pip -v install --index-url http://user@localhost:8000/simple/ custompkg==0.0.1
```

<details><summary>Full output</summary>
<p>

```
Using pip 23.2.1 from /redacted/env/lib/python3.11/site-packages/pip (python 3.11)
Looking in indexes: http://****@localhost:8000/simple/
Keyring provider requested: subprocess
Keyring provider set: subprocess with executable /redacted2/keyring
```

</p>
</details> 

Subprocess calls made to keyring:
1. `keyring get http://localhost:8000/simple/ user`
2. `keyring get localhost:8000 user`

### UV

```
RUST_LOG=uv=trace uv pip install --keyring-provider subprocess --extra-index-url=http://user@localhost:8000/simple/ custompkg==0.0.1 --verbose
```

<details><summary>Full output</summary>
<p>

```
DEBUG Searching for Python interpreter in virtual environments
TRACE Cached interpreter info for Python 3.11.6, skipping probing: env/bin/python3
DEBUG Found CPython 3.11.6 at `/redacted/env/bin/python3` (active virtual environment)
DEBUG Using Python 3.11.6 environment at env/bin/python3
TRACE Checking lock for `env`
DEBUG Acquired lock for `env`
DEBUG At least one requirement is not satisfied: custompkg==0.0.1
TRACE Caching credentials for http://user@localhost:8000/simple/
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.11.6
DEBUG Adding direct dependency: custompkg==0.0.1
TRACE Fetching metadata for custompkg from http://user@localhost:8000/simple/custompkg/
TRACE No cache entry exists for /home/redacted/.cache/uv/simple-v8/7fc49dbfbd1b2ee9/custompkg.rkyv
DEBUG No cache entry for: http://localhost:8000/simple/custompkg/
TRACE Sending fresh GET request for http://localhost:8000/simple/custompkg/
TRACE Handling request for http://user@localhost:8000/simple/custompkg/
TRACE Request for http://user@localhost:8000/simple/custompkg/ is missing a password, looking for credentials
TRACE No password in cache for realm user@http://localhost:8000
DEBUG Checking keyring for credentials for user@http://localhost:8000/simple/custompkg/
TRACE Checking keyring for URL http://localhost:8000/simple/custompkg/
TRACE Checking keyring for host localhost
TRACE Fetching metadata for custompkg from https://pypi.org/simple/custompkg/
TRACE No cache entry exists for /home/redacted/.cache/uv/simple-v8/pypi/custompkg.rkyv
DEBUG No cache entry for: https://pypi.org/simple/custompkg/
TRACE Sending fresh GET request for https://pypi.org/simple/custompkg/
TRACE Handling request for https://pypi.org/simple/custompkg/
TRACE Request for https://pypi.org/simple/custompkg/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/custompkg/
TRACE Attempting unauthenticated request for https://pypi.org/simple/custompkg/
TRACE Request for https://pypi.org/simple/custompkg/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for realm https://pypi.org
DEBUG Skipping keyring lookup for https://pypi.org/simple/custompkg/ with no username
TRACE Received package metadata for: custompkg
DEBUG Searching for a compatible version of custompkg (==0.0.1)
TRACE selecting candidate for package custompkg with range Range { segments: [(Included("0.0.1"), Included("0.0.1"))] } with 0 remote versions
DEBUG No compatible version found for: custompkg
  × No solution found when resolving dependencies:
  ╰─▶ Because custompkg was not found in the package registry and you require custompkg==0.0.1, we can conclude that the requirements
      are unsatisfiable.
```

</p>
</details> 

Subprocess calls made to keyring:
1. `keyring get http://localhost:8000/simple/custompkg/ user`
2. `keyring get localhost user`

---

_Comment by @zanieb on 2024-06-05 18:23_

Huh interesting. Thanks for the clear report. It's definitely a bug that we don't include the port in the second invocation. I'm not sure about the first one though. Our authentication is implemented as client middleware and it doesn't know that this request is for a package on an index, it just has a URL. We use the full URL first because the authentication _could_ differ per subpath (a good example being GitHub repositories). We might need to make our authentication middleware "aware" of some seeded URLs in order to support special authentication paradigms for indexes.

---

_Label `compatibility` added by @zanieb on 2024-06-05 18:24_

---

_Referenced in [astral-sh/uv#4061](../../astral-sh/uv/pulls/4061.md) on 2024-06-05 18:31_

---

_Comment by @Rogdham on 2024-06-06 07:28_

Here is an illustration with the [`artifacts-keyring`](https://pypi.org/project/artifacts-keyring/) to authenticate to Azure DevOps and fetch packages from an Azure Artifact feed.

From what I saw (I may be slightly wrong), how it works it that for each URL:
1. if it has a token in cache for that URL, it checks that the token is still valid and returns it
2. else it gets a new token by asking the user to make manual operations, then stores the token in cache for next time

For example, here I am already authenticated a specific _feed_ named `myfeed`:
```
$ keyring get 'https://pkgs.dev.azure.com/redacted/_packaging/myfeed/pypi/simple/' VssSessionToken
aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaa
```

But if I try to get a sub-url (like `uv` is doing), it tries to re-authenticate me (it waits for user interaction so I killed it with Ctrl+C at the end):
```
$ keyring get 'https://pkgs.dev.azure.com/redacted/_packaging/myfeed/pypi/simple/custompkg/' VssSessionToken
[Minimal] [CredentialProvider]DeviceFlow: https://pkgs.dev.azure.com/redacted/_packaging/myfeed/pypi/simple/custompkg/
[Minimal] [CredentialProvider]ATTENTION: User interaction required.

    **********************************************************************

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code REDACTED to authenticate.

    **********************************************************************

^C
```

I believe that as a result, I would need to authenticate for every single package downloaded from the _feed_. This would be quite cumbersome, especially since Azure Devops can be used as a proxy registry (i.e. configuring it to be a cache of PyPi). 

Hope this helps illustrate the issue with passing the package name in the URL to `keyring`!

---

_Comment by @zanieb on 2024-06-07 00:05_

Unfortunately the requirements for keyring plugins is entirely unspecified so while this is the case for Azure it may not be (probably isn't) the case for other plugins. It might be worth exploring a change to the Azure plugin itself. Regardless, I can look into this eventually. As I said though, it will be challenging to address.

---

_Comment by @Rogdham on 2024-06-08 15:50_

I agree that the absence of a clear specification is unfortunate. I would have expected `uv` to just copy what `pip` is doing in that case. Also my last message highlights a use case where `pip`'s calls make more sense to me. Maybe it is worth mentioning in the _pip compatibility_ file? 

In any case, I acknowledge that I have limited knowledge on the topic, so I leave it into your hands. But feel free to ping me if you want me to try something out!

---

_Referenced in [astral-sh/uv#3542](../../astral-sh/uv/issues/3542.md) on 2024-06-27 10:59_

---

_Referenced in [astral-sh/uv#4583](../../astral-sh/uv/issues/4583.md) on 2024-06-27 11:03_

---

_Referenced in [astral-sh/uv#4857](../../astral-sh/uv/pulls/4857.md) on 2024-07-08 09:18_

---

_Comment by @zanieb on 2025-03-20 22:03_

@jtfmumm I think we could solve this now that we have `authenticate = "always"`, basically we could eagerly fetch credentials from the keyring for unauthenticated index URLs? At that point, we'd have access to the "index root" and user-specified URL. The only downside to that is that we might fetch credentials before we need to make any network requests, which add overhead to various commands. Perhaps the answer is still to do https://github.com/astral-sh/uv/issues/4583 and tweak our keyring invocation accordingly for index URLs.

---

_Referenced in [astral-sh/uv#12280](../../astral-sh/uv/issues/12280.md) on 2025-03-20 22:06_

---

_Referenced in [astral-sh/uv#11236](../../astral-sh/uv/issues/11236.md) on 2025-03-20 22:07_

---

_Comment by @zanieb on 2025-03-20 22:07_

(I think @konstin is thinking about keyring behaviors too though, I'm not sure what the status of that work is or if it covers this)

---

_Referenced in [astral-sh/uv#11391](../../astral-sh/uv/issues/11391.md) on 2025-03-20 22:13_

---

_Referenced in [astral-sh/uv#12004](../../astral-sh/uv/issues/12004.md) on 2025-03-20 22:21_

---

_Referenced in [astral-sh/uv#12651](../../astral-sh/uv/pulls/12651.md) on 2025-04-03 14:05_

---

_Comment by @jtfmumm on 2025-04-03 14:08_

#12651 causes uv to check for keyring credentials for the index URL instead of the package URL, but it doesn't address the issue you've raised about excluding the port, which will need further investigation.

---

_Assigned to @jtfmumm by @jtfmumm on 2025-04-03 14:09_

---
