```yaml
number: 499
title: "CLI docs: autogeneration truncates some option descriptions"
type: issue
state: closed
author: brainwane
labels:
  - documentation
  - help wanted
assignees: []
created_at: 2025-05-23T16:05:30Z
updated_at: 2025-05-25T11:09:03Z
url: https://github.com/astral-sh/ty/issues/499
synced_at: 2026-01-12T15:54:23Z
```

# CLI docs: autogeneration truncates some option descriptions

---

_@brainwane_

### Summary

`docs/reference/cli.md` truncates the explanations of some (but not all) command-line options to `ty check` to the comments from a single code line, leaving out concluding punctuation and subsequent lines. Examples:

* https://github.com/astral-sh/ruff/blob/a1399656c9109293781405c8e33f4bf27f1b8694/crates/ty/src/args.rs#L325-L329 on `--config`:

~~~
/// A TOML `<KEY> = <VALUE>` pair
/// (such as you might find in a `ty.toml` configuration file)
/// overriding a specific configuration option.
/// Overrides of individual settings using this option always take precedence
/// over all configuration files.
~~~
 
 truncated in the [output](https://github.com/astral-sh/ruff/blob/main/crates/ty/docs/cli.md#ty-check--config) to just: "A TOML `KEY = VALUE` pair"

* https://github.com/astral-sh/ruff/blob/a1399656c9109293781405c8e33f4bf27f1b8694/crates/ty/src/args.rs#L286-L289 and https://github.com/astral-sh/ruff/blob/a1399656c9109293781405c8e33f4bf27f1b8694/crates/ty/src/args.rs#L293-L298 , on `--output-format`, similarly truncated in the [output](https://github.com/astral-sh/ruff/blob/main/crates/ty/docs/cli.md#ty-check--output-format)

This *doesn't* happen with [`--python`](https://github.com/astral-sh/ruff/blob/main/crates/ty/docs/cli.md#ty-check--python) and several other items, so I presume the inconsistent behavior is unwanted.

### Version

(not applicable)

---

_Label `documentation` added by @MichaReiser on 2025-05-23 16:06_

---

_Label `help wanted` added by @MichaReiser on 2025-05-23 16:07_

---

_Comment by @MichaReiser on 2025-05-23 16:07_

Huh, nice find. no, this not intentional. This would need to be fixed in https://github.com/astral-sh/ruff/blob/9ae698fe30cf3526f0e7ae237b800b3ed19a819f/crates/ruff_dev/src/generate_ty_cli_reference.rs#L1-L0

---

_Closed by @MichaReiser on 2025-05-25 11:09_

---
