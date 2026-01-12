```yaml
number: 12509
title: "Remove `salsa::report_untracked_read` when finding the dynamic module resolution paths"
type: pull_request
state: merged
author: MichaReiser
labels:
  - ty
assignees: []
merged: true
base: main
head: source-root
created_at: 2024-07-25T14:10:18Z
updated_at: 2024-08-02T16:47:15Z
url: https://github.com/astral-sh/ruff/pull/12509
synced_at: 2026-01-12T15:55:41Z
```

# Remove `salsa::report_untracked_read` when finding the dynamic module resolution paths

---

_@MichaReiser_

## Summary

This PR introduces a new `FileRoot` input ingredient. A file root is a root directory in which we track files. 
The main use case for `FileRoot` is to use it to determine a file's durability. Files in the `Workspace` have a `LOW` durability
where files on any module search path are probably unlikely to change. 

`FileRoot` also comes with a revision tracks the time when a file was last changed, added, or removed in that directory. 
I use the revision in this PR to make the editable installation paths query dependent on the site-package content, so that
we no longer need the `db.report_untracked_read` call


## Test Plan

I updated the test that @AlexWaygood added that shows that the editable paths are correctly re-resolved


---

_Review requested from @carljm by @MichaReiser on 2024-07-25 14:10_

---

_Review requested from @AlexWaygood by @MichaReiser on 2024-07-25 14:10_

---

_Label `red-knot` added by @MichaReiser on 2024-07-25 14:10_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:207 on 2024-07-25 14:25_

```suggestion
    let ModuleResolutionSettings { static_search_paths, site_packages_root, .. } = module_resolution_settings(db);

    let site_packages = static_search_paths
        .iter()
        .find(|path| path.is_site_packages());
```

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:224 on 2024-07-25 14:26_

```suggestion
	let mut dynamic_paths = Vec::new();

    let (Some(site_packages), Some(site_packages_root)) = (site_packages, site_packages_root)
    else {
        return dynamic_paths;
    };
```

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files/file_root.rs`:12 on 2024-07-25 14:29_

What's the reason for not doing so for editable-install search paths?

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files/file_root.rs`:72 on 2024-07-25 14:31_

Maybe we could implement `path_slash::PathExt` for `SystemPathBuf` in `ruff_db`

---

_@AlexWaygood approved on 2024-07-25 14:32_

Nice! So good to get rid of the `report_untracked_read()` call

---

_Comment by @github-actions[bot] on 2024-07-25 14:33_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Comment by @AlexWaygood on 2024-07-25 19:31_

(I rebased your PR on top of my `path.rs` refactor that's just landed in `main`)

---

_@MichaReiser reviewed on 2024-07-25 19:41_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files/file_root.rs`:72 on 2024-07-25 19:41_

I'm open to consider it when it is used more widely 😉

---

_@MichaReiser reviewed on 2024-07-25 19:48_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files/file_root.rs`:12 on 2024-07-25 19:48_

Hmm. Not sure. I think I don't understand them well enough. 

What I understand is that editable installs are for example used to make workspace packages available to other packages. Or to link in any external package that I plan to edit. The first is somewhat problematic because the path overlaps with the package path but we definitely want durability low for first party package files. 

For both cases I think what we want is durability low which will be the default for all files for which we can't find a file root. 

---

_Review comment by @carljm on `crates/ruff_db/src/files.rs`:62 on 2024-07-26 16:25_

Can we add a doc comment here explaining what this is and why it's needed, similar to the above attributes?

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:406 on 2024-07-26 16:26_

Can we add a doc comment for this?

---

_Review comment by @carljm on `crates/red_knot_module_resolver/src/resolver.rs`:403 on 2024-07-26 16:26_

Orthogonal to this PR, but this comment is confusing to me: where is it "also stored separately"?

---

_Review comment by @carljm on `crates/ruff_db/src/files.rs`:182 on 2024-07-26 16:27_

I don't think the word "routes" makes sense here?
```suggestion
        let roots = inner.roots.read().unwrap();
```

---

_Review comment by @carljm on `crates/ruff_db/src/files.rs`:207 on 2024-07-26 16:27_

```suggestion
        let roots = inner.roots.read().unwrap();
```

---

_@carljm approved on 2024-07-26 16:27_

Nice!

---

_@MichaReiser reviewed on 2024-07-26 17:21_

---

_Review comment by @MichaReiser on `crates/red_knot_module_resolver/src/resolver.rs`:403 on 2024-07-26 17:21_

No idea. @AlexWaygood ?

---

_@MichaReiser reviewed on 2024-07-26 17:21_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files.rs`:207 on 2024-07-26 17:21_

That's what you get for using Copilot lol

---

_@AlexWaygood reviewed on 2024-07-26 17:31_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:403 on 2024-07-26 17:31_

It's not anymore after Micha refactored the code in https://github.com/astral-sh/ruff/commit/91338ae9021d64ba39d82717d633a510d70649f7 ;-)

The comment is out of date following that commit

---

_@AlexWaygood reviewed on 2024-07-26 17:47_

---

_Review comment by @AlexWaygood on `crates/red_knot_module_resolver/src/resolver.rs`:403 on 2024-07-26 17:47_

Originally in https://github.com/astral-sh/ruff/commit/9a2dafb43d36b62d45a604dea58224a0884cd6e5, the `site-packages` search path was stored as a separate field on the struct as well as being present in the `static_search_paths` field; that's what this comment referred to, before the code was refactored

---

_@MichaReiser reviewed on 2024-07-29 09:27_

---

_Review comment by @MichaReiser on `crates/ruff_db/src/files/file_root.rs`:12 on 2024-07-29 09:27_

I'm landing this PR as is. I can create roots for editable installs when needed as part of a separate PR

---

_@AlexWaygood reviewed on 2024-07-29 09:29_

---

_Review comment by @AlexWaygood on `crates/ruff_db/src/files/file_root.rs`:12 on 2024-07-29 09:29_

Yes, fine by me!

---

_Merged by @MichaReiser on 2024-07-29 09:31_

---

_Closed by @MichaReiser on 2024-07-29 09:31_

---

_Branch deleted on 2024-07-29 09:31_

---
