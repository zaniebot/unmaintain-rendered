---
number: 14344
title: Version 0.7.16 invalidates lock files from <=0.7.15 due to an added trailing slash
type: issue
state: closed
author: midopa
labels:
  - bug
assignees: []
created_at: 2025-06-29T06:11:05Z
updated_at: 2025-06-29T19:28:39Z
url: https://github.com/astral-sh/uv/issues/14344
synced_at: 2026-01-10T01:25:43Z
---

# Version 0.7.16 invalidates lock files from <=0.7.15 due to an added trailing slash

---

_Issue opened by @midopa on 2025-06-29 06:11_

### Summary

Hi all,

A container is failing to start because uv tries to reinstall the deps, specifically for private packages. And I don't pass in private repo tokens to the container runtime env vars because those shouldn't be needed. My project's Docker setup is such that if deps or the lock file changes, a new image should be built (and the tokens are provided as secrets during the build).

It turns out uv considers the lock file invalid. I see this when I run 'uv -v tree' with the image:
> DEBUG Ignoring existing lockfile due to mismatched requirements for: `my-obfuscated-project==0.1.0`

The debug logs after that include the requested and existing dependencies. I copied each, formatted, and did a diff. These are the only lines that differ (red is from 'requested' and green is from 'existing'):

```diff
    }, Requirement { name: PackageName("my-private-package"), extras: [], groups: [], marker: true, source: Registry { specifier: VersionSpecifiers([VersionSpecifier { operator: EqualStar, version: "1"
                }
-           ]), index: Some(IndexMetadata { url: Url(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my-aws-project.d.codeartifact.us-east-1.amazonaws.com")), port: None, path: "/pypi/pypi/simple/", query: None, fragment: None
+           ]), index: Some(IndexMetadata { url: Url(VerbatimUrl { url: DisplaySafeUrl { scheme: "https", cannot_be_a_base: false, username: "", password: None, host: Some(Domain("my-aws-project.d.codeartifact.us-east-1.amazonaws.com")), port: None, path: "/pypi/pypi/simple", query: None, fragment: None
                    }, given: None
                }), format: Simple
            }), conflict: None
        }, origin: None
    },
```

Specifically:
```diff
- path: "/pypi/pypi/simple/"
+ path: "/pypi/pypi/simple"
```

It seems like something in uv is adding a trailing slash and considering that to be enough to invalidate the entire lock file and venv. While it makes sense to compare the lock file, a change like this is trivial and should be ignored. Ideally the trailing slash wouldn't be added at all and the lock file remains consistent across uv versions, but minor deltas like this will always happen (even if uv were to hit 1.x and consider itself 'stable').

I have a uv.lock file generated from 0.7.14.
In the Docker image, uv is at 0.7.16.
I tested 0.7.14 and 0.7.15 in the Docker image and they work fine, so the regression happened in 0.7.16.


### Platform

macOS Sequoia 15.5 (Darwin 24.5.0 arm64)

### Version

0.7.14 and 0.7.16

### Python version

3.10.18

---

_Label `bug` added by @midopa on 2025-06-29 06:11_

---

_Comment by @charliermarsh on 2025-06-29 13:00_

I'll look at this, I thought we intentionally added logic to consider these equivalent (i.e., it's not intentional that these are considered invalid).

---

_Assigned to @charliermarsh by @charliermarsh on 2025-06-29 13:03_

---

_Comment by @charliermarsh on 2025-06-29 13:07_

Do you have something like this in your `pyproject.toml`? Like an explicit index definition of PyPI, with the trailing slash? (If not, can you include your `pyproject.toml` or a minimal subset of it?)

```toml
[project]
name = "foo"
version = "0.1.0"
description = "Add your description here"
readme = "README.md"
requires-python = ">=3.13.2"
dependencies = [
    "anyio>=4.9.0",
]

[[tool.uv.index]]
url = "https://pypi.org/simple/"
name = "pypi"

[tool.uv.sources]
anyio = { index = "pypi" }
```

---

_Comment by @BjoernPetersen on 2025-06-29 13:22_

I'm not sure whether the issue I'm encountering is the same, but it seems related to me.

Starting with uv 0.7.16, `uv sync --locked` fails with this error message: 

```
error: The lockfile at `uv.lock` needs to be updated, but `--locked` was provided. To update the lockfile, run `uv lock`.
```

Even deleting the uv.lock file and running `uv lock` to create a fresh lockfile, `uv lock --check` will give the above error message.

---

I do have an explicit index definition in my `pyproject.toml`, and the error occurs whether I add a trailing slash to its URL or not. The issue only occurs in projects with this explicit index configuration.

```
[[tool.uv.index]]
name = "internal"
url = "https://pypi.example.com"
explicit = true
```

---

_Comment by @charliermarsh on 2025-06-29 13:23_

So it occurs regardless of whether you delete the lockfile or not _and_ regardless of whether you add a trailing slash to the URL or not?

---

_Comment by @charliermarsh on 2025-06-29 13:23_

I think https://github.com/astral-sh/uv/pull/14346 should fix this but it'd be helpful to have a minimal reproducible `pyproject.toml` if you can share one.

---

_Comment by @BjoernPetersen on 2025-06-29 13:30_

> So it occurs regardless of whether you delete the lockfile or not and regardless of whether you add a trailing slash to the URL or not?

Yes. Even a fresh lockfile created by uv 0.7.16 fails its lockfile check for me.

---

I tinkered a bit just now and realized that adding `/simple` or `/simple/` to the URL fixes it. I guess I should've done that anyway, so I'll do that everywhere now. Still, the uv patch release broke previously working behavior.

---

_Comment by @charliermarsh on 2025-06-29 13:35_

Okay, thanks. I don't fully understand your comment and it'd be super helpful to see an example, but yes, this is a bug and is marked as such.


---

_Comment by @BjoernPetersen on 2025-06-29 13:50_

Sorry for the confusion. It's kind of hard to create a minimal reproducible pyproject.toml because it would require access to the internal pypi registry. It would look kind of like this:

```toml
[project]
requires-python = "==3.13.*"
name = "example-project"
version = "1.0.0"
description = ""

dependencies = [
  "internal-package==1.0.0"
]

[[tool.uv.index]]
name = "internal"
url = "https://pypi.example.com"
explicit = true

[tool.uv.sources]
internal-package = { index = "internal" }
```

---

Focussing on the index configuration, I've tried a few variants with uv 0.7.16. For each of these, I've deleted the lockfile, ran `uv lock` and then checked using `uv lock --check`:

### Working Examples

```toml
[[tool.uv.index]]
name = "internal"
url = "https://pypi.example.com/simple"
explicit = true
```

```toml
[[tool.uv.index]]
name = "internal"
url = "https://pypi.example.com/simple/"
explicit = true
```

### Broken Examples

```toml
[[tool.uv.index]]
name = "internal"
url = "https://pypi.example.com/"
explicit = true
```

```toml
[[tool.uv.index]]
name = "internal"
url = "https://pypi.example.com"
explicit = true
```


---

_Comment by @midopa on 2025-06-29 13:56_

In my case, my pyproject.toml distills down to this:

```toml
[project]
name = 'my-project'
version = '0.1.0'
requires-python = "~=3.10.0"

dependencies = [
  "requests~=2.31.0",
  "some-private-package~=1.0",
]

[tool.uv.sources]
some-private-package = { index = "codeartifact" }

[[tool.uv.index]]
name = "pypi"
url = "https://pypi.org/simple/"

[[tool.uv.index]]
name = "codeartifact"
url = "https://my-aws-project.d.codeartifact.us-east-1.amazonaws.com/pypi/pypi/simple/"
```

I set it up this way bc I want uv to use the main public index for public packages (for now) and only use the private one when it's explicitly told to do so for certain packages.

When I remove the trailing slash on the codeartifact index, update my local lock file, and rebuild, uv 0.7.16 is happy again inside the container.

---

_Comment by @charliermarsh on 2025-06-29 13:58_

Thank you both. I expect the change I merged to fix the cases with trailing slashes on the index URLs. In @BjoernPetersen case, I'm less certain because I wouldn't have expected the absence or presence of `/simple` to matter...

---

_Comment by @charliermarsh on 2025-06-29 14:24_

Do you mind testing 0.7.17? (Out now on PyPI.)

---

_Comment by @midopa on 2025-06-29 14:36_

Works on 0.7.17 even with the trailing slash and lock file generated locally from 0.7.14! Thanks!

---

_Comment by @BjoernPetersen on 2025-06-29 14:49_

The behavior for my issue didn't change with 0.7.17. `uv lock --check` still complains if I leave off the `/simple` URL path.

---

_Comment by @zanieb on 2025-06-29 15:05_

I've reproduced, looking into that next

---

_Referenced in [astral-sh/uv#14348](../../astral-sh/uv/pulls/14348.md) on 2025-06-29 15:24_

---

_Closed by @charliermarsh on 2025-06-29 19:24_

---

_Comment by @charliermarsh on 2025-06-29 19:24_

The second case should be closed by #14349.

---

_Comment by @midopa on 2025-06-29 19:28_

Thanks for the fast fix!

---
