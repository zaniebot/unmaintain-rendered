```yaml
number: 11155
title: "Set `VIRTUAL_ENV` in Jupyter kernels"
type: pull_request
state: merged
author: charliermarsh
labels:
  - documentation
assignees: []
merged: true
base: main
head: charlie/jupyter
created_at: 2025-02-01T19:43:03Z
updated_at: 2025-02-01T21:57:47Z
url: https://github.com/astral-sh/uv/pull/11155
synced_at: 2026-01-12T16:09:42Z
```

# Set `VIRTUAL_ENV` in Jupyter kernels

---

_@charliermarsh_

## Summary

It turns out activating the kernel does not change `VIRTUAL_ENV`, so we still install into the environment the Jupyter environment, rather than the project environment.

Unfortunately, after this change, we do still show a warning on `uv add`:

```
warning: `VIRTUAL_ENV=/Users/crmarsh/.cache/uv/archive-v0/3bddKDdYXuX2w57Fu6itL` does not match the project environment path `.venv` and will be ignored
```

`uv pip install` works without warning.

Closes #11154.


---

_Review requested from @zanieb by @charliermarsh on 2025-02-01 19:43_

---

_Label `documentation` added by @charliermarsh on 2025-02-01 19:43_

---

_Comment by @charliermarsh on 2025-02-01 19:44_

@zanieb -- Do you think it's fair game to suppress that warning if `UV_PYTHON` is set?

---

_Comment by @indrayam on 2025-02-01 19:55_

Thank you for this, @charliermarsh Will try it once I am home.

---

_Comment by @charliermarsh on 2025-02-01 19:56_

Thanks so much for pointing it out. Not sure if I was confused when I wrote the docs or if something changed in Jupyter or uv.

---

_Comment by @zanieb on 2025-02-01 20:00_

> @zanieb -- Do you think it's fair game to suppress that warning if `UV_PYTHON` is set?

Not broadly. Like, `UV_PYTHON` just says which interpreter to use for _creating_ the project environment. It doesn't change that the active environment is inequal to the project environment path.

I'll need to look more closely at what's going on here though.

---

_Comment by @charliermarsh on 2025-02-01 20:05_

Jupyter runs in an ephemeral environment, since it's installed via `--with`. The project is located at `.venv`. The user creates a kernel for the project. We want users to be able to run `!uv add` or `!uv pip install`. `VIRTUAL_ENV` is set to the ephemeral environment, despite the kernel. `!uv pip install` fails because it installs into the ephemeral environment (so this change sets `UV_PYTHON` explicitly to solve that), and `!uv add` succeeds but erroneously warns since `VIRTUAL_ENV` does not equal the project path.


---

_Comment by @charliermarsh on 2025-02-01 20:05_

The other option is setting `VIRTUAL_ENV` in the kernel spec, as opposed to `UV_PYTHON`. I don't know what the implications of that are.

---

_Comment by @zanieb on 2025-02-01 20:11_

I see. It seems okay to silence the `VIRTUAL_ENV` warning if `UV_PYTHON` is set to the project environment path specifically / or (as a broader solution) if the requested interpreter matches the project environment, e.g., `-p .venv`.

It does seem more correct to set `VIRTUAL_ENV` if we can.

---

_Comment by @charliermarsh on 2025-02-01 20:12_

We can, yeah.

---

_@zanieb approved on 2025-02-01 20:13_

---

_Comment by @indrayam on 2025-02-01 21:04_

> Thanks so much for pointing it out. Not sure if I was confused when I wrote the docs or if something changed in Jupyter or uv.

I absolutely adore "uv". And since NOTHING I do these days does not use uv, Jupyter related challenges had been bugging me. Anyways, about to try your changes. Will keep you all posted.

---

_Comment by @danielhollas on 2025-02-01 21:11_

Btw: the warning situation is somewhat similar to what happens when uv is run from pre-commit

https://github.com/astral-sh/uv-pre-commit/issues/36

---

_Comment by @indrayam on 2025-02-01 21:45_

> > Thanks so much for pointing it out. Not sure if I was confused when I wrote the docs or if something changed in Jupyter or uv.
> 
> I absolutely adore "uv". And since NOTHING I do these days does not use uv, Jupyter related challenges had been bugging me. Anyways, about to try your changes. Will keep you all posted.

I recreated a new project and created a new ipython kernel using the updated `uv run ipython kernel` (as per @charliermarsh's commit diff). Everything worked like a charm! And by setting the `VIRTUAL_ENV` environment variable at the kernel level, there were no warnings that I saw, not even when I ran the `!uv add` command. Not sure if that was supposed to be the case.

<img width="1519" alt="image" src="https://github.com/user-attachments/assets/4a17f6e5-bc14-4e75-ab11-8767e4372d5f" />


---

_Comment by @charliermarsh on 2025-02-01 21:54_

Awesome, thank you for testing! It's good to see that the warning is gone too.

---

_Merged by @charliermarsh on 2025-02-01 21:54_

---

_Closed by @charliermarsh on 2025-02-01 21:54_

---

_Branch deleted on 2025-02-01 21:54_

---

_Renamed from "Set `UV_PYTHON` in Jupyter kernels" to "Set `VIRTUAL_ENV` in Jupyter kernels" by @zanieb on 2025-02-01 21:55_

---

_Comment by @indrayam on 2025-02-01 21:57_

> > > Thanks so much for pointing it out. Not sure if I was confused when I wrote the docs or if something changed in Jupyter or uv.
> > 
> > 
> > I absolutely adore "uv". And since NOTHING I do these days does not use uv, Jupyter related challenges had been bugging me. Anyways, about to try your changes. Will keep you all posted.
> 
> I recreated a new project and created a new ipython kernel using the updated `uv run ipython kernel` (as per @charliermarsh's commit diff). Everything worked like a charm! And by setting the `VIRTUAL_ENV` environment variable at the kernel level, there were no warnings that I saw, not even when I ran the `!uv add` command. Not sure if that was supposed to be the case.
> ...

Btw, on a somewhat related note, I am not entirely clear how `uv run --with jupyter jupyter notebook` invocations at times creates a new folder inside `$HOME/.cache/uv/archive-v0/..` with all the jupyter related dependencies and at other times reuses the existing one.

Last evening, as I was dabbling with this, I saw `uv run --with jupyter...` create a folder under `.../archive-v0/`. This morning, when I decided to rerun the same command as I continued my work, I noticed it created a brand new folder and redownloaded Jupyter. Moreover, the older folder was not there anymore. 

I understand that since it is inside `$HOME/.cache/uv/..`, it is ephemeral. I was just curious, especially about the logic you are using to clear the existing Jupyter-related dependencies folder from the cache.


---
