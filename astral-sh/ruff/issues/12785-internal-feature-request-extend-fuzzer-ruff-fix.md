```yaml
number: 12785
title: "Internal feature request: extend fuzzer `ruff_fix_validity` to non-default linter settings"
type: issue
state: open
author: dylwil3
labels:
  - internal
  - fuzzer
assignees: []
created_at: 2024-08-09T13:21:43Z
updated_at: 2024-08-12T05:44:07Z
url: https://github.com/astral-sh/ruff/issues/12785
synced_at: 2026-01-12T15:54:52Z
```

# Internal feature request: extend fuzzer `ruff_fix_validity` to non-default linter settings

---

_@dylwil3_

The fuzzer looks very cool and I'd love to use it!

When adding new preview rules, it would help catch edge cases faster if we were able to fuzz test against them. My understanding of the fuzzer documentation (which could be wrong!) is that it is not currently possible to do this.

Related to #4972.

---

_Comment by @dhruvmanila on 2024-08-09 13:56_

Yes, that's correct. The fuzzer uses the default linter settings:

https://github.com/astral-sh/ruff/blob/1f51048fa4a5954526e68a45cd4781040b502254/fuzz/fuzz_targets/ruff_fix_validity.rs#L18-L19

Is the feature request to manually create a config which the fuzzer uses? I think this would be really useful. While I was working on the parser, I manually updated the default settings in the code to reflect the config I wanted.

We could either use a command-line flag (if it's even possible) or an environment variable (`RUFF_FUZZ_CONFIG`).

---

_Label `internal` added by @dhruvmanila on 2024-08-09 13:56_

---

_Label `fuzzer` added by @dhruvmanila on 2024-08-09 13:56_

---

_Comment by @dylwil3 on 2024-08-10 14:13_

> Is the feature request to manually create a config which the fuzzer uses?

Yes, exactly!

The tasks it would be nice to easily perform would be:
1. Fuzz with _all_ (compatible) rules, including preview rules, turned on.
2. Fuzz with just a single new rule turned on.
3. (1) and (2) but with different target version.
4. (1) and (2) but iterated through all possible (or maybe randomly generated if the set is prohibitively large) auxiliary settings.

I'm not super familiar with `cargo fuzz`, but it seems like (1) would be the simplest to implement because it could just be a separate fuzzer (alternatively, we could replace the default linter settings in `ruff_fix_validity` with this setting).

The remaining ones seem more difficult... could [this section of the docs](https://rust-fuzz.github.io/book/cargo-fuzz/structure-aware-fuzzing.html) be relevant, or am I not understanding it very well?

I'm agnostic about whether this is callable via a command-line flag, environment variable, or even a reference to a `ruff.toml` file.

---

_Comment by @dhruvmanila on 2024-08-12 05:44_

> I'm agnostic about whether this is callable via a command-line flag, environment variable, or even a reference to a `ruff.toml` file.

The reason I suggested a command-line flag or an environment variable is because we don't run any fuzzer as part of the CI. It needs to be run locally. This means it might be more useful (and easier?) to just read a local config file which would be git-ignored but can be used to run the fuzzer for a limited time locally to catch any bugs if the contributor wants to.

---
