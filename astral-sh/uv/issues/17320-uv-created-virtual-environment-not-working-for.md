```yaml
number: 17320
title: uv created virtual environment not working for uWSGI when using --uid --gid
type: issue
state: closed
author: liszca
labels:
  - question
assignees: []
created_at: 2026-01-05T00:12:56Z
updated_at: 2026-01-06T18:27:00Z
url: https://github.com/astral-sh/uv/issues/17320
synced_at: 2026-01-12T16:02:48Z
```

# uv created virtual environment not working for uWSGI when using --uid --gid

---

_@liszca_

### Question

I got stuck. Here is what I am trying to do with uv:

```
rm -r /www
uv init /www
cd /www
uv add django uwsgi
uv run django-admin startproject bikes orbea
uv run orbea/manage.py migrate
uv run orbea/manage.py runserver
uv run uwsgi --chdir=/www/orbea --module=bikes.wsgi:application \
    --home=/www/.venv --http=:8000 --uid 1000 --gid 1000
```
It gives me following error:

```
compiled with version: 15.2.0 on 04 January 2026 22:00:42
os: Linux-6.8.0-64-generic #67-Ubuntu SMP PREEMPT_DYNAMIC Sun Jun 15 20:23:40 UTC 2025
nodename: django
machine: aarch64
clock source: unix
pcre jit disabled
detected number of CPU cores: 2
current working directory: /www
detected binary path: /www/.venv/bin/uwsgi
uWSGI running as root, you can use --uid/--gid/--chroot options
setgid() to 1000
setuid() to 1000
chdir() to /www/orbea
*** WARNING: you are running uWSGI without its master process manager ***
your memory page size is 4096 bytes
detected max file descriptor number: 1048576
lock engine: pthread robust mutexes
thunder lock: disabled (you can enable it with --thunder-lock)
uWSGI http bound on :8000 fd 4
spawned uWSGI http 1 (pid: 1391)
uwsgi socket 0 bound to TCP address 127.0.0.1:41423 (port auto-assigned) fd 3
Python version: 3.14.2 (main, Dec  5 2025, 20:18:10) [Clang 21.1.4 ]
PEP 405 virtualenv detected: /www/.venv
Set PythonHome to /www/.venv
Could not find platform independent libraries <prefix>
Could not find platform dependent libraries <exec_prefix>
Fatal Python error: Failed to import encodings module
Python runtime state: core initialized
ModuleNotFoundError: No module named 'encodings'

Current thread 0x0000f688ac236d20 (most recent call first):
  <no Python frame>
```
When removing option --uid and --gid it does work.

But when setting it up the old way it works with --uid and --gid:

```
rm -r /www
python -m venv /www
cd /www
source bin/activate
pip install --upgrade pip
pip install django uwsgi
django-admin startproject bikes orbea
cd orbea/
python manage.py migrate
python manage.py runserver
/www/bin/uwsgi --chdir=/www/orbea --module=bikes.wsgi:application \
    --home=/www/ --http=:8000 --uid 1000 --gid 1000
```
I remember setting up my python project like this and then switched over to uv and all worked. But I don't get what's missing this time when only using uv.

### Platform

Alpine 3.23

### Version

0.9.16

---

_Label `question` added by @liszca on 2026-01-05 00:12_

---

_Comment by @konstin on 2026-01-05 13:52_

Does upgrading to the latest uv and reinstalling the Python interpreter (to the latest patch version) help? We recently fixed this error in the managed Python builds: https://github.com/astral-sh/python-build-standalone/issues/380



---

_Comment by @liszca on 2026-01-05 15:06_

> Does upgrading to the latest uv and reinstalling the Python interpreter (to the latest patch version) help? We recently fixed this error in the managed Python builds: [astral-sh/python-build-standalone#380](https://github.com/astral-sh/python-build-standalone/issues/380)

Sadly no change.

Can you reproduce it on your system?

---

_Comment by @zanieb on 2026-01-05 18:53_

Can you share a reproduction in a Docker image? https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/#docker-image

---

_Comment by @liszca on 2026-01-05 23:38_

> Can you share a reproduction in a Docker image? https://docs.astral.sh/uv/reference/troubleshooting/reproducible-examples/#docker-image

I do not have Docker, but can you check if this actually run:
```
FROM ghcr.io/astral-sh/uv:0.5.24-debian-slim

WORKDIR /www

# set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    UV_LINK_MODE=copy \
    UV_PYTHON_DOWNLOADS=never \
    UV_PROJECT_ENVIRONMENT=/www/.venv
RUN apt update && apt upgrade -y
RUN apt install -y --no-install-recommends --no-install-suggests \
    curl python3 build-essential libpcre2-dev
RUN curl --proto '=https' --tlsv1.2 -LsSf https://github.com/astral-sh/uv/releases/download/0.9.21/uv-installer.sh | sh
RUN ln -s /root/.local/bin/uv /usr/bin/uv
RUN apt install -y libpython3-dev
RUN uv init /www \
    && cd /www \
    && uv add django uwsgi
RUN mkdir orbea \
    && uv run django-admin startproject bikes orbea \
    && uv run orbea/manage.py migrate

CMD ["uv", "run", "uwsgi", "--chdir=/www/orbea", "--module=bikes.wsgi:application", "--home=/www/.venv", "--http=:8000", "--uid", "1000", "--gid", "1000"]

```
After building it I figured out thatss working but why?

- Is it because of Debian?
- Is it because of x86?

I really would like to use Alpine, smaller storage footprint and that combined with arm64 ....

---

_Comment by @liszca on 2026-01-06 02:08_

I gave it another try, it also doesn't work with alpine on x86. But it does work using pip on alpine.

Is it a bug or am I missing something? As the wired thing is it is a different Error but als in uWSGI.

---

_Comment by @zanieb on 2026-01-06 04:08_

Can you share the reproduction for alpine?

---

_Comment by @liszca on 2026-01-06 17:33_

After some sleep one of my Dockerfiles actually worked, except it doesn't start the service when running the container, but I can go inside, start it and it does work.

```
FROM alpine:3.23

# set environment variables
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1 \
    UV_LINK_MODE=copy \
    UV_PYTHON_DOWNLOADS=never \
    UV_PROJECT_ENVIRONMENT=/www/.venv
RUN apk add curl build-base linux-headers pcre-dev python3-dev \
    && adduser django -D \
    && mkdir /var/log/uwsgi \
    && touch /var/log/uwsgi/uwsgi.log \
    && chown -R django: /var/log/uwsgi
RUN curl --proto '=https' --tlsv1.2 \
    -LsSf https://github.com/astral-sh/uv/releases/download/0.9.21/uv-installer.sh | sh \
    && ln -s /root/.local/bin/uv /usr/bin/uv

WORKDIR /www
RUN uv init /www && cd /www \
    && uv add django uwsgi \
    && mkdir orbea && uv run django-admin startproject bikes orbea \
    && uv run orbea/manage.py migrate

CMD [ "uv", "run", "uwsgi", "--chdir=/www/orbea", "--module=bikes.wsgi:application", "--home=/www/.venv", "--http=:8000", "--uid", "1000", "--gid", "1000", "--logger", "file:logfile=/var/log/uwsgi/uwsgi.log,maxsize=2000000", "--logto2", "/var/log/uwsgi/uwsgi.log" ]
```

---

_Closed by @liszca on 2026-01-06 17:38_

---

_Comment by @liszca on 2026-01-06 17:53_

Ok going into the container with `docker run -it container1 ash` stopps the running CMD command. So its actually working as intended.
But how to go inside without interrupting the containers buisness?

Like this:
`docker exec elastic_nobel netstat -tulpn`

---
