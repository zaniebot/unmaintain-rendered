```yaml
number: 10135
title: "Explicitly ban overriding `extend` as part of a --config flag"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - configuration
  - cli
assignees: []
merged: true
base: main
head: config-extend-cli
created_at: 2024-02-26T13:10:21Z
updated_at: 2024-07-01T10:29:59Z
url: https://github.com/astral-sh/ruff/pull/10135
synced_at: 2026-01-12T15:55:31Z
```

# Explicitly ban overriding `extend` as part of a --config flag

---

_@AlexWaygood_

## Summary

Relates to #10035. Attempting to override `extend` as part of a --config flag doesn't work because we currently apply the configuration overrides given in the `--config` flag _after_ any configuration options found in any `pyproject.toml`/`ruff.toml` files have been parsed and validated. But this particular override affects which config files ruff should be looking at in the first place, so we'd need to look for it at an earlier stage in the process.

Currently we just silently accept the argument but just don't do anything with it; this PR changes that so that we explicitly error out if a user tries to override `extend` using the `--config` flag. Longer term, we could possibly think about reworking how `--config` works so that it _is_ possible to override `extend` using `--config`; or, we could add a separate option so that people can override `extend` properly via the command line.

## Test Plan

`cargo test`, and manual testing


---

_Review comment by @AlexWaygood on `crates/ruff/tests/lint.rs`:1 on 2024-02-26 13:11_

This should also ban passing `extend` as part of a `--config` flag when doing `ruff format`, but I didn't bother adding an equivalent test in `crates/ruff/tests/format.rs`, as it just felt duplicative. Lmk if you disagree!

---

_@AlexWaygood reviewed on 2024-02-26 13:11_

---

_Label `configuration` added by @MichaReiser on 2024-02-26 13:12_

---

_Label `cli` added by @MichaReiser on 2024-02-26 13:12_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:767 on 2024-02-26 13:18_

Nit: Implementing `Display` is mandatory for proper "Error" types that also implement the `Error` trait. That's why I liked the old implementation more (might also be worth to implement `impl Error for InvalidConfligFlagError`). 

There's still a way for you to get to the error message by using `error.to_string()` (`to_string` is implemented by all types that implement `Display`. 

The downside is that it possibly requires an allocation. I think that's fine, considering that we generate at most one error ;)

---

_@MichaReiser approved on 2024-02-26 13:18_

---

_Comment by @MichaReiser on 2024-02-26 13:19_

This is, in theory, a breaking change because it could break invocations like `ruff check --config extend=test.toml`. I think it's okay to move forward with this change, considering that it never worked the way the user intended.

---

_Comment by @github-actions[bot] on 2024-02-26 13:23_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+1 -0 violations, +0 -0 fixes in 1 projects; 42 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+1 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/pandas-dev/pandas/blob/edd45f68c196334734125f330fbd62de20dfb6d3/pandas/_libs/json.pyi#L7'>pandas/_libs/json.pyi:7:5:</a> PLR0917 Too many positional arguments (8/5)
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| PLR0917 | 1 | 1 | 0 | 0 | 0 |

</p>
</details>

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
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
error: Failed to read examples/How_to_handle_rate_limits.ipynb: Expected a Jupyter Notebook, which must be internally stored as JSON, but this file isn't valid JSON: trailing comma at line 47 column 4
```

</p>
</details>




---

_@AlexWaygood reviewed on 2024-02-26 15:32_

---

_Review comment by @AlexWaygood on `crates/ruff/src/args.rs`:767 on 2024-02-26 15:32_

So, the reason why I got rid of the `Display` implementation is because I wanted to make invalid states unrepresentable in `ConfigArgumentParser::parse_ref`. With the new code structure, it's impossible to proceed beyond line 866 if we have an enum variant that doesn't have an underlying error -- and that's good, because those need to be treated differently when describing them in words than the enum variants that do have underlying errors. But if I implemented `Display` for this enum, it would work for all the variants alike, which actually feels like a bit of a footgun here.

Really this enum isn't an error type at all (we never wrap it with an `Err` and return it), just an enumeration of the various kinds of failure modes we could have had when trying to parse the a `--config` flag as TOML. Possibly it should just be renamed... but I'm still pretty new to Rust error-handling patterns :) What do you think?

---

_@MichaReiser reviewed on 2024-02-26 15:39_

---

_Review comment by @MichaReiser on `crates/ruff/src/args.rs`:767 on 2024-02-26 15:39_

I see. Maybe name it `InvalidConfigFlagReason` to make it clear that it's never used in an `Err` (which I assumed is the case)

---

_Merged by @AlexWaygood on 2024-02-26 16:07_

---

_Closed by @AlexWaygood on 2024-02-26 16:07_

---

_Branch deleted on 2024-07-01 10:29_

---
