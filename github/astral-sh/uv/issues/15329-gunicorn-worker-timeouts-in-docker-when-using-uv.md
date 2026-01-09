---
number: 15329
title: Gunicorn worker timeouts in Docker when using uv run
type: issue
state: closed
author: SoAp9035
labels:
  - question
assignees: []
created_at: 2025-08-17T08:37:06Z
updated_at: 2025-10-23T14:38:54Z
url: https://github.com/astral-sh/uv/issues/15329
synced_at: 2026-01-07T13:12:19-06:00
---

# Gunicorn worker timeouts in Docker when using uv run

---

_Issue opened by @SoAp9035 on 2025-08-17 08:37_

### Summary

When the Flask site is refreshed repeatedly (or sometimes after random idle periods), a Gunicorn worker times out and logs an error while parsing the HTTP request (Error handling request (no URI read)). The application previously ran stably using Docker when started without uv / virtual env tooling (plain python + gunicorn). After migrating to uv (using uv lock + uv run + gunicorn) the intermittent timeout began appearing.

This error does not occur when I run the project directly as uv run main.py without using Docker.

I tried other Python images and the other fixes that were suggested on the internet, but I couldn't fix it.

Logs
```
[2025-08-17 07:48:56 +0000] [10] [INFO] Starting gunicorn 23.0.0
[2025-08-17 07:48:56 +0000] [10] [INFO] Listening at: http://0.0.0.0:8080 (10)
[2025-08-17 07:48:56 +0000] [10] [INFO] Using worker: sync
[2025-08-17 07:48:56 +0000] [11] [INFO] Booting worker with pid: 11
[2025-08-17 07:49:43 +0000] [10] [CRITICAL] WORKER TIMEOUT (pid:11)
[2025-08-17 07:49:43 +0000] [11] [ERROR] Error handling request (no URI read)
Traceback (most recent call last):
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/workers/sync.py", line 133, in handle
    req = next(parser)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/parser.py", line 41, in __next__
    self.mesg = self.mesg_class(self.cfg, self.unreader, self.source_addr, self.req_count)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 259, in __init__
    super().__init__(cfg, unreader, peer_addr)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 60, in __init__
    unused = self.parse(self.unreader)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 271, in parse
    self.get_data(unreader, buf, stop=True)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 262, in get_data
    data = unreader.read()
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/unreader.py", line 36, in read
    d = self.chunk()
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/unreader.py", line 63, in chunk
    return self.sock.recv(self.mxchunk)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/workers/base.py", line 204, in handle_abort
    sys.exit(1)
SystemExit: 1
[2025-08-17 07:49:43 +0000] [11] [INFO] Worker exiting (pid: 11)
```

Previous Dockerfile (stable):
```
FROM python:3.13.5-alpine

WORKDIR /usr/src/app

COPY ./requirements.txt .

RUN pip install --no-cache-dir -r requirements.txt

COPY . .

EXPOSE 8080

CMD ["gunicorn", "--bind", "0.0.0.0:8080", "main:app"]
```

Current Dockerfile (problematic):
```
FROM python:3.13.6-slim-bookworm

COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

WORKDIR /app
ADD . /app

RUN uv sync --locked

EXPOSE 8080

CMD ["uv", "run", "--no-sync", "gunicorn", "--bind", "0.0.0.0:8080", "main:app"]
```

I also ran these commands in the Docker container, in case that helps. I don't know.
```
# uv run python -c "import sys; print(sys.executable)"
/app/.venv/bin/python3
# python -c "import sys; print(sys.executable)"
/usr/local/bin/python
```

Environment:
- uv: latest
- Python: 3.13.6
- Flask: 3.1.1
- gunicorn: 23.0.0 (default sync worker)
- Other deps: supabase client, google-cloud-storage, beautifulsoup4

Thanks!

### Platform

Windows 11

### Version

uv 0.8.11 (f892276ac 2025-08-14)

### Python version

Python 3.13.6

---

_Label `bug` added by @SoAp9035 on 2025-08-17 08:37_

---

_Comment by @charliermarsh on 2025-08-17 08:52_

Candidly I would be a little surprised if this were a uv issue, because it seems like application code, but if you'd like, you can change your invocation at the end to `CMD ["/app/.venv/bin/gunicorn", "--bind", "0.0.0.0:8080", "main:app"]` to avoid calling `uv run`.

---

_Comment by @SoAp9035 on 2025-08-17 19:01_

@charliermarsh I also tried it, but it didn’t work. I still have the same problem.

To test it, I created a small Flask app. But the issue still happens.

Here is what I did:

1. I ran this command:
   ```
   uv init
   ```

2. Then I installed the dependencies:
   ```
   uv add flask gunicorn
   ```

3. I created a `main.py` file with this content:
   ```python
   from flask import Flask

   app = Flask(__name__)

   @app.route("/")
   def home():
       return "<h1>It is working.</h1>"

   if __name__ == "__main__":
       app.run(debug=True)
   ```

4. When I run the app using `uv run main.py`, everything works fine.

5. Here is my `Dockerfile`:
   ```Dockerfile
   FROM python:3.13.6-slim-bookworm

   COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

   WORKDIR /app

   ADD . /app

   RUN uv sync --locked

   CMD ["uv", "run", "gunicorn", "--bind", "0.0.0.0:8080", "main:app"]
   ```

6. When I visit the web page and refresh several times (including hard refresh), I get this error:
   ```
   [2025-08-17 18:47:55 +0000] [10] [INFO] Starting gunicorn 23.0.0
   [2025-08-17 18:47:55 +0000] [10] [INFO] Listening at: http://0.0.0.0:8080 (10)
   [2025-08-17 18:47:55 +0000] [10] [INFO] Using worker: sync
   [2025-08-17 18:47:55 +0000] [11] [INFO] Booting worker with pid: 11
   [2025-08-17 18:48:40 +0000] [10] [CRITICAL] WORKER TIMEOUT (pid:11)
   [2025-08-17 18:48:40 +0000] [11] [ERROR] Error handling request (no URI read)
   Traceback (most recent call last):
     File "/app/.venv/lib/python3.13/site-packages/gunicorn/workers/sync.py", line 133, in handle
       req = next(parser)
     File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/parser.py", line 41, in __next__
       self.mesg = self.mesg_class(self.cfg, self.unreader, self.source_addr, self.req_count)
                   ~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
     File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 259, in __init__
       super().__init__(cfg, unreader, peer_addr)
       ~~~~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^
     File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 60, in __init__
       unused = self. Parse(self.unreader)
     File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 271, in parse
       self.get_data(unreader, buf, stop=True)
       ~~~~~~~~~~~~~^^^^^^^^^^^^^^^^^^^^^^^^^^
     File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 262, in get_data
       data = unreader.read()
     File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/unreader.py", line 36, in read
       d = self. Chunk()
     File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/unreader.py", line 63, in chunk
       return self.sock.recv(self.mxchunk)
              ~~~~~~~~~~~~~~^^^^^^^^^^^^^^
     File "/app/.venv/lib/python3.13/site-packages/gunicorn/workers/base.py", line 204, in handle_abort
       sys.exit(1)
       ~~~~~~~~^^^
   SystemExit: 1
   [2025-08-17 18:48:40 +0000] [11] [INFO] Worker exiting (pid: 11)
   [2025-08-17 18:48:40 +0000] [12] [INFO] Booting worker with pid: 12
   ```

This error keeps happening over and over when I refresh the page. I’m not sure what is wrong. Everything works fine locally with `uv run main.py`, but not in Docker using Gunicorn.

---

_Comment by @SoAp9035 on 2025-08-17 19:32_

I added a `.dockerignore` file and added `.venv` to it.

Then, I followed the official `uv-docker-example` from GitHub:  
https://github.com/astral-sh/uv-docker-example/blob/main/Dockerfile

I tried again with the two new Dockerfiles below. But I'm still getting the same error.

```Dockerfile
FROM python:3.13.6-slim-bookworm

COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

WORKDIR /app

ADD . /app

RUN uv sync --locked

ENV PATH="/app/.venv/bin:$PATH"

CMD ["gunicorn", "--bind", "0.0.0.0:8080", "main:app"]
```

```Dockerfile
FROM python:3.13.6-slim-bookworm

COPY --from=ghcr.io/astral-sh/uv:latest /uv /uvx /bin/

WORKDIR /app

COPY pyproject.toml uv.lock ./

RUN uv sync --locked

COPY . .

ENV PATH="/app/.venv/bin:$PATH"

EXPOSE 8080

CMD ["gunicorn", "--bind", "0.0.0.0:8080", "main:app"]
```

Both Dockerfiles give me the same timeout error.

I noticed that I can trigger the error immediately when I do a "hard refresh" (clear cache and refresh) on the website.

---

_Comment by @edmorley on 2025-08-17 19:36_

> Previous Dockerfile (stable):
> 
> ```
> FROM python:3.13.5-alpine
> ```

Your working `Dockerfile` is:
(a) using a different OS (Alpine instead of Debian),
(b) using a different patch version of Python (3.13.5 instead of 3.13.6)



---

_Comment by @SoAp9035 on 2025-08-17 19:48_

@edmorley Sorry, I forgot to mention. When I migrated the project to use `uv`, I also updated the versions. So I get the same results with Python 3.13.6 as well.

---

_Comment by @abkarada on 2025-08-19 11:02_

Well, if the problem is a network bottleneck it would be easier to investigate. I feel like it’s probably a socket bug from this:
[CRITICAL] WORKER TIMEOUT
[ERROR] Error handling request (no URI read)
That means the WORKER takes a request, but before the HTTP (TCP) request is processed, the socket is closed or frozen. The “frozen” option is tricky — I’m thinking about refusing or closing. If it’s about refusing, you could try from your PowerShell:
sudo ufw allow 8080/TCP

But it’s probably already in the open state. I don’t think it’s refusing, I bet it’s about blocking. If that’s the case, it’s because of the worker’s idle connection handling: in some cases the connection doesn’t close, and Gunicorn’s request parser receives an “empty” packet and gets stuck.

Or it could be crashing (because when started with uv run, Gunicorn’s worker fork/exec model and uv’s virtualenv isolation may conflict). Or closing (earlier you used Alpine, now slim-bookworm. These two differ in libc, network stack, and socket timeout behavior).

If this is a socket-related issue, you can test Docker network bridge behavior: in slim-bookworm + uv environments, test how sockets close / handle keep-alive (for example with netstat -anp | grep 8080).

Gunicorn timeout / keepalive settings to try:

--timeout 120 → longer timeout, see if behavior changes.

--keep-alive 2 → speeds up idle connection closure.

Alpine vs Slim difference: Alpine uses musl libc, Slim uses glibc. TCP socket closure behavior can differ slightly.

Error log (and why it matches my diagnosis)
[2025-08-17 18:47:55 +0000] [10] [INFO] Starting gunicorn 23.0.0
[2025-08-17 18:47:55 +0000] [10] [INFO] Listening at: http://0.0.0.0:8080 (10)
[2025-08-17 18:47:55 +0000] [10] [INFO] Using worker: sync
[2025-08-17 18:47:55 +0000] [11] [INFO] Booting worker with pid: 11
[2025-08-17 18:48:40 +0000] [10] [CRITICAL] WORKER TIMEOUT (pid:11)
[2025-08-17 18:48:40 +0000] [11] [ERROR] Error handling request (no URI read)
Traceback (most recent call last):
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/workers/sync.py", line 133, in handle
    req = next(parser)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/parser.py", line 41, in __next__
    self.mesg = self.mesg_class(self.cfg, self.unreader, self.source_addr, self.req_count)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 259, in __init__
    super().__init__(cfg, unreader, peer_addr)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 60, in __init__
    unused = self.parse(self.unreader)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 271, in parse
    self.get_data(unreader, buf, stop=True)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/message.py", line 262, in get_data
    data = unreader.read()
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/unreader.py", line 36, in read
    d = self.chunk()
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/http/unreader.py", line 63, in chunk
    return self.sock.recv(self.mxchunk)
  File "/app/.venv/lib/python3.13/site-packages/gunicorn/workers/base.py", line 204, in handle_abort
    sys.exit(1)
SystemExit: 1
[2025-08-17 18:48:40 +0000] [11] [INFO] Worker exiting (pid: 11)
[2025-08-17 18:48:40 +0000] [12] [INFO] Booting worker with pid: 12

This log shows exactly what I described: the worker accepts a TCP connection, then blocks on recv() waiting for data that never arrives. Eventually Gunicorn’s watchdog triggers a timeout and kills the worker. The error message “no URI read” literally means the HTTP request line never came in, which is consistent with my hypothesis.

---

_Label `bug` removed by @konstin on 2025-08-19 11:05_

---

_Label `question` added by @konstin on 2025-08-19 11:05_

---

_Comment by @konstin on 2025-08-19 11:12_

Do the errors also occur when just replacing `pip install` with `uv pip install`?

```
RUN pip install --no-cache-dir -r requirements.txt
```

to

```
RUN uv pip install --no-cache-dir -r requirements.txt
```

When you do `uv sync`, what Python interpreter is used (`-v` will give more information)?

The best way to debug this is changing the Dockerfile step-by-step, checking which change exactly makes the problem occur and looking at the logs during the docker build. 

---

_Comment by @SoAp9035 on 2025-08-19 13:14_

@abkarada thanks so much for the detailed suggestions.

I tried again on both alpine and slim-bookworm, the problem is still there. I noticed that this problem occurs even without uv.

But when i added your gunicorn commands (--timeout 120 and --keep-alive 2), after a long wait on refresh the page actually loads with no error. However, the random slow refresh is still there.

After all this testing, im pretty sure this is docker and gunicorn issue and not related to uv at all. So im going to close this issue. Thanks again to everyone for the help

---

_Closed by @SoAp9035 on 2025-08-19 13:14_

---

_Comment by @rubms on 2025-10-23 14:38_

I've had the same issues. It works locally but in ECS it times out. My recommendation: don't use `uv` inside the container, and use the system Python environment instead. I tried with `uv pip install --system` but it still failed. The only way I made it work:

```dockerfile
RUN uv export --frozen --no-hashes -o requirements.txt
RUN pip install -r requirements.txt
```

This way I have the advantages of uv locally, but in CI/CD I leverage the container Python instance and prevent the issue.

`uv`'s documentation itself touches on this in https://docs.astral.sh/uv/pip/environments/#deactivating-an-environment:
> Although we generally recommend using virtual environments for dependency management, --system is appropriate in continuous integration and containerized environments.


---
