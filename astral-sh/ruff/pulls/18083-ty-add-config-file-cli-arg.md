```yaml
number: 18083
title: "[ty] Add --config-file CLI arg"
type: pull_request
state: merged
author: thejchap
labels:
  - cli
  - ty
assignees: []
merged: true
base: main
head: feature/config-file
created_at: 2025-05-14T01:55:30Z
updated_at: 2025-05-27T06:00:38Z
url: https://github.com/astral-sh/ruff/pull/18083
synced_at: 2026-01-12T15:56:12Z
```

# [ty] Add --config-file CLI arg

---

_@thejchap_

## Summary
https://github.com/astral-sh/ty/issues/217

- Adds a `--config-file` CLI option which overrides discovered configuration files
- Introduces `MetaOptions` (not very sold on the name...) that encapsulates both options passed by the CLI, as well as the config file override, which influences how `ProjectMetadata` gets discovered/created (which the CLI options then get applied to).
- Ensure the config file passed by this arg is watched for changes

## Test Plan
- Snapshot test
- Updated file watching tests to accept `MetaOptions` and tested watching for changes on the provided config file override
- Manual e2e testing

---

_@thejchap reviewed on 2025-05-14 01:58_

---

_Review comment by @thejchap on `crates/ty/tests/cli.rs`:1460 on 2025-05-14 01:58_

unrelated rename in an old test

---

_Comment by @github-actions[bot] on 2025-05-14 02:02_

<!-- generated-comment mypy_primer -->
## `mypy_primer` results
No ecosystem changes detected ✅


---

_Label `cli` added by @AlexWaygood on 2025-05-14 03:20_

---

_Label `ty` added by @AlexWaygood on 2025-05-14 03:20_

---

_Review requested from @sharkdp by @sharkdp on 2025-05-14 06:35_

---

_Review request for @sharkdp removed by @sharkdp on 2025-05-14 06:35_

---

_@thejchap reviewed on 2025-05-17 00:08_

---

_Review comment by @thejchap on `crates/ty_project/src/metadata.rs`:308 on 2025-05-17 00:08_

renamed to be more generic - includes errors encountered during discovery + errors when discovery is skipped and it's loaded from a specific config file

---

_Renamed from "[ty] Add --config-file" to "[ty] Add --config-file CLI arg" by @thejchap on 2025-05-17 00:17_

---

_Comment by @thejchap on 2025-05-17 00:43_

@MichaReiser still needs tests but ready for some feedback on the direction

---

_Marked ready for review by @thejchap on 2025-05-17 00:43_

---

_Review requested from @carljm by @thejchap on 2025-05-17 00:43_

---

_Review requested from @MichaReiser by @thejchap on 2025-05-17 00:43_

---

_Review requested from @AlexWaygood by @thejchap on 2025-05-17 00:43_

---

_Review requested from @sharkdp by @thejchap on 2025-05-17 00:43_

---

_Review requested from @dcreager by @thejchap on 2025-05-17 00:43_

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata.rs`:31 on 2025-05-17 15:27_

I'm not sure about this, but we could consider combining it with `extra_configuration_paths`, considering that they're exclusive to each other. 

---

_@MichaReiser reviewed on 2025-05-17 15:28_

The direction looks great to me and it's a nice scoped change. This is turning out great

---

_Review request for @AlexWaygood removed by @AlexWaygood on 2025-05-17 15:29_

---

_Review request for @carljm removed by @carljm on 2025-05-18 17:59_

---

_Comment by @MichaReiser on 2025-05-18 18:20_

@thejchap let me know when this is ready for review (a bit hard to tell because it was already ready for review).

---

_Converted to draft by @thejchap on 2025-05-18 18:26_

---

_Comment by @thejchap on 2025-05-18 18:29_

@MichaReiser converted back to draft, sorry about that - will mark ready for review once i get tests in

for the future - what's the preferred workflow for this type of situation? (ie. looking for feedback on in-progress pr, but pr isn't quite "merge ready")

---

_Comment by @MichaReiser on 2025-05-18 18:59_

No worries. The way you asked for early feedback was great

> for the future - what's the preferred workflow for this type of situation? (ie. looking for feedback on in-progress pr, but pr isn't quite "merge ready")

I tend to only look at PRs that are ready for review unless I get tagged. What I find most useful is if the PRs is put back into draft mode if you plan on making more changes before getting a review. This makes it very clear to me that the PR is ready for review when you change its status again. 

---

_Marked ready for review by @thejchap on 2025-05-23 05:42_

---

_Comment by @thejchap on 2025-05-23 05:42_

@MichaReiser ready for review

---

_Review request for @sharkdp removed by @sharkdp on 2025-05-23 07:22_

---

_Review comment by @MichaReiser on `crates/ty/src/args.rs`:115 on 2025-05-23 13:25_

This is probably a bit more detailed than necessary for most users. We can adopt uv's documentation

```
The path to a `uv.toml` file to use for configuration.
While uv configuration can be included in a `pyproject.toml` file, it is not allowed in this context.
```

UV also has a `TY_CONFIG_FILE` environment variable. I'd be open to support this too (as simple as telling clap to respect the env variable)



---

_Review comment by @MichaReiser on `crates/ty/src/lib.rs`:111 on 2025-05-23 13:27_

Can we use `system.current_directory` instead of passing `cwd`?

---

_Review comment by @MichaReiser on `crates/ty/tests/file_watching.rs`:1839 on 2025-05-23 13:30_


```suggestion
    // Enable division-by-zero in the explicitly specified configuration with warning severity
```

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:53 on 2025-05-23 13:31_

Is it necessary that we canonicalize the path here or could we avoid it?

---

_Review comment by @MichaReiser on `crates/ty_project/src/metadata/options.rs`:582 on 2025-05-23 13:33_

I agree, this name isn't great :)

How about:

* `ConfigurationOverrides`
* `OptionOverrides`
* `ProjectOptionsOverrides`. I think I like this best because these options override the project's options.

We could then rename `cli_options` to just `options` because I forsee that we'll need this for the LSP too 

---

_@MichaReiser approved on 2025-05-23 13:34_

This is excellent work. Thank you! I've a few small nit comments but this is otherwise good to go)

---

_Comment by @github-actions[bot] on 2025-05-26 16:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@thejchap reviewed on 2025-05-26 16:44_

---

_Review comment by @thejchap on `crates/ty_project/src/db/changes.rs`:53 on 2025-05-26 16:44_

@MichaReiser in testing it seemed like `ChangeEvent::system_path` ([here](https://github.com/thejchap/ruff/blob/555ce858403a04a13747074cc43a8c902784faca/crates/ty_project/src/watch.rs#L75)) was returning an absolute path, whereas the `config_file` arg from clap was a relative path, breaking the comparison

open to doing this elsewhere - perhaps once when `ProjectOptionsOverrides` gets instantiated so this work doesn't get done on every change?

---

_@MichaReiser reviewed on 2025-05-26 16:49_

---

_Review comment by @MichaReiser on `crates/ty_project/src/db/changes.rs`:53 on 2025-05-26 16:49_

It probably makes sense to create an absolute path when constructing the `config_file_onverride`. Not because I'm concerned of the cost, more because it's probably what I would expect (we overall try to convert relative paths to absolute paths at the "boundaries"). If it's only about constructing an absolute path (and not resolving symlinks), then use `SystemPath::absolute` instead of `system.canonicalize`. 

---

_@thejchap reviewed on 2025-05-27 02:21_

---

_Review comment by @thejchap on `crates/ty_project/src/db/changes.rs`:53 on 2025-05-27 02:21_

@MichaReiser makes sense - thanks! this is ready for review

---

_@MichaReiser approved on 2025-05-27 06:00_

---

_Merged by @MichaReiser on 2025-05-27 06:00_

---

_Closed by @MichaReiser on 2025-05-27 06:00_

---
