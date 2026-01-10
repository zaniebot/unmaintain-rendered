---
number: 6669
title: Configurable project virtual environment paths (e.g., for Docker)
type: issue
state: closed
author: charliermarsh
labels:
  - needs-decision
assignees: []
created_at: 2024-08-27T02:45:36Z
updated_at: 2024-09-03T17:52:20Z
url: https://github.com/astral-sh/uv/issues/6669
synced_at: 2026-01-10T01:24:04Z
---

# Configurable project virtual environment paths (e.g., for Docker)

---

_Issue opened by @charliermarsh on 2024-08-27 02:45_

_No description provided._

---

_Assigned to @zanieb by @charliermarsh on 2024-08-27 02:45_

---

_Label `needs-decision` added by @zanieb on 2024-08-27 11:16_

---

_Comment by @gbdlin on 2024-08-27 23:50_

Currently `uv` uses `.venv` directory inside the project to store a virtualenv. This is not compatible with all usage scenarios, for example with Docker used for local development with bind mount for project directory, virtualenv will be taken from the host instead of one built inside the docker image. This causes some problems, especially when packages installed in the host system are not compatible with ones inside docker and user wants to access `uv` both from inside and outside of the docker image.

---

_Comment by @zanieb on 2024-08-28 00:07_

@gbdlin 

> for example with Docker used for local development with bind mount for project directory, virtualenv will be taken from the host instead of one built inside the docker image

This is solvable though https://docs.astral.sh/uv/guides/integration/docker/#developing-in-a-container

---

_Comment by @yasserfarouk on 2024-08-28 01:39_

Even though there is a solution for this need using docker, I think it is still helpful to have a way to configure the path for creating the .venv in uv. For example, I prefer to keep all my venvs under a folder in the home directory (i.e. `~/.myvenvs` ) to avoid cluttering my project code folders with `.venv` folders which tend to increase the size of the projects folder `~/code/projects` considerably. This is useful for syncing this folder across machines which I need when working with multiple servers. There are solutions to ignore .venv folders but it would be much better if they do not exist at all. 
Moreover, sometimes, it may beneficial to have multiple projects sharing the same venv when they work in conjunction for a single solution that will end up being deployed together.
Also, it seems not so difficult to add this feature anyway. The responsiblity for making sure no name-conflicts can arise can be the user's. For example, if the path already exists when uv is trying to create the venv, it can just fail.

---

_Comment by @zanieb on 2024-08-28 02:18_

@yasserfarouk Thanks for sharing. That's also discussed in #1495

---

_Comment by @gbdlin on 2024-08-28 09:23_

@zanieb 
> This is solvable though https://docs.astral.sh/uv/guides/integration/docker/#developing-in-a-container

Unfortunately, this is a workaround not a solution, as it has some drawbacks.

Solution with anonymous mount for .venv directory causes a rebuild of virtualenv on first use of command with docker compose, as it fully masks the original one created inside the docker image. It is desirable to use the exact virtualenv that was included in the image to have environment as close to the production one as possible.

Solution with `watch` requires the newest docker compose + on some platforms has a performance penalty, as it relies on watching for file changes instead of using a native bind mount of the file system.


---

_Comment by @terabitti on 2024-08-28 11:15_

Correct me if I’m wrong, but `watch` is only 1-way sync. You can’t run something like `uv add` from a container. The same problem occurs when running some scripts that needs to write in to your project’s directory (for example Django’s `makemigrations` or `makemessages`).

---

_Comment by @zanieb on 2024-08-28 13:12_

@gbdlin I don't think this is true

> Solution with anonymous mount for .venv directory causes a rebuild of virtualenv on first use of command with docker compose, as it fully masks the original one created inside the docker image. It is desirable to use the exact virtualenv that was included in the image to have environment as close to the production one as possible.

It inherits the content from the image. I tested this case explicitly.


---

_Comment by @zanieb on 2024-08-28 13:13_

> You can’t run something like uv add from a container. The same problem occurs when running some scripts that needs to write in to your project’s directory (for example Django’s makemigrations or makemessages).

Interesting, good to know. I'd say a bind mount is what you're looking for then.

---

_Comment by @gbdlin on 2024-08-28 13:42_

@zanieb 
> It inherits the content from the image. I tested this case explicitly.
With `watch` yes, with `volume` no. It will build the virtualenv again in such case, as `.venv` will be empty. It'll be fetched from the existing volume after container restarts, unless you clear volumes, but it's still a separately built environment outside of the docker build process.

You can check it out this way: make sure all volumes for the container are cleared, build the image with `docker build` or `docker compose build`, change project requirements by removing or modifying anything in a way you can check if it's installed from the old or new file, run the container with volumes attached, without rebuilding it, check the state of `.venv` directory. If `uv` command was run before you can access the container, you'll see `.venv` with updated requirements. If it didn't run, `.venv` directory will be empty.

Using `watch` works well, but as terabitti mentioned, it is only done in one direction + has performance issues on some platforms.

---

_Comment by @zanieb on 2024-08-28 13:52_

I feel like it's this simple:
```
❯ docker volume ls
DRIVER    VOLUME NAME
❯ docker run \
    --rm \
    --volume .:/app \
    --volume /app/.venv \
    --publish 8000:8000 \
    -it \
    $(docker build -q .) \
    uv sync
Resolved 28 packages in 0.97ms
Audited 27 packages in 0.12ms
```
Here I run the container with a bind mount and anonymous volume and run `uv sync`. Note there's nothing to do — all of the requirements are in the directory from the image build.

Similarly, you can see `.venv` is _not_ empty.

```
❯ docker run \
    --rm \
    --volume .:/app \
    --volume /app/.venv \
    --publish 8000:8000 \
    -it \
    $(docker build -q .) \
    ls -lah .venv
total 24K
drwxr-xr-x  4 root root 4.0K Aug 28 13:48 .
drwxr-xr-x 14 root root  448 Aug 28 13:45 ..
-rw-r--r--  1 root root    1 Aug 27 01:46 .gitignore
-rw-r--r--  1 root root   43 Aug 27 01:46 CACHEDIR.TAG
drwxr-xr-x  2 root root 4.0K Aug 28 13:48 bin
drwxr-xr-x  3 root root 4.0K Aug 28 13:48 lib
lrwxrwxrwx  1 root root    3 Aug 27 01:46 lib64 -> lib
-rw-r--r--  1 root root  137 Aug 27 01:46 pyvenv.cfg
```

> run the container with volumes attached, without rebuilding it, check the state of .venv directory. If uv command was run before you can access the container, you'll see .venv with updated requirements. If it didn't run, .venv directory will be empty.

If you don't rebuild the image, it won't reflect `pyproject.toml` changes in the environment until you run a `uv` command in it. That's the whole point — the virtual environment isn't mounted from the host to the container. 


---

_Comment by @gbdlin on 2024-08-28 15:01_

My bad, you are indeed right. Looks like my understanding of anonymous volumes was wrong. Thank you!

---

_Comment by @edmorley on 2024-08-29 14:00_

Is this a dupe of #5229?

---

_Comment by @zanieb on 2024-08-29 16:36_

@edmorley sort of. If I put on my pedantic hat, this is about configuring the path of the project's virtual environment and the other one is about allowing `uv sync` to target arbitrary virtual environments and the discussion there seems to focus on using `VIRTUAL_ENV` to target the active environment.

---

_Referenced in [astral-sh/uv#6834](../../astral-sh/uv/pulls/6834.md) on 2024-08-29 22:38_

---

_Closed by @zanieb on 2024-09-03 17:52_

---

_Closed by @zanieb on 2024-09-03 17:52_

---
