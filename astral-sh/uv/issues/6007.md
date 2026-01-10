```yaml
number: 6007
title: "Feature request: Generate `requirements.txt` from `uv.lock`"
type: issue
state: closed
author: zeevro
labels:
  - enhancement
assignees: []
created_at: 2024-08-11T14:32:54Z
updated_at: 2025-12-06T00:11:47Z
url: https://github.com/astral-sh/uv/issues/6007
synced_at: 2026-01-10T03:11:31Z
```

# Feature request: Generate `requirements.txt` from `uv.lock`

---

_Issue opened by @zeevro on 2024-08-11 14:32_

It would be beneficial to me (and I think in general) to be able to generate a `requirements.txt` file from the `uv.lock` file. It would be useful for installing requirements without having to resolve dependencies using a different installer in environments where `uv` is not installed, and also for having other tools that don't support the `uv.lock` file format (e.g. Dependabot) be able to know the project's pinned dependencies.

---

_Label `enhancement` added by @charliermarsh on 2024-08-11 18:52_

---

_Label `preview` added by @charliermarsh on 2024-08-11 18:52_

---

_Comment by @charliermarsh on 2024-08-11 18:53_

I think something like this is reasonable but it probably won't happen immediately. ([PEP 751](https://peps.python.org/pep-0751/) is ultimately a better solution but I'd still support `requirements.txt` in the short-term.)

---

_Comment by @zeevro on 2024-08-12 07:06_

Also, we already have a way to generate `requirements.txt` in the form of `uv pip compile` but it's rather tricky to have that match my `uv.lock`, especially if it's done after a while and some dependencies have had new versions released.

---

_Comment by @zanieb on 2024-08-12 12:28_

It doesn't really seem like a great outcome for users to have _both_ a `uv.lock` and `requirements.txt` though

---

_Comment by @SamuelT-Beslogic on 2024-08-14 18:24_

> It doesn't really seem like a great outcome for users to have _both_ a `uv.lock` and `requirements.txt` though

If using `uv sync` which creates a `uv.lock` file in a Docker, does `uv sync` take care of setting up references to the venv correctly ?
https://docs.astral.sh/uv/guides/integration/docker/#installing-a-package makes no mention of `uv sync` or `uv.lock`

---

_Comment by @zanieb on 2024-08-14 19:02_

Can you clarify what you mean by "references to the venv"?

I haven't written documentation for using the new APIs in Docker yet.

---

_Comment by @SamuelT-Beslogic on 2024-08-14 19:16_

> Can you clarify what you mean by "references to the venv"?

I'm talking about this section of the doc:
> To use the system Python environment by default, set the UV_SYSTEM_PYTHON variable:
> ```Dockerfile
> ENV UV_SYSTEM_PYTHON=1
> ```
> Alternatively, a virtual environment can be created and activated:
> ```Dockerfile
> RUN uv venv /opt/venv
> # Use the virtual environment automatically
> ENV VIRTUAL_ENV=/opt/venv
> # Place entry points in the environment at the front of the path
> ENV PATH="/opt/venv/bin:$PATH"
> ```

I'm assuming if I use `RUN uv sync --locked` instead (to make sure the installation respects the lock file), that I don't need to mess around with venv setup ? (I just need to update my docker entry point command to use `uv run` instead of `python`, ref to our current transition to UV https://github.com/BesLogic/releaf-canopeum/pull/195/files#diff-1fc9c5328b5af6b1065537cb89aa7298eb7ba1d338b5bee8fd07553b6f56576f )

> I haven't written documentation for using the new APIs in Docker yet.

That's a simple explanation as to why it's not referenced ^^

---

I'm just ensuring that the `uv.lock` file should be usable in Docker. Which removes the need for this feature request for me.

---

_Comment by @zanieb on 2024-08-14 19:45_

`uv sync` always creates a virtual environment right now, so yeah you'd either need to opt for configuring it (as described above) or use `uv run` instead of invoking `python`. 

We may add support for syncing to a system environment (as requested in https://github.com/astral-sh/uv/issues/5964) to make this a little simpler in CI and containers.

> I'm just ensuring that the uv.lock file should be usable in Docker

Yeah it definitely is. I'll try to update the documentation soon.

---

_Label `preview` removed by @zanieb on 2024-08-20 18:21_

---

_Comment by @matmair on 2024-08-20 19:14_

> It doesn't really seem like a great outcome for users to have _both_ a `uv.lock` and `requirements.txt` though

I have this use case right now; maybe the insight is useful. There are strict requirements only to deploy stable software in the deployment targets. That rules out uv on the deployment targets by our current rules. The devs still want to use uv and uv project management and it is pretty confusing to have the `requirements.txt` that is used for deployments and the `uv.lock` within the repo.

This can be solved by CI/CD but reducing steps and complexity is the signature of uv/ruff for me.
Loving the tools btw, y'all are doing gods work

---

_Comment by @zanieb on 2024-08-20 19:16_

What makes uv unstable?

Thanks for sharing your use-case! Glad you like the tools :)

---

_Comment by @matmair on 2024-08-20 19:17_

SemVer. I do not make the rules sadly.

---

_Comment by @jfgordon2 on 2024-08-22 05:23_

I also am finding a need to produce a `requirements.txt`, but just for non-dev packages.  The current path I have in our pipeline looks something like:

```shell
uv sync --no-dev
uv pip freeze > requirements.txt
```

Then we can check that requirements file against `safety` (or dependabot) for vulnerabilities, then do a `uv pip install -r requirements.txt --target dist` so we can package it up along with src into a zip file to deploy to a lambda function.

It definitely would be easier to operate directly against the `uv.lock`, but in the meantime converting that directly into a `requirements.txt` for compatibility with other tools would be great.

---

_Comment by @chrisrodrigue on 2024-08-22 11:19_

+1 to what @jfgordon2 said.

I think supporting `requirements.txt` input and output is wise to maintain compatibility with legacy tooling that isn’t yet caught up with PEP 621.

I have documented one such use case for `requirements.txt` here: #6422 

---

_Assigned to @charliermarsh by @charliermarsh on 2024-08-25 17:27_

---

_Comment by @phi-friday on 2024-08-26 02:14_

As you mentioned in #6429 , will there be a feature to create a `requirements.txt` including `tool.uv.dev-dependencies`?

---

_Comment by @charliermarsh on 2024-08-26 02:16_

Yeah, that would probably be included in the scope of this.

---

_Comment by @zeevro on 2024-08-26 19:55_

I'm using this as a make-shift solution for now.
```sh
uvx --from=yq tomlq -r '.package[]|.name+((.source|" @ "+(.url//"git+\(.git//empty)"))//"==\(.version)")' uv.lock >requirements.txt
```

---

_Comment by @charliermarsh on 2024-08-26 20:07_

Respect!

---

_Comment by @SamEdwardes on 2024-08-28 22:29_

I like the idea of `uv export`!

I am curious what are the implications of doing this:

```bash
uv pip compile pyproject.toml -o requirements.txt
```

In this case is my `uv.lock` ignored and there is a risk the generated requirements.txt will be different?

---

_Comment by @charliermarsh on 2024-08-28 22:34_

> In this case is my uv.lock ignored and there is a risk the generated requirements.txt will be different?

Yeah, the `uv pip` interface never touches the lockfile. You _can_ use the `uv pip` workflow if you want, but you lose the benefits of the unified `uv sync`, `uv lock`, `uv run` APIs.

---

_Closed by @charliermarsh on 2024-08-29 17:46_

---

_Comment by @C-Loftus on 2024-12-16 15:31_

Curious if anyone has tips / tools they like to keep requirements.txt in sync with uv.lock so you get the benefits of uv while still being easy to use for code reviewers. Even though I know it is recommended to just use uv, I develop libraries where it is easier to team members to quickly use pip and might not be familiar with uv. 

I am just using a simple github action but wasnt sure if there is a better way.

```yml
name: Validate Requirements

on:
  workflow_dispatch:
  push:
    paths:
      - "pyproject.toml"  # Trigger on changes to pyproject.toml
      - "requirements.txt"
  pull_request:
    paths:
      - "pyproject.toml"  # Trigger on changes to pyproject.toml
      - "requirements.txt"

jobs:
  validate-requirements:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout repository
        uses: actions/checkout@v4


      - name: Install uv
        uses: astral-sh/setup-uv@v4

      - name: "Set up Python"
        uses: actions/setup-python@v5
        with:
          python-version-file: ".python-version"

      - name: Install the project
        run: uv sync --all-extras --dev
        
      - name: Generate new requirements.txt
        run: |
          uv export --format requirements-txt > new_requirements.txt

      - name: Compare requirements.txt
        run: |
          if ! diff -q new_requirements.txt requirements.txt; then
            echo "requirements.txt is out of sync with the project configuration:"
            diff -u requirements.txt new_requirements.txt
            exit 1
          fi
        shell: bash
```

---

_Comment by @zanieb on 2024-12-16 19:44_

There's also support in the uv-pre-commit project.

---

_Comment by @leorochael on 2025-01-21 11:01_

@C-Loftus,

> ```yaml
> name: Validate Requirements
> 
> on:
>   workflow_dispatch:
>   push:
>     paths:
>       - "pyproject.toml"  # Trigger on changes to pyproject.toml
>       - "requirements.txt"
>   pull_request:
>     paths:
>       - "pyproject.toml"  # Trigger on changes to pyproject.toml
>       - "requirements.txt"
> ```

Shouldn't you also validate on changes to `uv.lock`? It's possible to do a dependency package update without touching `pyproject.toml`, after all.

---

_Comment by @simonprovost on 2025-04-11 01:11_

> Curious if anyone has tips / tools they like to keep requirements.txt in sync with uv.lock so you get the benefits of uv while still being easy to use for code reviewers. Even though I know it is recommended to just use uv, I develop libraries where it is easier to team members to quickly use pip and might not be familiar with uv.
> 
> I am just using a simple github action but wasnt sure if there is a better way.
> 
> name: Validate Requirements
> 
> on:
>   workflow_dispatch:
>   push:
>     paths:
>       - "pyproject.toml"  # Trigger on changes to pyproject.toml
>       - "requirements.txt"
>   pull_request:
>     paths:
>       - "pyproject.toml"  # Trigger on changes to pyproject.toml
>       - "requirements.txt"
> 
> jobs:
>   validate-requirements:
>     runs-on: ubuntu-latest
> 
>     steps:
>       - name: Checkout repository
>         uses: actions/checkout@v4
> 
> 
>       - name: Install uv
>         uses: astral-sh/setup-uv@v4
> 
>       - name: "Set up Python"
>         uses: actions/setup-python@v5
>         with:
>           python-version-file: ".python-version"
> 
>       - name: Install the project
>         run: uv sync --all-extras --dev
>         
>       - name: Generate new requirements.txt
>         run: |
>           uv export --format requirements-txt > new_requirements.txt
> 
>       - name: Compare requirements.txt
>         run: |
>           if ! diff -q new_requirements.txt requirements.txt; then
>             echo "requirements.txt is out of sync with the project configuration:"
>             diff -u requirements.txt new_requirements.txt
>             exit 1
>           fi
>         shell: bash

Are you sure that this is not outdated now? @C-Loftus 

---

_Comment by @codejoey on 2025-05-26 00:17_

`uv export --no-emit-workspace --no-dev --no-annotate --no-header --no-hashes --output-file requirements.txt`
thanks for the implement, this worked for me!

---

_Comment by @ilovefreesw on 2025-07-03 07:58_

```
(talitrain) root@aa0d5ec39dd8:~/talitrain# uv export --no-emit-workspace --no-dev --no-annotate --no-header --no-hashes --output-file requirements.txt
error: unexpected argument '--no-annotate' found

Usage: uv export --no-emit-workspace --no-dev

For more information, try '--help'.
```

I am sticking with: `uv pip freeze > requirements.txt`


---

_Comment by @vesnikos on 2025-09-19 09:51_

> ```
> (talitrain) root@aa0d5ec39dd8:~/talitrain# uv export --no-emit-workspace --no-dev --no-annotate --no-header --no-hashes --output-file requirements.txt
> error: unexpected argument '--no-annotate' found
> 
> Usage: uv export --no-emit-workspace --no-dev
> 
> For more information, try '--help'.
> ```
> 
> I am sticking with: `uv pip freeze > requirements.txt`

your uv version might be outdated.

---

_Comment by @hbelmiro on 2025-12-06 00:11_

I built a GitHub Action that specifically validates if your requirements.txt is in sync with pyproject.toml so you don't deploy stale dependencies.

https://github.com/hbelmiro/uv-lock-check

---
