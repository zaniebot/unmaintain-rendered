---
number: 7080
title: "Unable to create an editable .pth without 'uv pip install -e .'"
type: issue
state: closed
author: jklaiho
labels:
  - question
assignees: []
created_at: 2024-09-05T12:21:57Z
updated_at: 2024-09-10T12:24:50Z
url: https://github.com/astral-sh/uv/issues/7080
synced_at: 2026-01-10T01:24:10Z
---

# Unable to create an editable .pth without 'uv pip install -e .'

---

_Issue opened by @jklaiho on 2024-09-05 12:21_

I've got a Django project in development inside a Docker container, structured like this:

```
/code
  |_apps/
  |_project/
    |_sitename/
    |_manage.py
  |_(more dirs not relevant here)/
  |_pyproject.toml
  |_uv.lock
  |_(more files not relevant here)
```

I've used this structure for over a decade, starting with virtualenvs in vagrant boxes, and it's survived to this day. Of course, `pyproject.toml` is a more recent addition to the mix. I have not created that file or this structure with `uv init` originally, both were there before I started experimenting with uv. I also use the brand new `UV_PROJECT_ENVIRONMENT=/opt/venv` variable in the container, but that probably isn't relevant here.

`/code` is bind mounted from the host machine via Compose. `project` is the result of `django-admin startproject`. `apps` contains the Django apps (i.e. subdirs created with `/code/project/manage.py startapp`).

In earlier projects, I've used pip-tools, creating a requirement file with `pip-compile` that would get `pip-sync`ed. Additionally, on container startups, using `command:` in `docker-compose.yml`,  I'd run `pip install -e .` inside the `/code` directory to create the `.pth` file containing the string `/code/apps`, so our later `&& /code/project/manage.py runserver` invocation in the same `command` block works correctly and finds the Django apps.

What makes `pip install -e .` possible is this clause in `pyproject.toml`:

```
[tool.setuptools.packages.find]
where = ["apps"]
```

So, I'd essentially use the src layout, but with a folder called `apps` instead of `src`. Note that `pyproject.toml` does not contain a build system definition.

This still works the same as before if I run `uv pip install -e .` inside the `/code` dir. The `.pth` file is created. However, with `uv add --editable .`, it is not created. That command causes these to be added to `pyproject.toml`, though:

```
[project]
name=ProjectName
# ... other stuff

dependencies = [
    "Django==5.1.0",  # pre-existing
    "projectname"  # added by uv
]

# ... other stuff here, then this added by uv:

[tool.uv.sources]
projectname = { workspace = true }
```

I have no idea where to go from here to get the .pth file created. `uv sync` doesn't do it. This is almost certainly just my ignorance about how uv works, but I haven't found an answer in the docs, or at least haven't understood what I've read properly.

This isn't as much a bug report as a request for clarification, possibly at the documentation level. It feels weird to use the uv pip interface for creating the .pth file when everything else I can do without it.

---

_Comment by @davidharting on 2024-09-06 19:58_

I having this issue as well. `uv add --editable .` seems to try to get me into a workspace-oriented setup. But workspaces don't seem appropriate for me, because I truly only want one pyproject.toml.

I am also noticing that maybe there was a behavior change between 0.3.3 and 0.4.6.

I have a script that I run with:

```
uv run scripts/my_script.py
```

my_script.py imports from a local module:

```python
# scripts/my_script.py
from my_module.application import app

if __name__ == '__main__':
  print(app)
```

Under `uv 0.3.3` this worked. I didn't have to install my package as editable or use workspaces.

Under `uv 0.4.6` this no longer works: `ModuleImportError: Module my_module does not exit`

I can workaround this by running:

```
pip install -e .
```

After this: `uv run scripts/my_script.py` works. However, the next `uv sync` removes the local editable install. 

---

_Comment by @davidharting on 2024-09-06 20:06_

**Update:**

It looks like this diff from my uv.lock from upgrading from uv 0.3.3 to uv 0.4.6 might be related to my issue above.

```diff
[[package]]
name = "my-package"
version = "0.0.1"
- source = { editable = "." }
+ source = { virtual = "." }
```



---
I am happy to split off my discussion into a different issue. I'm not sure if @jklaiho and I are describing symptoms of the same problem or no

---

_Comment by @davidharting on 2024-09-06 20:21_

**Update 2**

I just noticed this from the release notes for uv 0.4.0:

> uv no longer attempts to package and install projects that do not define a [build-system].
While the project itself will not be installed into the virtual environment, its dependencies will still be included.
The previous behavior can be recovered by setting package = true in the [tool.uv] section of your pyproject.toml.

Doing as the docs suggest resolved my issue. I can now use uv 0.4.6 and `uv run scripts/my_script.py` successfully!


@jklaiho I wonder if you can do the same and add:

```
[uv.tool]
package = true
```

to your pyproject.toml. 

---

_Comment by @zanieb on 2024-09-06 20:23_

Yes I think you're looking for `package = true`. Documentation on this in: https://docs.astral.sh/uv/concepts/projects/#build-systems

---

_Label `question` added by @zanieb on 2024-09-06 20:24_

---

_Comment by @zanieb on 2024-09-07 15:15_

Let us know if you have more questions!

---

_Closed by @zanieb on 2024-09-07 15:15_

---

_Comment by @jklaiho on 2024-09-10 12:24_

Can confirm that this worked as intended for my use case as well. :+1:

---
