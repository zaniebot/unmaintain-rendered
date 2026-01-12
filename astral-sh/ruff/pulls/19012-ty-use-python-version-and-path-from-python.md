```yaml
number: 19012
title: "[ty] Use python version and path from Python extension"
type: pull_request
state: merged
author: MichaReiser
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: micha/python-extension-environment
created_at: 2025-06-28T16:27:40Z
updated_at: 2025-07-14T09:55:50Z
url: https://github.com/astral-sh/ruff/pull/19012
synced_at: 2026-01-12T15:56:29Z
```

# [ty] Use python version and path from Python extension

---

_@MichaReiser_

## Summary

Uses the python version and environment selected in the Python extension as a fallback if the configuration doesn't specify a version or environment respectively. 

Depends on https://github.com/astral-sh/ty-vscode/pull/65


Closes https://github.com/astral-sh/ty-vscode/issues/26

## Test Plan


https://github.com/user-attachments/assets/b53fa5d3-cd78-41c4-a4e7-4e96a3a8c4ef




---

_Label `ty` added by @MichaReiser on 2025-06-28 16:27_

---

_Label `server` added by @MichaReiser on 2025-06-28 16:27_

---

_Comment by @github-actions[bot] on 2025-06-28 16:30_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Comment by @github-actions[bot] on 2025-06-28 16:43_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @MichaReiser on 2025-07-10 12:39_

---

_Review requested from @carljm by @MichaReiser on 2025-07-10 12:39_

---

_Review requested from @AlexWaygood by @MichaReiser on 2025-07-10 12:39_

---

_Review requested from @sharkdp by @MichaReiser on 2025-07-10 12:39_

---

_Review requested from @dcreager by @MichaReiser on 2025-07-10 12:39_

---

_@AlexWaygood approved on 2025-07-10 16:00_

Nice.

If the extension isn't able to give us the Python version but is able to give us the Python executable (for some reason), will we infer the Python version from that executable the same way as we would with a virtual environment that had been activated?

---

_Comment by @MichaReiser on 2025-07-10 16:01_

> If the extension isn't able to give us the Python version but is able to give us the Python executable (for some reason), will we infer the Python version from that executable the same way as we would with a virtual environment that had been activated?

Yes, we would try to infer the version from the environment. 

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:676 on 2025-07-14 04:19_

```suggestion
		// SAFETY: This was initialized by the above `self.get` call
        self.0.get_mut().unwrap()
```

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:633 on 2025-07-14 04:20_

Assuming that this `url` is still the root path, can we document this fact?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:53 on 2025-07-14 04:37_

Unrelated to this PR, it would be useful to add some documentation around the default project concept, what's the purpose of it and when it should be used.

As I mentioned earlier, I thought this is to be used for non-project files _and_ virtual files but as you mentioned, and I think that's useful as well, that virtual files do not belong here. Currently, virtual files are being added in the default project but I'm changing that in #19264 from which I'll probably split out the part which just makes this change (store virtual files in non-default project db).

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:224 on 2025-07-14 04:39_

This doc doesn't seem correct? The default project is the one created for the current working directory.

It might be worth moving the definition of what a default project is to `DefaultProject` and just provide a reference from these methods like `[default project database](DefaultProject)`.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session.rs`:546 on 2025-07-14 04:44_

Unrelated to this PR, but it might be useful to document the difference between `register` and `initialize`.

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:160 on 2025-07-14 05:05_

Why do we need both the `executable` and the `environment`?

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:120 on 2025-07-14 05:14_

We could probably shorten these messages like "Fallback Python environment as selected in the VS Code Python extension: {python}" and "Fallback Python version as selected in the VS Code Python extension: {version}" if it's easier for us to tie the "fallback" keyword to these `fallback_` prefixed overrides.

---

_Review comment by @dhruvmanila on `crates/ty_project/src/metadata/options.rs`:712 on 2025-07-14 05:18_

If I understand this and other unreachables correctly, we need to decide whether we want to support these overrides (rules, includes, excludes) to be set in the editor settings, right?

---

_@dhruvmanila approved on 2025-07-14 05:19_

Looks great!

---

_Comment by @dhruvmanila on 2025-07-14 05:20_

That's a smart way of testing this out!

---

_@dhruvmanila reviewed on 2025-07-14 05:26_

---

_Review comment by @dhruvmanila on `crates/ty_server/src/session/options.rs`:160 on 2025-07-14 05:26_

Oh, this would probably allow us to show some kind of warning if the selected Python version in an editor is different than the configured Python version or even if there's a mismatch in the selected and used virtual environment even though the Python version might be the same.

---

_@MichaReiser reviewed on 2025-07-14 09:25_

---

_Review comment by @MichaReiser on `crates/ty_server/src/session/options.rs`:160 on 2025-07-14 09:25_

I mainly thought that I'm not to picky about what we should send to the server and let the server decide what information it needs

---

_@MichaReiser reviewed on 2025-07-14 09:26_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:712 on 2025-07-14 09:26_

Yes. Once we do, we'd need to implement the match arms. 

---

_Merged by @MichaReiser on 2025-07-14 09:47_

---

_Closed by @MichaReiser on 2025-07-14 09:47_

---

_Branch deleted on 2025-07-14 09:47_

---
