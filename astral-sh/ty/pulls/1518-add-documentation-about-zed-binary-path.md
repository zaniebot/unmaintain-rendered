```yaml
number: 1518
title: Add documentation about Zed binary path
type: pull_request
state: merged
author: MatthewMckee4
labels:
  - documentation
assignees: []
merged: true
base: main
head: zed-binary-path
created_at: 2025-11-10T19:07:47Z
updated_at: 2025-11-12T11:52:23Z
url: https://github.com/astral-sh/ty/pull/1518
synced_at: 2026-01-10T02:34:10Z
```

# Add documentation about Zed binary path

---

_Pull request opened by @MatthewMckee4 on 2025-11-10 19:07_

<!--
Thank you for contributing to ty! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

I completely understand if this is not a great change. It isn't amazing that the types are different for vscode and zed.

I found it hard to figure out how to do this in zed, so I hope it could be useful for others.

<img width="1703" height="1090" alt="image" src="https://github.com/user-attachments/assets/13217b69-2bf6-43b2-99b4-9ac6d53ee666" />

<img width="1703" height="1090" alt="image" src="https://github.com/user-attachments/assets/72c233ce-ffee-4a8d-95c8-bec4847514ac" />


---

_Marked ready for review by @MatthewMckee4 on 2025-11-10 19:11_

---

_Renamed from "Zed binary path" to "Add documentation about Zed binary path" by @MatthewMckee4 on 2025-11-10 19:12_

---

_Label `documentation` added by @AlexWaygood on 2025-11-10 19:38_

---

_@MichaReiser reviewed on 2025-11-11 09:46_

---

_Review comment by @MichaReiser on `docs/reference/editor-settings.md`:398 on 2025-11-11 09:46_

I suggest changing the title to something like: Specifying the ty binary or Changing the ty binary. This makes it clear that it's not one specific setting.

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-11 10:49_

---

_Comment by @dhruvmanila on 2025-11-12 07:10_

I think I'd prefer to keep this as is because this is part of the "References" section but I agree that this information would be useful and I think it should go under https://docs.astral.sh/ty/editors/#zed instead to specifically target the Zed editor. We can provide a short description on how to provide a custom `ty` binary and use the code snippet used in this PR before the "More information in [Zed's documentation](https://zed.dev/docs/languages/python#configure-python-language-servers-in-zed)." part.

---

_@sinon reviewed on 2025-11-12 11:27_

---

_Review comment by @sinon on `docs/reference/editor-settings.md`:428 on 2025-11-12 11:27_

I don't think this is quite right with this you receive in the lsp logs in Zed:
```
Language server ty:

initializing server ty, id 9: Server reset the connection
-- stderr --
An extremely fast Python type checker.

Usage: ty <COMMAND>

Commands:
  check    Check a project for type errors
  server   Start the language server
  version  Display ty's version
  help     Print this message or the help of the given subcommand(s)

Options:
  -h, --help     Print help
  -V, --version  Print version
```

I think it should be:
```suggestion
            "path": "/home/user/.local/bin/ty",
            "arguments": ["server"]
```

Relevant issue comment on zed: https://github.com/zed-industries/zed/issues/41020#issuecomment-3437767859

---

_@MichaReiser reviewed on 2025-11-12 11:49_

---

_Review comment by @MichaReiser on `docs/editors.md`:65 on 2025-11-12 11:49_

```suggestion
You can override the `ty` executable Zed uses by setting `lsp.ty.binary`:
```

---

_Merged by @MichaReiser on 2025-11-12 11:52_

---

_Closed by @MichaReiser on 2025-11-12 11:52_

---
