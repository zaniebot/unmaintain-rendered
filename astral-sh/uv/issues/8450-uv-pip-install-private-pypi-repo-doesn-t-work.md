---
number: 8450
title: "`uv pip install` private pypi repo doesn't work when used in docker by default"
type: issue
state: closed
author: Pebin
labels:
  - external
assignees: []
created_at: 2024-10-22T13:06:54Z
updated_at: 2025-11-06T18:39:27Z
url: https://github.com/astral-sh/uv/issues/8450
synced_at: 2026-01-10T01:24:28Z
---

# `uv pip install` private pypi repo doesn't work when used in docker by default

---

_Issue opened by @Pebin on 2024-10-22 13:06_


uv:0.4.24
linux, opensuse

Hi, there seems to be a difference how pip and uv searches for private pypi repos **in docker**, outside of docker both work just fine.

When I am using pip install, everything is installed properly but with uv it fails with the following error

```
#14 0.500 DEBUG Transient request failure for https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/, retrying: error sending request for url (https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/)
#14 0.500   Caused by: client error (Connect)
#14 0.500   Caused by: dns error: failed to lookup address information: Name does not resolve
#14 0.500   Caused by: failed to lookup address information: Name does not resolve
```

<details>

<summary>Here is the full dockerfile</summary>

```docker
    FROM ghcr.io/astral-sh/uv:0.4.24 AS uv

    FROM python:3.9.20-slim-bookworm
    
    # create virtual env to be copied into runtime image later
    RUN python -m venv /opt/venv
    
    ENV PATH="/opt/venv/bin:$PATH"
    
    COPY --from=uv /uv /bin/uv
    
    COPY requirements.txt ./
    
    RUN --mount=type=secret,id=netrc,required,target=/root/.netrc pip install --verbose -r requirements.txt
    
    # again the same for the uv
    
    RUN rm -rf /opt/venv
    
    # create virtual env to be copied into runtime image later
    RUN python -m venv /opt/venv
    
    RUN --mount=type=secret,id=netrc,required,target=/root/.netrc uv pip install -v -r requirements.txt

```
</details>


<details>

<summary>Here is the output for the docker build command. Both pip and uv pip. </summary>
Removed the unrelated layers to make it a little bit shorter

`docker build --secret id=netrc,src=$HOME/.netrc --progress=plain --no-cache . | tee build.log`
```logs
#8 [stage-1 2/8] RUN python -m venv /opt/venv
#8 DONE 3.4s

#9 [stage-1 3/8] COPY --from=uv /uv /bin/uv
#9 DONE 2.8s

#10 [stage-1 4/8] COPY requirements.txt ./
#10 DONE 0.2s

#11 [stage-1 5/8] RUN --mount=type=secret,id=netrc,required,target=/root/.netrc pip install --verbose -r requirements.txt
#11 0.561 Using pip 23.0.1 from /opt/venv/lib/python3.9/site-packages/pip (python 3.9)
#11 0.591 Looking in indexes: https://pypi.org/simple, https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple
#11 1.288 Collecting alembic==1.13.3
#11 1.502   Downloading alembic-1.13.3-py3-none-any.whl (233 kB)
#11 1.663      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 233.2/233.2 kB 1.4 MB/s eta 0:00:00
#11 2.255 Collecting tm-redis==2.1.1
#11 2.539   Downloading https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/files/6cc2163fbb385478d9f1319f62b351cafa2965d8b296c5e5cb118ac7270dc8bd/tm_redis-2.1.1-py3-none-any.whl (5.5 kB)
#11 3.977 Collecting SQLAlchemy>=1.3.0
#11 4.078   Downloading SQLAlchemy-2.0.36-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (3.1 MB)
#11 4.930      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 3.1/3.1 MB 3.6 MB/s eta 0:00:00
#11 5.442 Collecting Mako
#11 5.499   Downloading Mako-1.3.6-py3-none-any.whl (78 kB)
#11 5.529      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 78.6/78.6 kB 2.5 MB/s eta 0:00:00
#11 5.810 Collecting typing-extensions>=4
#11 5.866   Downloading typing_extensions-4.12.2-py3-none-any.whl (37 kB)
#11 6.245 Collecting redis<5.0.0,>=4.4.0
#11 6.303   Downloading redis-4.6.0-py3-none-any.whl (241 kB)
#11 6.350      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 241.1/241.1 kB 6.0 MB/s eta 0:00:00
#11 7.091 Collecting pydantic<3
#11 7.157   Downloading pydantic-2.9.2-py3-none-any.whl (434 kB)
#11 7.229      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 434.9/434.9 kB 6.1 MB/s eta 0:00:00
#11 7.502 Collecting annotated-types>=0.6.0
#11 7.558   Downloading annotated_types-0.7.0-py3-none-any.whl (13 kB)
#11 10.29 Collecting pydantic-core==2.23.4
#11 10.35   Downloading pydantic_core-2.23.4-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (2.1 MB)
#11 10.82      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 2.1/2.1 MB 4.4 MB/s eta 0:00:00
#11 11.29 Collecting async-timeout>=4.0.2
#11 11.34   Downloading async_timeout-4.0.3-py3-none-any.whl (5.7 kB)
#11 12.24 Collecting greenlet!=0.4.17
#11 12.29   Downloading greenlet-3.1.1-cp39-cp39-manylinux_2_24_x86_64.manylinux_2_28_x86_64.whl (597 kB)
#11 12.40      ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 597.4/597.4 kB 5.7 MB/s eta 0:00:00
#11 12.95 Collecting MarkupSafe>=0.9.2
#11 13.02   Downloading MarkupSafe-3.0.2-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl (20 kB)
#11 13.12 Installing collected packages: typing-extensions, MarkupSafe, greenlet, async-timeout, annotated-types, SQLAlchemy, redis, pydantic-core, Mako, pydantic, alembic, tm-redis
#11 14.23   changing mode of /opt/venv/bin/mako-render to 755
#11 14.55   changing mode of /opt/venv/bin/alembic to 755
#11 14.59 Successfully installed Mako-1.3.6 MarkupSafe-3.0.2 SQLAlchemy-2.0.36 alembic-1.13.3 annotated-types-0.7.0 async-timeout-4.0.3 greenlet-3.1.1 pydantic-2.9.2 pydantic-core-2.23.4 redis-4.6.0 tm-redis-2.1.1 typing-extensions-4.12.2
#11 14.86 
#11 14.86 [notice] A new release of pip is available: 23.0.1 -> 24.2
#11 14.86 [notice] To update, run: pip install --upgrade pip
#11 DONE 17.0s

#12 [stage-1 6/8] RUN rm -rf /opt/venv
#12 DONE 0.8s

#13 [stage-1 7/8] RUN python -m venv /opt/venv
#13 DONE 4.2s

#14 [stage-1 8/8] RUN --mount=type=secret,id=netrc,required,target=/root/.netrc uv pip install -v -r requirements.txt
#14 0.450 DEBUG uv 0.4.24
#14 0.451 DEBUG Searching for default Python interpreter in system path
#14 0.494 DEBUG Found `cpython-3.9.20-linux-x86_64-gnu` at `/opt/venv/bin/python` (search path)
#14 0.494 Using Python 3.9.20 environment at /opt/venv
#14 0.494 DEBUG Acquired lock for `/opt/venv`
#14 0.494 DEBUG At least one requirement is not satisfied: tm-redis==2.1.1
#14 0.495 DEBUG Using request timeout of 30s
#14 0.495 DEBUG Solving with installed Python version: 3.9.20
#14 0.495 DEBUG Solving with target Python version: >=3.9.20
#14 0.495 DEBUG Adding direct dependency: alembic==1.13.3
#14 0.495 DEBUG Adding direct dependency: tm-redis==2.1.1
#14 0.495 DEBUG No cache entry for: https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/
#14 0.496 DEBUG No cache entry for: https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/tm-redis/
#14 0.500 DEBUG Transient request failure for https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/, retrying: error sending request for url (https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/)
#14 0.500   Caused by: client error (Connect)
#14 0.500   Caused by: dns error: failed to lookup address information: Name does not resolve
#14 0.500   Caused by: failed to lookup address information: Name does not resolve
#14 0.500 DEBUG Transient request failure for https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/tm-redis/, retrying: error sending request for url (https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/tm-redis/)
#14 0.500   Caused by: client error (Connect)
#14 0.500   Caused by: dns error: failed to lookup address information: Name does not resolve
#14 0.500   Caused by: failed to lookup address information: Name does not resolve
#14 0.672 DEBUG Transient request failure for https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/, retrying: error sending request for url (https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/)
#14 0.672   Caused by: client error (Connect)
#14 0.672   Caused by: dns error: failed to lookup address information: Name does not resolve
#14 0.672   Caused by: failed to lookup address information: Name does not resolve
#14 1.252 DEBUG Transient request failure for https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/tm-redis/, retrying: error sending request for url (https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/tm-redis/)
#14 1.252   Caused by: client error (Connect)
#14 1.252   Caused by: dns error: failed to lookup address information: Name does not resolve
#14 1.252   Caused by: failed to lookup address information: Name does not resolve
#14 1.801 DEBUG Transient request failure for https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/, retrying: error sending request for url (https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/)
#14 1.801   Caused by: client error (Connect)
#14 1.801   Caused by: dns error: failed to lookup address information: Name does not resolve
#14 1.801   Caused by: failed to lookup address information: Name does not resolve
#14 1.837 DEBUG Transient request failure for https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/, retrying: error sending request for url (https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/)
#14 1.837   Caused by: client error (Connect)
#14 1.837   Caused by: dns error: failed to lookup address information: Name does not resolve
#14 1.837   Caused by: failed to lookup address information: Name does not resolve
#14 1.837 DEBUG Released lock at `/opt/venv/.lock`
#14 1.837 error: Could not connect, are you offline?
#14 1.837   Caused by: Request failed after 3 retries
#14 1.837   Caused by: error sending request for url (https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/)
#14 1.837   Caused by: client error (Connect)
#14 1.837   Caused by: dns error: failed to lookup address information: Name does not resolve
#14 1.837   Caused by: failed to lookup address information: Name does not resolve
#14 ERROR: process "/bin/sh -c uv pip install -v -r requirements.txt" did not complete successfully: exit code: 2
------
 > [stage-1 8/8] RUN --mount=type=secret,id=netrc,required,target=/root/.netrc uv pip install -v -r requirements.txt:
1.837   Caused by: client error (Connect)
1.837   Caused by: dns error: failed to lookup address information: Name does not resolve
1.837   Caused by: failed to lookup address information: Name does not resolve
1.837 DEBUG Released lock at `/opt/venv/.lock`
1.837 error: Could not connect, are you offline?
1.837   Caused by: Request failed after 3 retries
1.837   Caused by: error sending request for url (https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple/alembic/)
1.837   Caused by: client error (Connect)
1.837   Caused by: dns error: failed to lookup address information: Name does not resolve
1.837   Caused by: failed to lookup address information: Name does not resolve
------
Dockerfile:23
--------------------
  21 |     RUN python -m venv /opt/venv
  22 |     
  23 | >>> RUN --mount=type=secret,id=netrc,required,target=/root/.netrc uv pip install -v -r requirements.txt
  24 |     
--------------------
ERROR: failed to solve: process "/bin/sh -c uv pip install -v -r requirements.txt" did not complete successfully: exit code: 2
```
</details>


<details>

<summary>the installed requirements file looks like this</summary>

```
--index-url https://pypi.org/simple
--extra-index-url https://gitlab.tmloc.com/api/v4/groups/7/-/packages/pypi/simple

alembic==1.13.3

tm-redis==2.1.1
```
</details>

Do you have any idea what could be the issue?

It can be workarounded by manually writing the DNS ip addresses to `/etc/docker/daemon.json` but I guess that it should behave the same as pip :shrug: 




---

_Comment by @sparfenyuk on 2025-01-17 15:03_

Still happens to `0.5.20`. Reproduces in the freshly configured Ubuntu:22.04 (WSL distro)

---

_Comment by @atigbadr on 2025-01-17 15:15_

I'm getting this while building images on DinD environment as well. 

---

_Comment by @zanieb on 2025-01-17 15:18_

Sorry I'm not sure how uv could fix this or why it's different— we're just using a standard `reqwest` network stack.

---

_Comment by @atigbadr on 2025-01-17 15:22_

> Sorry I'm not sure how uv could fix this or why it's different— we're just using a standard `reqwest` network stack.

Using pip install works with no issue, apt install, every other command works except for UV 
Including UV pip install and UV python install

I think it is related to UV somehow 

---

_Comment by @zanieb on 2025-01-17 15:48_

As I said, it's likely this is a "Rust networking stack" problem not a "uv" problem. Some brief research

- https://stackoverflow.com/a/72545037
- https://stackoverflow.com/questions/68397898/minimal-docker-for-networking-rust-binary/68409203#68409203
- https://docs.rs/hickory-resolver/latest/hickory_resolver/

---

_Referenced in [home-assistant/core#135054](../../home-assistant/core/issues/135054.md) on 2025-01-31 06:16_

---

_Comment by @plusls on 2025-02-25 01:03_

A simple patch to fix it:

```diff
diff --git a/crates/uv-client/Cargo.toml b/crates/uv-client/Cargo.toml
index 529e4afcc..d8b6981d1 100644
--- a/crates/uv-client/Cargo.toml
+++ b/crates/uv-client/Cargo.toml
@@ -53,6 +53,7 @@ tokio = { workspace = true }
 tokio-util = { workspace = true }
 tracing = { workspace = true }
 url = { workspace = true }
+reqwest-hickory-resolver = "0.1.0"

 [dev-dependencies]
 anyhow = { workspace = true }
diff --git a/crates/uv-client/src/base_client.rs b/crates/uv-client/src/base_client.rs
index 3c7a5eb71..2a95e14de 100644
--- a/crates/uv-client/src/base_client.rs
+++ b/crates/uv-client/src/base_client.rs
@@ -21,6 +21,7 @@ use uv_platform_tags::Platform;
 use uv_static::EnvVars;
 use uv_version::version;
 use uv_warnings::warn_user_once;
+use reqwest_hickory_resolver::HickoryResolver;

 use crate::linehaul::LineHaul;
 use crate::middleware::OfflineMiddleware;
@@ -270,6 +271,7 @@ impl<'a> BaseClientBuilder<'a> {
     ) -> Client {
         // Configure the builder.
         let client_builder = ClientBuilder::new()
+            .dns_resolver(Arc::new(HickoryResolver::default()))
             .http1_title_case_headers()
             .user_agent(user_agent)
             .pool_max_idle_per_host(20)
```

---

_Label `external` added by @zanieb on 2025-02-25 01:33_

---

_Comment by @stevapple on 2025-03-04 01:46_

@plusls Would you mind proposing a PR with the fix? It is life-saving for distributing app with Docker,

---

_Comment by @plusls on 2025-03-04 06:48_

> [@plusls](https://github.com/plusls) Would you mind proposing a PR with the fix? It is life-saving for distributing app with Docker,

This patch modifies reqwest's DNS implementation to hickory. Therefore, it is not suitable for direct submission as a PR and should instead be made a configurable option in uv or a Rust feature.

---

_Referenced in [astral-sh/uv#12016](../../astral-sh/uv/issues/12016.md) on 2025-03-06 19:27_

---

_Comment by @zanieb on 2025-03-06 20:47_

I don't fully understand the implications of the hickory resolver, it'd be great if someone had more context on that.

---

_Referenced in [astral-sh/uv#11640](../../astral-sh/uv/issues/11640.md) on 2025-03-06 23:39_

---

_Comment by @plusls on 2025-03-07 00:08_

> I don't fully understand the implications of the hickory resolver, it'd be great if someone had more context on that.

Because uv default static link with musl libc，it do not support tcp dns resolve 
If dns package too large, it will resolv failed

---

_Comment by @zanieb on 2025-03-07 00:44_

mm does it work with our non-Docker distributions? Those don't statically link musl libc.

---

_Comment by @zanieb on 2025-03-07 00:44_

> If dns package too large, it will resolv failed

What do you mean here?

---

_Comment by @plusls on 2025-03-07 00:57_



> > If dns package too large, it will resolv failed
> 
> What do you mean here?

https://wiki.musl-libc.org/functional-differences-from-glibc.html#Name-Resolver/DNS

> musl’s resolver did not support the use of DNS over TCP until version 1.2.4. This difference prevented the use of larger packets produced by protocols such as DNSSEC and DKIM.



---

_Comment by @plusls on 2025-03-07 00:58_

> mm does it work with our non-Docker distributions? Those don't statically link musl libc.

But the uv install script will auto download the uv which static link with musl libc.

It will works if use gnu gcc toolchain

---

_Comment by @charliermarsh on 2025-03-07 01:01_

I don't think this is true? I believe we only use the musl build if we can't detect glibc.

---

_Comment by @plusls on 2025-03-07 01:04_

> I don't think this is true? I believe we only use the musl build if we can't detect glibc.

Oh, it is my fault, I direct use the  `ghcr.io/astral-sh/uv:debian`

```
$ docker run -it --rm ghcr.io/astral-sh/uv:debian bash
root@3fc89d4f80da:/# which uv
/usr/local/bin/uv
root@3fc89d4f80da:/# ldd /usr/local/bin/uv
        not a dynamic executable
root@3fc89d4f80da:/#
```

---

_Comment by @zanieb on 2025-03-07 13:59_

@charliermarsh yeah just to clarify, the Docker builds are always musl

https://github.com/astral-sh/uv/blob/04db70662d51675187c6adde9ff58aab54aad900/Dockerfile#L21-L25

> This difference prevented the use of larger packets produced by protocols such as DNSSEC and DKIM.

Ah this is interesting. Thank you!

---

_Comment by @zanieb on 2025-03-07 14:01_

Sounds like we should try to use a newer version of musl then? I'm not sure where that comes from.

cc @konstin 

---

_Comment by @konstin on 2025-03-07 14:47_

Judging from https://blog.rust-lang.org/2023/05/09/Updating-musl-targets.html / https://github.com/rust-lang/rust/pull/107129, the musl version is shipped with rust itself? Doing DNS ourselves without libc involvement sounds easier here.

---

_Comment by @stevapple on 2025-03-13 10:20_

> Doing DNS ourselves without libc involvement sounds easier here.

From my perspective it's worth applying a Rust DNS resolver (eg. `HickoryResolver`) at least for all Musl builds. Not sure if we want to keep the Glibc builds as it is now (pros: smaller size, no behavior change, less dependency; cons: divergence with Musl, complexity in build infra).

---

_Comment by @bondarev on 2025-03-14 09:14_

Same problem with uv and docker

---

_Comment by @RouquinBlanc on 2025-04-02 14:57_

I have an interesting finding:

Doing this in the Dockerfile based on `python:3.11-slim`:

```
COPY --from=ghcr.io/astral-sh/uv:0.4.24 /uv /bin/uv
```

Then result of a command using a private registry fails like this:

```
# uv pip install --no-cache -i http://devpi:3141/dev/+simple transitions
Using Python 3.11.11 environment at: /opt/venv
⠼ Resolving dependencies...                                                                                                                                                                                        error: Failed to fetch: `http://devpi:3141/mycompany/dev/+simple/transitions/`
  Caused by: Could not connect, are you offline?
  Caused by: Request failed after 3 retries
  Caused by: error sending request for url (http://devpi:3141/dev/+simple/transitions/)
  Caused by: client error (Connect)
  Caused by: dns error: failed to lookup address information: Name does not resolve
  Caused by: failed to lookup address information: Name does not resolve
```

But if instead we install uv with pip like this:

```
RUN pip install --no-cache-dir uv==0.4.24
```

Then there is no issue. The same behaviour is seen from copying from `ghcr.io/astral-sh/uv:latest` or `ghcr.io/astral-sh/uv:debian`.

There is visibly something incompatible between python-slim and those uv executable (having a different size and checksum) compared to the version installed with `pip`.

The "fun" think is that for one of our coworkers (and only one), it works without problem, without any obvious difference in the setup...


---

_Comment by @stevapple on 2025-04-08 01:59_

@RouquinBlanc This is rather expected, simply because you're using `python:3.11-slim` which is based on Debian (a Glibc-based distro), so it will use `manylinux` wheel with `pip install` and bypass all the issues raised above because they're Musl related.

Everything will change if you switch to a Musl-based image, eg. `python:3.11-alpine`.

---

_Assigned to @Gankra by @Gankra on 2025-04-11 14:54_

---

_Referenced in [rust-lang/rust#125692](../../rust-lang/rust/pulls/125692.md) on 2025-04-11 16:06_

---

_Comment by @Gankra on 2025-04-11 16:08_

It seems like musl 1.2.4 came with some non-trivial breaking changes that rust is struggling to integrate. There's been a branch open for 1.2.5 for a year:

* https://github.com/rust-lang/rust/pull/125692

As I understand it, musl libc had a bunch of aliases for symbols [that the libc crate was using and the musl team decided to drop because they were causing havoc with some C++ projects](https://github.com/rust-lang/libc/issues/2934). The libc crate was [changed to use the proper symbol names](https://github.com/rust-lang/libc/pull/2935) (released in 0.2.145 almost two years ago), but various crates in the ecosystem ([like getrandom](https://crates.io/crates/getrandom)) were on such old versions of the libc crate that tons of things broke when bumping to musl 1.2.4+, as of July 2024.

I'll poke upstream to see if progress can be made on the issue.

---

See also, other discussion of this in the ecosystem:

* https://github.com/rust-cross/rust-musl-cross/pull/152
* https://github.com/getsentry/sentry-cli/issues/1929

---

_Comment by @talg2324 on 2025-05-30 21:59_

> But if instead we install uv with pip like this:
> 
> ```
> RUN pip install --no-cache-dir uv==0.4.24
> ```
> 
> Then there is no issue. The same behaviour is seen from copying from `ghcr.io/astral-sh/uv:latest` or `ghcr.io/astral-sh/uv:debian`.
> 
> There is visibly something incompatible between python-slim and those uv executable (having a different size and checksum) compared to the version installed with `pip`.


This solved the problem for me on python:3.12-slim-bookworm with uv 0.7.9.
Previously tried directly starting from the release docker images ghcr.io/astral-sh/uv:latest` and`ghcr.io/astral-sh/uv:debian as well as from COPY --from=ghcr.io/astral-sh/uv. Only installing uv from pip solved the DNS issue and allowed me to download modules from pypi.



---

_Referenced in [rust-lang/rust#142682](../../rust-lang/rust/pulls/142682.md) on 2025-06-18 15:30_

---

_Referenced in [rust-lang/compiler-team#887](../../rust-lang/compiler-team/issues/887.md) on 2025-06-18 16:59_

---

_Referenced in [astral-sh/uv#14941](../../astral-sh/uv/issues/14941.md) on 2025-07-31 11:53_

---

_Referenced in [astral-sh/uv#16584](../../astral-sh/uv/pulls/16584.md) on 2025-11-03 23:54_

---

_Comment by @zanieb on 2025-11-06 18:39_

This should be fixed in our Docker images as of https://github.com/astral-sh/uv/pull/16584

---

_Closed by @zanieb on 2025-11-06 18:39_

---

_Referenced in [Homebrew/homebrew-core#253592](../../Homebrew/homebrew-core/pulls/253592.md) on 2025-11-07 23:32_

---

_Referenced in [astral-sh/uv#16741](../../astral-sh/uv/issues/16741.md) on 2025-11-14 17:47_

---
