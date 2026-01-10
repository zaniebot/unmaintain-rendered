---
number: 13886
title: Improve documentation for Keyring Credential Provider
type: issue
state: open
author: da1910
labels:
  - question
assignees: []
created_at: 2025-06-06T14:46:39Z
updated_at: 2025-07-24T16:12:08Z
url: https://github.com/astral-sh/uv/issues/13886
synced_at: 2026-01-10T01:25:39Z
---

# Improve documentation for Keyring Credential Provider

---

_Issue opened by @da1910 on 2025-06-06 14:46_

### Summary

The documentation for the keyring provider does indicate how to use it for some cases, but provides examples of package feeds that do not verify usernames, or use hard coded usernames.

In a team development setting it is not desirable to hard code usernames into the pyproject.toml file, and if we can avoid environment variables that's definitely preferable.

I'd suggest an example up front of using a generic index (I use artifactory which does this) that requires a valid username and token, along with an idea of what credentials must be stored in keyring to allow authentication. Right now the only way I could work out what was required was to inspect the logs with `uv sync -vv` and see what it was trying to fetch from the keyring backend.

This is complicated by the fact that if you provide `https://user@package.feed.com/pypi/simple/` as an index url then uv will look for `package.feed.com` in keyring with the username `user`, however if you use the (undocumented?) support for `--mode creds` to authenticate the url `https://package.feed.com/pypi/simple/` then uv looks for `https://package.feed.com/pypi/simple/` in keyring.

---

_Label `enhancement` added by @da1910 on 2025-06-06 14:46_

---

_Comment by @zanieb on 2025-06-06 17:41_

See https://docs.astral.sh/uv/configuration/indexes/#using-credential-providers — we'll use `--mode creds` if you set `authenticate = "always"`.

---

_Label `enhancement` removed by @zanieb on 2025-06-06 17:41_

---

_Label `question` added by @zanieb on 2025-06-06 17:41_

---

_Comment by @da1910 on 2025-06-07 12:05_

Indeed, I discovered this, but it's not explained anywhere that I could see?

Neither is exactly what secrets need to be stored in keyring for each mode, since it appears to be hard coded to look for specific secret names?

I can draft some text that would have helped me if you'd be open to a documentation enhancement? This could either in the form of "here's how to use jfrog artifactory", or more generically "here's how to store credentials for a generic mirror that requires a username and password/token"

---

_Comment by @da1910 on 2025-07-24 16:12_

It seems as though a recent fix for the authentication code changed the behaviour again, preventing redirects being followed, and breaking our workflow - the azure pypi provider redirects from `pkgs.dev.azure.com/org_name` to `pkgs.dev.azure.com/org_guid` which prevents authentication.

---
