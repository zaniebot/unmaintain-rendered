```yaml
number: 7419
title: Provide uv zipapp
type: issue
state: open
author: ksamuel
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-09-16T09:51:40Z
updated_at: 2024-12-03T15:30:48Z
url: https://github.com/astral-sh/uv/issues/7419
synced_at: 2026-01-12T15:59:13Z
```

# Provide uv zipapp

---

_@ksamuel_

Now that uv can run zipapps, there is the potential for a good story to build python projects before pushing them in production instead of doing the build on the server.

However, building a zipapp is currently too complicated for the average user:

- The format assumes the code base is zipsafe, but most projects aren't. Web frameworks like django, flasks or fastapi actually assume they have access to the file system directly.
- The stdlib zipapp packager (https://docs.python.org/3/library/zipapp.html) doesn't have any way to package dependencies.
- Even when using something like shiv (https://shiv.readthedocs.io/) to build zipapp with dependencies, creating one on Windows can lead to a Windows specific zipapp if c-extensions are installed, and the deployment on a Linux server will fail.
- It requires a nontrivial understanding of entry points to make that work. Between `__main__.py`, WSGI `app` var and `if __name__ == "__main__"`, most beginners have little chance to make the correct choice.
- Building a viable zipapp implies copying more than Python code. You may also want static files, and sometimes scripts that are not importable by default yet parts of the entry point (e.g: CSS files and `manage.py` with django).

There is an opportunity to make uv the first tool to allow fast and easy Python deployments by:

- Providing an "uv zipapp" subcommand that will turn your script or project into a zipapp.
- Adopting the shiv hack that unzip the project on the first run to allow transparent filesystem access.
- Giving hooks to build a zipapp for other OSes than the one you build it on, thanks to packse. 
- Making the common entry points trivial to declare. From a simple script with inline deps to complex web app.
- Allowing straightforward declaration of additional files to be added into the zip.
- Adding checks such as platform compat and Python version into the zipapp so that if it fails to run, the end-user has a nice error message to figure it out.
- Ensuring configurability from `pyproject.toml`, so you don't have to figure out the command line options every time for big projects.
- Letting you optionally clean up the older deployment artifacts.

Finding a clear solution for the that entry point is the hard part. It needs to be usable as an import (to use in a shell), with variables from the entry points (like access to the app var for wsgi) and as an executable script (for the cli use case), yet proxy all that to the unzipped cache transparently. 

uv already has all the primitives to do this: compression, deps download, and platform mapping. Plus, it can do it fast and correctly. It would be the first tool to actually make zipapps practical to use.

What's more, it would be a stepping stone for creating full standalone executable later on, with uv bundled into the zip to bootstrap python itself.


---

_Comment by @mattmess1221 on 2024-10-07 23:21_

For reference, [`zipapps`](https://github.com/ClericPy/zipapps) is able to bundle dependencies into a `.pyz` as well, including entire virtualenvs. It can also lazily install dependencies on first run (useful for programs with non-python dependencies)

---

_Label `enhancement` added by @zanieb on 2024-10-08 01:45_

---

_Label `wish` added by @zanieb on 2024-10-08 01:45_

---

_Comment by @zanieb on 2024-10-08 01:47_

Thanks for the write-up! We're focused on core functionality outside of bundling right now, but it's definitely something we're interested in doing someday. I'm not sure zipapp is the ideal format for us, but I don't know much about it yet.

Related

- https://github.com/astral-sh/uv/issues/7865
- https://github.com/astral-sh/uv/issues/5802

---

_Comment by @ksamuel on 2024-10-08 10:03_

Cheers.

I'd like to add that zipapps are different to the bundles defined in those 2 tickets since they explicitly don't bundle a Python environment.

This means:

- You can ship a zipapp to saas that don't allow you to spawn your own process like heroku, or only allow a WSGI entry point (like the popular pythonanywhere.com). 
- A zipapp with no c-extension is platform agnostic.
- The resulting zip is smaller.
- You can deploy a zipapp with secured systems that can't allow non white-listed executable to run, or use custom python builds.
- Zipapps work on alternative distributions like anaconda.

Of course, having a self-contained executable is a killer feature, but:

- I see them as different solutions to different problems.
- A contained executable is a much harder problem to tackle. And an even harder problem to tackle correctly. 
- There are actually better solutions currently to build stand alone executables than zipapps in the python ecosystem, surprisingly. 

---

_Comment by @clbarnes on 2024-12-03 15:22_

For my use case, an MVP would be simply an equivalent of `uv run` which, instead of building an environment and running the given command/ script, would create a zipapp including the project/script and all its runtime dependencies. Working with inline script metadata is most of what I want out of this tbh.

Bundling an interpreter in any kind of compiled package would be great, but IMO is outside of the scope of zipapps, as is any behaviour which would mean that the zipapp could only be run by uv.

---
