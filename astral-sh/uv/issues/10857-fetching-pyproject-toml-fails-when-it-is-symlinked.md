```yaml
number: 10857
title: Fetching pyproject.toml fails when it is symlinked
type: issue
state: closed
author: quis
labels:
  - bug
assignees: []
created_at: 2025-01-22T14:17:25Z
updated_at: 2025-01-23T09:48:09Z
url: https://github.com/astral-sh/uv/issues/10857
synced_at: 2026-01-12T16:00:22Z
```

# Fetching pyproject.toml fails when it is symlinked

---

_@quis_

### Summary

If a dependency’s `pyproject.toml` file is not in the root folder and its `./pyproject.toml` is a symlink to the actual location of the file then `uv pip install` fails.

## To reproduce

Run the following in any environment:
```bash
uv pip install "notifications-utils @ git+https://github.com/alphagov/notifications-utils.git@206f008ef017f8b5ec9b870df04bf35780b7717b"
```

## Error seen

```
  × Failed to download and build `notifications-utils @
  │ git+https://github.com/alphagov/notifications-utils.git@206f008ef017f8b5ec9b870df04bf35780b7717b`
  ├─▶ Failed to parse metadata from built wheel
  ├─▶ Invalid `pyproject.toml`
  ╰─▶ TOML parse error at line 1, column 20
        |
      1 | notifications_utils/version_tools/pyproject.toml
        |                    ^
      expected `.`, `=`
```

## `uv --version`

```
uv 0.5.22 (4574ced37 2025-01-21)
```

## Suspected cause 

Some Github API endpoints see the symlink and do a redirect, for example 

```bash
curl "https://api.github.com/repos/alphagov/notifications-utils/contents/pyproject.toml?206f008ef017f8b5ec9b870df04bf35780b7717b"
```

returns this (file contents are correct once decoded):
```json
{
  "name": "pyproject.toml",
  "path": "pyproject.toml",
  "sha": "798ceb3a471794cb75a8a937d985e82037900a51",
  "size": 735,
  "url": "https://api.github.com/repos/alphagov/notifications-utils/contents/pyproject.toml?ref=main",
  "html_url": "https://github.com/alphagov/notifications-utils/blob/main/pyproject.toml",
  "git_url": "https://api.github.com/repos/alphagov/notifications-utils/git/blobs/798ceb3a471794cb75a8a937d985e82037900a51",
  "download_url": "https://raw.githubusercontent.com/alphagov/notifications-utils/main/pyproject.toml",
  "type": "file",
  "content": "W3Rvb2wuYmxhY2tdCmxpbmUtbGVuZ3RoID0gMTIwCgpbdG9vbC5ydWZmXQps\naW5lLWxlbmd0aCA9IDEyMAoKdGFyZ2V0LXZlcnNpb24gPSAicHkzMTEiCgps\naW50LnNlbGVjdCA9IFsKICAgICJFIiwgICMgcHljb2Rlc3R5bGUKICAgICJX\nIiwgICMgcHljb2Rlc3R5bGUKICAgICJGIiwgICMgcHlmbGFrZXMKICAgICJJ\nIiwgICMgaXNvcnQKICAgICJCIiwgICMgZmxha2U4LWJ1Z2JlYXIKICAgICJD\nOTAiLCAgIyBtY2NhYmUgY3ljbG9tYXRpYyBjb21wbGV4aXR5CiAgICAiRyIs\nICAjIGZsYWtlOC1sb2dnaW5nLWZvcm1hdAogICAgIlQyMCIsICMgZmxha2U4\nLXByaW50CiAgICAiVVAiLCAgIyBweXVwZ3JhZGUKICAgICJDNCIsICAjIGZs\nYWtlOC1jb21wcmVoZW5zaW9ucwogICAgIklTQyIsICAjIGZsYWtlOC1pbXBs\naWNpdC1zdHItY29uY2F0CiAgICAiUlNFIiwgICMgZmxha2U4LXJhaXNlCiAg\nICAiUElFIiwgICMgZmxha2U4LXBpZQogICAgIk44MDQiLCAgIyBGaXJzdCBh\ncmd1bWVudCBvZiBhIGNsYXNzIG1ldGhvZCBzaG91bGQgYmUgbmFtZWQgYGNs\nc2AKXQpsaW50Lmlnbm9yZSA9IFtdCmV4Y2x1ZGUgPSBbCiAgICAibWlncmF0\naW9ucy92ZXJzaW9ucy8iLAogICAgInZlbnYqIiwKICAgICJfX3B5Y2FjaGVf\nXyIsCiAgICAibm9kZV9tb2R1bGVzIiwKICAgICJjYWNoZSIsCiAgICAibWln\ncmF0aW9ucyIsCiAgICAiYnVpbGQiLAogICAgInNhbXBsZV9jYXBfeG1sX2Rv\nY3VtZW50cy5weSIsCl0K\n",
  "encoding": "base64",
  "_links": {
    "self": "https://api.github.com/repos/alphagov/notifications-utils/contents/pyproject.toml?ref=main",
    "git": "https://api.github.com/repos/alphagov/notifications-utils/git/blobs/798ceb3a471794cb75a8a937d985e82037900a51",
    "html": "https://github.com/alphagov/notifications-utils/blob/main/pyproject.toml"
  }
}
```

Other Github endpoints do not redirect when they find a symlink, for example:

```bash
curl "https://raw.githubusercontent.com/alphagov/notifications-utils/206f008ef017f8b5ec9b870df04bf35780b7717b/pyproject.toml"
```

returns
```
notifications_utils/version_tools/pyproject.toml%
```

uv is using the latter `raw.githubusercontent.com` endpoint here: https://github.com/astral-sh/uv/blob/b58538fbf497283c4d012cda1f7a573ffdd708c6/crates/uv-distribution/src/source/mod.rs#L1797-L1798

It later fails because it tries to parse the string `notifications_utils/version_tools/pyproject.toml%` as a TOML file.

In previous versions of uv this worked fine. We suspect it has started failing because of this change: https://github.com/astral-sh/uv/pull/10765

### Platform

Mac OS 14.7

### Version

uv 0.5.22 (4574ced37 2025-01-21)

### Python version

Python 3.11.7

---

_Label `bug` added by @quis on 2025-01-22 14:17_

---

_Assigned to @charliermarsh by @charliermarsh on 2025-01-22 14:18_

---

_Comment by @charliermarsh on 2025-01-22 14:18_

Ahh thanks! I'll fix this today.

---

_Comment by @charliermarsh on 2025-01-22 14:18_

(Yes, it's definitely from the resolver fast-path.)

---

_Closed by @charliermarsh on 2025-01-22 16:54_

---

_Closed by @charliermarsh on 2025-01-22 16:54_

---

_Comment by @quis on 2025-01-23 09:48_

Amazing! Thanks for addressing this so quickly

---
