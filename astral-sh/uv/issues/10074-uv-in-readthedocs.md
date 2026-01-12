```yaml
number: 10074
title: uv in ReadTheDocs
type: issue
state: closed
author: cthoyt
labels:
  - show-and-tell
assignees: []
created_at: 2024-12-21T12:01:08Z
updated_at: 2025-09-02T13:48:44Z
url: https://github.com/astral-sh/uv/issues/10074
synced_at: 2026-01-12T16:00:06Z
```

# uv in ReadTheDocs

---

_@cthoyt_

Note, this issue has now been upstreamed by incorporating the discussion into the ReadTheDocs documentation upstream at https://docs.readthedocs.com/platform/stable/build-customization.html#install-dependencies-with-uv.

Thanks to the comment in https://github.com/astral-sh/uv/issues/10074#issuecomment-3245337840 that made me aware of this.

# Original Issue Text


This is more of a "show and tell" than an issue, but it could be something worth incorporating into the uv documentation.

## Using uv as the installer instead of pip

ReadTheDocs uses pip by default when installing sphinx, your package, and its dependencies. The build can be greatly sped up by switching to uv, but this isn't easy to configure without making a custom override in the commands. Here's an example `readthedocs.yml` that makes it work using `uv pip install` as an alternative, that uses the `asdf` program for configuring uv:

```yaml
# See https://docs.readthedocs.io/en/stable/config-file/v2.html for details
version: 2

build:
  os: ubuntu-22.04
  tools:
    python: "3.12"

  # adapted from uv recipe at https://docs.readthedocs.io/en/stable/build-customization.html#install-dependencies-with-uv
  # and comment at https://github.com/readthedocs/readthedocs.org/issues/11289#issuecomment-2103832834
  commands:
    - asdf plugin add uv
    - asdf install uv latest
    - asdf global uv latest
    - uv venv $READTHEDOCS_VIRTUALENV_PATH
    - VIRTUAL_ENV=$READTHEDOCS_VIRTUALENV_PATH uv --preview pip install .[docs,pandas,flask,fastapi,rdflib]
    - python -m sphinx -T -b html -d docs/_build/doctrees -D language=en docs/source $READTHEDOCS_OUTPUT/html
```

Note: this example is adapted from https://github.com/cthoyt/curies where the `docs` extra installs sphinx, sphinx-click, sphinx-rtd-theme, and other related documentation build dependencies.

Here's an example build log from the website for this project: https://app.readthedocs.org/projects/curies/builds/26642806/

## Getting uv's build backend to work while it's in preview

If you've already switched your package to using uv's preview build backend, then you have to make sure to pass either `--preview` when calling uv, or setting `UV_PREVIEW=1` in the command line. In the above scenario, where you have control over the commands that are being run, then this is pretty straightforward.

Something I am still working on: if you want to keep using the default installer (pip) because you don't want to configure too much here, but have a package that's using the uv build backend. If you try and pip install a package with the uv build backend without setting `UV_PREVIEW=1` in the environment, the build will fail. I wasn't yet able to figure out how to use another part of ReadTheDocs' [build process customization](https://docs.readthedocs.io/en/stable/build-customization.html) to set this environment variable in a way that worked. If anyone has a clean idea on this, then it would be great to comment here!



---

_Label `show-and-tell` added by @zanieb on 2025-01-07 19:10_

---

_Comment by @kmnhan on 2025-01-15 14:53_

For me, the following configuration worked:

```yaml
# See https://docs.readthedocs.io/en/stable/config-file/v2.html for details
version: 2

# Set the OS, Python version and other tools you might need
build:
  os: ubuntu-24.04
  tools:
    python: "3.12"
  jobs:
    post_install:
      - pip install uv
      - UV_PROJECT_ENVIRONMENT=$READTHEDOCS_VIRTUALENV_PATH uv sync --all-extras --group docs --link-mode=copy

# Build documentation in the "docs/" directory with Sphinx
sphinx:
  configuration: docs/source/conf.py
  fail_on_warning: true

# Optionally build your docs in additional formats such as PDF and ePub
formats:
  - pdf
```
I have all my documentation-related dependencies placed in a dependency group named `docs`.

I personally find this approach more comfortable since I can keep using `sphinx` and `formats` sections as I used to before migrating to uv. One caveat is that setuptools and sphinx is installed via pip by ReadTheDocs before uv is installed, so it has an unnecessary overhead of ~15 seconds.



---

_Comment by @coroa on 2025-02-28 21:54_

I combined both approaches to avoid tinkering with the sphinx-build steps including multiple formats, but can still replace the unnecessary sphinx venv setup and install steps (ie. fix the caveat @kmnhan mentioned)

```yaml
version: 2

# Specify os and python version
build:
  os: "ubuntu-24.04"
  tools:
    python: "3.12"
  jobs:
    create_environment:
      - asdf plugin add uv
      - asdf install uv latest
      - asdf global uv latest
      - UV_PROJECT_ENVIRONMENT=$READTHEDOCS_VIRTUALENV_PATH uv sync --all-extras --group docs
    install:
      - "true"

# Build documentation in the docs/ directory with Sphinx
sphinx:
  configuration: docs/conf.py

# Optionally build your docs in additional formats such as PDF and ePub
formats: all
```


---

_Comment by @hagenw on 2025-06-13 14:28_

Another simple solution (assuming your doc dependencies are part of the `dev` dependency group):

```yaml
version: 2

# Specify os and python version
build:
  os: "ubuntu-24.04"
  tools:
    python: "3.12"
  commands:
    - pip install uv
    - uv run python -m sphinx docs $READTHEDOCS_OUTPUT/html -b html -W

# Build documentation in the docs/ directory with Sphinx
sphinx:
  configuration: docs/conf.py
```

---

_Comment by @ego-thales on 2025-07-25 08:10_

@hagenw Is the `sphinx:` really necessary then? Since the build already happens above?
Thanks for sharing!

---

_Comment by @hagenw on 2025-07-25 08:16_

Good question, I guess I just left it there, because I used it before. I would also expect that it should work without the `sphinx:` section.

---

_Comment by @gaborbernat on 2025-07-28 17:17_

This works too:

```yaml
version: 2
build:
  os: ubuntu-lts-latest
  tools: {}
  commands:
    - curl -LsSf https://astral.sh/uv/install.sh | sh
    - ~/.local/bin/uv tool install tox --with tox-uv -p 3.13 --managed-python
    - ~/.local/bin/tox run -e docs --
```

---

_Comment by @tsvikas on 2025-09-02 13:23_

the official docs shows a recommended solution:
https://docs.readthedocs.com/platform/stable/build-customization.html#install-dependencies-with-uv

```yaml
   jobs:
      pre_create_environment:
         - asdf plugin add uv
         - asdf install uv latest
         - asdf global uv latest
      create_environment:
         - uv venv "${READTHEDOCS_VIRTUALENV_PATH}"
      install:
         - UV_PROJECT_ENVIRONMENT="${READTHEDOCS_VIRTUALENV_PATH}" uv sync --frozen --group docs
```

---

_Comment by @cthoyt on 2025-09-02 13:26_

amazing, the updates to the official docs are so comprehensive across many different installation tools!

---

_Comment by @konstin on 2025-09-02 13:44_

Do you consider this solved now that it's in the official docs?

---

_Comment by @cthoyt on 2025-09-02 13:47_

Yes, I do

---

_Closed by @cthoyt on 2025-09-02 13:48_

---
