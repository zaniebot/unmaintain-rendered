---
number: 15978
title: "Feature Request: Persist Environment Variables in pyproject.toml for Git Dependencies"
type: issue
state: open
author: luigibrb
labels:
  - enhancement
assignees: []
created_at: 2025-09-22T10:45:22Z
updated_at: 2025-09-22T11:19:42Z
url: https://github.com/astral-sh/uv/issues/15978
synced_at: 2026-01-10T01:26:01Z
---

# Feature Request: Persist Environment Variables in pyproject.toml for Git Dependencies

---

_Issue opened by @luigibrb on 2025-09-22 10:45_

### Summary

When adding a private Git dependency with uv add using environment variables for authentication, the environment variables are not persisted in the `pyproject.toml` file.
### Problem Description
Our current workflow involves using a Gitlab deploy token with environment variables to avoid storing secrets in the repository:
```
export GITLAB_DEPLOY_USER=<your_gitlab_user>
export GITLAB_DEPLOY_TOKEN=<your_gitlab_token>

uv add "git+https://${GITLAB_DEPLOY_USER}:${GITLAB_DEPLOY_TOKEN}@<private_gitlab_address>/<repo_path>/<repo-name>.git@main"
```


The uv add command successfully adds the dependency and resolves it. However, the resulting entry in `pyproject.toml` looks like this:
```
[tool.uv.sources]
<dependency_name> = { git = "https://<private_gitlab_address>/<repo_path>/<repo-name>.git", rev = "main" }
```


As you can see, the `${GITLAB_DEPLOY_USER}:${GITLAB_DEPLOY_TOKEN}` part is removed. This means that if another user clones the repository and tries to install dependencies with `uv install`, they will get an authentication error unless they manually edit their local `pyproject.toml` file. This defeats the purpose of using environment variables for secure, portable configurations.

### Steps to Reproduce
Set up environment variables for a private Git repository:
```
export GITLAB_DEPLOY_USER="oauth2"
export GITLAB_DEPLOY_TOKEN="glpat-<your-token>"
```


Run uv add with the environment variables in the URL:
```
uv add "git+https://${GITLAB_DEPLOY_USER}:${GITLAB_DEPLOY_TOKEN}@<private_gitlab_address>/<repo_path>/<repo-name>.git@main"
```


Check the `pyproject.toml` file. The URL will be missing the authentication part, instead of retaining the environment variables.

### Proposed Solution
I propose that `uv` should recognize and retain the environment variable syntax `(${VAR_NAME})` when adding a git-based dependency to the `pyproject.toml` file.
The desired output in `pyproject.toml` should be:
```
[tool.uv.sources]
<dependency_name> = { git = "https://${GITLAB_DEPLOY_USER}:${GITLAB_DEPLOY_TOKEN}@<private_gitlab_address>/<repo_path>/<repo-name>.git", rev = "main" }
```


This would allow teams to securely share projects without exposing secrets or requiring manual changes to the project's configuration file.

---

_Label `enhancement` added by @luigibrb on 2025-09-22 10:45_

---
