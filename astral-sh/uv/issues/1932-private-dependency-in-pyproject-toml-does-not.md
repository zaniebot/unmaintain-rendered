```yaml
number: 1932
title: Private dependency in pyproject.toml does not resolve
type: issue
state: closed
author: meridionaljet
labels: []
assignees: []
created_at: 2024-02-23T19:07:44Z
updated_at: 2024-06-07T22:42:33Z
url: https://github.com/astral-sh/uv/issues/1932
synced_at: 2026-01-10T05:31:36Z
```

# Private dependency in pyproject.toml does not resolve

---

_Issue opened by @meridionaljet on 2024-02-23 19:07_

Platform: Linux
uv version: 0.1.10

I can't seem to get `uv` to resolve private dependencies listed in a package's `pyproject.toml` file. For example:

```bash
$ uv pip install '<package> @ git+https://<username>@github.com/<organization>/<package>.git'
 Updated https://<username>@github.com/<organization>/<package>.git (3386a82)       
error: Package `<dependency>` attempted to resolve via URL: git+https://<username>@github.com/<organization>/<dependency>.git. 
URL dependencies must be expressed as direct requirements or constraints. Consider adding `<dependency> @ git+https://<username>@github.com/<organization>/<dependency>.git` to your dependencies or constraints file.
```

However, this is `package`'s `pyproject.toml`, which does indeed list the dependency in the format requested by `uv`:

```toml
[project]
name = 'package'
dependencies = [
  '...',
  '...',
  '<dependency> @ git+https://<username>@github.com/<organization>/<dependency>.git',
]

[build-system]
requires = ['setuptools', 'wheel']
```

I confirmed that the `pyproject.toml` content in the `uv` cache directory is correct.

---

_Comment by @charliermarsh on 2024-02-23 19:10_

I think you'd need to include `'<dependency> @ git+https://<username>@github.com/<organization>/<dependency>.git'` in the install command, like:

```
uv pip install \
  '<package> @ git+https://<username>@github.com/<organization>/<package>.git' \
  '<dependency> @ git+https://<username>@github.com/<organization>/<dependency>.git'
```

We don't allow transitive dependencies to introduce additional URLs right now.

---

_Comment by @meridionaljet on 2024-02-23 19:18_

> I think you'd need to include `'<dependency> @ git+https://<username>@github.com/<organization>/<dependency>.git'` in the install command, like:
> 
> ```
> uv pip install \
>   '<package> @ git+https://<username>@github.com/<organization>/<package>.git' \
>   '<dependency> @ git+https://<username>@github.com/<organization>/<dependency>.git'
> ```
> 
> We don't allow transitive dependencies to introduce additional URLs right now.

I see. Is it your intent to facilitate this in the future? Current behavior would make private Github package ecosystems difficult to deal with, and it's obviously a case that `pip` can handle.

---

_Comment by @charliermarsh on 2024-02-23 19:38_

Possibly... It's both a security issue (arbitrary dependencies in your tree can now introduce packages from arbitrary locations) and a correctness issue. For example:

- You depend on `package_a==1.0.0`
- You depend on `package_b @ some_url`
- `package_b @ some_url` depends on `package_a @ some_other_url` (where `package_a @ some_other_url` happens to build to a version called 1.0.0)

What should happen?


---

_Comment by @T-256 on 2024-02-23 21:25_

> What should happen?

IMO, then in that scenario, uv should fail with dependency conflict error.
But when that scenario didn't happen, I think it's valid to handle `package_b @ some_url` in lock file.
is it makes sense? WDYT?

---

_Comment by @simoncozens on 2024-02-27 13:17_

"⚖️ Drop-in replacement for common pip, pip-tools, and virtualenv commands."

> What should happen?

Whatever pip does.

---

_Comment by @charliermarsh on 2024-03-04 00:13_

Tracking here: https://github.com/astral-sh/uv/issues/1808

---

_Closed by @charliermarsh on 2024-03-04 00:13_

---

_Comment by @ringohoffman on 2024-06-07 22:40_

@charliermarsh it seems like you closed this as a duplicate of #1808, which was closed by #2684, but the behavior reported in this issue still remains in `v0.2.9`. Is the intention still to require changes to the install command in order to allow URL installations? I too am finding the divergent behavior from `pip` to be a surprise, though you list reasons for this decision.

As an anecdote, I am having to track the latest commit for a repo that won't cut a release for some time. I have a few different `uv pip install` commands throughout my build process that I find undesirable to have to update in order to allow for this URL installation.

---

_Comment by @charliermarsh on 2024-06-07 22:42_

Yeah, we support transitive URL dependencies now. Please feel free to file a new issue with a minimal reproduction if you're having trouble.

---
