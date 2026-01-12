```yaml
number: 7009
title: "Add hidden `--preview` / `--no-preview` options to `ruff check`"
type: pull_request
state: merged
author: zanieb
labels:
  - cli
assignees: []
merged: true
base: main
head: cli/preview
created_at: 2023-08-30T15:50:28Z
updated_at: 2023-08-31T15:05:20Z
url: https://github.com/astral-sh/ruff/pull/7009
synced_at: 2026-01-12T15:55:23Z
```

# Add hidden `--preview` / `--no-preview` options to `ruff check`

---

_@zanieb_

Per discussion at https://github.com/astral-sh/ruff/discussions/6998

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->

Adds a `--preview` and `--no-preview` option to the CLI for `ruff check` and corresponding settings. The CLI options are hidden for now.

Available in the settings as `preview = true` or `preview = false`.

Does not include environment variable configuration, although we may add it in the future.

## Test Plan

<!-- How was it tested? -->

`cargo build`

Future work will build on this setting, such as toggling the mode during a test.


---

_Review comment by @zanieb on `crates/ruff_workspace/src/options.rs`:495 on 2023-08-30 15:52_

Should I just use `preview` here too?

---

_@zanieb reviewed on 2023-08-30 15:52_

---

_@charliermarsh reviewed on 2023-08-30 15:54_

---

_Review comment by @charliermarsh on `crates/ruff_workspace/src/options.rs`:495 on 2023-08-30 15:54_

I would vote for `preview` here.

---

_@charliermarsh reviewed on 2023-08-30 15:55_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/args.rs`:118 on 2023-08-30 15:55_

Maybe a few-word description of what this will do?

---

_@charliermarsh approved on 2023-08-30 15:55_

---

_Label `cli` added by @charliermarsh on 2023-08-30 15:58_

---

_@zanieb reviewed on 2023-08-30 15:59_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/options.rs`:495 on 2023-08-30 15:59_

kk

---

_Comment by @MichaReiser on 2023-08-30 16:03_

Should we introduce a `PreviewMode` enum that has `Enabled` and `Disabled` variant to make it easier to search for preview usages (instead of having to search for booleans named `preview`)?

---

_Comment by @charliermarsh on 2023-08-30 16:04_

@MichaReiser - That's a nice idea.

---

_Comment by @zanieb on 2023-08-30 16:08_

I like it

---

_Comment by @github-actions[bot] on 2023-08-30 16:18_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no changes.



---

_@zanieb reviewed on 2023-08-30 16:23_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/options.rs`:495 on 2023-08-30 16:23_

So.. I think we could make this `Option<PreviewMode>` too but I'm not entirely sure how to make that play nicely with the `JsonSchema`. I'm also not sure if we want `preview = enabled | disabled` or `preview = true | false` for users.

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/types.rs`:108 on 2023-08-30 16:29_

This always reminds me of @charliermarsh's tweet about how high-performance code looks like :laughing: 

---

_@MichaReiser reviewed on 2023-08-30 16:29_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/options.rs`:495 on 2023-08-31 07:12_

I quickly looked into this but don't see a good solution for it other than implementing `Serialize`, `Deserialize` and `JsonSchema` manually on `PreviewMode` if we want `preview = true | false` (preview = enabled | disabled) should work out of the box. 



---

_@MichaReiser reviewed on 2023-08-31 07:12_

---

_@MichaReiser reviewed on 2023-08-31 07:13_

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/types.rs`:100 on 2023-08-31 07:13_

This type being inside of the `ruff` crate means that we can't use it in the formatter, or any other crate (e.g. consider we need to gate some semantic model change)

---

_@MichaReiser reviewed on 2023-08-31 07:13_

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/types.rs`:98 on 2023-08-31 07:13_

I would expect the default value to come first

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/types.rs`:95 on 2023-08-31 07:14_

```suggestion
#[derive(Clone, Copy, Debug, PartialEq, Eq, Default, CacheKey, is_macro::Is)]
```

I think we can remove the ordering from PreviewMode. It's unclear to me where this would be useful (and what the expected ordering should be)

---

_@MichaReiser reviewed on 2023-08-31 07:14_

---

_Review comment by @zanieb on `crates/ruff_workspace/src/options.rs`:495 on 2023-08-31 13:48_

Thanks! I'm happy to leave it as-is then

---

_@zanieb reviewed on 2023-08-31 13:48_

---

_@zanieb reviewed on 2023-08-31 13:49_

---

_Review comment by @zanieb on `crates/ruff/src/settings/types.rs`:100 on 2023-08-31 13:49_

Hm it seems wrong to put it elsewhere right now since there's a dedicated `settings/types.rs` module. This is a good callout though, we should probably have a `ruff_settings` crate so we can share with the formatter?

---

_@zanieb reviewed on 2023-08-31 13:50_

---

_Review comment by @zanieb on `crates/ruff/src/settings/types.rs`:95 on 2023-08-31 13:50_

👍 

---

_@MichaReiser reviewed on 2023-08-31 13:59_

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/types.rs`:100 on 2023-08-31 13:59_

I think we can just move the `PreviewMode` because the type itself isn't specific to settings. 

A `ruff_settings` crate isn't straightforward because the settings depend on the rules. So you would need to make the formatter depend on ruff. 

I consider (at least most of what is in settings) settings as the linter-specific settings. The formatter has its own resolved settings that only contain the formatter settings. Most of this already exists with `PyFormatOptions`, although we probably want to add a few more settings. 


What I have in mind is:

* each tool has its own resolved settings object
* `Configuration` is the unified configuration from which the workspace derives the tool Settings

---

_@zanieb reviewed on 2023-08-31 14:00_

---

_Review comment by @zanieb on `crates/ruff/src/settings/types.rs`:100 on 2023-08-31 14:00_

Where would you expect it to go?

---

_@MichaReiser reviewed on 2023-08-31 14:49_

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/types.rs`:100 on 2023-08-31 14:49_

I don't know :sweat_smile: 

It would need to be some rather low-level crate that other crates can depend on without pulling in too many dependencies. 

The alternative is to duplicate the enum in project where it is needed. I don't really mind that (e.g. the Linter has `LineLength` and the formatter has a very similar `LineWidth` option type)

---

_@zanieb reviewed on 2023-08-31 14:51_

---

_Review comment by @zanieb on `crates/ruff/src/settings/types.rs`:100 on 2023-08-31 14:51_

Alright let's consider this TBD when preview support is added to the formatter

---

_Merged by @zanieb on 2023-08-31 14:52_

---

_Closed by @zanieb on 2023-08-31 14:52_

---

_Branch deleted on 2023-08-31 14:52_

---

_@MichaReiser reviewed on 2023-08-31 15:05_

---

_Review comment by @MichaReiser on `crates/ruff/src/settings/types.rs`:100 on 2023-08-31 15:05_

Another way we could take is that `PreviewMode` remains in the configuration only and setting it to true or false changes individual settings in the tool-specific settings. Each tool can then decide whether it uses feature-specific flags or a single preview flag. For example, the formatter can either:

* Have a single preview setting that is set to enabled/disabled based on the preview mode
* Or an experimental-string-handling and potentially other settings that configure a specific preview feature. Setting `preview=true` would enable/disable all of them at once. 

The benefit of feature specific flags is that individual features can be moved out of preview mode (and makes it easier to find all places where the check now needs to be removed)


---
