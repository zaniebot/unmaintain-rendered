```yaml
number: 10345
title: "Use `--config` in ecosystem checks"
type: issue
state: open
author: zanieb
labels:
  - help wanted
  - ci
assignees: []
created_at: 2024-03-11T21:27:13Z
updated_at: 2024-03-17T03:48:25Z
url: https://github.com/astral-sh/ruff/issues/10345
synced_at: 2026-01-12T15:54:50Z
```

# Use `--config` in ecosystem checks

---

_@zanieb_

Our ecosystem checks manually overwrite the `pyproject.toml` file to perform some overrides but now that we have `--config` support we should use that instead.



---

_Label `help wanted` added by @zanieb on 2024-03-11 21:27_

---

_Label `ci` added by @zanieb on 2024-03-11 21:27_

---

_Comment by @hoel-bagard on 2024-03-13 14:13_

I haven't touched at this part of the ruff codebase yet, but I would be interested in trying to fix this issue.

Would you have links to help me get started ?

---

_Comment by @zanieb on 2024-03-13 14:24_

Yeah basically I think we can replace this whole blurb

https://github.com/astral-sh/ruff/blob/c269c1a7063bb27e75dc81c2a598eab13aa8b754/python/ruff-ecosystem/ruff_ecosystem/projects.py#L89-L165

with appending a `--config` option at 

https://github.com/astral-sh/ruff/blob/c269c1a7063bb27e75dc81c2a598eab13aa8b754/python/ruff-ecosystem/ruff_ecosystem/projects.py#L204-L225

The idea is to provide the options directly instead of writing them to a file

---

_Comment by @hoel-bagard on 2024-03-17 01:51_

@zanieb I started looking into this, and I have a few things I would like to ask:

#### 1. How to unset / restore to default using `--config`
One of the overrides is `required-version = null`. Currently this just removes the entry from the toml file, however I don't know how to do that from the CLI with `--config` (since toml does not have `null`).

#### 2. Where to make the changes

In order to keep `CheckOptions` and `ConfigOverrides` separated, I'm thinking of adding the overrides to the list returned by `CheckOptions.to_ruff_args()`:

```python
    ruff_args = options.to_ruff_args()
    ruff_args.append(overrides)
```

But I could also do it inside of the `to_ruff_args` method by changing its signature to the one below.

```python

    def to_ruff_args(
        self,
        config_overrides: ConfigOverrides | None = None,
    ) -> list[str]:
```

What do you think about it ?

---

_Comment by @zanieb on 2024-03-17 03:48_

1. I'm not sure, I'd have to check. @AlexWaygood added that feature, I presume there's a way to unset keys.
2. I don't have a strong preference. `options.to_ruff_args() + overrides.to_ruff_args()` seems reasonable to start.

---
