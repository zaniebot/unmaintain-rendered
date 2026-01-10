```yaml
number: 5632
title: Development dependency group support
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - needs-design
assignees: []
created_at: 2024-07-30T20:19:58Z
updated_at: 2024-10-25T20:16:00Z
url: https://github.com/astral-sh/uv/issues/5632
synced_at: 2026-01-10T04:36:20Z
```

# Development dependency group support

---

_Issue opened by @zanieb on 2024-07-30 20:19_

This is a tracking issue for the general idea of putting development dependencies into named groups for partial installs.

In part, we're waiting to see where [PEP 735](https://peps.python.org/pep-0735/) goes, which

> ... specifies a mechanism for storing package requirements in pyproject.toml files such that they are not included in any built distribution of the project.
> 
> This is suitable for creating named groups of dependencies, similar to requirements.txt files, which launchers, IDEs, and other tools can find and identify by name.
>
> The feature defined here is referred to as “Dependency Groups”.

Related:

- https://github.com/astral-sh/uv/issues/5278#issuecomment-2242053901
- https://github.com/astral-sh/uv/issues/3560#issuecomment-2258919283

---

_Comment by @zanieb on 2024-07-30 20:24_

@mishamsk regarding

> are there any particular plans for PEP 735 functionality? Like for example "we'll wait for PEP 735 to be accepted and will not tackle the case until it is pending", or maybe "we may get to it sooner o despite PEP 735 status but not earlier than X months from now"

We may tackle this behavior before PEP 735 is finalized, but we are not going to include this in our upcoming stable release (in the next couple months). In the meantime, we want to:

1) Learn more about use-cases, e.g. people say they need dependency groups for install speed but that it's largely less of a problem with uv's improved performance
2) Explore how this would work with uv's CLI
3) Plan for transition to PEP 735's spec if it looks like it will be accepted

---

_Label `needs-design` added by @zanieb on 2024-07-30 20:24_

---

_Label `enhancement` added by @zanieb on 2024-07-30 20:24_

---

_Comment by @mishamsk on 2024-07-30 22:07_

@zanieb thanks. 

> Learn more about use-cases, e.g. people say they need dependency groups for install speed but that it's largely less of a problem with uv's improved performance

I can elaborate a bit on my use case assuming this is the right place:

### So first a rough setup we have for context:

* A Commerical enterprise app (so no publishing to PyPi, no public Github Actions)
* Running on a hosted Gitlab
* we use tox to with a goal of allowing reproducible results on a local developer machine and CI. I.e. CI literally does `poetry install --only "tox"` and then `poetry run tox`. The rest is handled within tox environments.
* We have test, pre-commit, mypy & docs tox environments. Each try to install the minimum number of packages necessary to run
* In poetry we have dips group roughly matching tox envs + `tox` group (just three deps there) and a `build` group. The build one should not be confused with the regular python build backend. In our case build is running nuitka

### Group requirements

* The "speed" is indeed one of the main driving decisions behind trying to only install what's necessary. But this is not the only reason
* Some group dependencies may be incompatible with the project dependencies. 
  * We have at least one such case with `requests` which we locked and haven't yet updated, but all dev/test tools did, which cause trouble. 
  * TBH poetry doesn't handle this properly as well and we had to apply a workaround eventually... 
  * Also, for this specific case, I wanted to use uv's overrides, so uv is already better ;-)
  * But, you can check poetry discussions, this is one of the most requested features (independent locking and install of Dep groups) - this is a very real use case when some development dependency has conflict with the project one
* You may want to make sure that some packages are not installed in some context(s). E.g. nuitka picks up packages by running python and figuring out what's importable. I am not confident enough that it won't accidentally bundle something I do not want (or a version I do not want). This may be more of a paranoia, but making sure that nothing not directly relevant to the task at hand is installed is nice. In my case with nuitka it means the project dependencies and nuitka and nothing else 
  * Another more realistic example that we had are integration tests. We are doing e2e test that execute a compiled binary. The tests are the same python files and are executed by pytest, but we wanted to be DEAD sure that the project is not executed from source. But if we have to install it alongside test dependencies and tools, this is not easy

I think this is it... I'll add more if I remember something.

Sorry for a long read 

---

_Comment by @zanieb on 2024-07-30 22:15_

Thanks for the context!

> Some group dependencies may be incompatible with the project dependencies

You highlight this already, but this is a problem in Poetry because it solves for _all_ groups at once and does not allow incompatibilities. This is a problem for us too! If we allow incompatible groups, we have to solve for _all possible combinations of the groups_ to create a lockfile for the project. We're hoping this case is addressed by project-level tools or perhaps in PEP 735.




---

_Comment by @mishamsk on 2024-07-30 23:13_

yes, at least in my case, this is essentially about project-level tools. E.g., I have the latest ruff system wide, but lock different versions for specific projects.

From the vantage point of this use cases, it may make sense to have all the groups in their own isolated environments. I know that project-level tools are out of scope for now, but I can imagine a simple solution where "project-level tool" is just telling uv that a specific version of a tool should be used inside this specific project. Then it will work similar to how pyenv/asdf/mise/rye etc handle python itself - shims/path manipulation. No local secondary .venv's for tools, no cross compilation of dependencies etc.

But others may come up with other use cases. Poetry threads related to groups are numerous and very long!

---

_Comment by @chrisrodrigue on 2024-08-03 19:43_

I think basic support for development dependency groups and scripts could replace other popular environment management/command running tools like `hatch` and `tox` for use cases like @mishamsk’s. 

These features would provide a good way to organize dependencies, modularize CI/CD pipelines, and standardize common commands/aliases among teammates and CI/CD by centralizing them in `pyproject.toml`.

# Example 

## `pyproject.toml`

```toml
[tool.uv.dev-dependencies]
check = [
  "codespell>=2.3.0", 
  "mypy>=1.11.1", 
  "pre-commit>=3.8.0", 
  "ruff>=0.5.6",
]
test = [
  "pytest>=8.3.2",
  "pytest-cov>=5.0.0",
  "pytest-mock>=3.14.0",
]
doc = [
  "mkdocs>=1.6.0",
  "mkdocs-material>=9.5.31",
  "mkdocstrings[python]>=0.25.2",
]
release = [
  "commitizen>=3.28.0",
  "cyclonedx-bom>=4.5.0",
]

[tool.uv.commands]
check = ["codespell .", "ruff check .", "ruff format .", "mypy ."]
test = "pytest"
doc = "mkdocs serve"
cov = "pytest --cov --cov-report term-missing --cov-report xml --cov-report html"
hooks = "pre-commit run --all-files"
sbom = "cyclonedx-py environment .venv --outfile sbom.json --output-format json"
clean = "git clean -fdx"
```

## CLI Usage

### Dependency groups
```
uv add -g test pytest // add pytest to “test” dev dependency group
uv sync -dg test // only install deps and test deps to .venv
uv sync -dog test -g doc // deps, optional deps, test/doc deps
uv sync // install all deps, optional deps, and dev deps
```

### Command running
```
uv check // or uv run check
uv doc
uv clean
```

## CI/CD Usage
```yaml
jobs:
  check:
    name: "check"
    runs-on: alpine-latest
    steps:
       - name: "Prepare environment"
         run: uv sync -dg check
       - name: "Spell check"
         run: uv run codespell .
       - name: "Static analysis"
         run: uv run ruff check .
       - name: "Type check"
         run: uv run mypy .
  test:
    name: "test"
    runs-on: alpine-latest
    steps:
      - name: "Prepare environment"
        run: uv sync -dg test
      - name: "Code coverage"
        run: uv cov --run unit
      - name: "Integration tests"
        run: uv test --run integration
```

---

_Comment by @lewoudar on 2024-08-21 06:50_

Hi everyone, I have a workflow almost similar to @chrisrodrigue, so I really hope this feature will be added. It will help poetry (or pdm) users like me to switch to uv.

---

_Comment by @schlamar on 2024-08-22 18:13_

I think one important use case when talking about replacing tox or hatch wasn't explicitly mentioned yet. The possibility to target multiple python versions. When you develop libraries instead of applications you want to support a wide range of Python versions and define test environment for various Python versions. 

Another use case would be targeting different versions of a library. For example when you are developing a pytest plugin you want to test this against various versions of pytest.

These use cases should be considered when uv is providing a feature for multiple development environments.

See for example matrix documentation from hatch: https://hatch.pypa.io/1.12/environment/#matrix

---

_Comment by @schlamar on 2024-08-22 18:22_

Here are two real world tox configurations covering these use cases.

https://github.com/pallets/flask/blob/main/tox.ini
https://github.com/pytest-dev/pytest-django/blob/main/tox.ini

---

_Comment by @Samoed on 2024-08-23 16:07_

PDM support this and have some use cases examples (I have similar use cases) https://pdm-project.org/latest/usage/dependency/#add-development-only-dependencies

---

_Comment by @sebastian-correa on 2024-09-07 17:43_

Our usecase for dependency groups is "distribution".

We develop in a bigish application with many components in a single package. For example, imagine the package is `package` and inside we have `component_a`, `component_b` and `models`, which itself has 2 components inside (2 `.py` files).

These components are executed by running some Docker images we generate (one per component). These docker images install `package` but only install the dependency groups related to each component. For example, say `component_a` needs to interact with a cloud provider, a database and uses a specific framework but `component_b` doesn't need any of these. In that scenario, we do `poetry install --sync --only main --only cloud_provider --only database --only framework_and_related_deps`. This makes Docker builds faster and also results in smaller images.

In that same vein, some GitHub Actions only need some dependency groups to run, which makes the venv cache recovery faster (because resulting venvs are smaller).

I recognize that this compartmentalization is a bit horrible as it needs human knowledge of which sub-packages use which dependency groups, and only works because the packages imported when executing `component_a/entrypoint.py` are some of them (and not all), but it's been working for us to reduce Docker image sizes.

---

_Comment by @gotmax23 on 2024-09-16 21:38_

I like to have separation of concerns so I know which development dependencies are used for what, and for test sessions in nox (typing, linting, pytest unit tests), I don't want to have extra dependencies that a user won't have if they just install the package in a clean environment impact the results or have tests that only pass because of extra transitive dependencies pulled in by other development tools.

---

_Comment by @mmlynarik on 2024-09-18 09:09_

> Our usecase for dependency groups is "distribution".
> 
> We develop in a bigish application with many components in a single package. For example, imagine the package is `package` and inside we have `component_a`, `component_b` and `models`, which itself has 2 components inside (2 `.py` files).
> 
> These components are executed by running some Docker images we generate (one per component). These docker images install `package` but only install the dependency groups related to each component. For example, say `component_a` needs to interact with a cloud provider, a database and uses a specific framework but `component_b` doesn't need any of these. In that scenario, we do `poetry install --sync --only main --only cloud_provider --only database --only framework_and_related_deps`. This makes Docker builds faster and also results in smaller images.
> 
I totally agree with this distribution problem description and add one vote in favor of this use case that imho justifies the need for dependency groups in corporate setup. 

---

_Comment by @michelkok on 2024-10-10 07:24_

Another usecase is for part of the Machine Learning community where GPU dependencies should only be installed at training time, but during inference can be handled by the much smaller CPU dependencies.  

---

_Comment by @jankatins on 2024-10-15 21:54_

Seems this is now tracked in https://github.com/astral-sh/uv/issues/8090 so either this or the new issue could be closed as duplicate.

---

_Closed by @zanieb on 2024-10-25 18:27_

---

_Comment by @zanieb on 2024-10-25 20:15_

As of [0.4.27](https://github.com/astral-sh/uv/releases/tag/0.4.27) we support this. See #8272.

---
