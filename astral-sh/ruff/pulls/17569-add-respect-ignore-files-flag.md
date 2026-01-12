```yaml
number: 17569
title: Add --respect-ignore-files flag
type: pull_request
state: merged
author: thejchap
labels:
  - configuration
  - ty
assignees: []
merged: true
base: main
head: feature/red-knot-respect-ignore-files
created_at: 2025-04-23T01:59:28Z
updated_at: 2025-04-26T19:57:14Z
url: https://github.com/astral-sh/ruff/pull/17569
synced_at: 2026-01-12T15:56:02Z
```

# Add --respect-ignore-files flag

---

_@thejchap_

## Summary
astral-sh/ty#219

this PR adds a `--respect-ignore-files`, `--no-respect-ignore-files`, and `[knot.respect-ignore-files]` CLI/config file options that drive whether standard ignore files are respected when doing file discovery. Borrowed the CLI flag pattern (positive/negative CLI flags) from Ruff

## Test Plan
Added snapshot tests

---

_Label `configuration` added by @AlexWaygood on 2025-04-23 14:56_

---

_Label `red-knot` added by @AlexWaygood on 2025-04-23 14:56_

---

_Comment by @github-actions[bot] on 2025-04-23 15:05_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Marked ready for review by @thejchap on 2025-04-25 04:54_

---

_Review requested from @carljm by @thejchap on 2025-04-25 04:54_

---

_Review requested from @MichaReiser by @thejchap on 2025-04-25 04:54_

---

_Review requested from @AlexWaygood by @thejchap on 2025-04-25 04:54_

---

_Review requested from @sharkdp by @thejchap on 2025-04-25 04:54_

---

_Review requested from @dcreager by @thejchap on 2025-04-25 04:54_

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:146 on 2025-04-25 06:33_

Nit: We prefer either of the two formatting because it makes it clearer that the comment before the `else if` comments the `else if` and isn't a trailing comment of the `if` block
```suggestion
                let mut respect_ignore_files = None;
        // --no-respect-gitignore defaults to false and is set true by CLI flag. If passed,
        // override config file
        if self.no_respect_ignore_files {
            respect_ignore_files = Some(false);
        } 
        // Otherwise, only pass this through if explicitly set (don't default to anything here to
        // make sure that doesn't take precedence over an explicitly-set config file value)
        else if self.respect_ignore_files.is_some() {
            respect_ignore_files = Some(true);
        }
```

or

```suggestion
        let mut respect_ignore_files = None;
        // --no-respect-gitignore defaults to false and is set true by CLI flag. If passed,
        // override config file
        if self.no_respect_ignore_files {
            respect_ignore_files = Some(false);
        } else if self.respect_ignore_files.is_some() {
     		     // Otherwise, only pass this through if explicitly set (don't default to anything here to
	        // make sure that doesn't take precedence over an explicitly-set config file value)
            respect_ignore_files = Some(true);
        }
```

---

_Review comment by @MichaReiser on `crates/red_knot/src/args.rs`:146 on 2025-04-25 06:35_

Nit: I prefer to avoid `mut` variables where possible
```suggestion
        let respect_ignore_files = self.no_respect_ignore_files.then_some(false).or(self.respect_ignore_files);
```

---

_Review comment by @MichaReiser on `crates/red_knot_project/src/walk.rs`:140 on 2025-04-25 06:39_

Red Knot has two structs related to configurations:

* `Options`: The options without the defaults filled in. Options can be merged from differnt sources. Only used during the start process.
* `Settings`: Has a very similar structure to `Options` but all defaults are filled in and the options were validated. Created by calling `Options::to_settings`. Used inside red knot when looking up a configuration value


That's why we should add `respect_ignore_files: bool` to the `Settings` struct, initialize the field in `Options::to_settings`, and use `project.settings().respect_ignore_files()` here.


---

_Review comment by @MichaReiser on `crates/red_knot_project/src/walk.rs`:137 on 2025-04-25 06:39_

We can do this in another PR but I noticed that Ruff doesn't ignore hidden files. We should do the same.

---

_@MichaReiser requested changes on 2025-04-25 06:40_

Thank you. This is good. The only thing that needs changing before landing is that we introduce the same field on `Settings` and read it from there instead of from `Options`.

---

_Review request for @dcreager removed by @MichaReiser on 2025-04-25 06:40_

---

_Review request for @sharkdp removed by @MichaReiser on 2025-04-25 06:40_

---

_Review request for @AlexWaygood removed by @MichaReiser on 2025-04-25 06:40_

---

_Review request for @carljm removed by @MichaReiser on 2025-04-25 06:40_

---

_@thejchap reviewed on 2025-04-25 13:31_

---

_Review comment by @thejchap on `crates/red_knot/src/args.rs`:146 on 2025-04-25 13:31_

great - had imagined there was a more idiomatic way to do this, thanks!

---

_@thejchap reviewed on 2025-04-25 13:34_

---

_Review comment by @thejchap on `crates/red_knot_project/src/walk.rs`:137 on 2025-04-25 13:34_

@MichaReiser makes sense - added a new issue for that here https://github.com/astral-sh/ruff/issues/17626

---

_Review requested from @MichaReiser by @thejchap on 2025-04-26 01:22_

---

_@MichaReiser approved on 2025-04-26 09:45_

---

_Merged by @MichaReiser on 2025-04-26 10:02_

---

_Closed by @MichaReiser on 2025-04-26 10:02_

---

_Comment by @MichaReiser on 2025-04-26 10:27_

I've to revert this change for now. There's an issue with the path escaping on windows.

---

_Comment by @thejchap on 2025-04-26 19:39_

opened up a new pr [here](https://github.com/astral-sh/ruff/pull/17645)  to debug

---
