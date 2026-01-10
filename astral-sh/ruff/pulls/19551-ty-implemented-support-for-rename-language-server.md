```yaml
number: 19551
title: "[ty] Implemented support for \"rename\" language server feature"
type: pull_request
state: merged
author: UnboundVariable
labels:
  - server
  - ty
assignees: []
merged: true
base: main
head: rename
created_at: 2025-07-25T04:59:14Z
updated_at: 2025-08-07T10:28:19Z
url: https://github.com/astral-sh/ruff/pull/19551
synced_at: 2026-01-10T17:52:17Z
```

# [ty] Implemented support for "rename" language server feature

---

_Pull request opened by @UnboundVariable on 2025-07-25 04:59_

This PR adds support for the "rename" language server feature. It builds upon existing functionality used for "go to references". 

The "rename" feature involves two language server requests. The first is a "prepare rename" request that determines whether renaming should be possible for the identifier at the current offset. The second is a "rename" request that returns a list of file ranges where the rename should be applied.

Care must be taken when attempting to rename symbols that span files, especially if the symbols are defined in files that are not part of the project. We don't want to modify code in the user's Python environment or in the vendored stub files.

I found a few bugs in the "go to references" feature when implementing "rename", and those bug fixes are included in this PR.

---

_Review requested from @carljm by @UnboundVariable on 2025-07-25 04:59_

---

_Review requested from @AlexWaygood by @UnboundVariable on 2025-07-25 04:59_

---

_Review requested from @sharkdp by @UnboundVariable on 2025-07-25 04:59_

---

_Review requested from @dcreager by @UnboundVariable on 2025-07-25 04:59_

---

_Review requested from @MichaReiser by @UnboundVariable on 2025-07-25 04:59_

---

_Comment by @github-actions[bot] on 2025-07-25 05:03_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅
No memory usage changes detected ✅


---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-07-25 07:05_

---

_Label `server` added by @MichaReiser on 2025-07-25 07:10_

---

_Label `ty` added by @MichaReiser on 2025-07-25 07:10_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/rename.rs`:65 on 2025-07-25 07:13_

Do we need to perform any validation on whether the new name is a valid python name?

---

_Review comment by @MichaReiser on `crates/ty_ide/src/rename.rs`:95 on 2025-07-25 07:15_

`is_file_included` is not the right method here. It only returns whether the file matches any `include` pattern and isn't excluded by an `exclude` pattern but it might incorrectly return `true` for a file that's git ignored. 

We should use the more expensive `db.files().contains(file)` instead. There's an issue to make `is_file_included` more accurate so that we don't have to index all files to determine if a file is part of a project or not

---

_Review comment by @MichaReiser on `crates/ty_ide/src/rename.rs`:202 on 2025-07-25 07:17_

Same as what I mentioned on another PR. You could add multiple annotations to the same diagnostic instead where the first annotation highlights the identifier that the user wants to rename and all other annotations show where the renames are necessary.

That would reduce the redundant information in tests

---

_@MichaReiser approved on 2025-07-25 07:21_

This is great. 

Do you know how editors handle renames if multiple LSPs provide the feature? 

I'm asking because our go to definition implementation still has some "holes". Meaning, ty will miss renames where another LSP (that users might run side to side with ty) would do a proper rename. Or do editors merge the rename result from all LSPs (union vs taking the result from one LSP)? 

If editors only pick the result from one LSP, it migth then be best to wait landing this feature and add an explicit client setting for users to opt in to our experimental renaming. 

We're fine landing this if editors union the result.

---

_@UnboundVariable reviewed on 2025-07-25 15:58_

---

_Review comment by @UnboundVariable on `crates/ty_ide/src/rename.rs`:65 on 2025-07-25 15:58_

I checked, and other Python language servers don't prevent this. It's something that could be added in the future based on user feedback.

---

_Comment by @UnboundVariable on 2025-07-25 16:02_

> If editors only pick the result from one LSP, it migth then be best to wait landing this feature and add an explicit client setting for users to opt in to our experimental renaming.

Editors don't merge results from multiple language servers for the "rename" feature. There's really no good way to do it. Instead, they pick one language server and use its results. 

We can certainly wait to land this feature if you're concerned about this. It's up to you.

It sounds like we'll be able to plug the "nonlocal" and "global" hole soon, so maybe it's best if we wait for that fix to land first.

---

_Comment by @MichaReiser on 2025-07-25 17:11_

Yeah, it might be good to keep this disable for the moment. We could also land this but not register the provider and @dhruvmanila could enable it once he added support for client settings.

---

_Comment by @dhruvmanila on 2025-08-07 05:00_

I can take this up now that we have proper client settings infrastructure in place.


---

_Comment by @github-actions[bot] on 2025-08-07 05:02_

<!-- generated-comment typing_conformance_diagnostics_diff -->
## Diagnostic diff on [typing conformance tests](https://github.com/python/typing/tree/d4f39b27a4a47aac8b6d4019e1b0b5b3156fabdc/conformance)
No changes detected when running ty on typing conformance tests ✅


---

_@MichaReiser reviewed on 2025-08-07 07:27_

---

_Review comment by @MichaReiser on `crates/ty_ide/src/rename.rs`:65 on 2025-08-07 07:27_

Interesting, given that the spec explicilty mentiones this:

```
	/**
	 * The new name of the symbol. If the given name is not valid the
	 * request must return a [ResponseError](#ResponseError) with an
	 * appropriate message set.
	 */
```

---

_Merged by @dhruvmanila on 2025-08-07 10:28_

---

_Closed by @dhruvmanila on 2025-08-07 10:28_

---
