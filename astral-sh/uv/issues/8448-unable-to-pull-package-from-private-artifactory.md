```yaml
number: 8448
title: unable to pull package from private artifactory registry
type: issue
state: closed
author: danieleades
labels:
  - bug
assignees: []
created_at: 2024-10-22T12:46:19Z
updated_at: 2024-10-22T13:44:16Z
url: https://github.com/astral-sh/uv/issues/8448
synced_at: 2026-01-12T15:59:26Z
```

# unable to pull package from private artifactory registry

---

_@danieleades_

see discussion here - https://github.com/astral-sh/uv/pull/7481#issuecomment-2428423927

> You can use an API key in the PASSWORD variable — it works for anything that supports HTTP Basic Authentication.

thanks for the support!

i'm trying this now and seeing issues. For context, i have a project which has one dependency from a private artifactory registry.

with or without credentials exported to the environment i see the following output:

```
uv sync
  × No solution found when resolving dependencies:
  ╰─▶ Because my-private-package was not found in the package registry and your project depends on my-private-package>=2.0.0,<2.1.dev0, we can conclude that your project's requirements are unsatisfiable.

      hint: my-private-package was requested with a pre-release marker (e.g., sb-qag-sphinx>=2.0.0,<2.1.dev0), but pre-releases weren't enabled (try: `--prerelease=allow`)
```

the dependencies are specified as follows:

```toml
dependencies = [
    "my-private-package~=2.0.0",
    # ...
]

[tool.uv.sources]
my-private-package = {index = "mycompany-py-release"}

[[tool.uv.index]]
name = "artifactory-pypi"
url = "https://artifactory.mycompany.com/artifactory/api/pypi/pypi/simple"
default = true

[[tool.uv.index]]
# Requires:
name = "mycompany-py-release"
url = "https://artifactory.mycompany.com/artifactory/api/pypi/my-private-package/simple"
explicit = true
```

with

```sh
export UV_INDEX_MYCOMPANY_PY_RELEASE_USERNAME={username}
export UV_INDEX_MYCOMPANY_PY_RELEASE_PASSWORD={artifactory token}
```

---

_Comment by @charliermarsh on 2024-10-22 13:03_

Can you try using a name without dashes in it? We might not be normalizing those correctly.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-22 13:03_

---

_Label `bug` added by @charliermarsh on 2024-10-22 13:03_

---

_Comment by @danieleades on 2024-10-22 13:36_

> Can you try using a name without dashes in it? We might not be normalizing those correctly.

I wondered about that. There's no documentation on how the normalisation behaves.

Unfortunately all the registries I can access easily for testing have dashes

---

_Closed by @charliermarsh on 2024-10-22 13:37_

---

_Closed by @charliermarsh on 2024-10-22 13:37_

---

_Comment by @charliermarsh on 2024-10-22 13:38_

It's fixed in the next release.

---

_Comment by @danieleades on 2024-10-22 13:42_

> It's fixed in the next release.

Outstanding.

Will the normalisation behaviour be described in the docs?

---

_Comment by @charliermarsh on 2024-10-22 13:44_

Yeah, but for reference, we uppercase the index name and replace any non-alphanumeric characters with underscores.

---
