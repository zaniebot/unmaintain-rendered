```yaml
number: 2305
title: "monorepo overrides patterns don't seem to be reported for extension opened to sub-repo"
type: issue
state: closed
author: VerdantFox
labels:
  - question
assignees: []
created_at: 2026-01-02T17:44:33Z
updated_at: 2026-01-06T16:41:16Z
url: https://github.com/astral-sh/ty/issues/2305
synced_at: 2026-01-10T01:56:41Z
```

# monorepo overrides patterns don't seem to be reported for extension opened to sub-repo

---

_Issue opened by @VerdantFox on 2026-01-02 17:44_

### Summary

Hi, I couldn't find this bug already.

I love this change to add [configuration file parameter support](https://github.com/astral-sh/ty/issues/786). I set up our monorepo to use this feature. We have one configuration file for the monorepo at the root of the project, and we are gradually improving rules for the sub-repos by overriding rules with glob patterns in that file. This works well for our pre-commit hook at the project root, which respects the glob pattern overrides. However, the glob patterns don't seem to work correctly for the VS Code extension when I open a Workspace at a sub-repo level that matches one of the glob patterns. I'm guessing the glob patterns are working relative to the Workspace root, regardless of the `ty.toml` file existing several parent folders deep.

Example structure:

```txt
parent
parent/ty.toml
parent/src/sub_repo
parent/src/sub_repo/file_with_error.py
```

Configuration file:

```toml
invalid-assignment = "ignore"

[[overrides]]
include = ["parent/src/sub_repo/**"]

[overrides.rules]
invalid-assignment = "error"
```

If I open a VS Code workspace at `parent`, the error in `parent/src/sub_repo/file_with_error.py` shows as an error, as expected, because of the override. 🎉 But if I open a Workspace at `parent/src/sub_repo` and set the extension to look for the `parent/ty.toml` config file (by absolute path), the error in `parent/src/sub_repo/file_with_error.py` is ignored (I assume due to not finding the override relative to the config file). 🙁

Can I get this working for my use case? Is this a user error?

### Version

0.0.8

---

_Comment by @MichaReiser on 2026-01-04 17:29_

What's the reason for setting the configuration file in the VS Code project? ty should automatically discover the configuration file in the workspace's ancestor directories and use that configuration instead. The difference in that case is that the paths are relative to the configuration file rather than the workspace root.

---

_Label `question` added by @MichaReiser on 2026-01-04 17:29_

---

_Comment by @VerdantFox on 2026-01-06 16:41_

Yes, actually our setup does work correctly and correctly finds the parent `ty.toml` file when no configuration parameter is supplied to the VS Code extension. And when doing so, the configuration file properly follows the glob patterns. Thank you for the hint. Closing this issue.

---

_Closed by @VerdantFox on 2026-01-06 16:41_

---
