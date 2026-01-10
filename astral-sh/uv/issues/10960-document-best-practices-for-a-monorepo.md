---
number: 10960
title: Document best practices for a monorepo
type: issue
state: open
author: matthewadams
labels:
  - documentation
assignees: []
created_at: 2025-01-23T15:34:48Z
updated_at: 2025-11-23T10:20:12Z
url: https://github.com/astral-sh/uv/issues/10960
synced_at: 2026-01-10T01:24:59Z
---

# Document best practices for a monorepo

---

_Issue opened by @matthewadams on 2025-01-23 15:34_

Experienced developer here, but I'm a `python` & `uv` noob.  I suspect that it's current feature set supports monorepos, I'm just not clear how, and there is surprisingly little information on the interwebbernets on this topic.

This request is to fully document how a python monorepo should be organized and how `uv` should be configured (in `pyproject.toml` & possibly elsewhere) in this scenario to provide guidance & best practices
* for a near-zero-config, productive development experience out of the box (that is, post `git clone`) with major IDEs (PyCharm, VS Code, etc),
* on how to publish libraries & applications publicly & privately, preferably from CI pipelines, and
* on how to package private libraries so that they're effectively embedded in private applications while still consuming public packages conventionally.

It would be helpful to include common `uv` commands for monorepos for installing packages, packaging applications, publishing libraries & applications, etc.

The documentation should address at least the following two monorepo use cases:

1. One or more library projects with interdependencies that are each published publicly.
2. One or more applications that depend on one or more library projects within the same monorepo, only some of which are published publicly, and the rest are either published privately or are never published.

Edit: See https://github.com/astral-sh/uv/issues/11095 for the feature affecting/supporting this.

~~I see the completion of this issue potentially leading to new features in `uv` that might include monorepo scaffolding via something like `uv init --monorepo` and even, given an existing, `uv`-compliant monorepo, scaffolding where a developer could add new libraries and applications to the monorepo, like:~~

<strike>

```
mkdir -p "$MONOREPO_ROOT_DIR"
cd "$MONOREPO_ROOT_DIR"
uv init --monorepo --interactive # prompts for libraries & applications to be included, addressing public/private visibility
# perhaps adds monorepo metadata to the root pyproject.toml file or a file like monorepo.uv describing the projects comprising the monorepo

# ...some time later...

# thanks to uv monorepo metadata, uv is monorepo-aware

uv init --lib foobar # adds a new library project to the monorepo
# maybe supports visibility options like --public, --private, or even --unpublished
# for libraries intended to be embedded in apps
uv init --app myapp --embeds-libs foobar,snafu # adds a new app that embeds libraries foobar & snafu
```

</strike>

---

_Comment by @matthewadams on 2025-01-25 16:43_

I have begun a WIP git repository for this work, with my progress so far:  https://github.com/matthewadams/uv-monorepo

Feel free to file issues and/or submit PRs.

---

_Comment by @zanieb on 2025-01-25 16:46_

I'm going to move this over to the `uv` repository, this repository is just for deployment of our documentation.

---

_Label `documentation` added by @zanieb on 2025-01-25 16:46_

---

_Referenced in [astral-sh/uv#11070](../../astral-sh/uv/issues/11070.md) on 2025-01-30 13:50_

---

_Referenced in [astral-sh/uv#10934](../../astral-sh/uv/issues/10934.md) on 2025-01-30 13:50_

---

_Referenced in [astral-sh/uv#9626](../../astral-sh/uv/issues/9626.md) on 2025-01-30 13:53_

---

_Referenced in [astral-sh/uv#8302](../../astral-sh/uv/issues/8302.md) on 2025-01-30 13:55_

---

_Referenced in [astral-sh/uv#11095](../../astral-sh/uv/issues/11095.md) on 2025-01-30 14:19_

---

_Comment by @matthewadams on 2025-01-30 14:20_

FYI created https://github.com/astral-sh/uv/issues/11095 to track the `uv` feature that might go along with this.

---

_Comment by @Spenhouet on 2025-06-17 13:32_

@matthewadams I am currently migrating our larger microservice system to a monorepo and are currently evaluating uv workspaces for that.

In the monorepo I have two folders "apps" and "components".
- "apps" contain only the top-level API or CLI code (entrypoints). All code lives in components.
- "components" contains all the logic, structured as workspace packages.
The folder structure is inspired by [polylith](https://polylith.gitbook.io/polylith) but adopted to our liking.

```
.
├── apps/
│   ├── my-app-1/
│   │   ├── tests/
│   │   │   ├── __init__.py
│   │   │   └── test_main.py
│   │   ├── src/
│   │   │   └── my_app_1/
│   │   │       ├── __init__.py
│   │   │       ├── py.typed
│   │   │       └── main.py
│   │   ├── pyproject.toml
│   │   └── README.md
│   └── my-app-2/
│       └── ...
├── components/
│   ├── my-component-1/
│   │   ├── src/
│   │   │   └── my_component_1/
│   │   │       ├── __init__.py
│   │   │       ├── py.typed
│   │   │       └── my_module.py
│   │   ├── tests/
│   │   │   └── test_my_module.py
│   │   ├── pyproject.toml
│   │   └── README.md
│   └── my-component-2/
│       └── ...
├── pyproject.toml
├── uv.lock
└── ...
```

These are the uv commands I'm using to set this up:
```bash
# Create a new component or app
uv init --package --lib components/my-component-1

# Add my-component as dependency to my-app
uv add --package my-app-1 my-component-1
```

To work on all code at the same time, I'm installing the monorepo like this:
```
uv sync --all-packages --all-extras --all-groups
```

What I am missing is a command to detect affected "apps", i.e. apps where one of it's workspace dependencies changed (incl. transitive).
This I would need to make builds and test execution efficient.

EDIT: Here a script we are using to solve this: https://github.com/astral-sh/uv/issues/6356#issuecomment-2983217371

A nice to have would be a visualization (matrix) on workspace interdependencies of apps/components (incl. component->component).

That is all I'm missing right now, can share more if I encounter other obstacles.

I'm also looking into utilizing Dagger, as described here: [Cracking the Python Monorepo](https://gafni.dev/blog/cracking-the-python-monorepo/)

---

_Referenced in [astral-sh/uv#6356](../../astral-sh/uv/issues/6356.md) on 2025-06-17 13:36_

---

_Comment by @AndreuCodina on 2025-06-17 13:53_

`uv init` isn't appropriate for monorepos because it doesn't have good defaults, so you must create your own folders and files. You can check https://github.com/AndreuCodina/python-monorepo if you want a monorepo example.

Let's create a monorepo to show why `uv init` isn't appropriate:
1. Execute `uv init my-project`: If you're not creating an open-source package, it adds boilerplate: pyproject.toml with description and readme, and README.md
2. Execute `cd my-project`.
3. Execute `uv init --lib domain`: It creates `__init__.py` with code, README.md, pyproject.toml with description, readme, authors, an empty array of dependencies, and requires-python (you don't need it again because you already specified it in my-project). Also, it adds domain as a workspace (perfect), but it doesn't add it as a source dependency.

---

_Comment by @Spenhouet on 2025-07-08 06:04_

Another missing things is namespace support:
- https://github.com/astral-sh/uv/issues/7182
- https://github.com/astral-sh/uv/issues/11787

---

_Comment by @dfaivre-pcs on 2025-10-24 21:11_

I'm another experience dev, but python/uv noob. I'm seeing some guidance on setting up a multiple package workspace that "handles" namespaces (https://github.com/astral-sh/uv/issues/10960#issuecomment-3047487402) - but seems to require "double nesting" of the folder dir, then namespace folder dir. AI and Google all tell me this is just the way it is, but I feel like I'm missing something obvious. DX ergonomics in PyCharm is almost laughable.

**This is layout I keep getting pushed towards:**
<details>

```
├── acme
│   ├── apps
│   │   └── ceres
│   │       ├── pyproject.toml
│   │       ├── src
│   │       │   ├── acme
│   │       │   │   └── apps
│   │       │   │       └── ceres
│   │       │   │           ├── __init__.py
│   │       │   │           └── __pycache__
│   │       │   └── acme_apps_ceres.egg-info
│   │       └── tests
│   │           ├── __init__.py
│   │           └── __pycache__
│   ├── core
│   │   ├── pyproject.toml
│   │   ├── src
│   │   │   ├── acme
│   │   │   │   ├── __pycache__
│   │   │   │   └── core
│   │   │   │       ├── __init__.py
│   │   │   │       └── __pycache__
│   │   │   └── acme_core.egg-info
│   │   └── tests
│   │       ├── __init__.py
│   │       └── __pycache__
│   └── infra
│       └── db
│           └── boa
│               ├── pyproject.toml
│               ├── src
│               │   ├── acme
│               │   │   └── infra
│               │   │       └── db
│               │   │           └── boa
│               │   │               ├── __init__.py
│               │   │               └── __pycache__
│               │   └── acme_infra_db_boa.egg-info
│               └── tests
│                   ├── __init__.py
│                   └── __pycache__
└── pyproject.toml
```

</details>


**These are my "requirements"**
<details>

I want to build a Python large scale enterprise "app" - consisting of multiple deployable apps/jobs, and shareable base libraries (business domain, db access, core utils). My naive, new to Python, C# experienced brain imagines this for the set of "packages" I'd need:


- in dependency order - higher up refs lower down
- assume within in leaf node, there is src/, tests/

```
- acme (org-name)
    - apps (user facing apps)
        - app-1
        - app-2
    - jobs
        - default (backend jobs that can span apps)
        - offline-reporting
        - ...
    - domain (business logic)
    - infra
        - db (orm/modeling/db access code)
            - db-server-1
            - db-server-2
    - core (utils/common code refed by everything)
```

**Assuming:**
- I want to be able ref the `acme/infra/db/db-server-1` in an python idiomatic pattern and fully qualified - so:
    - `from acme.infra.db.db_server_1 import FooModel`
- I don't want to publish anything to PyPi, etc
- I want to use the latest and greatest python tools (uv, pyright, ruff, pytest) 
- I want to be able to run all pyright, pytest, ruff from the root and have it run for all sub-packages
- I want VS Code to understand the layout and work

*Is there a way to set this up sanely without using the "double nested" namespace python packages?*

</details>

---

_Comment by @Spenhouet on 2025-10-25 01:16_

@dfaivre-pcs I feel you on your urge to avoid the "double nested" folder structure but there are reasons for it. What you are looking at is the "flat" vs. "src" layout debate. You can find man sources debating the pros and cons e.g.: https://www.jcheng.org/post/python-and-the-src-vs-flat-layout-debate/

In setting up our monorepo I had the same urge as you. Eventually the flat layout did run me intro trouble and I was spending time fighting obscure python quirks. Switching to src layout solved all issues and is what we are using accross the board.
When you accept it, the "double nested" folders are less of an issue than it might feel right now. VS Code automatically collapses or opens these folders, making it a non-issue DX wise.

EDIT: See https://github.com/astral-sh/uv/issues/10960#issuecomment-2980405673

---

_Comment by @dfaivre-pcs on 2025-10-25 12:10_

Thanks @Spenhouet! Yeah, I definitely tried the "flat" before and it's a mess. I think I'm wondering more:

- should I just use a single `pyproject.toml` and treat the entire code base as a single project?
- what do I gain by having each conceptual package it's own pyproject.toml - especially if I'm not publishing anything? Are those gains worth it?
- in the "single project/package" approach - in the past, it's been tricky to co-locate react/ts frontend apps next to the python backend - is the best practice to basically have different project roots for python and react/ts then?

I don't want to pollute this issues' comments too much, so I also cross posted here: https://www.reddit.com/r/learnpython/comments/1of96ci/large_enterprise_app_code_setup_monorepo_uv/

Still - I would enjoy any "official" uv docs on how to setup larger/non-trivial projects, trade-offs, etc - so I appreciate this issue being open.

---

_Comment by @Spenhouet on 2025-10-25 12:43_

As this issue is about best practices, I think it is fine for us to discuss what work and what doesn't.

> what do I gain by having each conceptual package it's own pyproject.toml - especially if I'm not publishing anything? Are those gains worth it?

Depends on your use-case. Prior to our monorepo migration we had many python packages (build as such) and many microservices (importing the build packages). Everything was in separate repositories.
This feels clean and has benefits on access management and versioning but comes with a lot of downsides (we suffered under).
Our core problems stem from the fact that we always had a distributed monolith. 🚩

Migrating to the monorepo approach with separation between packages (we call "components") and microservices (we call "apps") enables easy code sharing and code changes across packages and components with a single PR and most importantly, it enables AI copilots to perform such changes across a larger microservice architecture (without breaking a part).
It forces us to directly update all components/apps for a change and to keep dependency versions in sync (which often requires more refactoring but keeps technical depth small).
Overall we are quite excited about our new approach.

We are no longer building packages and instead use them as workspace packages. Microservices are still build as container images.
Every microservice image only includes the code of workspace packages actually used. Even with the monorepo we do not need to include all code in all container images (which would be terrible for us as some of our service have dependencies like torch, ..., which would bloat a simple microservice). The code of apps is just a thin wrapper for the microservice interface. All code lives in components, promoting reuse.

If you are only building a single service/app, then there is no point for a pure python based monorepo as we use.

To me it seemed that there is no benefit to put non-python services in the same repo.


---

_Comment by @ixxie on 2025-10-25 13:29_

> Every microservice image only includes the code of workspace packages actually used. Even with the monorepo we do not need to include all code in all container images (which would be terrible for us as some of our service have dependencies like torch, ..., which would bloat a simple microservice). 

We recently were thinking about this topic. Based on [uv`s docker best practices](https://docs.astral.sh/uv/guides/integration/docker/) we ended up doing something like this:
```Dockerfile
  # Build stage
  FROM ghcr.io/astral-sh/uv:python3.13-alpine AS builder

  WORKDIR /app

  ENV UV_COMPILE_BYTECODE=1 \
      UV_LINK_MODE=copy

  # Copy workspace files
  COPY pyproject.toml uv.lock ./

  # Copy shared library (dependency)
  COPY lib/core/pyproject.toml lib/core/
  COPY lib/core/src/ lib/core/src/

  # Copy this service
  COPY service/foo/pyproject.toml service/foo/

  # Install dependencies (includes qcore_lib from workspace)
  RUN --mount=type=cache,target=/root/.cache/uv \
      uv sync --frozen --no-install-project --no-dev --package foo_service

  # Copy service source
  COPY service/foo/src/ service/foo/src/

  # Install project in non-editable mode
  RUN --mount=type=cache,target=/root/.cache/uv \
      uv sync --frozen --no-editable --no-dev --package foo_service

  # Final stage
  FROM python:3.13-alpine

  WORKDIR /app
  RUN apk add --no-cache curl

  # Copy .venv with both foo_service AND core_lib installed
  COPY --from=builder /app/.venv /app/.venv

  # etc....
```
Where in our terminology `service` is a microservice and `library` is a shared packaged used by one or more service.


---

_Comment by @ixxie on 2025-10-25 13:32_

Another reflection: its not necessarily a choice between workspace and path-based dependencies.

We had some CRUD API microservices running very modern Python, and then some ML stuff that needed pinning to older dependencies. We ended up with a structure with two workspaces as a way of solving the conflict.

---

_Comment by @NixBiks on 2025-10-25 21:35_

> Microservices are still build as container images.
Every microservice image only includes the code of workspace packages actually used. Even with the monorepo we do not need to include all code in all container images (which would be terrible for us as some of our service have dependencies like torch, ..., which would bloat a simple microservice). The code of apps is just a thin wrapper for the microservice interface. All code lives in components, promoting reuse.

@Spenhouet are you doing this "automatically" or are you kinda handholding which source code to include and which not to for each app? And do you have "smart" builds so you only rebuilt what's necessary (if some included source code or dependency changed)

---

_Comment by @Spenhouet on 2025-10-26 06:26_

@NixBiks First we detect what apps/components in the Monorepo had any change in a PR (I linked our script for that in a message above). Based on this we know which apps are affected by the change and then only rebuild these ones.

Second, as part of our docker build process, we parse the uv.lock for workspace packages which are part of the app that is build and then via dagger we dynamically create a dockerfile which only copies the code needed for that app.
It also dynamically builds a prod and dev stage build where the dev one also includes dev dependencies and test files.
Pytest and Ruff are then executed against this reduced code set, only covering the code actually part of the build.

---

_Comment by @NixBiks on 2025-10-26 08:36_

Ah didn't see that. My bad. I see you also linked to [Cracking the Python Monorepo](https://gafni.dev/blog/cracking-the-python-monorepo/) - great blog post on the subject!

And thanks for sharing.

---

_Comment by @Eremeyen on 2025-11-04 07:47_

maybe the monorepo was the friends we made along the way

---

_Comment by @Spenhouet on 2025-11-22 20:44_

@ixxie How are you running tests in that setup? In our setup in the dev stage we copy all code of an app, incl. its transitive workspace dependencies, with all their tests and then execute all included tests. While this tests a build app in its entirety, execution of workspace dep. tests is redundant when multiple apps with the same deps are build at the same time. We are suffering from long build times atm.


---

_Comment by @ixxie on 2025-11-22 21:42_

> @ixxie How are you running tests in that setup?

We aren't because we don't have any yet. 

> While this tests a build app in its entirety, execution of workspace dep. tests is redundant when multiple apps with the same deps are build at the same time. We are suffering from long build times atm.

This may be a naive suggestion, but why not skip those dependency tests in upstream builds, and instead run them in a decoupled CI job which only executes once for the shared library, and only when the code in that package changes?



---

_Comment by @Spenhouet on 2025-11-23 10:20_

> why not skip those dependency tests in upstream builds, and instead run them in a decoupled CI job which only executes once for the shared library, and only when the code in that package changes

I was thinking in a similar direction but I see two issues with that. 

You will need to run the tests in an environment where all workspace packages are available. This will not match the final build environment and can result in different test outcomes (e.g. importing another workspace package which is not available in the final build, having a workspace package monkey patching something or changing a global state, ...). Then the tests and build will pass but will fail during runtime, breaking our dev env and main branch in addition to being detected much later + adding confusion for dev "but the tests passed".

We are working in a highly regulated field. We need to execute tests on the final build, so that we can show that this build did pass the tests. Executing the workspace dependency tests prior, might not be possible for us.

---
