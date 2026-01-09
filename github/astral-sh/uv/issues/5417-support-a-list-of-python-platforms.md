---
number: 5417
title: Support a list of python platforms
type: issue
state: open
author: JonZeolla
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-07-24T16:51:46Z
updated_at: 2024-07-25T10:58:45Z
url: https://github.com/astral-sh/uv/issues/5417
synced_at: 2026-01-07T13:12:17-06:00
---

# Support a list of python platforms

---

_Issue opened by @JonZeolla on 2024-07-24 16:51_

To fit the space between `--python-platform` supporting a single platform at a time, and `--universal` which supports all platforms, I suggest allowing a list of platforms (perhaps a rename to `--python-platforms`) so lockfiles can be created that support only the supported/required runtime platforms. This would also reduce the surface area of issues, and speed up dependency resolution.

Examples (modifications to what I see in the docs today):

- `uv pip compile --python-platforms macos,linux requirements.in`
- ```toml
  [tool.uv.pip]
  python-platforms = "x86_64-unknown-linux-gnu,aaarch64-apple-darwin"
  ```

Sorry if some of the `uv` details above aren't accurate; I am a `rye` user but this appears to be a `uv` feature that `rye` could then adopt.

---

_Comment by @charliermarsh on 2024-07-24 16:54_

@konstin - I wonder if there's a way that we could leverage the "record fork markers" to support something like this (e.g., a list of markers as input, resolve both, reconcile...). Wishlist though.

---

_Label `enhancement` added by @charliermarsh on 2024-07-24 16:54_

---

_Label `wish` added by @charliermarsh on 2024-07-24 16:54_

---

_Comment by @konstin on 2024-07-24 17:26_

With the branch i'm working on atm we get the ability to feed to markers we want to resolve for in manually. We'd need to wire it up to `pyproject.toml` configuration, deactivate any full-universe checks, add a new check when installing that we're in one of the supported envs and write docs explaining how solving for a subset of environments works.

> This would also reduce the surface area of issues, and speed up dependency resolution.

Are you having a case where the universal resolution is noticeable slower than a single-platform one? If so and you have an example you can share it would be great, then we can optimize the universal resolver better. 


---

_Comment by @JonZeolla on 2024-07-25 10:58_

@konstin we currently are running into some issues which we believe are at least somewhat due to the version of `uv` bundled in `rye` so we're patiently waiting for https://github.com/astral-sh/rye/pull/1269 and then a `rye` release before further testing.

But we are using `universal = true` because our devs use Linux and Mac, and have a current issue with being unable to lock, and the error points to an issue with the Windows platform.

Some snippets from our `pyproject.toml`:

```toml
[project]
...
dependencies = [
    "langchain-text-splitters",
    "psycopg2-binary",
    "pypandoc",
    "pyyaml",
    "sqlalchemy",
    "<A private package>"
]
requires-python = ">= 3.12"

[tool.rye]
universal = true
managed = true
generate-hashes = true
dev-dependencies = [
    "coverage",
    "pytest",
    "pytest-cov",
    "pytest-mock",
    "refurb",
    "<A private package>"
]
```

---
