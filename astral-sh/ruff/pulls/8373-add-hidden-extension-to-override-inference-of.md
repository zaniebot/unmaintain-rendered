```yaml
number: 8373
title: "Add hidden `--extension` to override inference of source type from file extension"
type: pull_request
state: merged
author: felix-cw
labels:
  - cli
assignees: []
merged: true
base: main
head: felix-cw/cli-extension-map-linter
created_at: 2023-10-31T10:46:08Z
updated_at: 2023-11-08T02:32:41Z
url: https://github.com/astral-sh/ruff/pull/8373
synced_at: 2026-01-10T23:40:55Z
```

# Add hidden `--extension` to override inference of source type from file extension

---

_Pull request opened by @felix-cw on 2023-10-31 10:46_

## Summary

This PR addresses the incompatibility with `jupyterlab-lsp` + `python-lsp-ruff` arising from the inference of source type from file extension, raised in #6847.

In particular it follows the suggestion in https://github.com/astral-sh/ruff/issues/6847#issuecomment-1765724679 to specify a mapping from file extension to source type.

The source types are

- python
- pyi
- ipynb

Usage:

```sh
ruff check --no-cache --stdin-filename Untitled.ipynb --extension ipynb:python
```

Unlike the original suggestion, `:` instead of `=` is used to associate file extensions to language since that is what is used with `--per-file-ignores` which is an existing option that accepts a mapping.

## Test Plan

2 tests added to `integration_test.rs` to ensure the override works as expected

---

_Comment by @github-actions[bot] on 2023-10-31 10:59_

## PR Check Results
### Ecosystem
✅ ecosystem check detected no linter changes.




---

_Review requested from @charliermarsh by @charliermarsh on 2023-11-06 01:03_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/args.rs`:356 on 2023-11-06 01:05_

I'm somewhat partial to calling this `extension` rather than `extension-override`.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/commands/check.rs`:199 on 2023-11-06 01:06_

I think this should probably be on `settings`, rather than a standalone argument. That would also allow users to set this in their settings file (`pyproject.toml`, etc.), which seems important.

---

_@charliermarsh reviewed on 2023-11-06 01:06_

Thank you! I'm comfortable with this approach but can we try moving `extension` onto settings?

---

_@dhruvmanila reviewed on 2023-11-06 03:22_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/commands/check.rs`:199 on 2023-11-06 03:22_

Do we want to allow that as it's currently a hidden CLI argument?

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:205 on 2023-11-06 03:26_

nit: we could probably use a struct instead with a hidden field to hide the implementation details and provide a method to resolve the extension to the language. That way we could, if we want, change it or extend it in the future. This is totally fine as well :)

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:192 on 2023-11-06 03:30_

nit: we could probably implement `impl From<Language> for PySourceType` and use that here like `extension.get(ext).map(PySourceType::from)`.

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/settings/types.rs`:316 on 2023-11-06 03:32_

This is not a blocker but I don't think `Ipynb` is the correct term here as that's just an extension. Although I'm not sure what language name corresponds here. Pre-commit uses `jupyter` but we moved away from using that previously so I think this might be fine for now.

---

_@dhruvmanila approved on 2023-11-06 03:33_

Thanks for doing this! And sorry for the back and forth in the issue :)

---

_Review comment by @felix-cw on `crates/ruff_cli/src/commands/check.rs`:199 on 2023-11-06 09:47_

I went back and forth on this myself (see commit history!) but I thought if it was hidden it didn't make sense to be a setting, or possibly I couldn't see any examples of hidden settings so I didn't know how to make one.

---

_@felix-cw reviewed on 2023-11-06 09:47_

---

_@felix-cw reviewed on 2023-11-06 09:47_

---

_Review comment by @felix-cw on `crates/ruff_linter/src/settings/types.rs`:316 on 2023-11-06 09:47_

Could `Notebook` be the right word? Maybe it's not specific enough.  Equally, `Pyi` maybe should be `Stub`?

---

_@felix-cw reviewed on 2023-11-06 18:40_

---

_Review comment by @felix-cw on `crates/ruff_cli/src/args.rs`:356 on 2023-11-06 18:40_

Would you prefer it changed everywhere or just here and `CheckArguments`?
I used `extension_override` rather than `extension` just because it seemed more explicit to me.  Why do prefer `extension`?

---

_@dhruvmanila reviewed on 2023-11-07 02:51_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/commands/check.rs`:199 on 2023-11-07 02:51_

I think the `--line-length` is an example of a hidden CLI flag but can be configured via the [config file](https://docs.astral.sh/ruff/settings/#line-length):

https://github.com/astral-sh/ruff/blob/2d5ce4532ade11710ff854516cd085b9ea629cf2/crates/ruff_cli/src/args.rs#L274-L276

---

_@dhruvmanila reviewed on 2023-11-07 02:55_

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/settings/types.rs`:316 on 2023-11-07 02:55_

I think all of these file types contain Python syntax, although a Notebook might have additional syntax as well, so the language used is Python technically :)

It's fine to use the file extension as a fallback.

---

_@dhruvmanila reviewed on 2023-11-07 03:47_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:188 on 2023-11-07 03:47_

Sorry for not noticing this in my first review 😓 

How will this work if the cell contained IPython escape commands (`!pwd`, `%matplotlib inline`)? The parser mode is tied up with `PySourceType`. This mode is then used to allow the escape commands.

https://github.com/astral-sh/ruff/blob/2d5ce4532ade11710ff854516cd085b9ea629cf2/crates/ruff_python_parser/src/lib.rs#L325-L332

Now, if the cell contained escape commands and the override flag tells that this is a Python language (`--extension-override ipynb:python`), then it'll throw a syntax error.

---

_@felix-cw reviewed on 2023-11-07 10:07_

---

_Review comment by @felix-cw on `crates/ruff_cli/src/diagnostics.rs`:188 on 2023-11-07 10:07_

`jupyterlab-lsp` can  replace notebook magics with equivalent python code in most cases, so in this use case these syntax errors are avoided (I think the code is [here](https://github.com/jupyter-lsp/jupyterlab-lsp/blob/5a8686067bdeeb1f8ce5bc108d7fd047809b0f80/packages/jupyterlab-lsp/src/transclusions/ipython/overrides.ts#L58)).

Any python code affected by magic ends up in a string so doesn't get checked, for example with magics like `%time` and `%%time`.

---

_@felix-cw reviewed on 2023-11-07 10:12_

---

_Review comment by @felix-cw on `crates/ruff_cli/src/commands/check.rs`:199 on 2023-11-07 10:12_

Ah ok. `line_length` is still documented in `ruff_workspace`, so it's hidden in the CLI but visible in other documentation and in the schema.  Is that OK for this option?

---

_@felix-cw reviewed on 2023-11-07 10:17_

---

_Review comment by @felix-cw on `crates/ruff_cli/src/diagnostics.rs`:205 on 2023-11-07 10:17_

done

---

_@felix-cw reviewed on 2023-11-07 10:17_

---

_Review comment by @felix-cw on `crates/ruff_cli/src/diagnostics.rs`:192 on 2023-11-07 10:17_

done

---

_Comment by @github-actions[bot] on 2023-11-07 10:25_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@felix-cw reviewed on 2023-11-07 11:45_

---

_Review comment by @felix-cw on `crates/ruff_cli/src/args.rs`:356 on 2023-11-07 11:45_

renamed

---

_@felix-cw reviewed on 2023-11-07 11:45_

---

_Review comment by @felix-cw on `crates/ruff_cli/src/commands/check.rs`:199 on 2023-11-07 11:45_

I moved it to settings

---

_@dhruvmanila reviewed on 2023-11-07 13:13_

---

_Review comment by @dhruvmanila on `crates/ruff_cli/src/diagnostics.rs`:188 on 2023-11-07 13:13_

Thanks for letting us know about that.

Coming from a _public_ API perspective, this might still be a problem as, for example, if someone's trying to use it in a similar manner with an integration that doesn't perform those transformations, we'd throw an error.

I would love any other opinion on this. I'm fine with including the flag but if we include it in the config, then it's part of the public API which is where I'm unsure of.

---

_@felix-cw reviewed on 2023-11-07 16:20_

---

_Review comment by @felix-cw on `crates/ruff_cli/src/diagnostics.rs`:188 on 2023-11-07 16:20_

FWIW `jupyter nbconvert --to script` also seems to convert notebook magics to python in the same way. [`nbdev`](https://github.com/fastai/nbdev) has a routine callled `scrub_magics` which removes magics altogether, though it seems to be optional.

The docs could make it clear that `python` means `python` without notebookisms. Maybe something like:

> Users who use this option to lint code extracted from notebooks as python should ensure that the code should be free of IPython magics, as these will cause a syntax error.

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:191 on 2023-11-07 22:26_

You can likely remove this now?

---

_@charliermarsh reviewed on 2023-11-07 22:26_

---

_@charliermarsh reviewed on 2023-11-07 22:27_

---

_Review comment by @charliermarsh on `crates/ruff_cli/src/diagnostics.rs`:183 on 2023-11-07 22:27_

I believe this can be reduced to:

```rust
fn override_source_type(path: Option<&Path>, extension: &ExtensionMapping) -> Option<PySourceType> {
    let ext = path?.extension()?.to_str()?;
    extension.map_ext(ext).map(PySourceType::from)
}
```

---

_@charliermarsh reviewed on 2023-11-07 22:28_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/settings/types.rs`:320 on 2023-11-07 22:28_

Nit: use `anyhow::Result` instead here.

---

_@charliermarsh approved on 2023-11-07 22:35_

I pushed some nits, and I'll let @dhruvmanila approve and merge ultimately, but this looks good to me. I opted to remove the setting from `Options` for now, since @dhruvmanila is right that we only need to support it on the CLI -- but I do like that it's on `Settings` rather than a standalone argument, so I think that refactor was necessary anyway :)

---

_Label `cli` added by @charliermarsh on 2023-11-07 22:38_

---

_Renamed from "Add hidden `--extension-override` to override inference of source type from file extension" to "Add hidden `--extension` to override inference of source type from file extension" by @dhruvmanila on 2023-11-08 02:23_

---

_@dhruvmanila approved on 2023-11-08 02:32_

Thanks @felix-cw for contributing and welcome to the repo!

---

_Merged by @dhruvmanila on 2023-11-08 02:32_

---

_Closed by @dhruvmanila on 2023-11-08 02:32_

---
