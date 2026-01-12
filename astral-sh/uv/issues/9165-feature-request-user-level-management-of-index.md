```yaml
number: 9165
title: "[FEATURE REQUEST] User-level management of index credentials via uv"
type: issue
state: open
author: tunayokumus
labels:
  - needs-design
assignees: []
created_at: 2024-11-16T10:55:56Z
updated_at: 2024-12-12T06:57:52Z
url: https://github.com/astral-sh/uv/issues/9165
synced_at: 2026-01-12T15:59:43Z
```

# [FEATURE REQUEST] User-level management of index credentials via uv

---

_@tunayokumus_

I read the documentation on providing credentials for private registries. (https://docs.astral.sh/uv/configuration/indexes/#providing-credentials).

This is generally fine to start with. However, I wonder if a further development of this is planned. E.g. similar to how poetry is handling these(https://python-poetry.org/docs/repositories/#private-repository-example):

`poetry config http-basic.foo <username> <password> `

would there be something like:

1. `uv config uv_index.internal-proxy <username>`
2. And in the next step a temporary text editor opens and the user enters the password  (which is a more secure practice than entering in the cli, due to logging)

---

_Renamed from "[FEATURE REQUEST] User-level management of index credentials" to "[FEATURE REQUEST] User-level management of index credentials via uv" by @tunayokumus on 2024-11-16 10:56_

---

_Label `needs-design` added by @charliermarsh on 2024-11-18 01:15_

---

_Comment by @stoney95 on 2024-12-08 01:38_

Topic is also discussed in https://github.com/astral-sh/uv/issues/8810

---

_Comment by @farndt on 2024-12-12 06:57_

> I read the documentation on providing credentials for private registries. (https://docs.astral.sh/uv/configuration/indexes/#providing-credentials).
> 
> This is generally fine to start with. However, I wonder if a further development of this is planned. E.g. similar to how poetry is handling these(https://python-poetry.org/docs/repositories/#private-repository-example):
> 
> `poetry config http-basic.foo <username> <password> `
> 
> would there be something like:
> 
> 1. `uv config uv_index.internal-proxy <username>`
> 2. And in the next step a temporary text editor opens and the user enters the password  (which is a more secure practice than entering in the cli, due to logging)

For pair programming or shared sessions, I don't want to enter my password in plain text in editor windows. Why not use the Credential Manager built into the operating system or a third-party keystore? Login information is stored encrypted, the password entry is hidden and the login information can also be backed up and restored (depending on the credential manager). This would be much more secure than storing credentials in plain text in personal toml files.

I would like to see a way to configure private registrations in pyproject.toml without username or password so that they can be shared between different teams. Since now uv only takes:

```toml
[[tool.uv.index]]
name = "private-python"
url = "https://username:password@artifactory.company.com/*/simple"
```

and
```toml
[[tool.uv.index]]
name = "private-python"
url = "https://username@artifactory.company.com/*/simple"
```
to work with keyring.

My preferred solution I described here: https://github.com/astral-sh/uv/issues/8810#issuecomment-2521703207

Same ability would be nice to have for upload repositorys.
This would really enable to use uv in company teams with on prem indexes and repos.




---
