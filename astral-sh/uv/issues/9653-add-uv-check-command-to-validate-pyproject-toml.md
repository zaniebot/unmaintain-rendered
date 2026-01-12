```yaml
number: 9653
title: "Add `uv check` command to validate `pyproject.toml` `uv` configuration"
type: issue
state: open
author: johnthagen
labels:
  - enhancement
assignees: []
created_at: 2024-12-05T03:29:24Z
updated_at: 2025-11-29T19:30:53Z
url: https://github.com/astral-sh/uv/issues/9653
synced_at: 2026-01-12T15:59:55Z
```

# Add `uv check` command to validate `pyproject.toml` `uv` configuration

---

_@johnthagen_

Would it make sense to add some equivalent to `poetry check` into `uv`? 

- https://python-poetry.org/docs/cli#check

The idea is that you can add this to CI and if a PR makes any change to the `uv` configuration that is invalid, this will fail the CI.

Looking at Poetry's implementation:

- https://github.com/python-poetry/poetry/blob/bd4adcb749b9a9479039830117d2b4be0457ad3d/src/poetry/console/commands/check.py#L126

It currently validates:

- The Poetry configuration in `pyproject.toml`
- Trove classifiers are valid
- That READMEs that should exist do
- Dependency sources
- Lock file freshness (i.e., `uv lock --locked`)
- Optionally with `poetry check --lock` verify that a `poetry.lock` _exists_ (e.g., that someone didn't accidentally delete it)

In general, most of these checks are useful to enforce in CI for many projects. Would it make sense for `uv` to have an equivalent of some kind of `check` command?

---

_Comment by @9128305 on 2024-12-05 08:56_

Duplicate of https://github.com/astral-sh/uv/issues/7639?

---

_Comment by @johnthagen on 2024-12-05 15:04_

> Duplicate of #7639?

Certainly at least related and along a similar vein.

---

_Comment by @zanieb on 2024-12-05 15:18_

We have some other thoughts for a `uv check` command, but I think this is a reasonable ask.

I think we can consider this distinct from #7639 which seemed to focus on checking if the lockfile is up-to-date.

---

_Label `enhancement` added by @zanieb on 2024-12-05 15:19_

---

_Comment by @MichaReiser on 2024-12-05 15:49_

As reference, I think Cargo calls this command `verify-project`. 

---

_Comment by @charliermarsh on 2024-12-06 00:13_

Another interesting thing that a `uv check` could do: validate the recorded wheel hashes against the exact unzipped files on disk. Similar to https://manpages.ubuntu.com/manpages/trusty/man1/debsums.1.html.

---

_Comment by @rpdelaney on 2025-04-15 23:10_

`poetry check` validating the pyproject.toml file is useful as a [pre-commit](https://pre-commit.com) hook to catch errors early. It would be appreciated if `uv` could provide this functionality as well.

---

_Comment by @gboeing on 2025-06-18 00:17_

Agreed. I'm currently running `uvx --from=validate-pyproject[all] validate-pyproject ./pyproject.toml` to validate it, and it'd be great for streamlining if `uv` could provide this.

---

_Comment by @j-adamczyk on 2025-06-29 11:41_

+1, this is one of few things that Poetry supports and uv does not that I've run into

---

_Comment by @blag on 2025-11-29 19:30_

> Agreed. I'm currently running `uvx --from=validate-pyproject[all] validate-pyproject ./pyproject.toml` to validate it, and it'd be great for streamlining if `uv` could provide this.

@gboeing Thanks for posting your workaround. Not sure if `uvx` has changed since your comment, but I had to tweak your command a tiny bit to get it to work for me:

```bash
uvx --from "validate-pyproject[all]" validate-pyproject ./pyproject.toml
```

---
