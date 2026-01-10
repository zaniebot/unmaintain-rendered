---
number: 10793
title: "Add --frozen flag to \"sync-and-running\" documentation"
type: issue
state: closed
author: f0lie
labels:
  - documentation
  - needs-decision
assignees: []
created_at: 2025-01-20T22:51:34Z
updated_at: 2025-04-10T19:47:01Z
url: https://github.com/astral-sh/uv/issues/10793
synced_at: 2026-01-10T01:24:57Z
---

# Add --frozen flag to "sync-and-running" documentation

---

_Issue opened by @f0lie on 2025-01-20 22:51_

https://docs.astral.sh/uv/guides/integration/github/#syncing-and-running

We have:

```
name: Example

jobs:
  uv-example:
    name: python
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v5

      - name: Install the project
        run: uv sync --all-extras --dev

      - name: Run tests
        # For example, using `pytest`
        run: uv run pytest tests
```

And it seems like it should be:

```
name: Example

jobs:
  uv-example:
    name: python
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Install uv
        uses: astral-sh/setup-uv@v5

      - name: Install the project
        run: uv sync --all-extras --dev --frozen

      - name: Run tests
        # For example, using `pytest`
        run: uv run pytest tests
```

There should also be in the docs a blurb about how by default `uv sync` updates all the lockfile and using `--frozen` ensures that the same version of the packages on the dev machine is used by the github actions VM.

This behavior isn't very obvious for a newcomer reading the documentation and the integration section would be a great place to clarify that behavior.

---

_Label `documentation` added by @charliermarsh on 2025-01-21 03:16_

---

_Label `needs-decision` added by @charliermarsh on 2025-01-21 03:16_

---

_Comment by @konstin on 2025-01-21 07:41_

I think we should add this, though as `--locked` instead of `--frozen` to assert that the lockfile is up-to-date.

---

_Comment by @johnthagen on 2025-03-21 16:55_

I noticed this too at

- https://docs.astral.sh/uv/guides/integration/github/#syncing-and-running

Also reading the docs, the distinction between `--frozen` and `--locked` is a bit difficult to understand.

Also, in the Docker docs, `--frozen` is used rather than `--locked`:

https://docs.astral.sh/uv/guides/integration/docker/#installing-a-project

It seems like the user would normally want `--locked` to avoid the situation

### Other Thought

I was also pretty surprised that `--locked` wasn't the default for `uv sync`. To me when someone comes to a project, whether a new developer, a CI environment, a container image etc, to me enforcing consistency of dependencies by defaul with `uv sync` seems like the most ideal situation?

---

_Comment by @zanieb on 2025-03-21 17:04_

We might want to add a note on `--frozen` in https://docs.astral.sh/uv/concepts/projects/sync/#checking-if-the-lockfile-is-up-to-date or add a section to https://docs.astral.sh/uv/concepts/projects/sync/#syncing-the-environment

`--frozen` is used in the Docker example because we're using the `uv.lock` as the source of truth there. `--locked` may be a better option for people to avoid surprises.

>  was also pretty surprised that --locked wasn't the default for uv sync. To me when someone comes to a project, whether a new developer, a CI environment, a container image etc, to me enforcing consistency of dependencies by defaul with uv sync seems like the most ideal situation?

We're focusing on the development experience here which is that `uv sync` and `uv run` here should "just do what they're supposed to" not throw an error if you have modified your `pyproject.toml`. This is the same thing that Cargo does. In contrast, the _deployment_ experience is more of a "write it once" thing and it's reasonable to opt-in to additional safeguards. 

---

_Comment by @johnthagen on 2025-03-21 17:13_

@zanieb I'm sympathetic to your goals, and agree they are at odds with what I suggest in 

- #12372

I can concede the tradeoffs you're making and if Cargo works this way that is extra support to your position.

I still feel unsure of this myself given the potential for an unexpected footgun, but can respect the decision you all are making.

---

_Referenced in [astral-sh/uv#12372](../../astral-sh/uv/issues/12372.md) on 2025-03-21 17:13_

---

_Referenced in [astral-sh/uv#12818](../../astral-sh/uv/pulls/12818.md) on 2025-04-10 18:12_

---

_Referenced in [astral-sh/uv#12819](../../astral-sh/uv/pulls/12819.md) on 2025-04-10 18:15_

---

_Closed by @Gankra on 2025-04-10 19:47_

---

_Closed by @Gankra on 2025-04-10 19:47_

---
