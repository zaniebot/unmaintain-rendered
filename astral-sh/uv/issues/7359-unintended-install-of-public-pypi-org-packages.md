```yaml
number: 7359
title: Unintended install of public pypi.org packages when trying to install private packages from gitlabs pypi registry
type: issue
state: closed
author: alexatothermo
labels:
  - network
assignees: []
created_at: 2024-09-13T12:28:40Z
updated_at: 2025-03-20T22:00:36Z
url: https://github.com/astral-sh/uv/issues/7359
synced_at: 2026-01-12T15:59:13Z
```

# Unintended install of public pypi.org packages when trying to install private packages from gitlabs pypi registry

---

_@alexatothermo_

- Command: `uv pip install --extra-index-url|--index-url pypi.private.com/... private_package==1.0.0`
- Platform: `Linux scrappy 6.6.50 #1-NixOS SMP PREEMPT_DYNAMIC Sun Sep  8 05:54:49 UTC 2024 x86_64 GNU/Linux`
- `uv --version`: 0.4.7

When installing packages from a private registry in gitlab (specifically gitlab) that have name-conflicts with pypi.org (another package with the same name exists on pypi.org) uv tries to install **the pypi.org** package instead of the intended one.

I debugged this and found out that gitlab, when requested unauthenticated, returns a 404 and redirects the client to pypi.org. This is intended by gitlab and can only be deactivated in the premium plan :/

Gitlab docs on pypi forwarding: https://docs.gitlab.com/ee/administration/settings/continuous_integration.html#pypi-forwarding

The credentials for installing packages from the private registry are kept in `~/.netrc`. The debug logs show that uv first tries the private registry unauthenticated which causes gitlab to redirect and if the package is found on pypi.org uv happily installs it.

While one can argue that gitlab is at fault here for redirecting and no option in the free-plan to change this behavior, this situation would be avoided if uv would do an authenticated request if credentials are provided for the registry. Not sure if we can assume, that if a user places credentials for a registry that this always means the registry uses authentication, but I guess in most if not all cases this is fair. Plus arguing with gitlab to make this option part of the free plan could take ages I guess.


---

_Comment by @charliermarsh on 2024-09-13 13:01_

That's interesting. Does pip handle this correctly? I thought pip also started with an unauthenticated request. \cc @zanieb when you're back

---

_Label `network` added by @charliermarsh on 2024-09-13 13:01_

---

_Comment by @alexatothermo on 2024-09-13 13:16_

Yes, `pip install --index-url ... ...` works. So it seems it does an authenticated request right away

---

_Comment by @charliermarsh on 2024-09-13 13:26_

I think they do an authenticated request if you provide credentials directly, but only authenticate with keyring after trying a failed, unauthenticated request: https://github.com/pypa/pip/blob/111eed14b6e9fba7c78a5ec2b7594812d17b5d2b/src/pip/_internal/network/auth.py#L452

---

_Comment by @alexatothermo on 2024-09-13 13:36_

> if you provide credentials directly

Do you also count ~~.envrc~~ `~/.netrc` into this? Because that's the auth mechanism I use with both pip and uv. I don't provide credentials in the index-url 

---

_Comment by @charliermarsh on 2024-09-13 13:38_

Can you say more? We don't read `.envrc` automatically. AFAIK I don't think pip does either... Does your shell populate the env vars for any commands run in that directory or something? What env vars are you setting there?

---

_Comment by @alexatothermo on 2024-09-13 13:41_

Ah, sorry! I meant `.netrc`!

---

_Comment by @charliermarsh on 2024-09-13 13:46_

Ohhh hah. I'm guessing pip will automatically apply `.netrc` if that's what you're seeing. We might be able to do that too, but I have to defer to @zanieb since the auth stuff requires a lot of care / nuance given that all the registries behave so differently.

---

_Comment by @alexatothermo on 2024-09-13 13:48_

Here is the debug log

```
❯ RUST_LOG=trace uv pip install --verbose --extra-index-url https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple private_package==1.5.0 --no-cache
DEBUG uv 0.4.7
DEBUG Searching for Python interpreter in system path
TRACE Querying interpreter executable at .venv/bin/python3
DEBUG Found `cpython-3.11.9-linux-x86_64-gnu` at `.venv/bin/python3` (active virtual environment)
DEBUG Using Python 3.11.9 environment at .venv/bin/python3
TRACE Checking lock for `.venv` at `.venv/.lock`
DEBUG Acquired lock for `.venv`
DEBUG At least one requirement is not satisfied: private_package==1.5.0
DEBUG Using request timeout of 30s
DEBUG Solving with installed Python version: 3.11.9
DEBUG Solving with target Python version: >=3.11.9
DEBUG Adding direct dependency: private_package==1.5.0
INFO add_decision: root @ 0a0.dev0    
TRACE Fetching metadata for private_package from https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple/private_package/
TRACE No cache entry exists for /tmp/.tmp54OCvk/simple-v12/index/5d92d1f3e0c26f1d/private_package.rkyv
DEBUG No cache entry for: https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple/private_package/
TRACE Sending fresh GET request for https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple/private_package/
TRACE Handling request for https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple/private_package/
TRACE Request for https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple/private_package/ is unauthenticated, checking cache
TRACE No credentials in cache for URL https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple/private_package/
TRACE Attempting unauthenticated request for https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple/private_package/
TRACE checkout waiting for idle connection: ("https", gitlab.private.com)
DEBUG starting new connection: https://gitlab.private.com/    
TRACE Http::connect; scheme=Some("https"), host=Some("gitlab.private.com"), port=None
DEBUG resolving host="gitlab.private.com"
DEBUG connecting to 127.0.0.1:443
DEBUG connected to 127.0.0.1:443
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("https", gitlab.private.com)
TRACE put; add idle connection for ("https", gitlab.private.com)
DEBUG pooling idle connection for ("https", gitlab.private.com)
DEBUG redirecting 'https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple/private_package/' to 'https://pypi.org/simple/private_package/'
TRACE checkout waiting for idle connection: ("https", pypi.org)
DEBUG starting new connection: https://pypi.org/    
TRACE Http::connect; scheme=Some("https"), host=Some("pypi.org"), port=None
DEBUG resolving host="pypi.org"
DEBUG connecting to [::]:443
DEBUG connected to [::]:443
TRACE http1 handshake complete, spawning background dispatcher task
TRACE checkout dropped for ("https", pypi.org)
TRACE put; add idle connection for ("https", pypi.org)
DEBUG pooling idle connection for ("https", pypi.org)
TRACE cached request https://gitlab.private.com/api/v4/groups/private/-/packages/pypi/simple/private_package/ is storable because its response has a 'public' cache-control directive
TRACE Received package metadata for: private_package
TRACE Selecting candidate for private_package with range ==1.5.0 with 3 remote versions
TRACE Exhausted all candidates for package private_package with range ==1.5.0 after 3 steps
DEBUG Searching for a compatible version of private_package (==1.5.0)
TRACE Selecting candidate for private_package with range ==1.5.0 with 3 remote versions
TRACE Exhausted all candidates for package private_package with range ==1.5.0 after 3 steps
DEBUG No compatible version found for: private_package
INFO Start conflict resolution because incompat satisfied:
   private_package ==1.5.0 is forbidden    
INFO prior cause: root ==0a0.dev0 is forbidden    
TRACE Resolver derivation tree before reduction
  root==0a0.dev0 depends on private_package==1.5.0
  no versions of private_package==1.5.0
TRACE Resolver derivation tree after reduction
  root==0a0.dev0 depends on private_package==1.5.0
  no versions of private_package==1.5.0
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of private_package==1.5.0 and you require private_package==1.5.0, we can conclude that your requirements are unsatisfiable.
DEBUG Released lock at `.venv/.lock`

```

Note: this output is from when trying to install a version of the private package with one of the same name that is not on pypi.

When doing this with a version that the other package also has it is installed from pypi.org.

The interesting line here is the DEBUG redirect by gitlab

---

_Assigned to @zanieb by @zanieb on 2024-09-13 14:00_

---

_Comment by @zanieb on 2024-09-13 14:03_

Can you provide a username with your index URL? I believe then we'll look for a password before sending the request. 

We'll need to consider if we want to change the behavior for netrc. I'm pretty hesitant to have a different pattern for netrc vs keyring (not to mention the other authentication methods).

---

_Comment by @alexatothermo on 2024-09-13 15:23_

Interesting. The username indeed works when provided with `--extra-index-url` but not with `--index-url`. In the former case RUST_LOG=TRACE reveals `Request ... is missing a password, looking for credentials`. The latter does not check for credentials

---

_Comment by @zanieb on 2025-03-20 22:00_

You can solve this now by setting `authenticate = "always"` on your index URL (see #11896)

---

_Closed by @zanieb on 2025-03-20 22:00_

---
