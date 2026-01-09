---
number: 2323
title: 401 Installing from Internal Index due to .netrc
type: issue
state: closed
author: hmc-cs-mdrissi
labels:
  - bug
assignees: []
created_at: 2024-03-10T03:15:30Z
updated_at: 2024-03-10T15:02:26Z
url: https://github.com/astral-sh/uv/issues/2323
synced_at: 2026-01-07T13:12:17-06:00
---

# 401 Installing from Internal Index due to .netrc

---

_Issue opened by @hmc-cs-mdrissi on 2024-03-10 03:15_

I was excited by the work done in this [issue](https://github.com/astral-sh/uv/issues/2220), so I tried checking out main and testing uv to see if it improved performance on 1 package I found slow. Instead I found that using main uv now gives me a 401 error for installing any internal package with index server. Packages that previously worked to install now fail. I've bisected commits to find the first commit that fails is this [commit](https://github.com/astral-sh/uv/pull/2241).

The exact command being run is `cargo run -- pip install --index-url=SOME_URL numpy`. The url has credentials in it. I do have .netrc file, but they are not intended for this installation especially when --index-url has all credentials needed already and did work prior to that pr. Similarly pip works for similar command `pip install --index-url=SOME_URL numpy` and I'd guess is sticking to credentials inside index-url over mixing it with .netrc.

Here's the verbose logs from the first failing commit,

<details><summary>Logs</summary>

```
    0.000830s DEBUG uv_client::registry_client Using registry request timeout of 300s
 uv::requirements::from_source source=numpy
    0.257274s DEBUG uv_interpreter::python_environment Found a virtualenv through VIRTUAL_ENV at: /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2
    0.257714s DEBUG uv_interpreter::interpreter Probing interpreter info for: /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2/bin/python
    0.539855s DEBUG uv_interpreter::interpreter Found Python 3.9.16 for: /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2/bin/python
    0.540817s DEBUG uv::commands::pip_install Using Python 3.9.16 environment at /Users/pa-loaner/Snapchat/Dev/.venvs/bento_uv2/bin/python
 uv_client::flat_index::from_entries
 uv_resolver::resolver::solve
      0.548281s   0ms DEBUG uv_resolver::resolver Solving with target Python version 3.9.16
   uv_resolver::resolver::choose_version package=root
   uv_resolver::resolver::get_dependencies package=root, version=0a0.dev0
        0.548614s   0ms DEBUG uv_resolver::resolver Adding direct dependency: numpy*
   uv_resolver::resolver::choose_version package=numpy
     uv_resolver::resolver::package_wait package_name=numpy
 uv_resolver::resolver::process_request request=Versions numpy
   uv_client::registry_client::simple_api package=numpy
     uv_client::cached_client::get_cacheable
       uv_client::cached_client::read_and_parse_cache file=/Users/pa-loaner/Library/Caches/uv/simple-v3/dc13ddb23a33f0c4/numpy.rkyv
 uv_resolver::resolver::process_request request=Prefetch numpy *
 uv_client::cached_client::from_path_sync path="/Users/pa-loaner/Library/Caches/uv/simple-v3/dc13ddb23a33f0c4/numpy.rkyv"
          0.549991s   0ms DEBUG uv_client::cached_client No cache entry for: https://registry.snapchat.com/python/virtual/numpy/
       uv_client::cached_client::fresh_request url="https://registry.snapchat.com/python/virtual/numpy/"
error: HTTP status client error (401 Unauthorized) for url (https://registry.snapchat.com/python/virtual/numpy/)
```


</details> 

Like before, I'm happy to test out any specific commands/extra log statements that would be helpful.

edit: Credentials in netrc are not wrong though and maybe could be used somehow. The netrc looks like,

```
machine registry.snapchat.com login LCAv1 password AUTH
```

while --index-url passed is `https://LCAv1:password@registry.snapchat.com/python/virtual`. Login and password are same either way, but guessing the two aren't mixing as expected. I wonder if `/python/virtual` part is lost.

---

_Renamed from "401 Installing from Internal Index" to "401 Installing from Internal Index due to .netrc" by @hmc-cs-mdrissi on 2024-03-10 03:15_

---

_Comment by @charliermarsh on 2024-03-10 03:20_

Yeah this sounds like a mistake. Thanks for reporting.

---

_Label `bug` added by @charliermarsh on 2024-03-10 03:20_

---

_Comment by @charliermarsh on 2024-03-10 03:21_

Perhaps @zanieb can take a look when they're back since they've been owning auth.

---

_Comment by @hmc-cs-mdrissi on 2024-03-10 03:26_

I did one extra test and main works if I include index url without auth.

```
uv pip install --index-url=https://LCAv1:password@registry.snapchat.com/python/virtual # Fail
uv pip install --index-url=https://registry.snapchat.com/python/virtual # Works
```

So it's using netrc + index-url well if index-url doesn't also have authenication data in it. But if they both have it even if it's same password/login, then it fails.


---

_Comment by @charliermarsh on 2024-03-10 03:31_

I can try to take a look at it tomorrow.

---

_Comment by @charliermarsh on 2024-03-10 03:36_

The code for the netrc middleware is actually pretty small, and I'm surprised it's failing to handle this. It _does_ look like it would override the credentials though. I'm not sure what's expected.

---

_Comment by @charliermarsh on 2024-03-10 03:37_

It looks like pip does explicitly prioritize existing credentials: https://github.com/pypa/pip/pull/10998. I'm still not sure why it would _fail_ here though since the credentials are exactly the same, right?

---

_Comment by @charliermarsh on 2024-03-10 03:37_

Oh wait, it was then reverted: https://github.com/pypa/pip/pull/11134.

---

_Comment by @hmc-cs-mdrissi on 2024-03-10 03:39_

The credentials are identical. At work we have some script that we need to run daily to refresh our credentials. That script updates both .netrc and pip.conf with same credentials. I then manually copy the index url in pip.conf and pass it to uv.

---

_Comment by @charliermarsh on 2024-03-10 03:39_

Makes sense. You could try running with `RUST_LOG=trace cargo run -- pip install --index-url=SOME_URL numpy --verbose` to see if there's any other helpful info.

---

_Comment by @charliermarsh on 2024-03-10 03:43_

Okay, the error I get when I provide both sets of credentials is:

```
error: error sending request for url (https://pkgs.dev.azure.com/charlie0806/_packaging/charlie0806/pypi/simple/requests/): http2 error: stream error received: endpoint requires HTTP/1.1
  Caused by: http2 error: stream error received: endpoint requires HTTP/1.1
  Caused by: stream error received: endpoint requires HTTP/1.1
```

---

_Comment by @charliermarsh on 2024-03-10 03:46_

I think the problem is that we now send up _two_ authorization headers.

---

_Comment by @hmc-cs-mdrissi on 2024-03-10 03:52_

I can confirm there's two headers for me too when it fails. I added the logs from this [pr](https://github.com/astral-sh/uv/pull/2175/files) you did before. I also tried `RUST_LOG=trace` but that doesn't log headers. 

For my use case it does not really matter which of the two credentials you pick. My personal intuition though is if url has credentials it'd be used first over a separate file.

---

_Comment by @charliermarsh on 2024-03-10 03:54_

Just trying to figure out how to _replace_ a header with reqwest, the API doesn't seem to want you to do this...

---

_Comment by @charliermarsh on 2024-03-10 04:04_

@zanieb - I think this might be impossible to do with middleware, because the reqwest middleware seems to be purely additive. We might need to remove this middleware, and add a `safe_copy_url_auth`-like method to every HTTP path that gets the relevant netrc credentials, and replaces the authorization header with them.

---

_Comment by @charliermarsh on 2024-03-10 04:13_

Think I figured out a way to do it with the middleware API.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-03-10 04:13_

---

_Referenced in [astral-sh/uv#2325](../../astral-sh/uv/pulls/2325.md) on 2024-03-10 04:16_

---

_Comment by @charliermarsh on 2024-03-10 04:19_

Should be fixed in https://github.com/astral-sh/uv/pull/2325, thanks for filing and sorry about that regression.

---

_Comment by @hmc-cs-mdrissi on 2024-03-10 04:27_

I’ll test the PR in ~1 hour when I’m back home. The diagnosis makes full sense to me.

---

_Comment by @charliermarsh on 2024-03-10 04:34_

No rush! I’m going to bed anyway.

---

_Comment by @hmc-cs-mdrissi on 2024-03-10 05:37_

I've tested it and confirmed your fix resolves the issue. Installation works well trying both internal and public packages.

---

_Closed by @charliermarsh on 2024-03-10 15:02_

---
