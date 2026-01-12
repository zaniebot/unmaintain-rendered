```yaml
number: 8272
title: Support for PEP 735
type: pull_request
state: merged
author: zanieb
labels:
  - tracking
assignees: []
merged: true
base: main
head: tracking/735
created_at: 2024-10-16T21:35:12Z
updated_at: 2024-10-28T13:57:38Z
url: https://github.com/astral-sh/uv/pull/8272
synced_at: 2026-01-12T16:08:14Z
```

# Support for PEP 735

---

_@zanieb_

Closes https://github.com/astral-sh/uv/issues/8090
Closes https://github.com/astral-sh/uv/issues/5632

Includes:

- #8104 
- #8108
- #8110
- #8266
- #8274 
- #8309 
- #8332 
- #8338 
- #8394 
- #8390 
- #8391
- #8393
- #8454 
- #8471 
- #8477
- #8567 
- #8566 
- #8570 


Needed:

- ~Add tests for using dependency groups without `[project]` table~ (this is not allowed yet)

Future:

- Add support for using dependency groups without a `[project]` table
- Refactor "dev dependencies" concept to "groups" internally
- Add automatic migration
- Add warnings for use of `tool.uv.dev-dependencies`
- Design for project-level tools using dependency groups

---

_Label `tracking` added by @zanieb on 2024-10-16 21:35_

---

_Comment by @charliermarsh on 2024-10-18 14:56_

Before I forget, we also need to ensure we error on:

```toml
[dependency-groups]
foo_bar = []
foo-bar = []
```

I.e., if they normalize to the same value. We already do this for `[[tool.uv.sources]]`, and it's in the PEP IIRC. I can do it at some point.

---

_Comment by @charliermarsh on 2024-10-18 18:03_

@zanieb -- I'm thinking we change `[package.dev-dependencies]` in the lockfile to `package.dependency-groups` (in a backwards-compatible way).

---

_Comment by @charliermarsh on 2024-10-20 23:02_

On "Error when groups do not exist" -- we don't currently do this for extras, I'm tempted to say we can ship without it.

---

_Comment by @zanieb on 2024-10-20 23:51_

It seems a little different with extras, my understand is that behavior exists to avoid breaking your project when a dependency version changes and the available extras change and we extend this behavior to local projects for consistency (which, arguably, we should not). I'd be pretty surprised if I used `--group` and we silently ignored that it does not exist. Notably, `--group` is always local and it should always be a mistake if you try to use a non-existent group.

(I see you did it anyway? 😄)

---

_Comment by @charliermarsh on 2024-10-22 15:55_

Current thinking is to ship as-is, but make `default-groups` configurable (and default to `["dev"]`).

---

_Comment by @Stealthii on 2024-10-24 20:23_

I'm likely doing something wrong here, but tested the latest Actions build against fc77c56, and seeing the helptext changes for `--extra` but not the new option `--group`:
```console
❯ uv --version
uv 0.4.25 (d4991d0ff 2024-10-23)
❯ uv pip install --help | grep -E -- '--(extra|group)'
      --extra <EXTRA>                        Include optional dependencies from the specified extra name; may be provided more than once
      --extra-index-url <EXTRA_INDEX_URL>          (Deprecated: use `--index` instead) Extra URLs of package indexes to use, in addition
❯ uv pip install -r pyproject.toml --group dev
error: unexpected argument '--group' found

  tip: to pass '--group' as a value, use '-- --group'

Usage: uv pip install <PACKAGE|--requirement <REQUIREMENT>|--editable <EDITABLE>>

For more information, try '--help'.
```

---

_Comment by @charliermarsh on 2024-10-24 20:24_

It's not supported in the `uv pip` interface yet, since `pip` itself doesn't support it, and we'd like to be informed by their API choices.

---

_Comment by @Stealthii on 2024-10-24 20:56_

Soon realised that, apologies!

https://github.com/pypa/pip/issues/12963 tracks PEP 735 for pip for anyone interested, and the proposal there also suggests [dependency-groups](https://pypi.org/project/dependency-groups/) as a current workaround:
`uv pip install $(python -m dependency_groups -f pyproject.toml dev)`

---

_Marked ready for review by @zanieb on 2024-10-25 18:16_

---

_Merged by @zanieb on 2024-10-25 18:27_

---

_Closed by @zanieb on 2024-10-25 18:27_

---

_Branch deleted on 2024-10-25 18:27_

---

_Comment by @czechnology on 2024-10-25 18:55_

I'm amazed at the speed of adding such features, big thanks! Great to see uv following the new PEP standards, even if it means eventually deprecating own configuration options.

---

_Comment by @phi-friday on 2024-10-26 15:12_

Since this is a new setting, I have a question.
Is there any way to reference this value from an existing `optional-dependencies`?
If not, is this value used for something completely different?
I know I can use `include-group` to reference another `dependency-group` from `dependency-group`, 
but I don't know how to reference `dependency-group` from `optional-dependencies`.

---

_Comment by @zanieb on 2024-10-26 16:06_

No you can't reference dependency groups from the `[project]` table. This was initially proposed in PEP 735 but was removed. You can see context about that [in the PEP](https://peps.python.org/pep-0735/#why-not-support-dependency-group-includes-in-project-dependencies-or-project-optional-dependencies) and [in the PEP discussion](https://discuss.python.org/t/pep-735-dependency-groups-in-pyproject-toml/39233/282).

You might be able to do the other way around and use `<project-name>[extra]` in the dependency-group?

---

_Comment by @phi-friday on 2024-10-26 16:08_

> No you can't reference dependency groups from the `[project]` table. This was initially proposed in PEP 735 but was removed. You can see context about that [in the PEP](https://peps.python.org/pep-0735/#why-not-support-dependency-group-includes-in-project-dependencies-or-project-optional-dependencies) and [in the PEP discussion](https://discuss.python.org/t/pep-735-dependency-groups-in-pyproject-toml/39233/282).
> 
> You might be able to do the other way around and use `<project-name>[extra]` in the dependency-group?

This made it clear how to use it.
Thank you.

---

_Comment by @Eyal-Shalev on 2024-10-28 09:34_

Can the dependency groups be used with the `uv pip install` command?

I tried adding `--group=dev` and it didn't work, and the docs were not that clear which commands can use this new flag.

_Note that I cannot use `uv sync` due to [uv sync fails to install remote private dependencies when poetry is involved #6244](https://github.com/astral-sh/uv/issues/6244)_

---

_Comment by @czechnology on 2024-10-28 09:48_

> Can the dependency groups be used with the uv pip install command?

Charlie [mentioned](https://x.com/charliermarsh/status/1849908091372044355) on Twi..X that it's not supported yet, they strive to keep the uv pip CLI similar to that of pip, but PIP does not support this yet.

> It’s not supported in the uv pip API yet — just uv run, uv sync, uv lock, uv add, etc. We’d like to use a CLI that looks similar to pip there, but pip doesn’t support it yet.

---

_Comment by @zanieb on 2024-10-28 13:57_

We're engaging in the discussion over at https://github.com/pypa/pip/issues/12963 — you can track that for upstream support.

---
