```yaml
number: 9517
title: "Enable package overrides for 'tool install'"
type: issue
state: closed
author: restlessronin
labels:
  - cli
assignees: []
created_at: 2024-11-29T09:17:50Z
updated_at: 2024-12-03T01:14:42Z
url: https://github.com/astral-sh/uv/issues/9517
synced_at: 2026-01-10T04:36:21Z
```

# Enable package overrides for 'tool install'

---

_Issue opened by @restlessronin on 2024-11-29 09:17_

I have a CLI app that is currently being built by poetry (https://pypi.org/project/llm-context/). 'pipx' is used to install it. There are no dependency issues at either build or resolution times. And the package runs fine.

I want to be able to do a 'uv tool install' for this package.

There are no problems when I build using 'uv'. When I try to install, I get

```
 uv tool install "llm-context @ ."
  × No solution found when resolving dependencies:
  ╰─▶ Because tree-sitter-languages==1.10.2 has no wheels with a matching Python ABI tag
      and llm-context==0.1.1 depends on tree-sitter-languages==1.10.2, we can conclude that
      llm-context==0.1.1 cannot be used.
      And because only llm-context==0.1.1 is available and you require llm-context, we can
      conclude that your requirements are unsatisfiable.
```
Since I know it is currently running without apparent problems with the pipx install, I would like to force the override. Ideally I would do this at build time, rather than at install time.

I found this https://github.com/astral-sh/uv/pull/631 which seems to do what I want (although it's at install time), but this is for pip-install. Apparently the equivalent is not set up for tool install?

I'm wondering if there are plans to enable this for tools as well. As I said it's ideally something that goes into the metadata at build time, so that uv knows to override the dependency at install time.

Great project! Thanks for all the work.

---

_Comment by @charliermarsh on 2024-11-29 16:56_

Hmm yeah, we probably need to accept `--with-overrides` or `--with-constraints` or something like that here. \cc @zanieb 

---

_Label `cli` added by @charliermarsh on 2024-11-29 16:56_

---

_Comment by @restlessronin on 2024-11-29 17:07_

Great.

'uv pip install" appears to work with project metadata, i.e. if I build with

```
[tool.uv]
override-dependencies = [
  "tree-sitter-languages==1.10.2"
]
```

in the 'pyproject.toml' file, I don't seem to need to specify overrides at install time. That would be the ideal solution.


---

_Comment by @charliermarsh on 2024-11-29 17:09_

`uv tool` intentionally doesn't read project configuration, though, since it's a "global" command.

---

_Comment by @restlessronin on 2024-11-29 17:17_

ah. i thought the project metadata (including the override-dependencies) might be getting baked into the package at build time, and available to 'uv' at install time. i guess that's not what's happening here? I'm not very familiar with python packaging so I'm ignorant of the details.

That would seem to indicate that there would be no way to do `uvx <package-name / script-name>` on a new machine and have it just work (since my package has this resolution issue). That's a shame. Seeing that work at speed with a python package was quite magical.

---

_Comment by @charliermarsh on 2024-11-29 17:31_

Ah yeah, that metadata isn't available at `uv tool install` time if the package is coming from PyPI. It's not included in the distribution, since that piece is all standardized.

---

_Comment by @charliermarsh on 2024-11-29 17:32_

I'm confused though, how / why does your package work if it has this problem? Why does `pipx` let you install it...?

---

_Comment by @restlessronin on 2024-11-29 17:36_

I'm afraid I don't know enough about python installation to answer that question. I don't get any warnings from pipx install, and the package functionality works fine.

---

_Comment by @charliermarsh on 2024-11-29 17:40_

My guess is that you're using different Python versions -- `tree-sitter-languages` doesn't have wheels for Python 3.13, but I'm guessing that's what you're using by default, whereas pipx is using some other, older Python version. What does ` uv tool install --python 3.12 "llm-context @ ."` give you?

---

_Comment by @restlessronin on 2024-11-30 05:35_

Yup. That worked. Thanks! 🙏🏾

I had already set ".python-version" to 3.12 to see if that was the issue, but I guess tool install doesn't look at the project settings.

Now if there were a way to run 'uvx' directly with the CLI scripts within the package, that would be my ideal scenario. But this is already really good.

Once again, thanks. Keep up the amazing work!

UPDATE:

I tried
```
requires-python = ">=3.10,<3.13"
```

To see if 'uv tool install' would solve correctly with the modified python version constraint, but it didn't work.


---

_Assigned to @charliermarsh by @charliermarsh on 2024-12-01 03:39_

---

_Closed by @charliermarsh on 2024-12-03 01:14_

---
