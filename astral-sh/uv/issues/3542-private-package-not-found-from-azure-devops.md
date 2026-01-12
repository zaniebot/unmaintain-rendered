```yaml
number: 3542
title: Private package not found from Azure DevOps
type: issue
state: closed
author: AndreuCodina
labels:
  - question
assignees: []
created_at: 2024-05-13T11:25:34Z
updated_at: 2024-07-26T23:41:48Z
url: https://github.com/astral-sh/uv/issues/3542
synced_at: 2026-01-12T15:58:44Z
```

# Private package not found from Azure DevOps

---

_@AndreuCodina_

I tested the latest fix (https://github.com/astral-sh/uv/issues/3291) and still can't install a private package using `uv` due to another error.

**requirements.txt**

```
sqlmodel==0.0.14
my-private-package==1.0.0
```

**Execution:**

```bash
$ uv --version
uv 0.1.42
```

```bash
$ uv pip install --system keyring artifacts-keyring

$ az login

$ uv pip install --system --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple --keyring-provider subprocess --requirement requirements.txt --verbose

DEBUG Starting interpreter discovery for default Python
DEBUG Cached interpreter info for Python 3.12.3, skipping probing: /usr/local/bin/python3
DEBUG Using Python 3.12.3 environment at /usr/local/bin/python3
DEBUG Trying to lock if free: /tmp/uv-39ee7c309557c5e9.lock
DEBUG At least one requirement is not satisfied: my-private-package==1.0.0
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: sqlmodel==0.0.14
DEBUG Adding direct dependency: my-private-package==1.0.0
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
DEBUG No cache entry for: https://pypi.org/simple/my-private-package/
DEBUG Found stale response for: https://pypi.org/simple/sqlmodel/
DEBUG Sending revalidation request for: https://pypi.org/simple/sqlmodel/
DEBUG Found not-modified response for: https://pypi.org/simple/sqlmodel/
DEBUG Searching for a compatible version of sqlmodel (==0.0.14)
DEBUG Selecting: sqlmodel==0.0.14 (sqlmodel-0.0.14-py3-none-any.whl)
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
DEBUG Adding transitive dependency for sqlmodel==0.0.14: sqlalchemy>=2.0.0, <2.1.0
DEBUG Adding transitive dependency for sqlmodel==0.0.14: pydantic>=1.10.13, <3.0.0
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
DEBUG Found stale response for: https://pypi.org/simple/pydantic/
DEBUG Sending revalidation request for: https://pypi.org/simple/pydantic/
DEBUG Searching for a compatible version of my-private-package (==1.0.0)
DEBUG No compatible version found for: my-private-package
  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-package was not found in the package registry and you require my-private-package==1.0.0, we
      can conclude that the requirements are unsatisfiable.
```

If I use `pip`, I can:

```bash
$ pip install --root-user-action=ignore --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple --requirement requirements.txt

Looking in indexes: https://pypi.org/simple, https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple
[Warning] [CredentialProvider]Warning: Cannot persist Microsoft authentication token cache securely!
[Warning] [CredentialProvider]Warning: Using plain-text fallback token cache
[Minimal] [CredentialProvider]DeviceFlow: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/
[Minimal] [CredentialProvider]ATTENTION: User interaction required.

    **********************************************************************

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code ... to authenticate.

    **********************************************************************
[Information] [CredentialProvider]VstsCredentialProvider - Acquired bearer token using 'MSAL Device Code'
[Information] [CredentialProvider]VstsCredentialProvider - Attempting to exchange the bearer token for an Azure DevOps session token.
...
```

---

_Comment by @zanieb on 2024-05-13 13:45_

Thanks for the well written report. Can you include trace logs for the uv invocation? `RUST_LOG=uv=trace uv pip install ...`

---

_Assigned to @zanieb by @zanieb on 2024-05-13 13:46_

---

_Comment by @AndreuCodina on 2024-05-13 15:34_

> RUST_LOG=uv=trace uv pip install

```bash
$ RUST_LOG=uv=trace uv pip install --system --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple --keyring-provider subprocess --requirement requirements.txt --verbose

DEBUG Starting interpreter discovery for default Python
DEBUG Cached interpreter info for Python 3.12.3, skipping probing: /usr/local/bin/python3
DEBUG Using Python 3.12.3 environment at /usr/local/bin/python3
DEBUG Trying to lock if free: /tmp/uv-39ee7c309557c5e9.lock
DEBUG At least one requirement is not satisfied: my-private-package==1.0.0
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: sqlmodel==0.0.14
DEBUG Adding direct dependency: my-private-package==1.0.0
TRACE Fetching metadata for sqlmodel from https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Fetching metadata for my-private-package from https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/sqlmodel.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Handling request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Attempting unauthenticated request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/my-private-package.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Handling request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Attempting unauthenticated request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://pkgs.dev.azure.com
TRACE Skipping keyring lookup for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ with no username
TRACE Fetching metadata for my-private-package from https://pypi.org/simple/my-private-package/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/my-private-package.rkyv
DEBUG No cache entry for: https://pypi.org/simple/my-private-package/
TRACE Sending fresh GET request for https://pypi.org/simple/my-private-package/
TRACE Handling request for https://pypi.org/simple/my-private-package/
TRACE Request for https://pypi.org/simple/my-private-package/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/my-private-package/
TRACE Attempting unauthenticated request for https://pypi.org/simple/my-private-package/
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://pkgs.dev.azure.com
TRACE Skipping fetch of credentials for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/, previous attempt failed
TRACE Fetching metadata for sqlmodel from https://pypi.org/simple/sqlmodel/
TRACE cached request https://pypi.org/simple/sqlmodel/ is storable because its response has a 'public' cache-control directive
TRACE cached request https://pypi.org/simple/sqlmodel/ has a cached response that does not allow staleness
TRACE request https://pypi.org/simple/sqlmodel/ does not have a fresh cache because its age is 2361 seconds, it is greater than the freshness lifetime of 600 seconds and stale cached responses are not allowed
DEBUG Found stale response for: https://pypi.org/simple/sqlmodel/
DEBUG Sending revalidation request for: https://pypi.org/simple/sqlmodel/
TRACE Handling request for https://pypi.org/simple/sqlmodel/
TRACE Request for https://pypi.org/simple/sqlmodel/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sqlmodel/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sqlmodel/
TRACE not modified because old and new etag values ([34, 82, 70, 105, 108, 122, 112, 114, 100, 117, 115, 113, 55, 106, 107, 102, 82, 83, 83, 53, 102, 74, 81, 34]) match
DEBUG Found not-modified response for: https://pypi.org/simple/sqlmodel/
TRACE Received package metadata for: sqlmodel
TRACE selecting candidate for package sqlmodel with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } with 21 remote versions
TRACE found candidate for package PackageName("sqlmodel") with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } after 8 steps: "0.0.14" version
DEBUG Searching for a compatible version of sqlmodel (==0.0.14)
TRACE selecting candidate for package sqlmodel with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } with 21 remote versions
TRACE found candidate for package PackageName("sqlmodel") with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } after 8 steps: "0.0.14" version
DEBUG Selecting: sqlmodel==0.0.14 (sqlmodel-0.0.14-py3-none-any.whl)
TRACE cached request https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: sqlmodel==0.0.14
DEBUG Adding transitive dependency for sqlmodel==0.0.14: sqlalchemy>=2.0.0, <2.1.0
DEBUG Adding transitive dependency for sqlmodel==0.0.14: pydantic>=1.10.13, <3.0.0
TRACE Fetching metadata for sqlalchemy from https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Fetching metadata for pydantic from https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/sqlalchemy.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Handling request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Attempting unauthenticated request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/pydantic.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Handling request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Attempting unauthenticated request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://pkgs.dev.azure.com
TRACE Skipping fetch of credentials for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/, previous attempt failed
TRACE Fetching metadata for sqlalchemy from https://pypi.org/simple/sqlalchemy/
TRACE Request for https://pypi.org/simple/my-private-package/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for realm https://pypi.org
TRACE Skipping keyring lookup for https://pypi.org/simple/my-private-package/ with no username
TRACE Received package metadata for: my-private-package
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/ failed with 401 Unauthorized, checking for credentials
TRACE No credentials in cache for realm https://pkgs.dev.azure.com
TRACE Skipping fetch of credentials for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/, previous attempt failed
TRACE Fetching metadata for pydantic from https://pypi.org/simple/pydantic/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/sqlalchemy.rkyv
DEBUG No cache entry for: https://pypi.org/simple/sqlalchemy/
TRACE Sending fresh GET request for https://pypi.org/simple/sqlalchemy/
TRACE Handling request for https://pypi.org/simple/sqlalchemy/
TRACE Request for https://pypi.org/simple/sqlalchemy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sqlalchemy/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sqlalchemy/
DEBUG Searching for a compatible version of my-private-package (==1.0.0)
TRACE selecting candidate for package my-private-package with range Range { segments: [(Included("1.0.0"), Included("1.0.0"))] } with 0 remote versions
DEBUG No compatible version found for: my-private-package
  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-package was not found in the package registry and you require my-private-package==1.0.0, we can conclude that the requirements are unsatisfiable.
```

---

_Comment by @zanieb on 2024-05-13 15:43_

Thanks!

So this difference here is that `pip` is able to use `keyring`'s Python API which allows retrieval of both a username _and_ password. We can't use that, we're limited to the CLI API which only allows retrieval of a password _for a given username_. Since your URL does not include a username, we skip the keyring lookup:

> Skipping keyring lookup for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ with no username

If you add a username to your index URL, we'll look up the credentials as you'd expect. 

---

_Label `question` added by @zanieb on 2024-05-13 15:44_

---

_Comment by @AndreuCodina on 2024-05-13 16:06_

> Thanks!
> 
> So this difference here is that `pip` is able to use `keyring`'s Python API which allows retrieval of both a username _and_ password. We can't use that, we're limited to the CLI API which only allows retrieval of a password _for a given username_. Since your URL does not include a username, we skip the keyring lookup:
> 
> > Skipping keyring lookup for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ with no username
> 
> If you add a username to your index URL, we'll look up the credentials as you'd expect.

We don't have a username. Azure DevOps works with our company email (a Microsoft account), and when we install a private package using `pip`, a personal access token is created automatically with `keyring` to get access to the private feed.

---

_Comment by @zanieb on 2024-05-13 16:13_

Can you see the username that pip retrieves? It might just be a hard-coded string like "token".

Their keyring plugin does return a username https://github.com/microsoft/artifacts-keyring/blob/master/src/artifacts_keyring/plugin.py#L135-L136

If you're on the latest keyring version (v25.2.0) there's a new flag so you can view the username (https://github.com/jaraco/keyring/pull/678), e.g.

```
keyring get --mode creds <url>
```

---

_Comment by @AndreuCodina on 2024-05-13 16:27_

> Can you see the username that pip retrieves? It might just be a hard-coded string like "token".
> 
> Their keyring plugin does return a username https://github.com/microsoft/artifacts-keyring/blob/master/src/artifacts_keyring/plugin.py#L135-L136
> 
> If you're on the latest keyring version (v25.2.0) there's a new flag so you can view the username ([jaraco/keyring#678](https://github.com/jaraco/keyring/pull/678)), e.g.
> 
> ```
> keyring get --mode creds <url>
> ```

```bash
$ pip list | grep keyring

artifacts-keyring  0.3.5
keyring            25.2.0

$ keyring --mode creds get https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple

Traceback (most recent call last):
  File "/usr/local/bin/keyring", line 8, in <module>
    sys.exit(main())
             ^^^^^^
  File "/usr/local/lib/python3.12/site-packages/keyring/cli.py", line 212, in main
    return cli.run(argv)
           ^^^^^^^^^^^^^
  File "/usr/local/lib/python3.12/site-packages/keyring/cli.py", line 111, in run
    return method()
           ^^^^^^^^
  File "/usr/local/lib/python3.12/site-packages/keyring/cli.py", line 120, in do_get
    credential = getattr(self, f'_get_{self.get_mode}')()
                 ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
AttributeError: 'CommandLineTool' object has no attribute '_get_creds'. Did you mean: '_get_cred'?
```

---

_Comment by @zanieb on 2024-05-13 17:31_

Maybe like they implemented that wrong? cc @BakerNet 

You can use `python -c 'import keyring; keyring.get_credential("<url>", username=None)'` instead


---

_Comment by @BakerNet on 2024-05-13 17:57_

> Maybe like they implemented that wrong? cc @BakerNet 
> 
> You can use `python -c 'import keyring; keyring.get_credential("<url>", username=None)'` instead
> 

Looks like a typo in keyring @jaraco

`AttributeError: 'CommandLineTool' object has no attribute '_get_creds'. Did you mean: '_get_cred'?`

---

_Comment by @AndreuCodina on 2024-05-13 18:47_

> Maybe like they implemented that wrong? cc @BakerNet
> 
> You can use `python -c 'import keyring; keyring.get_credential("<url>", username=None)'` instead

It works, but no. If I don't wait some seconds after executing `keyring.get_credential`, it can't resolve the dependencies.

```bash
$ pip install --root-user-action=ignore uv && uv pip install --system keyring artifacts-keyring && python -c 'import keyring; keyring.get_credential("https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple", username=None)' && RUST_LOG=uv=trace uv pip install --system --keyring-provider subprocess --extra-index-url=https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple --requirement requirements.txt --verbose

Collecting uv
  Downloading uv-0.1.42-py3-none-manylinux_2_28_aarch64.whl.metadata (32 kB)
Downloading uv-0.1.42-py3-none-manylinux_2_28_aarch64.whl (12.0 MB)
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 12.0/12.0 MB 53.5 MB/s eta 0:00:00
Installing collected packages: uv
Successfully installed uv-0.1.42
Resolved 16 packages in 1.01s
Downloaded 16 packages in 323ms
Installed 16 packages in 8ms
 + artifacts-keyring==0.3.5
 + certifi==2024.2.2
 + cffi==1.16.0
 + charset-normalizer==3.3.2
 + cryptography==42.0.7
 + idna==3.7
 + jaraco-classes==3.4.0
 + jaraco-context==5.3.0
 + jaraco-functools==4.0.1
 + jeepney==0.8.0
 + keyring==25.2.0
 + more-itertools==10.2.0
 + pycparser==2.22
 + requests==2.31.0
 + secretstorage==3.3.3
 + urllib3==2.2.1
[Warning] [CredentialProvider]Warning: Cannot persist Microsoft authentication token cache securely!
[Warning] [CredentialProvider]Warning: Using plain-text fallback token cache
[Minimal] [CredentialProvider]DeviceFlow: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple
[Minimal] [CredentialProvider]ATTENTION: User interaction required.

    **********************************************************************

    To sign in, use a web browser to open the page https://microsoft.com/devicelogin and enter the code ... to authenticate.

    **********************************************************************

[Information] [CredentialProvider]VstsCredentialProvider - Acquired bearer token using 'MSAL Device Code'
[Information] [CredentialProvider]VstsCredentialProvider - Attempting to exchange the bearer token for an Azure DevOps session token.
DEBUG Starting interpreter discovery for default Python
DEBUG Cached interpreter info for Python 3.12.3, skipping probing: /usr/local/bin/python3
DEBUG Using Python 3.12.3 environment at /usr/local/bin/python3
DEBUG Trying to lock if free: /tmp/uv-39ee7c309557c5e9.lock
DEBUG At least one requirement is not satisfied: my-private-package==1.0.0
TRACE Caching credentials for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: sqlmodel==0.0.14
DEBUG Adding direct dependency: my-private-package==1.0.0
TRACE Fetching metadata for sqlmodel from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Fetching metadata for my-private-package from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/sqlmodel.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
DEBUG Checking keyring for credentials for VssSessionToken@https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Checking keyring for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/my-private-package.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
DEBUG Found credentials in keyring for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Using credentials from previous fetch for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Fetching metadata for sqlmodel from https://pypi.org/simple/sqlmodel/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/sqlmodel.rkyv
DEBUG No cache entry for: https://pypi.org/simple/sqlmodel/
TRACE Sending fresh GET request for https://pypi.org/simple/sqlmodel/
TRACE Handling request for https://pypi.org/simple/sqlmodel/
TRACE Request for https://pypi.org/simple/sqlmodel/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sqlmodel/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sqlmodel/
TRACE cached request https://pypi.org/simple/sqlmodel/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: sqlmodel
TRACE selecting candidate for package sqlmodel with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } with 21 remote versions
TRACE found candidate for package PackageName("sqlmodel") with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } after 8 steps: "0.0.14" version
DEBUG Searching for a compatible version of sqlmodel (==0.0.14)
TRACE selecting candidate for package sqlmodel with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } with 21 remote versions
TRACE found candidate for package PackageName("sqlmodel") with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } after 8 steps: "0.0.14" version
DEBUG Selecting: sqlmodel==0.0.14 (sqlmodel-0.0.14-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/sqlmodel/sqlmodel-0.0.14-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: sqlmodel==0.0.14
DEBUG Adding transitive dependency for sqlmodel==0.0.14: sqlalchemy>=2.0.0, <2.1.0
DEBUG Adding transitive dependency for sqlmodel==0.0.14: pydantic>=1.10.13, <3.0.0
TRACE Fetching metadata for sqlalchemy from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Fetching metadata for pydantic from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/sqlalchemy.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE Using credentials from previous fetch for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/pydantic.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE Using credentials from previous fetch for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Fetching metadata for my-private-package from https://pypi.org/simple/my-private-package/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/my-private-package.rkyv
DEBUG No cache entry for: https://pypi.org/simple/my-private-package/
TRACE Sending fresh GET request for https://pypi.org/simple/my-private-package/
TRACE Handling request for https://pypi.org/simple/my-private-package/
TRACE Request for https://pypi.org/simple/my-private-package/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/my-private-package/
TRACE Attempting unauthenticated request for https://pypi.org/simple/my-private-package/
TRACE Fetching metadata for pydantic from https://pypi.org/simple/pydantic/
TRACE Fetching metadata for sqlalchemy from https://pypi.org/simple/sqlalchemy/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/pydantic.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pydantic/
TRACE Sending fresh GET request for https://pypi.org/simple/pydantic/
TRACE Handling request for https://pypi.org/simple/pydantic/
TRACE Request for https://pypi.org/simple/pydantic/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pydantic/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pydantic/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/sqlalchemy.rkyv
DEBUG No cache entry for: https://pypi.org/simple/sqlalchemy/
TRACE Sending fresh GET request for https://pypi.org/simple/sqlalchemy/
TRACE Handling request for https://pypi.org/simple/sqlalchemy/
TRACE Request for https://pypi.org/simple/sqlalchemy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sqlalchemy/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sqlalchemy/
TRACE cached request https://pypi.org/simple/pydantic/ is storable because its response has a 'public' cache-control directive
TRACE cached request https://pypi.org/simple/sqlalchemy/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: pydantic
TRACE Request for https://pypi.org/simple/my-private-package/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for realm https://pypi.org
TRACE Skipping keyring lookup for https://pypi.org/simple/my-private-package/ with no username
TRACE Received package metadata for: my-private-package
TRACE selecting candidate for package pydantic with range Range { segments: [(Included("1.10.13"), Excluded("3.0.0"))] } with 139 remote versions
TRACE found candidate for package PackageName("pydantic") with range Range { segments: [(Included("1.10.13"), Excluded("3.0.0"))] } after 1 steps: "2.7.1" version
DEBUG Searching for a compatible version of my-private-package (==1.0.0)
TRACE selecting candidate for package my-private-package with range Range { segments: [(Included("1.0.0"), Included("1.0.0"))] } with 0 remote versions
DEBUG No compatible version found for: my-private-package
  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-package was not found in the package registry and you require my-private-package==1.0.0, we can conclude that the requirements are unsatisfiable.
```

**After waiting some seconds:**

```bash
RUST_LOG=uv=trace uv pip install --system --keyring-provider subprocess --extra-index-url=https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple --requirement requirements.txt --verbose

DEBUG Starting interpreter discovery for default Python
DEBUG Cached interpreter info for Python 3.12.3, skipping probing: /usr/local/bin/python3
DEBUG Using Python 3.12.3 environment at /usr/local/bin/python3
DEBUG Trying to lock if free: /tmp/uv-39ee7c309557c5e9.lock
DEBUG At least one requirement is not satisfied: my-private-package==1.0.0
TRACE Caching credentials for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: sqlmodel==0.0.14
DEBUG Adding direct dependency: my-private-package==1.0.0
TRACE Fetching metadata for sqlmodel from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Fetching metadata for my-private-package from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/sqlmodel.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
DEBUG Checking keyring for credentials for VssSessionToken@https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Checking keyring for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/my-private-package.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
DEBUG Found credentials in keyring for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Using credentials from previous fetch for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Fetching metadata for sqlmodel from https://pypi.org/simple/sqlmodel/
TRACE Updating cached credentials for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ to Credentials { username: Username(Some("VssSessionToken")), password: Some("...") }
TRACE cached request https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ is not storable because its response has a 'no-store' cache-control directive
TRACE Received package metadata for: my-private-package
TRACE cached request https://pypi.org/simple/sqlmodel/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/sqlmodel/
TRACE Received package metadata for: sqlmodel
TRACE selecting candidate for package my-private-package with range Range { segments: [(Included("1.0.0"), Included("1.0.0"))] } with 2 remote versions
TRACE found candidate for package PackageName("my-private-package") with range Range { segments: [(Included("1.0.0"), Included("1.0.0"))] } after 1 steps: "1.0.0" version
TRACE selecting candidate for package sqlmodel with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } with 21 remote versions
TRACE found candidate for package PackageName("sqlmodel") with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } after 8 steps: "0.0.14" version
DEBUG Searching for a compatible version of sqlmodel (==0.0.14)
TRACE selecting candidate for package sqlmodel with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } with 21 remote versions
TRACE found candidate for package PackageName("sqlmodel") with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } after 8 steps: "0.0.14" version
DEBUG Selecting: sqlmodel==0.0.14 (sqlmodel-0.0.14-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/index/29fb3946eb316a55/my-private-package/my_private_package-1.0.0-py3-none-any.msgpack
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Sending fresh HEAD request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Handling request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Attempting unauthenticated request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE cached request https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Received built distribution metadata for: sqlmodel==0.0.14
DEBUG Adding transitive dependency for sqlmodel==0.0.14: sqlalchemy>=2.0.0, <2.1.0
DEBUG Adding transitive dependency for sqlmodel==0.0.14: pydantic>=1.10.13, <3.0.0
DEBUG Searching for a compatible version of my-private-package (==1.0.0)
TRACE selecting candidate for package my-private-package with range Range { segments: [(Included("1.0.0"), Included("1.0.0"))] } with 2 remote versions
TRACE found candidate for package PackageName("my-private-package") with range Range { segments: [(Included("1.0.0"), Included("1.0.0"))] } after 1 steps: "1.0.0" version
DEBUG Selecting: my-private-package==1.0.0 (my_private_package-1.0.0-py3-none-any.whl)
TRACE Fetching metadata for sqlalchemy from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Fetching metadata for pydantic from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/sqlalchemy.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/pydantic.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
WARN Range requests not supported for my_private_package-1.0.0-py3-none-any.whl; streaming wheel
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/index/29fb3946eb316a55/my-private-package/my_private_package-1.0.0-py3-none-any.msgpack
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Handling request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Attempting unauthenticated request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Fetching metadata for sqlalchemy from https://pypi.org/simple/sqlalchemy/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/sqlalchemy.rkyv
DEBUG No cache entry for: https://pypi.org/simple/sqlalchemy/
TRACE Sending fresh GET request for https://pypi.org/simple/sqlalchemy/
TRACE Handling request for https://pypi.org/simple/sqlalchemy/
TRACE Request for https://pypi.org/simple/sqlalchemy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sqlalchemy/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sqlalchemy/
TRACE cached request https://pypi.org/simple/sqlalchemy/ is storable because its response has a 'public' cache-control directive
TRACE Fetching metadata for pydantic from https://pypi.org/simple/pydantic/
TRACE cached request https://pypi.org/simple/pydantic/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/pydantic/
TRACE Received package metadata for: pydantic
TRACE selecting candidate for package pydantic with range Range { segments: [(Included("1.10.13"), Excluded("3.0.0"))] } with 139 remote versions
TRACE found candidate for package PackageName("pydantic") with range Range { segments: [(Included("1.10.13"), Excluded("3.0.0"))] } after 1 steps: "2.7.1" version
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/pydantic/pydantic-2.7.1-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ed/76/9a17032880ed27f2dbd490c77a3431cbc80f47ba81534131de3c2846e736/pydantic-2.7.1-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/ed/76/9a17032880ed27f2dbd490c77a3431cbc80f47ba81534131de3c2846e736/pydantic-2.7.1-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/ed/76/9a17032880ed27f2dbd490c77a3431cbc80f47ba81534131de3c2846e736/pydantic-2.7.1-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/ed/76/9a17032880ed27f2dbd490c77a3431cbc80f47ba81534131de3c2846e736/pydantic-2.7.1-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/ed/76/9a17032880ed27f2dbd490c77a3431cbc80f47ba81534131de3c2846e736/pydantic-2.7.1-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/ed/76/9a17032880ed27f2dbd490c77a3431cbc80f47ba81534131de3c2846e736/pydantic-2.7.1-py3-none-any.whl.metadata
TRACE Received package metadata for: sqlalchemy
TRACE selecting candidate for package sqlalchemy with range Range { segments: [(Included("2.0.0"), Excluded("2.1.0"))] } with 299 remote versions
TRACE found candidate for package PackageName("sqlalchemy") with range Range { segments: [(Included("2.0.0"), Excluded("2.1.0"))] } after 1 steps: "2.0.30" version
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/sqlalchemy/sqlalchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0 failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://pkgs.dev.azure.com
TRACE Retrying request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0 with credentials from cache Credentials { username: Username(Some("VssSessionToken")), password: Some("...") }
TRACE cached request https://files.pythonhosted.org/packages/ed/76/9a17032880ed27f2dbd490c77a3431cbc80f47ba81534131de3c2846e736/pydantic-2.7.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: pydantic==2.7.1
TRACE Received built distribution metadata for: sqlalchemy==2.0.30
TRACE cached request https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0 is storable because its response has a heuristically cacheable status code 200
TRACE Received built distribution metadata for: my-private-package==1.0.0
DEBUG Adding transitive dependency for my-private-package==1.0.0: httpx==0.26.0
DEBUG Adding transitive dependency for my-private-package==1.0.0: pydantic==2.5.3
DEBUG Searching for a compatible version of pydantic (==2.5.3)
TRACE selecting candidate for package pydantic with range Range { segments: [(Included("2.5.3"), Included("2.5.3"))] } with 139 remote versions
TRACE found candidate for package PackageName("pydantic") with range Range { segments: [(Included("2.5.3"), Included("2.5.3"))] } after 10 steps: "2.5.3" version
DEBUG Selecting: pydantic==2.5.3 (pydantic-2.5.3-py3-none-any.whl)
TRACE Fetching metadata for httpx from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpx/
TRACE selecting candidate for package pydantic with range Range { segments: [(Included("2.5.3"), Included("2.5.3"))] } with 139 remote versions
TRACE found candidate for package PackageName("pydantic") with range Range { segments: [(Included("2.5.3"), Included("2.5.3"))] } after 10 steps: "2.5.3" version
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/httpx.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpx/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpx/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpx/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpx/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/pydantic/pydantic-2.5.3-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: pydantic==2.5.3
DEBUG Adding transitive dependency for pydantic==2.5.3: annotated-types>=0.4.0
DEBUG Adding transitive dependency for pydantic==2.5.3: pydantic-core==2.14.6
DEBUG Adding transitive dependency for pydantic==2.5.3: typing-extensions>=4.6.1
TRACE Fetching metadata for annotated-types from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/annotated-types/
TRACE Fetching metadata for pydantic-core from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic-core/
TRACE Fetching metadata for typing-extensions from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/typing-extensions/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/annotated-types.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/annotated-types/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/annotated-types/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/annotated-types/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/annotated-types/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/pydantic-core.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic-core/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic-core/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic-core/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic-core/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/typing-extensions.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/typing-extensions/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/typing-extensions/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/typing-extensions/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/typing-extensions/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE Fetching metadata for httpx from https://pypi.org/simple/httpx/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/httpx.rkyv
DEBUG No cache entry for: https://pypi.org/simple/httpx/
TRACE Sending fresh GET request for https://pypi.org/simple/httpx/
TRACE Handling request for https://pypi.org/simple/httpx/
TRACE Request for https://pypi.org/simple/httpx/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/httpx/
TRACE Attempting unauthenticated request for https://pypi.org/simple/httpx/
TRACE Fetching metadata for pydantic-core from https://pypi.org/simple/pydantic-core/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/pydantic-core.rkyv
DEBUG No cache entry for: https://pypi.org/simple/pydantic-core/
TRACE Sending fresh GET request for https://pypi.org/simple/pydantic-core/
TRACE Handling request for https://pypi.org/simple/pydantic-core/
TRACE Request for https://pypi.org/simple/pydantic-core/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/pydantic-core/
TRACE Attempting unauthenticated request for https://pypi.org/simple/pydantic-core/
TRACE cached request https://pypi.org/simple/httpx/ is storable because its response has a 'public' cache-control directive
TRACE Fetching metadata for annotated-types from https://pypi.org/simple/annotated-types/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/annotated-types.rkyv
DEBUG No cache entry for: https://pypi.org/simple/annotated-types/
TRACE Sending fresh GET request for https://pypi.org/simple/annotated-types/
TRACE Handling request for https://pypi.org/simple/annotated-types/
TRACE Request for https://pypi.org/simple/annotated-types/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/annotated-types/
TRACE Attempting unauthenticated request for https://pypi.org/simple/annotated-types/
TRACE Received package metadata for: httpx
TRACE selecting candidate for package httpx with range Range { segments: [(Included("0.26.0"), Included("0.26.0"))] } with 67 remote versions
TRACE found candidate for package PackageName("httpx") with range Range { segments: [(Included("0.26.0"), Included("0.26.0"))] } after 3 steps: "0.26.0" version
DEBUG Searching for a compatible version of httpx (==0.26.0)
TRACE selecting candidate for package httpx with range Range { segments: [(Included("0.26.0"), Included("0.26.0"))] } with 67 remote versions
TRACE found candidate for package PackageName("httpx") with range Range { segments: [(Included("0.26.0"), Included("0.26.0"))] } after 3 steps: "0.26.0" version
DEBUG Selecting: httpx==0.26.0 (httpx-0.26.0-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/httpx/httpx-0.26.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl.metadata
TRACE cached request https://pypi.org/simple/pydantic-core/ is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: httpx==0.26.0
DEBUG Adding transitive dependency for httpx==0.26.0: anyio*
DEBUG Adding transitive dependency for httpx==0.26.0: certifi*
DEBUG Adding transitive dependency for httpx==0.26.0: httpcore>=1.dev0, <2.dev0
DEBUG Adding transitive dependency for httpx==0.26.0: idna*
DEBUG Adding transitive dependency for httpx==0.26.0: sniffio*
TRACE Fetching metadata for anyio from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/anyio/
TRACE Fetching metadata for certifi from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/certifi/
TRACE Fetching metadata for httpcore from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpcore/
TRACE Fetching metadata for idna from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/idna/
TRACE Fetching metadata for sniffio from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sniffio/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/anyio.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/anyio/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/anyio/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/anyio/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/anyio/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/certifi.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/certifi/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/certifi/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/certifi/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/certifi/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/httpcore.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpcore/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpcore/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpcore/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/httpcore/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/idna.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/idna/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/idna/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/idna/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/idna/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/sniffio.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sniffio/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sniffio/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sniffio/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sniffio/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE cached request https://pypi.org/simple/annotated-types/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: annotated-types
TRACE selecting candidate for package annotated-types with range Range { segments: [(Included("0.4.0"), Unbounded)] } with 7 remote versions
TRACE found candidate for package PackageName("annotated-types") with range Range { segments: [(Included("0.4.0"), Unbounded)] } after 1 steps: "0.6.0" version
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/annotated-types/annotated_types-0.6.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl.metadata
TRACE Received package metadata for: pydantic-core
TRACE selecting candidate for package pydantic-core with range Range { segments: [(Included("2.14.6"), Included("2.14.6"))] } with 97 remote versions
TRACE found candidate for package PackageName("pydantic-core") with range Range { segments: [(Included("2.14.6"), Included("2.14.6"))] } after 10 steps: "2.14.6" version
DEBUG Searching for a compatible version of pydantic-core (==2.14.6)
TRACE selecting candidate for package pydantic-core with range Range { segments: [(Included("2.14.6"), Included("2.14.6"))] } with 97 remote versions
TRACE found candidate for package PackageName("pydantic-core") with range Range { segments: [(Included("2.14.6"), Included("2.14.6"))] } after 10 steps: "2.14.6" version
DEBUG Selecting: pydantic-core==2.14.6 (pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/pydantic-core/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: annotated-types==0.6.0
TRACE cached request https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: pydantic-core==2.14.6
DEBUG Adding transitive dependency for pydantic-core==2.14.6: typing-extensions>=4.6.0, <4.7.0 | >4.7.0
DEBUG Searching for a compatible version of sqlalchemy (>=2.0.0, <2.1.0)
TRACE selecting candidate for package sqlalchemy with range Range { segments: [(Included("2.0.0"), Excluded("2.1.0"))] } with 299 remote versions
TRACE found candidate for package PackageName("sqlalchemy") with range Range { segments: [(Included("2.0.0"), Excluded("2.1.0"))] } after 1 steps: "2.0.30" version
DEBUG Selecting: sqlalchemy==2.0.30 (SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
DEBUG Adding transitive dependency for sqlalchemy==2.0.30: typing-extensions>=4.6.0
DEBUG Adding transitive dependency for sqlalchemy==2.0.30: greenlet<0.4.17 | >0.4.17
DEBUG Searching for a compatible version of annotated-types (>=0.4.0)
TRACE selecting candidate for package annotated-types with range Range { segments: [(Included("0.4.0"), Unbounded)] } with 7 remote versions
TRACE found candidate for package PackageName("annotated-types") with range Range { segments: [(Included("0.4.0"), Unbounded)] } after 1 steps: "0.6.0" version
DEBUG Selecting: annotated-types==0.6.0 (annotated_types-0.6.0-py3-none-any.whl)
TRACE Fetching metadata for greenlet from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/greenlet/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/greenlet.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/greenlet/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/greenlet/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/greenlet/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/greenlet/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE Fetching metadata for typing-extensions from https://pypi.org/simple/typing-extensions/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/typing-extensions.rkyv
DEBUG No cache entry for: https://pypi.org/simple/typing-extensions/
TRACE Sending fresh GET request for https://pypi.org/simple/typing-extensions/
TRACE Handling request for https://pypi.org/simple/typing-extensions/
TRACE Request for https://pypi.org/simple/typing-extensions/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/typing-extensions/
TRACE Attempting unauthenticated request for https://pypi.org/simple/typing-extensions/
TRACE cached request https://pypi.org/simple/typing-extensions/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: typing-extensions
TRACE selecting candidate for package typing-extensions with range Range { segments: [(Included("4.6.1"), Excluded("4.7.0")), (Excluded("4.7.0"), Unbounded)] } with 35 remote versions
TRACE found candidate for package PackageName("typing-extensions") with range Range { segments: [(Included("4.6.1"), Excluded("4.7.0")), (Excluded("4.7.0"), Unbounded)] } after 1 steps: "4.11.0" version
TRACE selecting candidate for package typing-extensions with range Range { segments: [(Included("4.6.1"), Unbounded)] } with 35 remote versions
TRACE found candidate for package PackageName("typing-extensions") with range Range { segments: [(Included("4.6.1"), Unbounded)] } after 1 steps: "4.11.0" version
DEBUG Searching for a compatible version of typing-extensions (>=4.6.1, <4.7.0 | >4.7.0)
TRACE selecting candidate for package typing-extensions with range Range { segments: [(Included("4.6.1"), Excluded("4.7.0")), (Excluded("4.7.0"), Unbounded)] } with 35 remote versions
TRACE found candidate for package PackageName("typing-extensions") with range Range { segments: [(Included("4.6.1"), Excluded("4.7.0")), (Excluded("4.7.0"), Unbounded)] } after 1 steps: "4.11.0" version
DEBUG Selecting: typing-extensions==4.11.0 (typing_extensions-4.11.0-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/typing-extensions/typing_extensions-4.11.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: typing-extensions==4.11.0
TRACE Fetching metadata for httpcore from https://pypi.org/simple/httpcore/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/httpcore.rkyv
DEBUG No cache entry for: https://pypi.org/simple/httpcore/
TRACE Sending fresh GET request for https://pypi.org/simple/httpcore/
TRACE Handling request for https://pypi.org/simple/httpcore/
TRACE Request for https://pypi.org/simple/httpcore/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/httpcore/
TRACE Attempting unauthenticated request for https://pypi.org/simple/httpcore/
TRACE Fetching metadata for certifi from https://pypi.org/simple/certifi/
TRACE cached request https://pypi.org/simple/certifi/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/certifi/
TRACE Received package metadata for: certifi
DEBUG Found installed version of certifi==2024.2.2 that satisfies preference in *
TRACE Received installed distribution metadata for: certifi==2024.2.2
TRACE cached request https://pypi.org/simple/httpcore/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: httpcore
TRACE selecting candidate for package httpcore with range Range { segments: [(Included("1.dev0"), Excluded("2.dev0"))] } with 60 remote versions
TRACE found candidate for package PackageName("httpcore") with range Range { segments: [(Included("1.dev0"), Excluded("2.dev0"))] } after 1 steps: "1.0.5" version
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/httpcore/httpcore-1.0.5-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl.metadata
TRACE Fetching metadata for anyio from https://pypi.org/simple/anyio/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/anyio.rkyv
DEBUG No cache entry for: https://pypi.org/simple/anyio/
TRACE Sending fresh GET request for https://pypi.org/simple/anyio/
TRACE Handling request for https://pypi.org/simple/anyio/
TRACE Request for https://pypi.org/simple/anyio/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/anyio/
TRACE Attempting unauthenticated request for https://pypi.org/simple/anyio/
TRACE cached request https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: httpcore==1.0.5
TRACE cached request https://pypi.org/simple/anyio/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: anyio
TRACE selecting candidate for package anyio with range Range { segments: [(Unbounded, Unbounded)] } with 51 remote versions
TRACE found candidate for package PackageName("anyio") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "4.3.0" version
DEBUG Searching for a compatible version of anyio (*)
TRACE selecting candidate for package anyio with range Range { segments: [(Unbounded, Unbounded)] } with 51 remote versions
TRACE found candidate for package PackageName("anyio") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "4.3.0" version
DEBUG Selecting: anyio==4.3.0 (anyio-4.3.0-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/anyio/anyio-4.3.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: anyio==4.3.0
DEBUG Adding transitive dependency for anyio==4.3.0: idna>=2.8
DEBUG Adding transitive dependency for anyio==4.3.0: sniffio>=1.1
DEBUG Searching for a compatible version of certifi (*)
DEBUG Found installed version of certifi==2024.2.2 that satisfies preference in *
DEBUG Selecting: certifi==2024.2.2 (installed)
DEBUG Searching for a compatible version of httpcore (>=1.dev0, <2.dev0)
TRACE selecting candidate for package httpcore with range Range { segments: [(Included("1.dev0"), Excluded("2.dev0"))] } with 60 remote versions
TRACE found candidate for package PackageName("httpcore") with range Range { segments: [(Included("1.dev0"), Excluded("2.dev0"))] } after 1 steps: "1.0.5" version
DEBUG Selecting: httpcore==1.0.5 (httpcore-1.0.5-py3-none-any.whl)
DEBUG Adding transitive dependency for httpcore==1.0.5: certifi*
DEBUG Adding transitive dependency for httpcore==1.0.5: h11>=0.13, <0.15
TRACE Fetching metadata for h11 from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/h11/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/h11.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/h11/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/h11/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/h11/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/h11/ is missing a password, looking for credentials
TRACE Found cached credentials for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE Fetching metadata for idna from https://pypi.org/simple/idna/
TRACE cached request https://pypi.org/simple/idna/ is storable because its response has a 'public' cache-control directive
DEBUG Found fresh response for: https://pypi.org/simple/idna/
TRACE Received package metadata for: idna
DEBUG Found installed version of idna==3.7 that satisfies preference in >=2.8
TRACE Received installed distribution metadata for: idna==3.7
DEBUG Found installed version of idna==3.7 that satisfies preference in *
DEBUG Searching for a compatible version of idna (>=2.8)
DEBUG Found installed version of idna==3.7 that satisfies preference in >=2.8
DEBUG Selecting: idna==3.7 (installed)
TRACE Fetching metadata for h11 from https://pypi.org/simple/h11/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/h11.rkyv
DEBUG No cache entry for: https://pypi.org/simple/h11/
TRACE Sending fresh GET request for https://pypi.org/simple/h11/
TRACE Handling request for https://pypi.org/simple/h11/
TRACE Request for https://pypi.org/simple/h11/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/h11/
TRACE Attempting unauthenticated request for https://pypi.org/simple/h11/
TRACE cached request https://pypi.org/simple/h11/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: h11
TRACE selecting candidate for package h11 with range Range { segments: [(Included("0.13"), Excluded("0.15"))] } with 11 remote versions
TRACE found candidate for package PackageName("h11") with range Range { segments: [(Included("0.13"), Excluded("0.15"))] } after 1 steps: "0.14.0" version
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/h11/h11-0.14.0-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: h11==0.14.0
TRACE Fetching metadata for sniffio from https://pypi.org/simple/sniffio/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/sniffio.rkyv
DEBUG No cache entry for: https://pypi.org/simple/sniffio/
TRACE Sending fresh GET request for https://pypi.org/simple/sniffio/
TRACE Handling request for https://pypi.org/simple/sniffio/
TRACE Request for https://pypi.org/simple/sniffio/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sniffio/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sniffio/
TRACE cached request https://pypi.org/simple/sniffio/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: sniffio
TRACE selecting candidate for package sniffio with range Range { segments: [(Included("1.1"), Unbounded)] } with 6 remote versions
TRACE found candidate for package PackageName("sniffio") with range Range { segments: [(Included("1.1"), Unbounded)] } after 1 steps: "1.3.1" version
TRACE selecting candidate for package sniffio with range Range { segments: [(Unbounded, Unbounded)] } with 6 remote versions
TRACE found candidate for package PackageName("sniffio") with range Range { segments: [(Unbounded, Unbounded)] } after 1 steps: "1.3.1" version
DEBUG Searching for a compatible version of sniffio (>=1.1)
TRACE selecting candidate for package sniffio with range Range { segments: [(Included("1.1"), Unbounded)] } with 6 remote versions
TRACE found candidate for package PackageName("sniffio") with range Range { segments: [(Included("1.1"), Unbounded)] } after 1 steps: "1.3.1" version
DEBUG Selecting: sniffio==1.3.1 (sniffio-1.3.1-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/sniffio/sniffio-1.3.1-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: sniffio==1.3.1
TRACE Fetching metadata for greenlet from https://pypi.org/simple/greenlet/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/greenlet.rkyv
DEBUG No cache entry for: https://pypi.org/simple/greenlet/
TRACE Sending fresh GET request for https://pypi.org/simple/greenlet/
TRACE Handling request for https://pypi.org/simple/greenlet/
TRACE Request for https://pypi.org/simple/greenlet/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/greenlet/
TRACE Attempting unauthenticated request for https://pypi.org/simple/greenlet/
TRACE cached request https://pypi.org/simple/greenlet/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: greenlet
TRACE selecting candidate for package greenlet with range Range { segments: [(Unbounded, Excluded("0.4.17")), (Excluded("0.4.17"), Unbounded)] } with 50 remote versions
TRACE found candidate for package PackageName("greenlet") with range Range { segments: [(Unbounded, Excluded("0.4.17")), (Excluded("0.4.17"), Unbounded)] } after 1 steps: "3.0.3" version
DEBUG Searching for a compatible version of greenlet (<0.4.17 | >0.4.17)
TRACE selecting candidate for package greenlet with range Range { segments: [(Unbounded, Excluded("0.4.17")), (Excluded("0.4.17"), Unbounded)] } with 50 remote versions
TRACE found candidate for package PackageName("greenlet") with range Range { segments: [(Unbounded, Excluded("0.4.17")), (Excluded("0.4.17"), Unbounded)] } after 1 steps: "3.0.3" version
DEBUG Selecting: greenlet==3.0.3 (greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/greenlet/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: greenlet==3.0.3
DEBUG Searching for a compatible version of h11 (>=0.13, <0.15)
TRACE selecting candidate for package h11 with range Range { segments: [(Included("0.13"), Excluded("0.15"))] } with 11 remote versions
TRACE found candidate for package PackageName("h11") with range Range { segments: [(Included("0.13"), Excluded("0.15"))] } after 1 steps: "0.14.0" version
DEBUG Selecting: h11==0.14.0 (h11-0.14.0-py3-none-any.whl)
DEBUG Tried 16 versions: annotated-types 1, anyio 1, my-private-package 1, certifi 1, greenlet 1, h11 1, httpcore 1, httpx 1, idna 1, pydantic 1, pydantic-core 1, root 1, sniffio 1, sqlalchemy 1, sqlmodel 1, typing-extensions 1
Resolved 15 packages in 2.35s
DEBUG Identified uncached requirement: annotated-types==0.6.0
DEBUG Identified uncached requirement: anyio==4.3.0
DEBUG Identified uncached requirement: my-private-package==1.0.0
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("certifi"), version: "2024.2.2", path: "/usr/local/lib/python3.12/site-packages/certifi-2024.2.2.dist-info" }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "2024.2.2" }]), index: None }
DEBUG Requirement already installed: certifi==2024.2.2
DEBUG Identified uncached requirement: greenlet==3.0.3
DEBUG Identified uncached requirement: h11==0.14.0
DEBUG Identified uncached requirement: httpcore==1.0.5
DEBUG Identified uncached requirement: httpx==0.26.0
TRACE Comparing installed with source: Registry(InstalledRegistryDist { name: PackageName("idna"), version: "3.7", path: "/usr/local/lib/python3.12/site-packages/idna-3.7.dist-info" }) Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: Equal, version: "3.7" }]), index: None }
DEBUG Requirement already installed: idna==3.7
DEBUG Identified uncached requirement: pydantic==2.5.3
DEBUG Identified uncached requirement: pydantic-core==2.14.6
DEBUG Identified uncached requirement: sniffio==1.3.1
DEBUG Identified uncached requirement: sqlalchemy==2.0.30
DEBUG Identified uncached requirement: sqlmodel==0.0.14
DEBUG Identified uncached requirement: typing-extensions==4.11.0
DEBUG Unnecessary package: secretstorage==3.3.3
DEBUG Unnecessary package: artifacts-keyring==0.3.5
DEBUG Unnecessary package: cffi==1.16.0
DEBUG Unnecessary package: charset-normalizer==3.3.2
DEBUG Unnecessary package: cryptography==42.0.7
DEBUG Unnecessary package: jaraco-classes==3.4.0
DEBUG Unnecessary package: jaraco-context==5.3.0
DEBUG Unnecessary package: jaraco-functools==4.0.1
DEBUG Unnecessary package: jeepney==0.8.0
DEBUG Unnecessary package: keyring==25.2.0
DEBUG Unnecessary package: more-itertools==10.2.0
DEBUG Preserving seed package: pip==24.0
DEBUG Unnecessary package: pycparser==2.22
DEBUG Unnecessary package: requests==2.31.0
DEBUG Preserving seed package: setuptools==69.5.1
DEBUG Unnecessary package: urllib3==2.2.1
DEBUG Preserving seed package: uv==0.1.42
DEBUG Preserving seed package: wheel==0.43.0
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/index/29fb3946eb316a55/my-private-package/my_private_package-1.0.0-py3-none-any.http
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Handling request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0 is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE Attempting unauthenticated request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/sqlalchemy/sqlalchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Handling request for https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Request for https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/pydantic-core/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Handling request for https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Request for https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/greenlet/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Handling request for https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Request for https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/pydantic/pydantic-2.5.3-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/anyio/anyio-4.3.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/httpcore/httpcore-1.0.5-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/h11/h11-0.14.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/httpx/httpx-0.26.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/typing-extensions/typing_extensions-4.11.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/sqlmodel/sqlmodel-0.0.14-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/annotated-types/annotated_types-0.6.0-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/sniffio/sniffio-1.3.1-py3-none-any.http
DEBUG No cache entry for: https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE Handling request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE Request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl
TRACE cached request https://files.pythonhosted.org/packages/5f/92/db44ea3953e1f3b81a9c2a2852aa7542839da3300e50ee5615a67c3932b0/SQLAlchemy-2.0.30-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/ba/98/fb42628ed811643c364e05353d3a015c74859402994420aeba8e3e34a54c/pydantic_core-2.14.6-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl is storable because its response has a 'public' cache-control directive
TRACE Request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0 failed with 401 Unauthorized, checking for credentials
TRACE Found cached credentials for realm https://pkgs.dev.azure.com
TRACE Retrying request for https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0 with credentials from cache Credentials { username: Username(Some("VssSessionToken")), password: Some("...") }
TRACE cached request https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/01/f3/936e209267d6ef7510322191003885de524fc48d1b43269810cd589ceaf5/typing_extensions-4.11.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/e9/55/2c3cfa3cdbb940cf7321fbcf544f0e9c74898eed43bf678abf416812d132/greenlet-3.0.3-cp312-cp312-manylinux_2_17_aarch64.manylinux2014_aarch64.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/39/9b/4937d841aee9c2c8102d9a4eeb800c7dad25386caabb4a1bf5010df81a57/httpx-0.26.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/28/78/d31230046e58c207284c6b2c4e8d96e6d3cb4e52354721b944d3e1ee4aa5/annotated_types-0.6.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/14/fd/2f20c40b45e4fb4324834aea24bd4afdf1143390242c0b33774da0e2e34f/anyio-4.3.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/e9/44/75a9c9421471a6c4805dbf2356f7c181a29c1879239abab1ea2cc8f38b40/sniffio-1.3.1-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/78/d4/e5d7e4f2174f8a4d63c8897d79eb8fe2503f7ecc03282fee1fa2719c2704/httpcore-1.0.5-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/95/04/ff642e65ad6b90db43e668d70ffb6736436c7ce41fcc549f4e9472234127/h11-0.14.0-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://files.pythonhosted.org/packages/dd/b7/9aea7ee6c01fe3f3c03b8ca3c7797c866df5fecece9d6cb27caa138db2e2/pydantic-2.5.3-py3-none-any.whl is storable because its response has a 'public' cache-control directive
TRACE cached request https://pkgs.dev.azure.com/mycompany/_packaging/d1ba2728-1239-4be4-88aa-17a0e38a23cb/pypi/download/my-private-package/1.0.0/my_private_package-1.0.0-py3-none-any.whl#sha256=37a6cab6bccf58e83cb0b697b7550c646bb4f9d1df2b95e31bbcc5d5e51386a0 is storable because its response has a heuristically cacheable status code 200
Downloaded 13 packages in 491ms
Installed 13 packages in 12ms
 + annotated-types==0.6.0
 + anyio==4.3.0
 + my-private-package==1.0.0
 + greenlet==3.0.3
 + h11==0.14.0
 + httpcore==1.0.5
 + httpx==0.26.0
 + pydantic==2.5.3
 + pydantic-core==2.14.6
 + sniffio==1.3.1
 + sqlalchemy==2.0.30
 + sqlmodel==0.0.14
 + typing-extensions==4.11.0
```



---

_Comment by @jaraco on 2024-05-13 19:02_

> > Maybe like they implemented that wrong? cc @BakerNet
> > You can use `python -c 'import keyring; keyring.get_credential("<url>", username=None)'` instead
> 
> Looks like a typo in keyring @jaraco
> 
> `AttributeError: 'CommandLineTool' object has no attribute '_get_creds'. Did you mean: '_get_cred'?`

Fixed in v25.2.1. Probably need to get better coverage on that CLI module.

---

_Comment by @charliermarsh on 2024-05-13 19:02_

Thanks @jaraco 

---

_Comment by @AndreuCodina on 2024-05-13 20:00_

After installing the new version of `keyring` and using the username `VssSessionToken` in the private feed URL, I'm getting this error:

```
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
```

The difference is that with `pip`, the authentication is required through a link, but no with `uv`.

Full log:

```bash
$ pip install --root-user-action=ignore uv && uv pip install --system keyring artifacts-keyring

$ pip list | grep keyring

artifacts-keyring  0.3.5
keyring            25.2.1

$ RUST_LOG=uv=trace uv pip install --system --keyring-provider subprocess --extra-index-url=https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple --requirement requirements.txt --verbose

DEBUG Starting interpreter discovery for default Python
DEBUG Cached interpreter info for Python 3.12.3, skipping probing: /usr/local/bin/python3
DEBUG Using Python 3.12.3 environment at /usr/local/bin/python3
DEBUG Trying to lock if free: /tmp/uv-39ee7c309557c5e9.lock
DEBUG At least one requirement is not satisfied: my-private-package==1.0.0
TRACE Caching credentials for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple
DEBUG Using registry request timeout of 30s
DEBUG Solving with target Python version 3.12.3
DEBUG Adding direct dependency: sqlmodel==0.0.14
DEBUG Adding direct dependency: my-private-package==1.0.0
TRACE Fetching metadata for sqlmodel from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Fetching metadata for my-private-package from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/sqlmodel.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
DEBUG Checking keyring for credentials for VssSessionToken@https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Checking keyring for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/my-private-package.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE Checking keyring for host pkgs.dev.azure.com
TRACE Skipping fetch of credentials for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/my-private-package/, previous attempt failed
TRACE Fetching metadata for my-private-package from https://pypi.org/simple/my-private-package/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/my-private-package.rkyv
DEBUG No cache entry for: https://pypi.org/simple/my-private-package/
TRACE Sending fresh GET request for https://pypi.org/simple/my-private-package/
TRACE Handling request for https://pypi.org/simple/my-private-package/
TRACE Request for https://pypi.org/simple/my-private-package/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/my-private-package/
TRACE Attempting unauthenticated request for https://pypi.org/simple/my-private-package/
TRACE Fetching metadata for sqlmodel from https://pypi.org/simple/sqlmodel/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/sqlmodel.rkyv
DEBUG No cache entry for: https://pypi.org/simple/sqlmodel/
TRACE Sending fresh GET request for https://pypi.org/simple/sqlmodel/
TRACE Handling request for https://pypi.org/simple/sqlmodel/
TRACE Request for https://pypi.org/simple/sqlmodel/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sqlmodel/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sqlmodel/
TRACE cached request https://pypi.org/simple/sqlmodel/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: sqlmodel
TRACE selecting candidate for package sqlmodel with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } with 21 remote versions
TRACE found candidate for package PackageName("sqlmodel") with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } after 8 steps: "0.0.14" version
DEBUG Searching for a compatible version of sqlmodel (==0.0.14)
TRACE selecting candidate for package sqlmodel with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } with 21 remote versions
TRACE found candidate for package PackageName("sqlmodel") with range Range { segments: [(Included("0.0.14"), Included("0.0.14"))] } after 8 steps: "0.0.14" version
DEBUG Selecting: sqlmodel==0.0.14 (sqlmodel-0.0.14-py3-none-any.whl)
TRACE No cache entry exists for /root/.cache/uv/wheels-v1/pypi/sqlmodel/sqlmodel-0.0.14-py3-none-any.msgpack
DEBUG No cache entry for: https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Sending fresh GET request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Handling request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata is unauthenticated, checking cache
TRACE No credentials in cache for URL https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE Attempting unauthenticated request for https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata
TRACE cached request https://files.pythonhosted.org/packages/55/b2/ea0c31d2bb8d22dffd83f43028d8d1b18301702725d6b918ac15f99b6de7/sqlmodel-0.0.14-py3-none-any.whl.metadata is storable because its response has a 'public' cache-control directive
TRACE Received built distribution metadata for: sqlmodel==0.0.14
DEBUG Adding transitive dependency for sqlmodel==0.0.14: sqlalchemy>=2.0.0, <2.1.0
DEBUG Adding transitive dependency for sqlmodel==0.0.14: pydantic>=1.10.13, <3.0.0
TRACE Fetching metadata for sqlalchemy from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Fetching metadata for pydantic from https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/sqlalchemy.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE Skipping fetch of credentials for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlalchemy/, previous attempt failed
TRACE No cache entry exists for /root/.cache/uv/simple-v7/29fb3946eb316a55/pydantic.rkyv
DEBUG No cache entry for: https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Sending fresh GET request for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Handling request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/
TRACE Request for https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/ is missing a password, looking for credentials
TRACE No password in cache for realm VssSessionToken@https://pkgs.dev.azure.com
TRACE Skipping fetch of credentials for https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/pydantic/, previous attempt failed
TRACE Fetching metadata for sqlalchemy from https://pypi.org/simple/sqlalchemy/
TRACE No cache entry exists for /root/.cache/uv/simple-v7/pypi/sqlalchemy.rkyv
DEBUG No cache entry for: https://pypi.org/simple/sqlalchemy/
TRACE Sending fresh GET request for https://pypi.org/simple/sqlalchemy/
TRACE Handling request for https://pypi.org/simple/sqlalchemy/
TRACE Request for https://pypi.org/simple/sqlalchemy/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://pypi.org/simple/sqlalchemy/
TRACE Attempting unauthenticated request for https://pypi.org/simple/sqlalchemy/
TRACE cached request https://pypi.org/simple/sqlalchemy/ is storable because its response has a 'public' cache-control directive
TRACE Request for https://pypi.org/simple/my-private-package/ failed with 404 Not Found, checking for credentials
TRACE No credentials in cache for realm https://pypi.org
TRACE Skipping keyring lookup for https://pypi.org/simple/my-private-package/ with no username
TRACE Received package metadata for: my-private-package
TRACE Fetching metadata for pydantic from https://pypi.org/simple/pydantic/
DEBUG Searching for a compatible version of my-private-package (==1.0.0)
TRACE selecting candidate for package my-private-package with range Range { segments: [(Included("1.0.0"), Included("1.0.0"))] } with 0 remote versions
DEBUG No compatible version found for: my-private-package
  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-package was not found in the package registry and you require my-private-package==1.0.0, we can conclude that the requirements are
      unsatisfiable.
```

---

_Comment by @zanieb on 2024-05-13 20:26_

Sorry about the trouble and thanks for the additional debugging info. It looks like the keyring did not respond with credentials:

```
DEBUG Checking keyring for credentials for VssSessionToken@https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Checking keyring for URL https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/sqlmodel/
TRACE Checking keyring for host pkgs.dev.azure.com
```

Perhaps their plugin only works with the username omitted / via the `get_creds` API that we do not support?

---

_Comment by @AndreuCodina on 2024-05-20 16:20_

Being able to install private packages from Azure DevOps is essential in my company to use `uv`. Could you please tell us if you're going to invest resources to solve it in the near time?

Thank you :)

---

_Comment by @zanieb on 2024-06-27 10:57_

I'd love for this to be fixed too, but it's not entirely clear what we need to change.

Perhaps we need to be requesting `https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple` specifically? I believe this was discussed some at https://github.com/astral-sh/uv/issues/4056

Every keyring plugin works differently and it is difficult to give them all what they want. Some clarity on the exact requirements of the Azure plugin would help move this issue forward.

---

_Comment by @RaphaelMelanconAtBentley on 2024-06-27 13:08_

It works for me... Are you getting your credentials first before installing packages with uv? `keyring --mode creds get https://pkgs.dev.azure.com/orgname/_packaging/Packages/pypi/simple/`

---

_Comment by @AndreuCodina on 2024-06-27 16:10_

> It works for me... Are you getting your credentials first before installing packages with uv? `keyring --mode creds get https://pkgs.dev.azure.com/orgname/_packaging/Packages/pypi/simple/`

It doesn't work only with that command. I'm executing all commands in a DevContainer, so I don't have environmental contamination.

```bash
$ pip install --root-user-action=ignore uv

$ uv pip install --system keyring artifacts-keyring

$ keyring --mode creds get https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/

$ uv pip install --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ --keyring-provider subprocess my-private-package==1.0.0

  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-package was not found in the package registry and you require my-private-package==1.0.0, we can conclude that the requirements are unsatisfiable.
```

---

_Comment by @RaphaelMelanconAtBentley on 2024-06-27 18:11_

Humm, and the keyring command works fine? You are able to sign-in and if you do it again it prints the username-token combo?

---

_Comment by @AndreuCodina on 2024-06-27 21:03_

> Humm, and the keyring command works fine? You are able to sign-in and if you do it again it prints the username-token combo?

Yes. It prints `VssSessionToken` and the token.

---

_Comment by @benjamin-hodgson on 2024-07-04 21:28_

I'd like to donate some time to this issue (as my team is unhappy with Pip's performance), hopefully this weekend or next week.

It's a little unclear to me from the above discussion whether you think the bug is due to `uv` requesting the credentials incorrectly or due to the Artifacts keyring plugin being too strict about the format it accepts. Do you have any guidance you can offer here @zanieb? Perhaps you don't yet know which it is? (If that's the case I'm happy to debug by myself.) I saw #4583 which suggests that you think it's a `uv` bug but I wanted to confirm.

For posterity: [the Artifacts keyring plugin](https://github.com/microsoft/artifacts-keyring/tree/master/src/artifacts_keyring) is quite simple; it delegates most of the work to [`CredentialProvider.Microsoft`](https://github.com/microsoft/artifacts-credprovider/tree/master/CredentialProvider.Microsoft), so if there is indeed an upstream bug then I think it's likely to be in the C# code.

---

_Comment by @RaphaelMelanconAtBentley on 2024-07-04 21:44_

> > Humm, and the keyring command works fine? You are able to sign-in and if you do it again it prints the username-token combo?
> 
> Yes. It prints `VssSessionToken` and the token.

That's weird, because I'm using uv through Pixi and have no issue installing from my private feeds...

---

_Comment by @PanTheDev on 2024-07-04 23:40_

Not sure if related, but there was an issue with artifacts-keyring that was fixed 1-2 weeks ago. It was sometimes printing stuff other than the credentials to stdout. https://github.com/microsoft/artifacts-keyring/pulls

---

_Comment by @nathanjmcdougall on 2024-07-05 02:15_

> It works for me... Are you getting your credentials first before installing packages with uv? `keyring --mode creds get https://pkgs.dev.azure.com/orgname/_packaging/Packages/pypi/simple/`

Thanks, this works for me too.

---

_Comment by @AndreuCodina on 2024-07-05 12:25_

> Not sure if related, but there was an issue with artifacts-keyring that was fixed 1-2 weeks ago. It was sometimes printing stuff other than the credentials to stdout. https://github.com/microsoft/artifacts-keyring/pulls

I've rebuilt the DevContainer (I'm using python:3.12-bookworm) and I forced the latest versions, but I got the same result:

```bash
$ pip install --root-user-action=ignore uv==0.2.21

$ uv pip install --system keyring==25.2.1 artifacts-keyring==0.3.6

$ keyring --mode creds get https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/

$ uv pip install --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ --keyring-provider subprocess my-private-package==1.0.0

  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-package was not found in the package registry and you require my-private-package==1.0.0, we can conclude that the requirements are unsatisfiable.
```

After this, if I don't use `uv`:

```bash
$ pip install --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ my-private-package==1.0.0
```

I can.

---

_Comment by @PanTheDev on 2024-07-05 12:41_

> > Not sure if related, but there was an issue with artifacts-keyring that was fixed 1-2 weeks ago. It was sometimes printing stuff other than the credentials to stdout. https://github.com/microsoft/artifacts-keyring/pulls
> 
> I've rebuilt the DevContainer (I'm using python:3.12-bookworm) and I forced the latest versions, but I got the same result:
> 
> ```shell
> $ pip install --root-user-action=ignore uv==0.2.21
> 
> $ uv pip install --system keyring==25.2.1 artifacts-keyring==0.3.6
> 
> $ keyring --mode creds get https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/
> 
> $ uv pip install --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ --keyring-provider subprocess my-private-package==1.0.0
> 
>   × No solution found when resolving dependencies:
>   ╰─▶ Because my-private-package was not found in the package registry and you require my-private-package==1.0.0, we can conclude that the requirements are unsatisfiable.
> ```
> 
> After this, if I don't use `uv`:
> 
> ```shell
> $ pip install --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ --keyring-provider subprocess my-private-package==1.0.0
> ```
> 
> This requires me the username and password:
> 
> `User for pkgs.dev.azure.com: `

Try this for your extra index URL: 
pip install --extra-index-url=https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/

---

_Comment by @AndreuCodina on 2024-07-05 14:17_

> > > Not sure if related, but there was an issue with artifacts-keyring that was fixed 1-2 weeks ago. It was sometimes printing stuff other than the credentials to stdout. https://github.com/microsoft/artifacts-keyring/pulls
> > 
> > 
> > I've rebuilt the DevContainer (I'm using python:3.12-bookworm) and I forced the latest versions, but I got the same result:
> > ```shell
> > $ pip install --root-user-action=ignore uv==0.2.21
> > 
> > $ uv pip install --system keyring==25.2.1 artifacts-keyring==0.3.6
> > 
> > $ keyring --mode creds get https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/
> > 
> > $ uv pip install --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ --keyring-provider subprocess my-private-package==1.0.0
> > 
> >   × No solution found when resolving dependencies:
> >   ╰─▶ Because my-private-package was not found in the package registry and you require my-private-package==1.0.0, we can conclude that the requirements are unsatisfiable.
> > ```
> > 
> > 
> >     
> >       
> >     
> > 
> >       
> >     
> > 
> >     
> >   
> > After this, if I don't use `uv`:
> > ```shell
> > $ pip install --extra-index-url=https://pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ --keyring-provider subprocess my-private-package==1.0.0
> > ```
> > 
> > 
> >     
> >       
> >     
> > 
> >       
> >     
> > 
> >     
> >   
> > This requires me the username and password:
> > `User for pkgs.dev.azure.com: `
> 
> Try this for your extra index URL: pip install --extra-index-url=https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/

Thanks! I missed to delete two parameters.

I confirm this works with `uv` too modifying the URL and adding `VssSessionToken` (could `uv` send it automatically?).

**Case 1: uv using --system. This works.**

```bash
$ pip install --root-user-action=ignore uv

$ uv pip install --system keyring artifacts-keyring

$ uv pip install --extra-index-url=https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ --keyring-provider subprocess --system my-private-package==1.0.0
```

**Case 2: uv using venv. This doesn't work.**

```bash
$ pip install --root-user-action=ignore uv

$ uv venv

$ uv pip install keyring artifacts-keyring

$ uv pip install --extra-index-url=https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ --keyring-provider subprocess my-private-package==1.0.0
```

**Case 3: uv using venv, but installing keyring packages with --system. This works.**

```bash
$ pip install --root-user-action=ignore uv

$ uv venv

$ uv pip install --system keyring artifacts-keyring

$ uv pip install --extra-index-url=https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/ --keyring-provider subprocess my-private-package==1.0.0
```

---

_Comment by @zanieb on 2024-07-05 15:57_

Hey @benjamin-hodgson, thanks for the offer!

The biggest problem here is that I am not an Azure user and am getting mixed signals on whether or not things are working so I'm hesitant to sink a bunch of time into setting it up.

> It's a little unclear to me from the above discussion whether you think the bug is due to uv requesting the credentials incorrectly or due to the Artifacts keyring plugin being too strict about the format it accepts. 

I think it'd be great if the Azure keyring plugin authorized URLs that are children of the registry URL. It's still not clear to me if it does from the discussion in this thread. Regardless, various keyring plugins require various formats and we need to do the best we can to get the credentials with the minimum number of invocations. (Keyring invocations are relatively expensive since they require a subprocess so can't just try a bunch of things). The idea in #4583 is that it would move us to a more consistent format and reduce the number of required keyring requests.

---

_Comment by @zanieb on 2024-07-05 16:03_

> I confirm this works with uv too modifying the URL and adding VssSessionToken (could uv send it automatically?).

We can't make assumptions about the default username — this differs per registry. `pip` doesn't need this because it uses the Python API which supports retrieving the username. We got support for this added to keyring's CLI recently, but it's tricky to add support in uv because _most_ people are probably not on the latest keyring version and it'd take an extra invocation to check if the new CLI format is supported.

Another option here is to add support for querying keyring via Python — we already have the ability to find a Python interpreter and check if keyring is installed. This would be pretty hard to get right, I think.

---

_Comment by @benjamin-hodgson on 2024-07-05 21:29_

> I confirm this works with uv too modifying the URL and adding `VssSessionToken`

This is working for me too.

> **Case 2: uv using venv. This doesn't work.**

@AndreuCodina I think the reason this is not working for you is simply that you didn't `activate` the venv after creating it. `keyring` was installed into the venv so won't be on the PATH until you activate it.

----------

@zanieb Based on the discussion above and my own testing, **everything appears to work correctly once you add `VssSessionToken@` to the URL**. (I _think_ you can actually use any old dummy username here, though I haven't tested that.) This is different than `pip`'s behaviour, which doesn't require the username in the URL.

> it's tricky to add support in uv because most people are probably not on the latest keyring version

It sounds like invoking the keyring CLI from `uv` **requires** a username today, and always fails when the username is missing, even with recent versions of keyring. So if we reach the point that we're about to call keyring but we don't have a username, we could just assume that we're running a recent keyring and make the call without checking the version in advance. Since the alternative would be to fail anyway. (There might be room for improvement in the error message on the way out, ie, "we called keyring without a username but it failed; this could be a real bug or you might just need to update keyring".)

That said, I don't think any code changes are _required_ here as adding a dummy username to the URL does not seem like an onerous requirement for users. I think the only thing missing (and the likely cause of all this confusion) is simply some **documentation** of how to use `uv` with Azure Artifacts (and/or the above-mentioned difference with Pip). I'd be happy to contribute that documentation (but also happy to leave it to someone else); I could alternatively write something on my personal website if you'd prefer not to have repository-specific info in the `uv` docs.

Let me know! Thanks!

---

_Comment by @zanieb on 2024-07-05 23:30_

> This is different than pip's behaviour, which doesn't require the username in the URL.

Yep this differs from pip since we're using the subprocess interface for keyring while they default to using the Python interface (which allows fetching usernames). I imagine if you requested the subprocess interface in pip you'd see the same behavior.

> So if we reach the point that we're about to call keyring but we don't have a username, we could just assume that we're running a recent keyring and make the call without checking the version in advance. Since the alternative would be to fail anyway.

We don't attempt to use keyring for URLs without usernames right now, so this would be a new behavior to attempt to use the new CLI interface when a username isn't present. This seems pretty reasonable to me, but not urgent if we have have a clear working behavior.

> I think the only thing missing (and the likely cause of all this confusion) is simply some documentation of how to use uv with Azure Artifacts

Totally this sounds great. I'm in the process of writing real documentation for the project. Feel free to create a new integration guide at `docs/guides` or add content to `docs/configuration/authentication.md` — I'll deal with making sure it all fits together in the end.

Thank you!

---

_Comment by @benjamin-hodgson on 2024-07-06 20:05_

In fact [Pip’s docs specifically mention](https://pip.pypa.io/en/stable/topics/authentication/#using-keyring-as-a-command-line-application) that subprocess mode requires a username in the URL.

Will have a doc ready for you to review early next week 😉

---

_Comment by @nathanjmcdougall on 2024-07-10 05:43_

In case this helps others, I was having intermittent (yet frequent) issues with authentication associated with #3260
Upgrading to `artifacts-keyring==0.3.6` solved the issue (I was at "artifacts-keyring==0.3.5").

---

_Comment by @fralik on 2024-07-13 19:03_

I can +1 that index url like `https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/` works (windows OS, artifacts-keyring installed). 

The annoying part (for me) at the moment is that one has to add `--keyring-provider=subprocess` to every uv call. All our packages must come from a private Azure DevOps feed.

---

_Comment by @zanieb on 2024-07-13 19:18_

@fralik you should be able to add

```toml
[tool.uv]
keyring-provider = "subprocess"
index-url = "https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/"
```

to your `pyproject.toml` or to a `uv.toml` file (without the `[tool.uv]` section)

---

_Comment by @AndreuCodina on 2024-07-14 07:14_

> @fralik you should be able to add
> 
> ```toml
> [tool.uv]
> keyring-provider = "subprocess"
> index-url = "https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/"
> ```
> 
> to your `pyproject.toml` or to a `uv.toml` file (without the `[tool.uv]` section)

Thanks, Zanie.
As a note, you can skip `keyring` and just install `artifacts-keyring`.

This is our pyproject. We use PyPi and private packages.

```toml
[tool.uv.pip]
keyring-provider = "subprocess"
extra-index-url = [
    "https://VssSessionToken@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/",
]
```

With that configuration, in the CI/CD you have to override these environment variables:

```
UV_KEYRING_PROVIDER="disabled"
UV_EXTRA_INDEX_URL="https://$ARTIFACT_FEED_ACCESS_TOKEN@pkgs.dev.azure.com/mycompany/_packaging/MyFeed/pypi/simple/"
```

---

_Comment by @benjamin-hodgson on 2024-07-26 22:02_

Can this issue be closed now that the documentation is merged into main? Has anyone had difficulty connecting to Artifacts using the guide?

---

_Comment by @charliermarsh on 2024-07-26 23:41_

Seems reasonable to me.

---

_Closed by @charliermarsh on 2024-07-26 23:41_

---
