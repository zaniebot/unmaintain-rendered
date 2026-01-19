```yaml
number: 17608
title: "Default to `__token__` during `uv publish` if no username value is provided"
type: issue
state: open
author: xavdid
labels:
  - enhancement
assignees: []
created_at: 2026-01-19T09:34:28Z
updated_at: 2026-01-19T09:34:28Z
url: https://github.com/astral-sh/uv/issues/17608
synced_at: 2026-01-19T10:28:48Z
```

# Default to `__token__` during `uv publish` if no username value is provided

---

_@xavdid_

### Summary

When publishing packages with `uv publish`, there are interactive inputs for collecting credentials.

```
% uv publish
Publishing 2 files https://upload.pypi.org/legacy/
Enter username ('__token__' if using a token): some_username
Enter password: <password input>
...
```

Everything works if I type `__token__` and paste my token, but I find myself typing `__token__` a lot.

It would be nice if the username value defaulted to `__token__` if no input was provided. So publishing would be:

```
% uv publish
Publishing 2 files https://upload.pypi.org/legacy/
Enter username ('__token__' if using a token): <enter>
Enter password: <paste> <enter>
...
```

I tried running `uv publish --username __token__`, expecting it to prompt me for my token, but it looks like that expected the token to be available in the env or as an arg also, so the upload starts immediately (and fails with a 403, as expected).

Happy to put a PR up for this! Should be a small change to: https://github.com/astral-sh/uv/blob/abd6b8f56c6a48783550d776f22695999c19399a/crates/uv/src/commands/publish.rs#L503

### Example

It would be smoother to publish packages locally and interactively!

---

_Label `enhancement` added by @xavdid on 2026-01-19 09:34_

---
