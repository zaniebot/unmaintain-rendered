---
number: 8381
title: panic when sync with a related local editable package 
type: issue
state: closed
author: idertator
labels:
  - bug
assignees: []
created_at: 2024-10-20T12:29:02Z
updated_at: 2024-10-20T20:09:20Z
url: https://github.com/astral-sh/uv/issues/8381
synced_at: 2026-01-10T01:24:27Z
---

# panic when sync with a related local editable package 

---

_Issue opened by @idertator on 2024-10-20 12:29_

To give some context i have two projects (which are not in a workspace) with the following layout:

 📁 backend/
            pyproject.toml
            ...
 📁 django-t3/
            pyproject.toml      
            ...

In `backend/pyproject.toml` i have:
```toml
[project]
name = "t3backend"
version = "4.0.0"
requires-python = ">=3.13"
description = "T3 Backend"
readme = "README.md"

dependencies = [
    # Django Dependencies
    "Django>=5.1",
    "django-cors-headers>=4.5",
    "django-extensions>=3.2",
    "django-modeltranslation>=0.19",
    "django-ninja>=1.3",
    "django-ulid>=0.0",
    # External API
    "djangorestframework>=3.15",
    "django-filter>=24.3",
    "drf-yasg>=1.21",
    "drf-standardized-errors>=0.14",
    # External Services Integrations
    "mailjet_rest>=1.3",
    "mandrill>=1.0",
    "mailchimp-transactional>=1.0",
    "sendgrid>=6.11",
    # Data Integrations
    "elasticsearch>=8.15",
    "hiredis>=3.0",
    "minio>=7.2",
    "psycopg>=3.2",
    "psycopg-binary>=3.2",
    "psycopg-pool>=3.2",
    "redis>=5.1",
    # Email Handling
    "imap_tools>=1.7",
    "minify-html>=0.15",
    # Datas-Science Research
    "matplotlib>=3.9",
    "numpy>=2.1",
    # Tools
    "ipython>=8.28",
    # General Utilities
    "Authlib>=1.3",
    "Jinja2>=3.1",
    "Pillow>=11.0",
    "Unidecode>=1.3",
    "cryptography>=43.0",
    "deepdiff>=8.0",
    "email-validator>=2.2",
    "geopy>=2.4",
    # TODO: Check if possible to be replaced by django
    "lz4>=4.3",
    "phonenumbers>=8.13",
    "pyexcel>=0.7",
    "pyexcel-xlsx>=0.6",
    "python-stdnum>=1.20",
    "reportlab>=4.2",
    "requests>=2.32",
    "schwifty>=2024.9",
    "shortuuid>=1.0",
    "tablib>=3.7",
    "tablib[xlsx]>=3.7",
    "tqdm>=4.66",
    "user-agents>=2.2",
    "xxhash>=3.5",
    # Sentry
    "sentry-sdk[django]>=2.17",
    # Production
    "gunicorn>=23.0",
    "django-t3",
]

[tool.uv]
environments = [
    "sys_platform == 'darwin'",
    "sys_platform == 'linux'",
]
dev-dependencies = [
    # CI
    "pre-commit>=4.0",

    # Development Tools
    "isort>=5.13", # TODO: Replace with ruff later
    "pynvim>=0.5",

    # Tools
    "django-zeal>=1.4",

    # Testing Tools
    "factory_boy>=3.3",
    "polyfactory>=2.17",
]

[tool.uv.sources]
django-t3 = [
    { path = "../django-t3", editable = true, marker = "sys_platform == 'darwin'" },
    { git = "https://***:***@git.topgroups.travel/t3/django-t3.git", rev = "v1.6.15", marker = "sys_platform == 'linux'"},
]
```

In `django-t3/pyproject.toml` i have:
```toml
[project]
name = "django-t3"
version = "1.6.15"
description = "Django T3"
requires-python = ">=3.13"
readme = "README.md"

dependencies = [
    "django",
    "django-ninja",
    "pydantic",
    "meilisearch",
    "phonenumbers",
    "termcolor",
    "reportlab",
    "deepdiff"
]

[tool.uv]
environments = [
    "sys_platform == 'darwin'",
    "sys_platform == 'linux'",
]
```

When i run the `RUST_BACKTRACE=full uv sync` command inside the backend folder i get the following error:
```
Resolved 131 packages in 66ms
thread 'main' panicked at crates/uv-resolver/src/resolution/graph.rs:742:34:
no entry found for key
stack backtrace:
   0:        0x1051e7a50 - __mh_execute_header
   1:        0x104ef1690 - __mh_execute_header
   2:        0x1051b431c - __mh_execute_header
   3:        0x1051eaa88 - __mh_execute_header
   4:        0x1051ea4d8 - __mh_execute_header
   5:        0x1051ec1f0 - __mh_execute_header
   6:        0x1051eb480 - __mh_execute_header
   7:        0x1051eb3f4 - __mh_execute_header
   8:        0x1051eb3e8 - __mh_execute_header
   9:        0x105f99e40 - __mh_execute_header
  10:        0x105f9a280 - __mh_execute_header
  11:        0x105d1624c - __mh_execute_header
  12:        0x104dd8fbc - __mh_execute_header
  13:        0x104dcffac - __mh_execute_header
  14:        0x104d58108 - __mh_execute_header
  15:        0x104d5cb3c - __mh_execute_header
  16:        0x104e32570 - __mh_execute_header
  17:        0x105741788 - __mh_execute_header
  18:        0x1058999cc - __mh_execute_header
  19:        0x1055a8584 - __mh_execute_header
  20:        0x105899518 - __mh_execute_header
```

I have attached also the full output with --verbose flag of the command in [output.txt](https://github.com/user-attachments/files/17450232/output.txt)

The `uv --version` output is:

````
uv 0.4.24 (b9cd54913 2024-10-17)
```

I'm on a M3 MacBookPro and the output of the `uname -a` command is:
```
Darwin MacBook-Pro-de-Roberto.local 24.0.0 Darwin Kernel Version 24.0.0: Tue Sep 24 23:37:25 PDT 2024; root:xnu-11215.1.12~1/RELEASE_ARM64_T6030 arm64
```

With these layout i want to have a local editable version of my django-t3 package and use the one in the git repository in production. I don't know if a doing something wrong, can you guys help me with this?


---

_Label `bug` added by @charliermarsh on 2024-10-20 16:25_

---

_Comment by @charliermarsh on 2024-10-20 16:43_

Thanks! Definitely a bug.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-20 16:44_

---

_Comment by @charliermarsh on 2024-10-20 17:18_

I think you can reproduce this as easily as:
```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.9"
dependencies = [
   "anyio @ git+https://github.com/agronholm/anyio@4.6.2 ; sys_platform == 'linux'",
   "anyio ; sys_platform == 'darwin'"
]
```

---

_Referenced in [astral-sh/uv#8388](../../astral-sh/uv/pulls/8388.md) on 2024-10-20 17:44_

---

_Closed by @charliermarsh on 2024-10-20 18:42_

---

_Closed by @charliermarsh on 2024-10-20 18:42_

---

_Comment by @idertator on 2024-10-20 20:09_

Thanks a lot for the quick response @charliermarsh. You are doing an amazing job!!!!!

---
