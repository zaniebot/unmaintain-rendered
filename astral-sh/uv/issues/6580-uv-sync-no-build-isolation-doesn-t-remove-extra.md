```yaml
number: 6580
title: "`uv sync --no-build-isolation` doesn't remove extra dependencies"
type: issue
state: closed
author: kabouzeid
labels:
  - question
assignees: []
created_at: 2024-08-24T12:12:27Z
updated_at: 2024-08-26T18:05:00Z
url: https://github.com/astral-sh/uv/issues/6580
synced_at: 2026-01-12T15:59:05Z
```

# `uv sync --no-build-isolation` doesn't remove extra dependencies

---

_@kabouzeid_

Whenever `uv sync` is called with `--no-build-isolation`, any extra packages that shouldn't be there aren't automatically removed.

```
➜ uv --version
uv 0.3.3 (Homebrew 2024-08-23)

➜ uv add flask
Using Python 3.12.5
Creating virtualenv at: .venv
Resolved 9 packages in 2ms
   Built bug @ file:///private/tmp/bug
Prepared 8 packages in 144ms
Installed 8 packages in 3ms
 + blinker==1.8.2
 + bug==0.1.0 (from file:///private/tmp/bug)
 + click==8.1.7
 + flask==3.0.3
 + itsdangerous==2.2.0
 + jinja2==3.1.4
 + markupsafe==2.1.5
 + werkzeug==3.0.4

➜ uv add --optional build setuptools
Resolved 10 packages in 69ms
   Built bug @ file:///private/tmp/bug
Prepared 2 packages in 145ms
Uninstalled 1 package in 0.34ms
Installed 2 packages in 16ms
 ~ bug==0.1.0 (from file:///private/tmp/bug)
 + setuptools==73.0.1

➜ uv sync --no-build-isolation
Resolved 10 packages in 2ms
Audited 8 packages in 0.04ms

➜ uv sync
Resolved 10 packages in 0.50ms
Uninstalled 1 package in 26ms
 - setuptools==73.0.1
 
 ➜ uv pip install ruff
Resolved 1 package in 2ms
Installed 1 package in 1ms
 + ruff==0.6.2

➜ uv sync --no-build-isolation
Resolved 10 packages in 0.66ms
Audited 8 packages in 0.05ms

➜ uv sync
Resolved 10 packages in 0.63ms
Uninstalled 1 package in 0.65ms
 - ruff==0.6.2
```

---

_Comment by @charliermarsh on 2024-08-24 12:19_

I think this is intentional. If you're using `--no-build-isolation`, then you likely have packages in the environment that are necessary as build dependencies, but not first-party dependencies. I think this would end up causing people a lot of confusion.

---

_Comment by @kabouzeid on 2024-08-24 12:29_

Yes, but those should be removed after building, right?

```shell
uv sync
uv sync --extra build-deps
uv sync --extra compiled --no-build-isolation  # build-deps should be removed afterwards
```

---

_Comment by @charliermarsh on 2024-08-24 12:38_

It's a tough call, because you then run the risk that subsequent syncs fail due to lacking your build dependencies. (It makes sense in the context of the setup you have there, but in other cases, users install build dependencies into venv "manually" prior to `uv sync`, and don't even declare them as project dependencies.) I might suggest an extra `uv sync` at the end to explicitly remove them.

---

_Label `question` added by @charliermarsh on 2024-08-25 02:29_

---

_Comment by @charliermarsh on 2024-08-25 02:30_

I'm going to call this working as intended, since we used to do the opposite and received issues that it was confusing.

---

_Closed by @charliermarsh on 2024-08-25 02:30_

---

_Comment by @kabouzeid on 2024-08-25 09:56_

Thank you for your consideration. I needed some time to fully think through this problem.

I may have misunderstood how the various APIs are intended to be used. In my understanding, for projects you would typically use the `uv {add, remove, sync, lock, run}` project interface. In this context, the `uv pip` interface would only be used for temporarily installing packages that won't be locked and will be removed after the next sync. Here, I see `uv pip` as a low-level escape hatch for manually fixing-up the project. In git terms, `add` commits, `pip` leaves the project in dirty state and `sync` hard resets. That being said, I found the hidden side effect of `--no-build-isolation` quite confusing. 

I'm struggling to think of a use case where you'd want to install some dependencies using `pip` instead of `add`, choosing not to include them in your project's dependencies, yet still not wanting them to be removed by `sync`. Even if you really do want to keep undeclared dependencies, there's already the `--inexact` flag that serves that purpose. (I also just discovered that `--exact` has no effect when used with `--no-build-isolation` https://github.com/astral-sh/uv/issues/6599.)

Perhaps I'm missing the bigger picture here regarding what people generally use `--no-build-isolation` for. Personally, I only use it for building torch extensions. Here, the extension always needs to be built with the exact version of torch that is used in the project, which is also the reason why torch extensions never specify torch as a build dependency.


---

_Comment by @charliermarsh on 2024-08-25 11:39_

> In this context, the uv pip interface would only be used for temporarily installing packages that won't be locked and will be removed after the next sync. Here, I see uv pip as a low-level escape hatch for manually fixing-up the project. In git terms, add commits, pip leaves the project in dirty state and sync hard resets. 

This is all correct!

The problem here is somewhat fundamental to `--no-build-isolation`, which is that the dependencies _must_ be in the venv prior to running the install. So, like, if you just have `torch` and `flash-attn` in your dependency list, and run with `--no-build-isolation`, I expect that to fail, since `flash-attn` would be installed in the empty environment.

I like what you're doing above, where you have separate extras for your build dependencies and those dependencies that require build isolation. In _that_ case, it would be safe for you to run with `--exact` semantics. The question is just whether we use that behavior by default.

We'd like to come up with a better solution for these kinds of dependencies that doesn't require `--no-build-isolation` at all (i.e., some other kind of API). In the meantime, I could consider adding documentation based on the workflow you described above, and changing the behavior to avoid special-casing `--no-build-isolation`. I have to look back at the issues that led us to add it.

---

_Comment by @charliermarsh on 2024-08-25 11:41_

I think I will change the defaults here.

---

_Reopened by @charliermarsh on 2024-08-25 11:41_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-25 11:41_

---

_Closed by @charliermarsh on 2024-08-26 18:05_

---
