---
number: 8202
title: Install packages in managed python
type: issue
state: closed
author: ion-elgreco
labels:
  - question
assignees: []
created_at: 2024-10-15T08:59:44Z
updated_at: 2024-12-26T16:54:19Z
url: https://github.com/astral-sh/uv/issues/8202
synced_at: 2026-01-10T01:24:25Z
---

# Install packages in managed python

---

_Issue opened by @ion-elgreco on 2024-10-15 08:59_

Currently when you install packages with `--system` it will use the system installation. Is there a way to force to install packages in the managed installation without creating a venv?



---

_Comment by @charliermarsh on 2024-10-15 13:55_

Are you asking if there's a way to achieve the `--system` behavior without passing `--system`?

---

_Label `question` added by @charliermarsh on 2024-10-15 13:55_

---

_Comment by @zanieb on 2024-10-15 14:01_

We explicitly don't allow installing packages into the managed Python installation environments.

---

_Comment by @ion-elgreco on 2024-10-15 15:08_

> We explicitly don't allow installing packages into the managed Python installation environments.

Why not? 

---

_Comment by @zanieb on 2024-10-15 15:25_

Because they're only intended to be sources for creating virtual environments. We are trying to move away from mutable global state.

---

_Comment by @ion-elgreco on 2024-10-15 15:41_

> Because they're only intended to be sources for creating virtual environments. We are trying to move away from mutable global state.

Hmm I need to setup a system wide installation which has all the packages since the environment has no internet access. I think there is still some domains that need this 

---

_Comment by @zanieb on 2024-10-15 16:11_

Perhaps our [Docker guide](https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment) would be helpful to demonstrate how to setup a system-wide environment.

---

_Comment by @ion-elgreco on 2024-10-15 16:15_

> Perhaps our [Docker guide](https://docs.astral.sh/uv/guides/integration/docker/#using-the-environment) would be helpful to demonstrate how to setup a system-wide environment.

I'm using a custom base image, so I wanted to avoid the hassle of installing a specific python version and use UV for that, and then install packages in that managed python version. 

But I will install the python version I need explicitly then

---

_Comment by @zanieb on 2024-10-15 16:19_

You can still do that, you just need to put create a virtual environment and put it on the path (as described in the linked guide). Is there a reason that's not an option?

---

_Comment by @ion-elgreco on 2024-10-15 16:25_

> You can still do that, you just need to put create a virtual environment and put it on the path (as described in the linked guide). Is there a reason that's not an option?

There is no project to install so to say, also the directory where stuff will happen is not fixed 



---

_Comment by @zanieb on 2024-10-15 17:06_

I'm not sure I really understand your point, why isn't this sufficient:

```
uv venv
export VIRTUAL_ENV="$(pwd)/.venv"
export PATH="$VIRTUAL_ENV/bin:$PATH"
```

---

_Comment by @ion-elgreco on 2024-10-18 07:29_

> I'm not sure I really understand your point, why isn't this sufficient:
> 
> ```
> uv venv
> export VIRTUAL_ENV="$(pwd)/.venv"
> export PATH="$VIRTUAL_ENV/bin:$PATH"
> ```

Because the working directory might not be known, so someone has to specifically know the venv path since our IDE's don't recognize it automatically. 

I tried this now with a small test but having a system installation with packages installed in it, has the preference

---

_Comment by @zanieb on 2024-10-18 14:30_

> so someone has to specifically know the venv path since our IDE's don't recognize it automatically.

Why don't you make the virtual environment in a specific location then? One that your IDE looks in?

The IDE won't look in the managed Python environment either, it's in an arbitrary location.

---

_Closed by @charliermarsh on 2024-12-26 16:54_

---
