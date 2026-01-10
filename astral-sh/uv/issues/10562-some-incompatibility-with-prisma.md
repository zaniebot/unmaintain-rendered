---
number: 10562
title: Some incompatibility with Prisma
type: issue
state: closed
author: Somebi
labels:
  - needs-mre
assignees: []
created_at: 2025-01-13T13:12:14Z
updated_at: 2025-01-25T02:51:13Z
url: https://github.com/astral-sh/uv/issues/10562
synced_at: 2026-01-10T01:24:55Z
---

# Some incompatibility with Prisma

---

_Issue opened by @Somebi on 2025-01-13 13:12_

```
root@tester:~/uploaded_folder/python# ./generate_prisma_cli.sh 
Environment variables loaded from .env
Prisma schema loaded from prisma/schema.prisma

✔ Generated Prisma Client Python (v0.15.0) to ./../../../usr/local/lib/python3.10/dist-packages/prisma in 756ms

root@tester:~/uploaded_folder/python# ./generate_prisma_cli.sh ls -lah ^C
root@tester:~/uploaded_folder/python# ls -lah ./../../../usr/local/lib/python3.10/dist-packages/prisma
total 956K
drwxr-xr-x   8 root root 4.0K Oct  3 12:18 .
drwxr-xr-x 153 root root  12K Jan 12 10:57 ..
-rw-r--r--   1 root root 2.2K Oct  3 12:18 __init__.py
-rw-r--r--   1 root root   86 Oct  3 12:18 __main__.py
drwxr-xr-x   2 root root 4.0K Jan 13 13:08 __pycache__
-rw-r--r--   1 root root 1.9K Oct  3 12:18 _async_http.py
-rw-r--r--   1 root root  18K Oct  3 12:18 _base_client.py
-rw-r--r--   1 root root  25K Oct  3 12:18 _builder.py
-rw-r--r--   1 root root 9.5K Oct  3 12:18 _compat.py
-rw-r--r--   1 root root 4.1K Oct  3 12:18 _config.py
-rw-r--r--   1 root root  691 Oct  3 12:18 _constants.py
-rw-r--r--   1 root root 5.8K Oct  3 12:18 _fields.py
-rw-r--r--   1 root root  978 Oct  3 12:18 _metrics.py
-rw-r--r--   1 root root 1.3K Oct  3 12:18 _proxy.py
-rw-r--r--   1 root root 4.9K Oct  3 12:18 _raw_query.py
-rw-r--r--   1 root root 1.7K Oct  3 12:18 _registry.py
-rw-r--r--   1 root root 1.6K Oct  3 12:18 _sync_http.py
-rw-r--r--   1 root root 7.7K Oct  3 12:18 _transactions.py
-rw-r--r--   1 root root 2.1K Oct  3 12:18 _types.py
-rw-r--r--   1 root root  200 Oct  3 12:18 _typing.py
drwxr-xr-x   3 root root 4.0K Oct  3 12:18 _vendor
-rw-r--r--   1 root root 228K Jan 13 13:08 actions.py
-rw-r--r--   1 root root 4.7K Jan 13 13:08 bases.py
drwxr-xr-x   3 root root 4.0K Oct  3 12:18 binaries
drwxr-xr-x   4 root root 4.0K Oct  3 12:18 cli
-rw-r--r--   1 root root  34K Jan 13 13:08 client.py
drwxr-xr-x   3 root root 4.0K Oct  3 12:18 engine
-rw-r--r--   1 root root  926 Jan 13 13:08 enums.py
-rw-r--r--   1 root root 5.1K Oct  3 12:18 errors.py
-rw-r--r--   1 root root   23 Oct  3 12:18 fields.py
drwxr-xr-x   5 root root 4.0K Oct  3 12:18 generator
-rw-r--r--   1 root root  798 Jan 13 13:08 http.py
-rw-r--r--   1 root root 3.0K Oct  3 12:18 http_abstract.py
-rw-r--r--   1 root root 1.3K Jan 13 13:08 metadata.py
-rw-r--r--   1 root root  60K Jan 13 13:08 models.py
-rw-r--r--   1 root root  13K Oct  3 12:18 mypy.py
-rw-r--r--   1 root root 1003 Jan 13 13:08 partials.py
-rw-r--r--   1 root root   65 Oct  3 12:18 py.typed
-rw-rw-r--   1 root root 4.1K Jan 13 13:08 schema.prisma
-rw-r--r--   1 root root  908 Oct  3 12:18 testing.py
-rw-r--r--   1 root root 373K Jan 13 13:08 types.py
-rw-r--r--   1 root root 3.7K Oct  3 12:18 utils.py
-rw-r--r--   1 root root 3.2K Oct  3 12:18 validator.py
root@tester:~/uploaded_folder/python# 
root@tester:~/uploaded_folder/python# 
root@tester:~/uploaded_folder/python# /root/.local/bin/uv run python3 -m module.start
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/root/uploaded_folder/python/module/start.py", line 6, in <module>
    from module.db import FeedDB
  File "/root/uploaded_folder/python/module/db.py", line 3, in <module>
    from prisma import Prisma
  File "<frozen importlib._bootstrap>", line 1412, in _handle_fromlist
  File "/root/uploaded_folder/python/.venv/lib/python3.12/site-packages/prisma/__init__.py", line 53, in __getattr__
    raise RuntimeError(
RuntimeError: The Client hasn't been generated yet, you must run `prisma generate` before you can use the client.
See https://prisma-client-py.readthedocs.io/en/stable/reference/troubleshooting/#client-has-not-been-generated-yet
root@tester:~/uploaded_folder/python# 
```

---

_Comment by @Somebi on 2025-01-13 13:12_

It shows that is has generated client, but when running script, it doesn't find it...

---

_Comment by @charliermarsh on 2025-01-13 13:50_

Can you please create a Git repo or Dockerfile that I can use to reproduce this from scratch? My guess is this is going to be something extremely specific to Prisma and how they find their client.

---

_Label `needs-mre` added by @charliermarsh on 2025-01-13 13:50_

---

_Comment by @Somebi on 2025-01-13 14:39_

It's some conflict with pip i guess. I had to uninstall prisma from pip and only then it started working. I guess that you are not overriding some pip or python related envs to point to current uv venv.

---

_Comment by @Somebi on 2025-01-13 14:39_

Now it is properly loading it from .venv folder:
`prisma located at: /root/uploaded_folder/python/.venv/lib/python3.12/site-packages/prisma`

---

_Comment by @Somebi on 2025-01-13 14:40_

```
package_path = os.path.abspath(prisma.__file__)
package_dir = os.path.dirname(package_path)
print(f"prisma located at: {package_dir}")
```

---

_Comment by @Somebi on 2025-01-13 14:42_

Well, as no changes were made to prisma package itself. It only proves that it's your conflict with pip and how paths or env are handled or overwritten. Maybe some symlinks are missing.
Also i'm using different python versions.

---

_Comment by @charliermarsh on 2025-01-13 15:34_

The logging suggests that you created the Prisma client in a global environment (`✔ Generated Prisma Client Python (v0.15.0) to ./../../../usr/local/lib/python3.10/dist-packages/prisma in 756ms`) but then tried to run it in a virtual environment.

---

_Comment by @Somebi on 2025-01-13 15:39_

The thing is, that i have install prisma via uv, like `uv add prisma` then tried to generate prisma client and it failed, because it was still pointing & searching in the old pip location, instead of looking in the currently active uv venv. So i guess something was still pointing to the old env, which prisma was using. After specifically uninstalling it via pip, it was able to read from uv venv folder...

---

_Comment by @charliermarsh on 2025-01-25 02:51_

Closing as I don't see a clear actionable task here.

---

_Closed by @charliermarsh on 2025-01-25 02:51_

---
