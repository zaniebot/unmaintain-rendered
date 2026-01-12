```yaml
number: 8781
title: "Warn when using the keyring in `uv publish` but it doesn't have credentials"
type: issue
state: closed
author: konstin
labels:
  - enhancement
  - error messages
assignees: []
created_at: 2024-11-03T20:08:59Z
updated_at: 2024-11-27T19:54:50Z
url: https://github.com/astral-sh/uv/issues/8781
synced_at: 2026-01-12T15:59:35Z
```

# Warn when using the keyring in `uv publish` but it doesn't have credentials

---

_@konstin_

See https://github.com/astral-sh/uv/issues/7963#issuecomment-2453558043: We should warn in this case that the keyring didn't return any password, since using the keyring with publishing only makes sense if it has a password for the publish URL

---

_Label `enhancement` added by @konstin on 2024-11-03 20:08_

---

_Label `error messages` added by @konstin on 2024-11-03 20:08_

---

_Comment by @cthoyt on 2024-11-04 07:50_

Thanks @konstin for following up on this. Such a test could also make a specific test when you use `--publish-url https://test.pypi.org/legacy/` and there's no credentials but there is something like `https://test.pypi.org/legacy/?PACKAGE` available - it could say "hey, you might want to use a package-specific publish URL that has the following form where ?PACKAGE is your package`. 

Should uv go even further to guess the right publish URL in keyring based on the current package name? Can uv publish even introspect on that metadata?

---

_Closed by @konstin on 2024-11-27 19:54_

---
