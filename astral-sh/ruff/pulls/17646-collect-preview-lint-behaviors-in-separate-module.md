```yaml
number: 17646
title: Collect preview lint behaviors in separate module
type: pull_request
state: merged
author: dylwil3
labels:
  - internal
assignees: []
merged: true
base: main
head: collect-preview-behaviors
created_at: 2025-04-26T19:38:51Z
updated_at: 2025-04-28T14:12:26Z
url: https://github.com/astral-sh/ruff/pull/17646
synced_at: 2026-01-10T19:03:00Z
```

# Collect preview lint behaviors in separate module

---

_Pull request opened by @dylwil3 on 2025-04-26 19:38_

This PR collects all behavior gated under preview into a new module `ruff_linter::preview` that exposes functions like `is_my_new_feature_enabled` - just as is done in the formatter crate.

One thing I'd like to do before moving this out of draft is see if there is a way to enforce that `ruff_linter::rules` can never access the `preview` field in `LinterSettings`.

Here is one approach to this:
- Make the `preview` field private
- Make a public trait like `QueryPreview` with one member `fn preview(&self) -> PreviewMode;`, then implement this member for `LinterSettings`
- Inside `ruff_linter::preview`, import the trait. But _don't_ import it in `ruff_linter::rules`

The downside of this approach is that there are several places where we _initialize_ `LinterSettings` by specifying all of its fields. So we'd have to change those to builder methods.

I would not be surprised if there is a simple and standard way to enforce the visibility I'm after, I just don't quite know which `pub(in ...)`, `pub(super)`, `pub(crate)`, etc. thing I should be doing that would basically make `preview` accessible everywhere _except_ in `ruff_linter::rules`. Suggestions?

---

_Label `internal` added by @dylwil3 on 2025-04-26 19:38_

---

_Comment by @github-actions[bot] on 2025-04-26 19:45_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2025-04-27 09:51_

You could probably do a `pub(in crate::preview)` but I'm not sure if we have to enforce this statically. 

---

_Marked ready for review by @dylwil3 on 2025-04-27 20:28_

---

_Review requested from @AlexWaygood by @dylwil3 on 2025-04-27 20:28_

---

_Comment by @dylwil3 on 2025-04-27 20:43_

> You could probably do a pub(in crate::preview) but I'm not sure if we have to enforce this statically.

Unfortunately that's not an allowed visibility (it has to be an ancestor module). But if you think it's okay to not enforce this statically, then I'm on board too!

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-04-27 20:44_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/preview.rs`:14 on 2025-04-28 06:36_

Nit: It would be nice if each function had a link to the PR/issue that introduced/documents the new preview behavior. 

Adding it now would mainly be nice to set a good example for future added preview features

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/preview.rs`:70 on 2025-04-28 06:37_

Nit, let's be more consistent with the empty lines between the functions
```suggestion
pub(crate) const fn is_shell_injection_only_trusted_input_enabled(
    settings: &LinterSettings,
) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_suspicious_function_reference_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_bool_subtype_of_annotation_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_comprehension_with_min_max_sum_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_check_comprehensions_in_tuple_call_enabled(
    settings: &LinterSettings,
) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_bad_version_info_in_non_stub_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_fix_future_annotations_in_stub_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_only_add_return_none_at_end_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_simplify_ternary_to_binary_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_fix_manual_dict_comprehension_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}
pub(crate) const fn is_fix_manual_list_comprehension_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_dunder_init_fix_unused_import_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_defer_optional_to_up045_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_unicode_to_unicode_confusables_enabled(settings: &LinterSettings) -> bool {
    settings.preview.is_enabled()
}

pub(crate) const fn is_support_slices_in_literal_concatenation_enabled(
    settings: &LinterSettings,
) -> bool {
    settings.preview.is_enabled()
}
```

---

_@MichaReiser approved on 2025-04-28 06:42_

Nice. Thanks for going through all existing preview checks. I'm surprised that there aren't more.

Would the visibility restriction work if you moved `preview.rs` to `settings/preview`?

As an "alternative", we could consider making `preview` private and instead expose it as a `preview` method and document that it's recommended to add a new function to `preview.rs`.

---

_Comment by @dylwil3 on 2025-04-28 14:03_

> Would the visibility restriction work if you moved preview.rs to settings/preview?

> As an "alternative", we could consider making preview private and instead expose it as a preview method and document that it's recommended to add a new function to preview.rs.

So I tried something like this, but the issue was:

> The downside of this approach is that there are several places where we initialize LinterSettings by specifying all of its fields. So we'd have to change those to builder methods.

So it's less about controlling the visibility of _querying_ preview, and more about figuring out how to deal with the resulting difficulty in _setting_ preview. I'm also happy to leave this as is for now if there isn't a simple solution, or if we want to do a follow up later.

---

_Comment by @MichaReiser on 2025-04-28 14:07_

Oh, I missed that. I'm fine leaving it `pub`. It's pub in the formatter and that hasn't been a problem.

---

_Merged by @dylwil3 on 2025-04-28 14:12_

---

_Closed by @dylwil3 on 2025-04-28 14:12_

---
