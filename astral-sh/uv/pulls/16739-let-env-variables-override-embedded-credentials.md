```yaml
number: 16739
title: Let env variables override embedded credentials for private registries
type: pull_request
state: closed
author: MarthinusBosman
labels: []
assignees: []
base: main
head: main
created_at: 2025-11-14T16:53:51Z
updated_at: 2025-11-15T13:29:05Z
url: https://github.com/astral-sh/uv/pull/16739
synced_at: 2026-01-10T05:58:11Z
```

# Let env variables override embedded credentials for private registries

---

_Pull request opened by @MarthinusBosman on 2025-11-14 16:53_


This PR fixes an issue where environment variable credentials (`UV_INDEX_*_USERNAME` and `UV_INDEX_*_PASSWORD`) were not properly overriding credentials embedded in index URLs.

When an index URL contained embedded credentials (e.g., `https://VssSessionToken@pkgs.dev.azure.com/.../simple/`) and environment variables were set to override them, uv would fail to authenticate properly. This was particularly problematic for Azure Artifacts users who wanted to use `VssSessionToken@` in the URL for local keyring-based authentication while overriding with explicit credentials in CI/CD pipelines via environment variables.

## Summary

This PR implements a two-part fix to ensure environment variable credentials always override URL-embedded credentials for named indexes:

1. **Credential caching improvement** (`index_url.rs`): When environment credentials are provided for a named index, the embedded URL credentials are now stripped before caching. This ensures environment-provided credentials are cached against a clean URL without the embedded username/password.

2. **Auth middleware fallback** (`middleware.rs`): Added a username-agnostic credential lookup fallback in the authentication middleware. When a request contains a username in the URL but no cached credentials match that username, the middleware now falls back to looking up credentials without considering the username. This allows environment-provided credentials to be used even when the URL contains a different username.

These changes enable the following workflow:
- Developers can use `https://VssSessionToken@pkgs.dev.azure.com/.../simple/` in `pyproject.toml` for local keyring authentication
- CI/CD pipelines can override with explicit credentials via `UV_INDEX_PRIVATE_REGISTRY_USERNAME=dummy` and `UV_INDEX_PRIVATE_REGISTRY_PASSWORD=$(System.AccessToken)`
- No changes to `pyproject.toml` are needed between environments

## Test Plan

- Added `env_credentials_override_url_credentials()` test in `pip_compile.rs` that verifies:
  - Wrong credentials in URL cause authentication to fail (401 Unauthorized)
  - Environment variables successfully override the URL credentials and allow authentication
  
- Added `lock_env_credentials_override_url_credentials()` test in `lock.rs` with the same verification

- Updated documentation in `alternative-indexes.md` to clarify that environment variables override URL credentials

Both tests pass successfully, confirming that environment credentials now properly override URL-embedded credentials for named indexes.

---

_Converted to draft by @MarthinusBosman on 2025-11-14 17:06_

---

_Marked ready for review by @MarthinusBosman on 2025-11-14 17:09_

---

_Closed by @MarthinusBosman on 2025-11-15 13:17_

---

_Comment by @MarthinusBosman on 2025-11-15 13:17_

Messed up the fork, not dealing with it now

---
