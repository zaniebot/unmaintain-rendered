```yaml
number: 12947
title: Align supported Config-settings with example in docs
type: pull_request
state: merged
author: maxmynter
labels:
  - bug
assignees: []
merged: true
base: main
head: config-settings
created_at: 2025-04-17T15:13:39Z
updated_at: 2025-04-17T16:59:31Z
url: https://github.com/astral-sh/uv/pull/12947
synced_at: 2026-01-10T11:10:40Z
```

# Align supported Config-settings with example in docs

---

_Pull request opened by @maxmynter on 2025-04-17 15:13_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->
Closes #12929
## Summary
Untag the `config-settings` value to support JSON schema according to the [docs](https://docs.astral.sh/uv/reference/settings/#config-settings). 
```toml
[tool.uv]
config-settings = { editable_mode = "compat" }
```
<!-- What's the purpose of the change? What does it do, and why? -->

## Test Plan
Verified using the "Even Better TOML" extension with paths to old and new `uv.schema.json`.


## Notes
I could not reproduce the issue with either the `taplo` (on which Even Better Toml is built, afaik) and `check-jsonschema` CLI tools; with both old and new versions of the `uv.schema.json` validated the `pyproject.toml`.

Maybe for these there is some additional regularization going on and that's also how a breaking case ended up in the docs? 
I'm unsure on how to test for this.

After about an hour, the Even better TOML VSCode extension was the only way to reproduce failing validation.

Let me know if I can do something else.

<!-- How was it tested? -->


---

_@charliermarsh approved on 2025-04-17 16:57_

Awesome, thank you!

---

_Label `bug` added by @charliermarsh on 2025-04-17 16:57_

---

_Merged by @charliermarsh on 2025-04-17 16:57_

---

_Closed by @charliermarsh on 2025-04-17 16:57_

---

_Comment by @udifuchs on 2025-04-17 16:59_


> I could not reproduce the issue with either the `taplo` (on which Even Better Toml is built, afaik) and `check-jsonschema` CLI tools; with both old and new versions of the `uv.schema.json` validated the `pyproject.toml`.

My guess is that you were using the command:
```
check-jsonschema --schemafile uv.json
```
In that case, your test file should be without the `[tools.uv]` header:
```
config-settings = { editable_mode = "compat" }
```
The test examples I gave in the issue were for a `pyproject.toml`. To check it, you need to specify the pyproject schema file from the schemastore.

The reason the file did validate for you is that uv.json is missing an `"additionalProperties": false` at the top level. I am not if this is intentional or not.

---
