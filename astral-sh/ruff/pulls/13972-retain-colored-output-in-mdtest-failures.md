```yaml
number: 13972
title: "Retain colored output in `mdtest` failures"
type: pull_request
state: closed
author: charliermarsh
labels:
  - testing
  - ty
assignees: []
base: main
head: charlie/color
created_at: 2024-10-29T01:12:48Z
updated_at: 2024-11-12T21:39:43Z
url: https://github.com/astral-sh/ruff/pull/13972
synced_at: 2026-01-12T15:55:46Z
```

# Retain colored output in `mdtest` failures

---

_@charliermarsh_

## Summary

Rather than using the `"no-color"` feature, we can use `anstream` to strip the color codes, as we do in uv.

Closes https://github.com/astral-sh/ty/issues/235.


---

_Review requested from @MichaReiser by @charliermarsh on 2024-10-29 01:12_

---

_Review requested from @AlexWaygood by @charliermarsh on 2024-10-29 01:12_

---

_Label `testing` added by @charliermarsh on 2024-10-29 01:12_

---

_Label `red-knot` added by @charliermarsh on 2024-10-29 01:13_

---

_@charliermarsh reviewed on 2024-10-29 01:21_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters.py.snap`:49 on 2024-10-29 01:21_

Are these bugs in anstream? Or is there something strange going on with the invalid characters here?

---

_Comment by @github-actions[bot] on 2024-10-29 01:56_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters.py.snap`:49 on 2024-10-29 06:41_

Not sure. Could you take a look at the file in a hex viewer? Maybe the file ending changed or it's something else but I'm surprised that there are changed snapshots.

Anoter possibility is that your `cargo insta` version is too old/new


---

_@MichaReiser requested changes on 2024-10-29 06:44_

Thanks. This is much better than my hacky solution.

We should have a closer look at what's changing in the snapshot. 

---

_@sharkdp reviewed on 2024-10-29 08:20_

---

_Review comment by @sharkdp on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters.py.snap`:49 on 2024-10-29 08:20_

On `main`, this line contains a [substitute control character](https://en.wikipedia.org/wiki/Substitute_character) (`␚`, `^Z`; hex `\x1a`), which is not present on this branch:

![image](https://github.com/user-attachments/assets/272dba32-427b-43e4-901c-006fb9d265a3)

This character gets stripped by `anstream`, as demonstrated by this snippet:
```rs
let styled_text = "foo\x1abar";
let plain_str = anstream::adapter::strip_str(&styled_text).to_string();
assert_eq!(plain_str, "foobar");
```
Possibly because they use `0x1a` to switch to another mode here: https://github.com/rust-cli/anstyle/blob/5628f47a37e059d468c9ea2eab5ce69950e794e9/crates/anstyle-parse/src/state/codegen.rs#L59 — I haven't investigated further.

I don't know if this is a bug in anstream or expected behavior, but I think this reveals a problem with this strip-ansi-escape-sequences approach. Some of these snapshot files could contain actual (non-escaped) `1b 5b` escape sequences that should not be stripped.

`colored` offers a way to [disable colors at runtime](https://docs.rs/colored/latest/colored/control/struct.SHOULD_COLORIZE.html#method.set_override). Would that be an alternative approach?

---

_@MichaReiser reviewed on 2024-10-29 08:30_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters.py.snap`:49 on 2024-10-29 08:30_

> colored offers a way to [disable colors at runtime](https://docs.rs/colored/latest/colored/control/struct.SHOULD_COLORIZE.html#method.set_override). Would that be an alternative approach?

One issue with this approach is that the colorize "leaks" into other tests and it would disable colorized output in mdtests depending on the test-execution order.

---

_@MichaReiser reviewed on 2024-10-29 08:32_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters.py.snap`:49 on 2024-10-29 08:32_

> I don't know if this is a bug in anstream or expected behavior, but I think this reveals a problem with this strip-ansi-escape-sequences approach.

I do expect that this is intended because the strip stream strips any non-printable characters. I don't think this is necessarily desired in our case because we want to test and catch unprintable characters in our test outputs (we don't handle them today but we definitely should)

---

_@AlexWaygood reviewed on 2024-10-29 08:36_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pylint/snapshots/ruff_linter__rules__pylint__tests__PLE2510_invalid_characters.py.snap`:49 on 2024-10-29 08:36_

> `colored` offers a way to [disable colors at runtime](https://docs.rs/colored/latest/colored/control/struct.SHOULD_COLORIZE.html#method.set_override). Would that be an alternative approach?

This is how we currently disable colours in the tests for mdtest itself, FWIW:

https://github.com/astral-sh/ruff/blob/2fe203292aa17a34dc878684ad2f1003504ea7f5/crates/red_knot_test/src/matcher.rs#L367

Maybe we need to fix that as well in this PR if it leaks into other tests 

---

_Comment by @charliermarsh on 2024-10-29 12:44_

I think this is the right approach but I don't have a suggestion for fixing the non-printable characters, so unless folks want to merge I'd likely just close this for now.

---

_@MichaReiser reviewed on 2024-10-29 13:42_

An alterantive to what's proposed here is to avoid emitting ansi escape sequences in the first place by changing `TextEmitter` by a `colored` option. 

The emitter can internally use a style sheet that defines what color to use for different elements. The default style uses the colors that we use in the CLI but we can have a no-color style that defaults to black (and no bold). 

---

_Comment by @charliermarsh on 2024-10-30 02:57_

If we do that, we'll still need a separate solution for red-knot diagnostics which are more disparate (or we'd have to use anstream over there).

---

_Comment by @MichaReiser on 2024-10-30 07:45_

> If we do that, we'll still need a separate solution for red-knot diagnostics which are more disparate (or we'd have to use anstream over there).

That's correct. Ideally we would have less code that implements some sort of a diagnostic system and instead have a single one where we do all the color handling. Either way. I think both systems could use the same approach. We define a stylesheet somewhere and pass that through. 

---

_Comment by @MichaReiser on 2024-11-12 11:20_

I suggest that we revisit this issue after we have the new diagnostic system in place.

---

_Comment by @charliermarsh on 2024-11-12 21:39_

Sounds good.

---

_Closed by @charliermarsh on 2024-11-12 21:39_

---
