```yaml
number: 9576
title: NETRC environment variable ignored for git installs
type: issue
state: open
author: remidebette
labels:
  - bug
  - external
assignees: []
created_at: 2024-12-02T14:13:20Z
updated_at: 2025-06-04T03:55:56Z
url: https://github.com/astral-sh/uv/issues/9576
synced_at: 2026-01-12T15:59:53Z
```

# NETRC environment variable ignored for git installs

---

_@remidebette_

Hi, 

Thank you for the nice tool.

I have been testing `.netrc` support to implement installs from private packages.
I also need NETRC env var support to be able to inject the `.netrc` file in various Docker contexts without having to worry where is the home folder in each case (debian, alpine, ...)

The error is that this works for private pypi packages, but not for `git+https://` packages.

In my dockerfiles:
```dockerfile
RUN --mount=type=secret,id=netrc \
  NETRC=/run/secrets/netrc \
  uv sync
```

In my `pyproject.toml`, if I set 
```toml
[tool.uv]
extra-index-url = ["https://<my private pypi index>"]
```
Everything is fine, specific packages coming from the extra index get installed (if I pass the secret properly at build time)

But now, for libraries that are installed from git pull do not work:
```toml
dependencies = [
  "something @ git+https://github.com/something",
]
```
in the case `something` is a private repo only accessible with a github token with enough rights

The failure logs:
```bash
#28 0.334 Using CPython 3.10.12 interpreter at: /usr/local/bin/python3
#28 0.334 Creating virtual environment at: .venv
#28 0.374 Resolved 305 packages in 2ms
#28 0.971   × Failed to download and build `<EDITED> @
#28 0.971   │ git+[https://github.com/<EDITED>@<EDITED>`](https://github.com/<EDITED>@<EDITED>%60)
#28 0.971   ├─▶ Git operation failed
#28 0.971   ├─▶ failed to clone into: /root/.cache/uv/git-v0/db/456d050564230643
#28 0.971   ├─▶ failed to fetch commit `<EDITED>`
#28 0.971   ╰─▶ process didn't exit successfully: `/usr/bin/git fetch --force
#28 0.971       --update-head-ok 'https://github.com/<EDITED>'
#28 0.971       '+<EDITED>:refs/remotes/origin/HEAD'`
#28 0.971       (exit status: 128)
#28 0.971       --- stderr
#28 0.971       fatal: could not read Username for 'https://github.com/': No such device
#28 0.971       or address
```


My guess is that the git binary is doing the authentication in that case, and might not get the env var content.


Alternately, When forcing `.netrc` to be in the home, it then works ; but it goes against good practice not to copy build secrets in the image layers.
```dockerfile
RUN --mount=type=secret,id=netrc \
  cp /run/secrets/netrc ~/.netrc && \
  uv sync && \
  rm ~/.netrc
```

---

_Renamed from "NETRC environment variables ignored for git installs" to "NETRC environment variable ignored for git installs" by @remidebette on 2024-12-02 14:25_

---

_Comment by @zanieb on 2024-12-02 15:48_

Yeah we call into the `git` CLI. Perhaps it does not support the `NETRC` environment variable?

---

_Comment by @charliermarsh on 2024-12-03 01:12_

Hmm yeah, that's a good guess... Looking at the docs, it looks like Git does respect `~/.netrc`, but doesn't provide any way to set it to a custom path?

---

_Comment by @remidebette on 2024-12-03 08:40_

Hi, 
I could not find a mention of NETRC on the online git SCM documentation. I sent an email to the git mailing list to ask if there is an alternative.

Is this env var a standard or was it created for uv?

---

_Comment by @zanieb on 2024-12-03 14:16_

Some historical context in https://github.com/pypa/pip/issues/11023

It's supported in the netrc crate we use https://github.com/gribouille/netrc/blob/f8b614440a8d45cae8a5bd90a43266d8f487b93b/src/lib.rs#L85-L105

---

_Label `bug` added by @charliermarsh on 2024-12-06 00:34_

---

_Label `upstream` added by @charliermarsh on 2024-12-06 00:34_

---

_Comment by @remidebette on 2024-12-10 08:28_

Hi, 

I have been a bit slow, I don't understand how to get the responses of the git mailing lists.

Anyway the answer can be seen here:

https://lore.kernel.org/git/20241202211156.GG776185@coredump.intra.peff.net/

Would you think it is better to get support from git or libcurl?

---

_Comment by @zanieb on 2024-12-10 14:09_

It seems like support in curl makes sense? I'd be curious to hear what they say at least.

---

_Comment by @remidebette on 2024-12-10 23:13_

Asked: https://github.com/curl/curl/discussions/15713

According to this doc https://curl.se/libcurl/c/libcurl-env.html, they might accept `HOME` (will test it)
But it could have side effects in some situations

---

_Comment by @zanieb on 2024-12-11 00:33_

A good effort, thanks for following up!

---

_Comment by @remidebette on 2024-12-11 10:04_

So at the very least `HOME` works with git dependencies, but the file has to be named `.netrc`
```Dockerfile
RUN --mount=type=secret,id=netrc,dst=/run/secrets/.netrc \
  HOME=/run/secrets \
  uv sync
```

but again, I worry of the side effects, and I guess `NETRC` still has to be set for usual python packages

---

_Comment by @oconnor663 on 2025-06-04 03:55_

There's some discussion of alternatives to `~/.netrc` here: https://github.com/astral-sh/uv/issues/11342#issuecomment-2938352842

---
