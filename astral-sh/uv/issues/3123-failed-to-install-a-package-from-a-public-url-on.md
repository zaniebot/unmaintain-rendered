---
number: 3123
title: Failed to install a package from a public url on 0.1.33
type: issue
state: closed
author: mgaitan
labels:
  - bug
assignees: []
created_at: 2024-04-18T15:09:00Z
updated_at: 2024-04-22T18:06:58Z
url: https://github.com/astral-sh/uv/issues/3123
synced_at: 2026-01-10T01:23:25Z
---

# Failed to install a package from a public url on 0.1.33

---

_Issue opened by @mgaitan on 2024-04-18 15:09_

since yesterday, we are experimented this issue

```
 > [worker  7/13] RUN uv pip install --system --no-cache -r /tmp/shipping/reqs/requirements-dev.txt:
0.416 error: Failed to download: pyrovider @ https://github.com/Shiphero/pyrovider/releases/download/1.2.4/pyrovider-1.2.4-py3-none-any.whl
0.416   Caused by: HTTP status client error (401 Unauthorized) for url (https://objects.githubusercontent.com/github-production-release-asset-2e65be/144867083/d56d51b0-fe2d-4eda-a5f2-8f9d6503ce92?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240418%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240418T144100Z&X-Amz-Expires=300&X-Amz-Signature=6b628fe5fc427071d8bc8bc84688ecda750075f74d01e4fc236ee4a1cc9eb9af&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=144867083&response-content-disposition=attachment%3B%20filename%3Dpyrovider-1.2.4-py3-none-any.whl&response-content-type=application%2Foctet-stream)
```

As you can see, the package is public
https://github.com/Shiphero/pyrovider/releases/download/1.2.4/pyrovider-1.2.4-py3-none-any.whl

Pinned uv to 0.1.32 worked for now, but is it related to the breaking changes introduced in the 0.1.33 releases? Do we need to upgrade something in our requirement file? 

---

_Comment by @zanieb on 2024-04-18 15:16_

Hi! Sorry about that, this looks like a bug. 

Can you provide logs with `RUST_LOG=trace uv pip install -v ...` and/or a minimal example I could run? 

---

_Label `bug` added by @zanieb on 2024-04-18 15:16_

---

_Assigned to @zanieb by @zanieb on 2024-04-18 15:16_

---

_Comment by @ChannyClaus on 2024-04-18 15:20_

fwiw it seems to work okay on my machine
```
$ uv --version
uv 0.1.33 (2cee7525c 2024-04-17)
chan.kang@Chans-MBP-4123 ~/test/uv - 
$ rm -rf .venv && uv venv && (echo 'pyrovider @ https://github.com/Shiphero/pyrovider/releases/download/1.2.4/pyrovider-1.2.4-py3-none-any.whl' | uv pip compile  - > requirements.txt) && uv pip install -r requirements.txt --no-cache
Using Python 3.12.1 interpreter at: /Users/chan.kang/.pyenv/versions/3.12.1/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
Resolved 3 packages in 52ms
Resolved 3 packages in 675ms
Downloaded 3 packages in 84ms
Installed 3 packages in 8ms
 + pyrovider==1.2.4 (from https://github.com/Shiphero/pyrovider/releases/download/1.2.4/pyrovider-1.2.4-py3-none-any.whl)
 + python-dotenv==1.0.1
 + pyyaml==6.0.1
```

---

_Comment by @mgaitan on 2024-04-18 16:02_

my teammate @romeroyonatan just discovered that it does not happen when installing the public package alone but in conjunction with another package from an url that requires authentication.  He will share an example in a moment. 

---

_Comment by @mgaitan on 2024-04-18 16:09_

btw, I just realized that this misbehavior looks a lot like #3122 and is probably related to the same root cause.

---

_Comment by @romeroyonatan on 2024-04-18 17:04_

Hi team, I found an easy way to reproduce the issue

```sh
uv pip install 'uv-3123-bug @ https://romeroyonatan:a@github.com/romeroyonatan/uv-3123-bug/releases/download/v0.1.4/uv_3123_bug-0.0.1-py3-none-any.whl'
```

This is the output

```sh
$ uv pip install 'uv-3123-bug @ https://romeroyonatan:a@github.com/romeroyonatan/uv-3123-bug/releases/download/v0.1.4/uv_3123_bug-0.0.1-py3-none-any.whl'
error: Failed to download: uv-3123-bug @ https://romeroyonatan:a@github.com/romeroyonatan/uv-3123-bug/releases/download/v0.1.4/uv_3123_bug-0.0.1-py3-none-any.whl
  Caused by: HTTP status client error (401 Unauthorized) for url (https://objects.githubusercontent.com/github-production-release-asset-2e65be/788539373/37fcd109-909c-4ed1-928c-2354c9a79a48?X-Amz-Algorithm=AWS4-HMAC-SHA256&X-Amz-Credential=AKIAVCODYLSA53PQK4ZA%2F20240418%2Fus-east-1%2Fs3%2Faws4_request&X-Amz-Date=20240418T165731Z&X-Amz-Expires=300&X-Amz-Signature=01a9bf253fb9083fd65e5b37c8b366e6349ca078644a32bac2e309e7a7ea69b2&X-Amz-SignedHeaders=host&actor_id=0&key_id=0&repo_id=788539373&response-content-disposition=attachment%3B%20filename%3Duv_3123_bug-0.0.1-py3-none-any.whl&response-content-type=application%2Foctet-stream)
```

This is the pip behavior

```sh
$ pip install 'uv-3123-bug @ https://romeroyonatan:a@github.com/romeroyonatan/uv-3123-bug/releases/download/v0.1.4/uv_3123_bug-0.0.1-py3-none-any.whl'
Collecting uv-3123-bug@ https://romeroyonatan:a@github.com/romeroyonatan/uv-3123-bug/releases/download/v0.1.4/uv_3123_bug-0.0.1-py3-none-any.whl
  Downloading https://romeroyonatan:****@github.com/romeroyonatan/uv-3123-bug/releases/download/v0.1.4/uv_3123_bug-0.0.1-py3-none-any.whl (2.7 kB)

[notice] A new release of pip is available: 23.2.1 -> 24.0
[notice] To update, run: pip install --upgrade pip
```

-----
I tried to reproduce the bug using a private repo, but I found that the issue happens even with public repositories with valid or invalid credentials.

I suspect the credentials are cached and we use the private credentials when we shouldn't (public repositories for example)

Edit: I found the auth cache https://github.com/astral-sh/uv/blob/0.1.33/crates/uv-auth/src/cache.rs#L98-L107
~Edit 2: This is the tracelog with the real instalation. I masked some sensitive data from the urls, sorry for that~ I did not reproduce the bug
Edit 3: New logs  [bug.log](https://github.com/astral-sh/uv/files/15028400/bug.log)



---

_Comment by @zanieb on 2024-04-18 17:25_

Thanks a bunch ya'll! Will investigate.

---

_Comment by @inoa-jboliveira on 2024-04-18 17:29_

There is a regression in 0.1.33 where user/pass combo on custom repos no longer work. It is also affecting us here. 

---

_Comment by @zanieb on 2024-04-18 17:30_

@inoa-jboliveira can you be more explicit? Providing a username and password doesn't work for a private repository anymore? This issue is for applying credentials from private repositories to public repositories.

---

_Comment by @inoa-jboliveira on 2024-04-18 17:34_

@zanieb Yes, the credentials are being ignored. I tested 0.1.31 and 0.1.32 and they work. 

This and UV_INDEX_URL no longer work with user/pass combo
`uv pip install --index-url https://myuser:mypass@example.com.br/repository/pypi-all/simple/`


---

_Comment by @romeroyonatan on 2024-04-18 17:36_

I found in the logs that the cached credentials are used when it shouldn't (public repositories)

```
TRACE Sending fresh HEAD request for https://github.com/Shiphero/pyrovider/releases/download/1.2.4/pyrovider-1.2.4-py3-none-any.whl
TRACE Handling request for https://github.com/Shiphero/pyrovider/releases/download/1.2.4/pyrovider-1.2.4-py3-none-any.whl
DEBUG resolving host="github.com"
TRACE No credentials on request, checking cache...
TRACE Found cached credentials.
TRACE checkout waiting for idle connection: ("https", github.com)
```

---

_Comment by @inoa-jboliveira on 2024-04-18 17:37_

Maybe this is a slightly different issue I'm having, but they seem to be caused by the same regression on v0.1.33
```
error: Failed to download: django==2.2.28
  Caused by: HTTP status client error (401 Unauthorized) for url  (https://example.com.br/repository/pypi-all/packages/django/2.2.28/Django-2.2.28-py3-none-any.whl#sha256=365429d07c1336eb42ba15aa79f45e1c13a0b04d5c21569e7d596696418a6a45)```
  


---

_Comment by @zanieb on 2024-04-18 17:41_

I think there are two things going on here:

1. We fixed our caching to actually apply per network location as intended in the original implementation, but this means we can apply credentials to requests that don't need it.
2. We stopped pre-seeding the cache with the URLs passed via the CLI, I think this is causing some sort of race condition where we don't apply credentials to _some_ in flight requests because they're not cached yet.


---

_Referenced in [astral-sh/uv#3124](../../astral-sh/uv/pulls/3124.md) on 2024-04-18 17:53_

---

_Referenced in [astral-sh/uv#3130](../../astral-sh/uv/pulls/3130.md) on 2024-04-18 22:44_

---

_Comment by @zanieb on 2024-04-19 00:30_

If anyone is interested in trying out https://github.com/astral-sh/uv/pull/3130, it should resolve these issues but coming up with integration test cases is quite challenging and will take a bit longer.

We're also considering reverting the problematic change at #3131, though that will cause different regressions.

---

_Comment by @romeroyonatan on 2024-04-19 10:37_

Hi @zanieb, I tested the changes and it works. Great job!

---

_Comment by @inoa-jboliveira on 2024-04-19 13:02_

Seems to be working on my issue as well, thank you

---

_Comment by @zanieb on 2024-04-19 14:06_

Thanks for giving it a try! I'll get that cleaned up and try to release today.

---

_Comment by @zanieb on 2024-04-19 21:09_

This may go out on Monday instead.

---

_Closed by @zanieb on 2024-04-22 18:06_

---
