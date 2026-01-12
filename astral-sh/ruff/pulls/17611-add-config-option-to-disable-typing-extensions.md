```yaml
number: 17611
title: "Add config option to disable `typing_extensions` imports "
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - configuration
assignees: []
merged: true
base: main
head: brent/typing-extensions-config
created_at: 2025-04-24T15:31:39Z
updated_at: 2025-04-28T18:57:38Z
url: https://github.com/astral-sh/ruff/pull/17611
synced_at: 2026-01-12T15:56:02Z
```

# Add config option to disable `typing_extensions` imports 

---

_@ntBre_

Summary
--

This PR resolves https://github.com/astral-sh/ruff/issues/9761 by adding a linter configuration option to disable
`typing_extensions` imports. As mentioned [here], it would be ideal if we could
detect whether or not `typing_extensions` is available as a dependency
automatically, but this seems like a much easier fix in the meantime.

The default for the new option, `typing-extensions`, is `true`,
preserving the current behavior. Setting it to `false` will bail out of the new
`Checker::typing_importer` method, which has been refactored from the 
`Checker::import_from_typing` method in https://github.com/astral-sh/ruff/pull/17340),
with `None`, which is then handled specially by each rule that calls it.

I considered some alternatives to a config option, such as checking if `typing_extensions` has been imported or checking for a `TYPE_CHECKING` block we could use, but I think defaulting to allowing `typing_extensions` imports and allowing the user to disable this with an option is both simple to implement and pretty intuitive.

[here]: https://github.com/astral-sh/ruff/issues/9761#issuecomment-2790492853

Test Plan
--

New linter tests exercising several combinations of Python versions and the new config option for PYI019. I also added tests for the other affected rules, but only in the case where the new config option is enabled. The rules' existing tests also cover the default case.

---

_Label `rule` added by @ntBre on 2025-04-24 15:31_

---

_Label `configuration` added by @ntBre on 2025-04-24 15:31_

---

_Comment by @github-actions[bot] on 2025-04-24 15:38_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-04-24 16:02_

---

_Review requested from @AlexWaygood by @ntBre on 2025-04-24 16:02_

---

_Review requested from @MichaReiser by @ntBre on 2025-04-25 15:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:553 on 2025-04-28 08:16_

We could use a pattern similar to @BurntSushi's `InferContext::report_lint` where the inital function returns an `Option<Builder>` and the builder allows you to perform the typing import. 

```rust
pub(crate) fn typing_importer(&self, version_added_to_typing: PythonVersion) -> Option<TypingImporte> {
	let source_module = if self.target_version() >= version_added_to_typing {            "typing"
	} else if self.settings.disable_typing_extensions {
  	return None;
	} else {
		"typing_extensions"
	};
	
	Some(TypingImporter { checker: self, source_module })
}

pub struct TypingImporter<'a> {
	checker: &'a Checker,
	source_module: &'static src
}

impl TypingImporter<'_> {
	pub fn import(member: &str, position: TextSize) -> Result<(Edit, String), ResolutionError> {
		... 
	}
}
```

I'm a bit undecided if `member` should be passed to `typing_importer` to avoid accidential reuse. 

A simpler alternative would be to return `Option<Result<...>`, but that's a bit less common but could be fine too.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:321 on 2025-04-28 08:21_

Do we need to update the rule's fix availability documentation (same for other rules) to explain that the fix isn't available if typing extensions isn't available?

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:531 on 2025-04-28 08:27_

bikeshedding: The `disable` prefix is uncommon in our existing settings and simply `typing_extensions` would be more idiomatic or `allow_typing_extensions`. 

---

_@MichaReiser approved on 2025-04-28 08:28_

This looks good to me. I agree with you, the way `typing_import` works today is a bit awkward where it's very easy to omit the fix instead of omitting the diagnostic. I added an inline suggestion on what you could try to improve the ergonomics.

---

_@ntBre reviewed on 2025-04-28 12:56_

---

_Review comment by @ntBre on `crates/ruff_workspace/src/options.rs`:531 on 2025-04-28 12:56_

Ah, I always like options that default to `false` so that I can use `bool::default` :laughing: but either of those names is fine with me

---

_@ntBre reviewed on 2025-04-28 12:56_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:321 on 2025-04-28 12:56_

Yes, very good point, thanks!

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:553 on 2025-04-28 13:02_

Oh, I like that idea, I'll give it a try! 

I vaguely remember trying `Option<Result>` and not liking it for some reason, but maybe that was when I thought `try_set_optional_fix` was still going to work.

---

_@ntBre reviewed on 2025-04-28 13:02_

---

_@ntBre reviewed on 2025-04-28 13:06_

---

_Review comment by @ntBre on `crates/ruff_linter/src/checkers/ast/mod.rs`:553 on 2025-04-28 13:06_

I feel like we do want `member` passed to `typing_importer` because it's the piece that needs to be tied most closely to the `PythonVersion`. I don't remember seeing any places where intentional reuse would be very helpful, so avoiding the accidental cases makes sense to me.

---

_Comment by @ntBre on 2025-04-28 16:11_

Thanks again for the suggestions! I've updated the code to:
- use your `TypingImporter` idea
- add an `## Availability` section to each affected rule
  - I called it `Availability` instead of the more typical `Fix availability` because it affects the diagnostic too
  - I left out any rules using `PythonVersion::lowest`, which should not be possible to affect with this change
- rename the option to `typing_extensions` with a default value of `true`

---

_@MichaReiser reviewed on 2025-04-28 16:29_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:246 on 2025-04-28 16:29_

Should we move this now out of `try_generate_fix` because it also affects the diagnostic generation?

---

_@MichaReiser reviewed on 2025-04-28 16:31_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:134 on 2025-04-28 16:31_

Is it okay that we treat "typing-extensions" not being available the same as the import failing (will this suppress the diagnostic or only fail to generate the fix?)



---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/flake8_pyi/rules/custom_type_var_for_self.rs`:187 on 2025-04-28 16:32_

Same here. I think it would be nice if we resolve the importer before calling `replace_custom_typevar_with_self` to make it clear that a failure to resolve the importer means we shouldn't emit a diagnostic.

---

_@MichaReiser reviewed on 2025-04-28 16:32_

---

_@ntBre reviewed on 2025-04-28 16:55_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/fastapi/rules/fastapi_non_annotated_dependency.rs`:246 on 2025-04-28 16:55_

Yes, good catch, thank you! We can actually revert most of the new code here.

---

_@ntBre reviewed on 2025-04-28 17:05_

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/flake8_annotations/helpers.rs`:134 on 2025-04-28 17:05_

I believe all of the calls in this file are okay because they use `PythonVersion::lowest`. I think we could even `expect` or `unwrap` instead of the first `?`, if we wanted, because we should always get a `TypingImporter` for any supported Python version.

---

_Comment by @ntBre on 2025-04-28 17:55_

I went through all of the changed rules and made changes similar to your suggestions above to restructure the code to call `typing_importer` first, return early if `None`, and only then to call `import`. This reverted many of the other code changes I had to make in the first draft. 

It also made two functions that were only very similar before now identical: `duplicate_union_member::generate_union_fix` and `redundant_numerical_union::generate_union_fix`, so I factored these out into the parent module.

The only change we might not is in the very last commit, where I used `anyhow::Context` to convert one of the `PythonVersion::lowest` cases directly into an `Err`. As mentioned above, I think this is safe because this import should always be available in `typing` for any Python version we support. We could even `unwrap` if we wanted, but this is a bit safer just in case.

Thanks for catching these additional benefits of the new API!

---

_@MichaReiser approved on 2025-04-28 17:58_

---

_Merged by @ntBre on 2025-04-28 18:57_

---

_Closed by @ntBre on 2025-04-28 18:57_

---

_Branch deleted on 2025-04-28 18:57_

---
