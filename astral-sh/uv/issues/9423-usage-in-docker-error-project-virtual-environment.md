```yaml
number: 9423
title: "Usage in docker: `error: Project virtual environment directory /app/.venv cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment"
type: issue
state: closed
author: ben-xD
labels:
  - great writeup
assignees: []
created_at: 2024-11-25T17:04:22Z
updated_at: 2025-07-23T09:53:42Z
url: https://github.com/astral-sh/uv/issues/9423
synced_at: 2026-01-12T15:59:50Z
```

# Usage in docker: `error: Project virtual environment directory /app/.venv cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment

---

_@ben-xD_

Thanks for the detailed docker docs! I'm trying to use uv in docker compose for local development. Trying to bind mount my application code **and** an anonymous volume for `app/.venv`. This is similar to the approach shown in [docs with `docker run`](https://docs.astral.sh/uv/guides/integration/docker/#mounting-the-project-with-docker-run) which aims to avoid overwriting host vs docker container venv files, and keep them separate. 

Sadly, `uv sync` inside the container errors with:

```bash
error: Project virtual environment directory /app/.venv cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment
```

This might be because uv tries to replace the `.venv` folder, but it can't since it's a volume?

## Docker compose settings
```yaml
services:
  reproduce:
    build:
      context: .
      dockerfile: Dockerfile
    entrypoint: "tail -f /dev/null"
    volumes:
      - ./:/app
      - /app/.venv
```

## Details
- version: uv 0.5.4
- OS: debian 12

## Reproducible setup
- `git clone git@github.com:ben-xD/reproduce-uv-docker-compose-error.git`
- `docker-compose up -d`
- Enter shell: `docker exec -it uv-reproduce-error-reproduce-1 bash`
- Run uv: `uv sync`
- Observe error: 
```
error: Project virtual environment directory `/app/.venv` cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment
```

## Following documentation's docker run

I also tried following the docs, by running:
```
docker run -it --rm --volume .:/app --volume /app/.venv uv-reproduce-error-reproduce bash
```
Then running
```
uv sync
```

but i get the same error:
```
root@ea61fab74354:/app# uv sync
Using CPython 3.11.10
error: Project virtual environment directory `/app/.venv` cannot be used because it is not a compatible environment but cannot be recreated because it is not a virtual environment
```

---

_Comment by @ben-xD on 2024-11-25 17:12_

The [docker compose watch feature]( https://docs.astral.sh/uv/guides/integration/docker/#configuring-watch-with-docker-compose) isn't ideal though because it the python dependencies are redownloaded whenever the image needs to be rebuild - it doesn't cache `.venv`, just ignores it.

I also can't enter the container shell to run commands interactively (it really helps experimenting with container/linux tools used in CI quickly). Also, the sync is 1 way: i cant make changes inside the container.

---

_Comment by @zanieb on 2024-11-25 18:06_

Thank you for the reproduction!

The reason you get that error is because the `.venv` directory exists, but is not a virtual environment. This is a safeguard to avoid mutating target directories that we should not. We might want to relax that to allow mutating an empty directory, but this conflicts with our removal of the directory to create a virtual environment:

```
root@8ceb311d28c1:/app# uv venv
Using CPython 3.11.10
Creating virtual environment at: .venv
uv::venv::creation

  x Failed to create virtualenv
  `-> failed to remove directory `/app/.venv`: Resource busy (os error 16)
```

>  the python dependencies are redownloaded whenever the image needs to be rebuild - it doesn't cache .venv, just ignores it.

Perhaps you're looking to use a [cache mount](https://github.com/astral-sh/uv-docker-example/blob/a14ebc89e3a5e5b33131284968d8969ae054ed0d/Dockerfile#L13-L23) to store dependency downloads?

> Also, the sync is 1 way: i cant make changes inside the container.

Here the idea is that you would be able to modify the `pyproject.toml` with `uv add` and have it reflected inside and outside of the container?




---

_Comment by @ben-xD on 2024-11-25 18:32_

Thanks for your reply! Yes it makes sense that it doesn't yet work but I can't find another way of isolating the 2 python environments (docker/linux vs. host/macOS) to avoid needing to download/reinstall everything when switching between testing my python app in my docker and host environments. I can't "exclude" the `.env`.

I don't think cache mount would work since I want to update the cache whilst inside the container like with `docker exec -it $container_name bash`? Cache mounts are for build time?

> Here the idea is that you would be able to modify the pyproject.toml with uv add and have it reflected inside and outside of the container?

The 2 way sync is not super important tbh. But yea, it would be ideal if `uv add` updates the lock file, and that's synced to my host file. system. But definitely syncing from host to linux. It would be nice if `uv sync` just downloads the package that was missing.


---

_Comment by @zanieb on 2024-11-25 18:36_

Okay confusing, we _do_ ignore empty directories but it's different in this case? Presumably because it's a mounted volume? If I run `uv venv` twice, we create it:

```
root@8ceb311d28c1:/app# uv venv -v
DEBUG uv 0.5.4
DEBUG Found project root: `/app`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/app/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Searching for Python 3.11 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.11.10-linux-aarch64-gnu`
DEBUG Found `cpython-3.11.10-linux-aarch64-gnu` at `/root/.local/share/uv/python/cpython-3.11.10-linux-aarch64-gnu/bin/python3.11` (managed installations)
Using CPython 3.11.10
Creating virtual environment at: .venv
DEBUG Removing existing directory
uv::venv::creation

  x Failed to create virtualenv
  `-> failed to remove directory `/app/.venv`: Resource busy (os error 16)
root@8ceb311d28c1:/app# uv venv -v
DEBUG uv 0.5.4
DEBUG Found project root: `/app`
DEBUG No workspace root found, using project root
DEBUG Reading Python requests from version file at `/app/.python-version`
DEBUG Using Python request `3.11` from version file at `.python-version`
DEBUG Searching for Python 3.11 in managed installations or search path
DEBUG Searching for managed installations at `/root/.local/share/uv/python`
DEBUG Found managed installation `cpython-3.11.10-linux-aarch64-gnu`
DEBUG Found `cpython-3.11.10-linux-aarch64-gnu` at `/root/.local/share/uv/python/cpython-3.11.10-linux-aarch64-gnu/bin/python3.11` (managed installations)
Using CPython 3.11.10
Creating virtual environment at: .venv
DEBUG Ignoring empty directory
```

---

_Comment by @zanieb on 2024-11-25 18:44_

> I don't think cache mount would work since I want to update the cache whilst inside the container like with docker exec -it $container_name bash? Cache mounts are for build time?

I see. I was responding specifically to "whenever the image needs to be rebuild" — you could also mount the cache at runtime — though I'm not sure it'd be safe to concurrent access since we can't lock files across the host and container.

> But yea, it would be ideal if uv add updates the lock file, and that's synced to my host file. system.

This sort of works, but Docker syncs the the virtual environment back to the host and breaks things. It seems feasible with some work.

---

_Comment by @zanieb on 2024-11-25 19:03_

I'm not sure what the best next step here is. Perhaps you can use `UV_PROJECT_ENVIRONMENT` to try to move the virtual environment outside of the project directory while in the container to avoid collisions.

---

_Comment by @ben-xD on 2024-11-25 19:46_

That's great! Thanks a lot! I was trying to use `VIRTUAL_ENV` but that was being ignored.

---

_Comment by @zanieb on 2024-11-25 20:25_

Does that solve all your problems here?

---

_Label `great writeup` added by @zanieb on 2024-11-26 17:51_

---

_Comment by @leorochael on 2025-05-22 11:57_

@zanieb, I can no longer reproduce the error following the reproduction instructions on this issue report.

It seems #9427 indeed fixes this issue and it could be closed.

---

_Comment by @ben-xD on 2025-05-22 14:38_

Not sure why I didn't see this, but yes that solves my problem. Thank you!

---

_Closed by @ben-xD on 2025-05-22 14:38_

---

_Comment by @mayrop on 2025-07-22 20:42_

What is your final setup @ben-xD? 

---

_Comment by @ben-xD on 2025-07-23 09:53_

My issue was solved by a PR linked above

Now, I don't even use docker for my project, much simpler!

---
