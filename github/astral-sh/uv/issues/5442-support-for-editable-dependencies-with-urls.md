---
number: 5442
title: Support for editable dependencies with URLs
type: issue
state: closed
author: krmax44
labels:
  - question
assignees: []
created_at: 2024-07-25T14:01:08Z
updated_at: 2025-02-19T00:02:14Z
url: https://github.com/astral-sh/uv/issues/5442
synced_at: 2026-01-07T13:12:17-06:00
---

# Support for editable dependencies with URLs

---

_Issue opened by @krmax44 on 2024-07-25 14:01_

While uv supports dependencies from URLs (like `uv pip install git+https://github.com/pallets/flask.git`), they can't be installed with the `-e` flag:

```
$ uv pip install -e git+https://github.com/pallets/flask.git
error: Editable must refer to a local directory, not a Git URL
```

For multi-repo projects, this would be nice to have.

---

_Comment by @zanieb on 2024-07-25 14:06_

Hi! What does it mean to have an editable installation of a remote git dependency?

---

_Comment by @krmax44 on 2024-07-25 14:14_

Thanks for your quick reply! From my understanding, there is little difference than with the `-e` flag omitted. With the `-e` flag however, an existing repository is updated upon re-running the install command (see below), without a new clone is made.

```
Updating ./.venv/src/froide-fax clone (to revision main)
  Running command git fetch -q --tags
  Running command git reset --hard -q 09a7bcec76d3da6b2003516a775ed5657a7cc062
```

---

_Comment by @charliermarsh on 2024-07-25 14:18_

As of now we intentionally don't support this. It adds a lot of complexity and we actually don't re-clone at all upon re-running the install command (we have a cache that contains a copy of each repository, and we checkout-and-install as needed with incremental fetches).

---

_Comment by @zanieb on 2024-07-25 14:19_

I think we always cache git clones anyway, though I'm not sure about the nuances of our behavior here. Are you having problems with our behavior specifically?

---

_Comment by @krmax44 on 2024-07-25 14:42_

Just dropping the `-e` works fine as of now :) 

---

_Label `question` added by @zanieb on 2024-07-25 15:06_

---

_Comment by @zanieb on 2024-07-25 15:49_

Thanks! Let us know if you have more questions / a use-case that's not addressed.

---

_Closed by @zanieb on 2024-07-25 15:49_

---

_Comment by @krmax44 on 2024-07-25 17:01_

I found another difference: With `-e`, all files from the repository are available within `.venv/src`. WIthout, only relevant files (presumably as configured with setuptools) will be stored in `site-packages`. This is quite important to us, as we also include JavaScript files in the repo we need to access. (We can probably include these somehow with setuptools.)

---

_Referenced in [avilaton/dependabot-core#2](../../avilaton/dependabot-core/pulls/2.md) on 2024-08-18 21:25_

---

_Referenced in [dependabot/dependabot-core#10040](../../dependabot/dependabot-core/pulls/10040.md) on 2024-08-19 06:51_

---

_Referenced in [overhangio/tutor#1088](../../overhangio/tutor/pulls/1088.md) on 2024-10-16 20:02_

---

_Referenced in [astral-sh/uv#10992](../../astral-sh/uv/issues/10992.md) on 2025-01-27 16:37_

---

_Comment by @edmorley on 2025-01-27 16:51_

Adding some keywords since I couldn't find this issue initially:
`uv add --editable`, Git, VCS

 ```
error: Failed to parse: `pyproject.toml`
  Caused by: TOML parse error at line 12, column 12
   |
12 | gunicorn = { git = "https://github.com/benoitc/gunicorn", editable = true }
   |            ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^
cannot specify both `git` and `editable`
```

---

_Comment by @charliermarsh on 2025-01-27 16:59_

Thanks, good form!

---

_Comment by @ThrawnCA on 2025-02-19 00:00_

This can create problems if a package doesn't specify the metadata around its dependencies very well and they need manual handling.

For example, https://github.com/ckan/ckanext-dcat has a `requirements.txt` file to install what it needs, but that file won't be used automatically, and if you use a non-editable install, you won't get a copy of the file to install everything manually. And `uv` cannot perform an editable install from that URL.

With `pip` it's relatively straightforward:

    pip install -e git+https://github.com/ckan/ckanext-dcat.git@v2.2.0#egg=ckanext-dcat
    cd $SRC_DIR/ckanext-dcat
    pip install -r requirements.txt

---
