```yaml
number: 13747
title: "[red-knot] Add a build.rs file to `red_knot_python_semantic`, and document pitfalls of using `rstest` in combination with `mdtest`"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - testing
  - ty
assignees: []
merged: true
base: main
head: alex/build-rs
created_at: 2024-10-14T11:07:14Z
updated_at: 2024-10-14T18:23:38Z
url: https://github.com/astral-sh/ruff/pull/13747
synced_at: 2026-01-12T15:55:45Z
```

# [red-knot] Add a build.rs file to `red_knot_python_semantic`, and document pitfalls of using `rstest` in combination with `mdtest`

---

_@AlexWaygood_

## Summary

Fixes #13732. `rstest` generates tests at build time rather than at runtime, so it doesn't necessarily notice new Markdown tests being added to a `resources` directory unless you add some mechanism to force a rebuild of the crate. The [`rstest` docs](https://docs.rs/rstest/0.23.0/rstest/attr.rstest.html#files-path-as-input-arguments) recommend using a `build.rs` script for this purpose; I've done so here for the `red_knot_python_semantic` crate, and added some docs to the `red_knot_test` README so that we don't forget to do so when adding `mdtest`-based tests to other red-knot crates.

## Test Plan

I locally tested by adding a new Markdown test file to `crates/red_knot_python_semantic/resources/mdtest` with some deliberate errors. I then ran `cargo test -p red_knot_python_semantic` (without having made any other changes to the crate that would have triggered a rebuild). The `build.rs` file successfully forced a rebuild, and the test correctly failed.

~~Here's a screenshot of how the `NOTE` block I've added to the README is rendered on GitHub:~~

<details>
<summary>Screenshot</summary>

![image](https://github.com/user-attachments/assets/830cbf2b-7237-4be1-ab61-09c70d5b076c)

</details>

~~Unfortunately it doesn't seem to be universal Markdown syntax, e.g. VSCode doesn't render the `NOTE` block in the same way.~~

(Edit: I abandoned the fancy Markdown because our linters hated it.)

---

_Label `red-knot` added by @AlexWaygood on 2024-10-14 11:07_

---

_Review requested from @carljm by @AlexWaygood on 2024-10-14 11:07_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-10-14 11:07_

---

_Label `testing` added by @AlexWaygood on 2024-10-14 11:08_

---

_Comment by @github-actions[bot] on 2024-10-14 11:22_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @AlexWaygood on 2024-10-14 11:26_

> Here's a screenshot of how the `NOTE` block I've added to the README is rendered on GitHub:

Ugh, `mdformat` doesn't like this. I tried adding these plugins to `.pre-commit-config.yaml`:

```diff
diff --git a/.pre-commit-config.yaml b/.pre-commit-config.yaml
index 710e128b2..92330acef 100644
--- a/.pre-commit-config.yaml
+++ b/.pre-commit-config.yaml
@@ -29,6 +29,8 @@ repos:
           - mdformat-mkdocs
           - mdformat-admon
           - mdformat-footnote
+          - mdformat-gfm
+          - mdformat-gfm-alerts
         exclude: |
           (?x)^(
             docs/formatter/black\.md
```

but then mdformat fails to parse some other files (I think probably because mkdocs-flavoured Markdown and GitHub-flavoured Markdown are different, so `mdformat-mkdocs` and `mdformat-gfm` are pulling it in different directions?

```
Error: Could not format "/Users/alexw/dev/ruff/crates/red_knot/docs/tracing.md".

The formatted Markdown renders to different HTML than the input Markdown. This
is likely a bug in mdformat. Please create an issue report here, including the
input Markdown: https://github.com/executablebooks/mdformat/issues
```

I could use embedded HTML, but that seems over the top; I guess I'll just not be fancy.

---

_@sharkdp reviewed on 2024-10-14 11:40_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/build.rs`:3 on 2024-10-14 11:40_

I ran into #13732 as well this morning, so I appreciate that we try to fix it. But it looks like doing this with a glob forces a recompilation *every time*, even if nothing has changed in the Markdown files(?).

When working on these tests, most of the time we're probably just editing Markdown files, not adding new ones. With this change, we make all of of those `cargo test`/`cargo test mdtest`/… calls much slower. We can probably workaround this locally by running the test executable directly (not via `cargo`). But that's another thing that developers would need to be taught :face_with_diagonal_mouth: 

---

_@AlexWaygood reviewed on 2024-10-14 11:50_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/build.rs`:3 on 2024-10-14 11:50_

> I ran into #13732 as well this morning, so I appreciate that we try to fix it. But it looks like doing this with a glob forces a recompilation _every time_, even if nothing has changed in the Markdown files(?).

Great catch! It looks like maybe we're not really meant to use globs here. (The [`build.rs` docs](https://doc.rust-lang.org/cargo/reference/build-scripts.html#rerun-if-changed) don't really seem to mention whether you are or aren't.) I pushed https://github.com/astral-sh/ruff/pull/13747/commits/af0466023e9a9503287c89fa95d4d764e7779aa2 which just points straight to the `resources/mdtest` directory -- for me locally, it now rebuilds the crate if a file _changes_ in the `resources/mdtest` directory, but not otherwise. This means that the crate will also be rebuilt if non-Markdown files are added/removed from `resources/mdtest`, but there's unlikely to be many of those anyway (certainly they won't be added/removed on a regular basis).

> When working on these tests, most of the time we're probably just editing Markdown files, not adding new ones. With this change, we make all of of those `cargo test`/`cargo test mdtest`/… calls much slower.

Yeah, that's harder to fix :/ Luckily recompilation still seems to be faster for me locally than it was using our previous test "framework" that you see for the existing tests in `infer.rs`. I think the slowdown in having to rebuild the crate to run the tests is a _reasonable_ price to pay for avoiding the confusion from #13732 -- but I do agree it's not ideal.

---

_@sharkdp reviewed on 2024-10-14 11:57_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/build.rs`:3 on 2024-10-14 11:57_

> It looks like maybe we're not really meant to use globs here

Ah, yes. That was the problem. I can confirm that it works as intended now :+1: 

> I think the slowdown in having to rebuild the crate to run the tests is a _reasonable_ price to pay for avoiding the confusion from #13732

Agreed, #13732 should be fixed. And I don't have any other ideas on how to solve it... except for a hard-coded list of Markdown files somewhere, and that would be a big downside in terms of ergonomics.

---

_@sharkdp approved on 2024-10-14 12:00_

---

_Merged by @AlexWaygood on 2024-10-14 12:02_

---

_Closed by @AlexWaygood on 2024-10-14 12:02_

---

_Branch deleted on 2024-10-14 12:02_

---

_Comment by @carljm on 2024-10-14 18:23_

Looks great, thank you!!

---
