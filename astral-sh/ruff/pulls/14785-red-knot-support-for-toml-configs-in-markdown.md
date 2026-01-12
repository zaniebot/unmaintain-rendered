```yaml
number: 14785
title: "[red-knot] Support for TOML configs in Markdown tests"
type: pull_request
state: merged
author: sharkdp
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: david/mdtest-toml-config
created_at: 2024-12-05T10:00:33Z
updated_at: 2024-12-06T09:22:10Z
url: https://github.com/astral-sh/ruff/pull/14785
synced_at: 2026-01-12T15:55:49Z
```

# [red-knot] Support for TOML configs in Markdown tests

---

_@sharkdp_

## Summary

This adds support for specifying the target Python version from a Markdown test. It is a somewhat limited ad-hoc solution, but designed to be future-compatible. TOML blocks can be added to arbitrary sections in the Markdown block. They have the following format:

````markdown
```toml
[tool.knot.environment]
target-version = "3.13"
```
````

So far, there is nothing else that can be configured, but it should be straightforward to extend this to things like a custom typeshed path.

This is in preparation for the statically-known branches feature where we are going to have to specify the target version for lots of tests.

## Test Plan

- New Markdown test that fails without the explicitly specified `target-version`.
- Manually tested various error paths when specifying a wrong `target-version` field.
- Made sure that running tests is as fast as before.

---

_Label `red-knot` added by @sharkdp on 2024-12-05 10:00_

---

_Review requested from @carljm by @sharkdp on 2024-12-05 10:00_

---

_Review requested from @MichaReiser by @sharkdp on 2024-12-05 10:00_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-12-05 10:00_

---

_@MichaReiser approved on 2024-12-05 10:03_

I'd suggest waiting on @carljm review. I remember he had concerns around exposing the "full" configuration. 

I'd remove the `tool.knot` prefix for a more concise configuration similar to the format we'd use when configuring Red Knot in a `knot.toml`

---

_@sharkdp reviewed on 2024-12-05 10:04_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/lib.rs`:32 on 2024-12-05 10:04_

Makes it possible to see the full error trace, not just the topmost context. For example:
```
Error parsing `/home/shark/ruff/crates/red_knot_python_semantic/resources/mdtest/sys_version_info_310.md`: Error while parsing Markdown TOML config

Caused by:
    TOML parse error at line 2, column 23
      |
    2 | target-version = "3.13
      |                       ^
    invalid basic string
```

---

_Comment by @github-actions[bot] on 2024-12-05 10:09_

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

_Comment by @sharkdp on 2024-12-05 10:14_

> I'd remove the `tool.knot` prefix for a more concise configuration similar to the format we'd use when configuring Red Knot in a `knot.toml`

The documentation in https://github.com/astral-sh/ruff/tree/main/crates/red_knot_test#configuration suggested that we want these blocks to contain a `pyproject.toml`-like config with a `[tool.knot]` section. I'm personally happy with both solutions, I don't mind the additional `tool.knot.` prefix too much. Maybe we want to support both eventually (using `file=knot.toml` or `file=pyproject.toml`?)

---

_Comment by @MichaReiser on 2024-12-05 10:22_

> The documentation in [main/crates/red_knot_test#configuration](https://github.com/astral-sh/ruff/tree/main/crates/red_knot_test?rgh-link-date=2024-12-05T10%3A14%3A18Z#configuration) suggested that we want these blocks to contain a pyproject.toml-like config with a [tool.knot] section. I'm personally happy with both solutions, I don't mind the additional tool.knot. prefix too much. Maybe we want to support both eventually (using file=knot.toml or file=pyproject.toml?)

I'd prefer to default to a `knot.toml` configuration and only use the `pyproject.toml` layout when `file=pyproject.toml` (if there's even a use case for it because Carl argued in his proposal that end-to-end tests might be a better fit outside of mdtests and resolving options from the `pyproject.toml` (like `requires-python`) seems more like an end-to-end test case

---

_Comment by @sharkdp on 2024-12-05 10:33_

Okay, I collapsed it down to a `knot.toml`-like configuration for now and adapted the documentation. If someone wants to see the original version, the first commit is still there.

---

_Label `testing` added by @AlexWaygood on 2024-12-05 11:28_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:6 on 2024-12-05 15:58_

hmm... not the end of the world, but on the red-knot CLI we set a target version using `--target-version=py39` rather than `--target-version="3.9"`. Similarly for Ruff, the option is `--target-version=py39` and in a configuration file it's `target-version = "py39"`.

I don't actually have a strong opinion on which is better, but it'd be nice to keep the red-knot CLI and the configuration setting here in sync, or it's hard to remember the difference between the two.

---

_@carljm approved on 2024-12-05 15:58_

Nice, thank you!

I think it shouldn't be too hard to make this hierarchical as mentioned in the design doc, but it's fine to start with one-per-file. My main concern is that people may put it nested and misunderstand which tests it will apply to. Ideally IMO if we only support one-per-file we would error if it appears anywhere other than top-level. But if that's hard to do I don't want it to be a blocker.

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info_310.md`:4 on 2024-12-05 16:00_

nit: It might be good to be a bit more explicit that this is also a unit test for the `red_knot_test` crate in some ways.

---

_@AlexWaygood reviewed on 2024-12-05 16:00_

Nice!

---

_@MichaReiser reviewed on 2024-12-05 16:01_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:6 on 2024-12-05 16:01_

uv uses `3.9` and it's what I proposed in the CLI document

---

_@AlexWaygood reviewed on 2024-12-05 16:02_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:6 on 2024-12-05 16:02_

I see. In that case I'm fine with this, but we should open an issue to change how the CLI works.

---

_@sharkdp reviewed on 2024-12-05 16:37_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info_310.md`:4 on 2024-12-05 16:37_

This is exactly what this comment was all about, but I can try to make it more explicit :smile: 

---

_@AlexWaygood reviewed on 2024-12-05 16:39_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info_310.md`:4 on 2024-12-05 16:39_

Yeah I think all I'm really asking for is

```suggestion
This test makes sure that `red_knot_test` correctly parses the `target-version` option in a
toml configuration block. See `sys_version_info.md` for the actual tests for
`sys.version_info`.
```

---

_@AlexWaygood approved on 2024-12-05 16:40_

---

_@MichaReiser reviewed on 2024-12-05 17:44_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:6 on 2024-12-05 17:44_

I plan to scope out the CLI work once the proposal is in a good shape

---

_@AlexWaygood reviewed on 2024-12-05 17:46_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:6 on 2024-12-05 17:46_

However it ends up, we need to make sure that we remember to align the two though

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:6 on 2024-12-05 17:58_

Absolutely. Consider me responsible for it

---

_@MichaReiser reviewed on 2024-12-05 17:58_

---

_@MichaReiser reviewed on 2024-12-05 17:58_

---

_Review comment by @MichaReiser on `crates/red_knot_python_semantic/resources/mdtest/sys_version_info.md`:6 on 2024-12-05 17:58_

Absolutely. Consider me responsible for it

---

_Renamed from "[red-knot] Preliminary support for TOML configs in Markdown tests" to "[red-knot] Support for TOML configs in Markdown tests" by @sharkdp on 2024-12-06 08:57_

---

_Comment by @sharkdp on 2024-12-06 09:13_

> I think it shouldn't be too hard to make this hierarchical as mentioned in the design doc

You're right. I didn't realize that we could simply change the Python version using `Program::get(…).set_target_version(…).to(…)` (thanks Micha), and didn't want to re-create the salsa DB for every test, because that resulted in a 3x slowdown.

Multiple (hierarchical) configurations are now fully supported. I verified that the whole test suite still runs in the same time as before.

---

_@sharkdp reviewed on 2024-12-06 09:19_

---

_Review comment by @sharkdp on `crates/red_knot_test/src/parser.rs`:186 on 2024-12-06 09:19_

I found this function name confusing. It's the current section's ID that's on top of the stack, not the ID of the parent section. When we see a new heading in the parser, this now changes the code to:
```rs
let parent = self.stack.top(); // instead of self.stack.parent()

let section = Section {
    // …
    parent_id: Some(parent),
};

// …

let section_id = self.sections.push(section);
self.stack.push(section_id);

// now `self.stack.top()` refers to the child section
```
but this seems sensible to me. Before we push the new section, the current section is the parent section.

---

_Merged by @sharkdp on 2024-12-06 09:22_

---

_Closed by @sharkdp on 2024-12-06 09:22_

---

_Branch deleted on 2024-12-06 09:22_

---
