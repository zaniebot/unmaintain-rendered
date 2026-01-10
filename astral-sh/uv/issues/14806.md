```yaml
number: 14806
title: uv tool upgrade does not authenticate against GitLab private pypi package registry
type: issue
state: closed
author: lgatellier
labels:
  - bug
assignees: []
created_at: 2025-07-22T08:42:39Z
updated_at: 2025-08-05T15:17:06Z
url: https://github.com/astral-sh/uv/issues/14806
synced_at: 2026-01-10T03:32:45Z
```

# uv tool upgrade does not authenticate against GitLab private pypi package registry

---

_Issue opened by @lgatellier on 2025-07-22 08:42_

### Summary

Hi,

I have a CLI package `myapp` published on a self-hosted GitLab Package Registry. The GitLab project hosting the package has **Private** visibility : you must authenticate to access the project's package registry.

Here is my `uv.toml` config file :
```toml
native-tls = true

[[index]]
url = "https://gitlab-deploy-token:my-gitlab-deploy-token-value@gitlab.mycompany.com/api/v4/projects/1234/packages/pypi/simple"
ignore-error-codes = [401] # Necessary for uv to detect new pacakge versions from my GitLab package repository

[[index]]
url = "https://mirror.mycompany.com/repository/pypi.python.org/simple"
default = true
```

When I use `uv tool install myapp` command, it properly installs the `myapp` package.

When I try to upgrade the tool to a newly released version with `uv tool upgrade myapp`, I get this error :
> error: Failed to upgrade myapp
  Caused by: Failed to fetch: `https://gitlab.mycompany.com/api/v4/projects/1234/packages/pypi/files/e09e2e982ff5314c738e8f2478f6e7a656210b829bac89fc38cf127c96aee2d9/myapp-1.16.0-py3-none-any.whl`
  Caused by: HTTP status client error (401 Unauthorized) for url (https://gitlab.mycompany.com/api/v4/projects/1234/packages/pypi/files/e09e2e982ff5314c738e8f2478f6e7a656210b829bac89fc38cf127c96aee2d9/myapp-1.16.0-py3-none-any.whl)

What I tried :
- `curl` this URL while being unauthenticated : I get the same 401 error
- `curl -u username:my-gitlab-deploy-token` (with HTTP Basic auth) : I'm allowed to download the **whl** file.
- Configure the GitLab project's package registry to be unauthenticated : everything works fine, uv upgrades the package.

The full `uv -vv tool upgrade myapp` log is in the attached [uv-tool-upgrade.log](https://github.com/user-attachments/files/21363765/uv-tool-upgrade.log).

I've read the docs about tools and index auth, but didn't find anything which could solve my problem.

Did I misconfigured something ? Or is this a bug ?

Thanks !

### Platform

Windows 11 x86_64 WSL Ubuntu 24.04 LTS

### Version

0.8.0

### Python version

3.13.2

---

_Label `bug` added by @lgatellier on 2025-07-22 08:42_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-07-23 02:45_

---

_Comment by @charliermarsh on 2025-07-23 02:48_

Hmm, okay. The first problem I see is that we're redacting credentials in the receipt (we should be removing them entirely):

```toml
[tool]
requirements = [{ name = "flask" }]
entrypoints = [
    { name = "flask", install-path = "/Users/crmarsh/.local/bin/flask" },
]

[tool.options]
index = [{ url = "https://public:****@pypi-proxy.fly.dev/basic-auth/simple", explicit = false, default = true, format = "simple", authenticate = "auto" }]
```

The second problem is that we should be transferring the credentials from your `uv.toml` file.


---

_Comment by @charliermarsh on 2025-07-23 03:03_

I believe I see the issues here. It is indeed a bug (or maybe two bugs), sorry about that.

---

_Comment by @lgatellier on 2025-07-23 09:10_

Don't worry, I'll patiently wait for the bug fix(es) 😉

---

_Closed by @charliermarsh on 2025-07-23 21:23_

---
