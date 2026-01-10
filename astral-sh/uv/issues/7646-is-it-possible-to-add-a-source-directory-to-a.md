---
number: 7646
title: is it possible to add a source directory to a project?
type: issue
state: closed
author: ophiry
labels:
  - question
assignees: []
created_at: 2024-09-23T17:09:24Z
updated_at: 2025-04-12T14:21:50Z
url: https://github.com/astral-sh/uv/issues/7646
synced_at: 2026-01-10T01:24:17Z
---

# is it possible to add a source directory to a project?

---

_Issue opened by @ophiry on 2024-09-23 17:09_

I want to add either the projects source directory - expecting it to be added to sys.path of the venv

similar to the effect of pip install -e .

my assumption was that uv add -editable . would do this, but it doesn't seem to work




---

_Comment by @zanieb on 2024-09-23 19:47_

It sounds like you want to enable a [build system](https://docs.astral.sh/uv/concepts/projects/#build-systems)

---

_Label `question` added by @zanieb on 2024-09-23 19:47_

---

_Comment by @ophiry on 2024-09-24 12:56_

for future use, adding the lines:

```
[tool.uv]
package = true

```


---

_Comment by @zanieb on 2024-09-24 13:02_

Yep!

---

_Closed by @zanieb on 2024-09-24 13:02_

---

_Comment by @ophiry on 2024-09-24 14:44_

follow up:
is it possible to add an external directory (in this case - the parent directory)?

the use case is a monorepo with several different apps
they are basically standalone applications, but the internal imports rely on fully qualified names based on the repo root directory

for example:
```
a/main.py
a/lib/somelib.py
b/main.py
b/lib/somelib.py

```
where a/main.py has:
`import a.lib.somelib`
a and b have different dependencies, so relying on a single pyproject.toml file will be problematic

---

_Comment by @NicholasPini on 2024-11-27 12:49_

> follow up: is it possible to add an external directory (in this case - the parent directory)?
> 
> the use case is a monorepo with several different apps they are basically standalone applications, but the internal imports rely on fully qualified names based on the repo root directory
> 
> for example:
> 
> ```
> a/main.py
> a/lib/somelib.py
> b/main.py
> b/lib/somelib.py
> ```
> 
> where a/main.py has: `import a.lib.somelib` a and b have different dependencies, so relying on a single pyproject.toml file will be problematic

I would also like to know if something like this is possible. I can add an external package using `uv add`, but it's a bit annoying that despite appearing in the toml file, this local package is not in `syspath`.

---

_Comment by @flipbit03 on 2025-03-30 12:35_

@NicholasPini 

I believe you might be able to get by with something like this
```toml
[tool.setuptools]
py-modules = ["<main_package>", "<your>", "<other>", "<packages>"]
```

---

_Comment by @ezyang on 2025-04-12 14:21_

Sorry, adding a comment here because this is the top result for "uv add directory use source directly". In my case, I wanted to add a local directory to a uv project as a dependency using its source code directly,  for a case where I needed to temporarily vendor a dependency. `uv add --editable --reinstall-package PATH/TO/PACKAGE` is probably what you want here (don't forget the editable!)

---
