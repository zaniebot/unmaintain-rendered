```yaml
number: 11265
title: "Split `add_noqa` process into distinctive edit generation and edit application stages"
type: pull_request
state: merged
author: snowsignal
labels:
  - linter
assignees: []
merged: true
base: main
head: jane/linter/noqa-edits
created_at: 2024-05-03T16:04:11Z
updated_at: 2024-05-10T23:22:15Z
url: https://github.com/astral-sh/ruff/pull/11265
synced_at: 2026-01-10T22:05:26Z
```

# Split `add_noqa` process into distinctive edit generation and edit application stages

---

_Pull request opened by @snowsignal on 2024-05-03 16:04_

## Summary

`--add-noqa` now runs in two stages: first, the linter finds all diagnostics that need noqa comments and generate edits on a per-line basis. Second, these edits are applied, in order, to the document.

A public-facing function, `generate_noqa_edits`, has also been introduced, which returns noqa edits generated on a per-diagnostic basis. This will be used by `ruff server` for noqa comment quick-fixes.

## Test Plan

Unit tests have been updated.


---

_Label `linter` added by @snowsignal on 2024-05-03 16:04_

---

_Comment by @github-actions[bot] on 2024-05-03 16:17_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-05-03 16:18_

Would you mind running some hyperfine benchmarks on this change. The `Contributing.md` explains how. I'm asking because hashing `Diagnostic` can be somewhat expensive. 

I wonder if we should do some manual testing. What's your experience @charliermarsh with this kind of change? 


I won't have time to review this today. I plan to look into it on Monday.

---

_Comment by @charliermarsh on 2024-05-03 19:56_

Is there an LSP PR that I can look at in tandem to understand how the API will be used?

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/noqa.rs`:1165 on 2024-05-03 20:06_

Is this necessary? (Is the type not inferred from the RHS?)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/noqa.rs`:27 on 2024-05-03 20:06_

Rustdoc please :)

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/noqa.rs`:31 on 2024-05-03 20:10_

I can't know without seeing the usage, but could this instead return `Vec<Option<Edit>>`, so that we don't need to hash? I.e., return a parallel array that can be zipped with `diagnostics`?

---

_@charliermarsh reviewed on 2024-05-03 20:10_

---

_Comment by @charliermarsh on 2024-05-03 20:11_

I think `--add-noqa` has effectively no tests... @snowsignal, would you minding first opening a PR on main to add a few basic tests to `crates/ruff/tests/lint.rs`?

---

_Comment by @snowsignal on 2024-05-04 01:10_

@charliermarsh Sure, I can do that 😄 

---

_Review comment by @snowsignal on `crates/ruff_linter/src/noqa.rs`:31 on 2024-05-04 01:11_

I think that could work!

---

_@snowsignal reviewed on 2024-05-04 01:11_

---

_Comment by @snowsignal on 2024-05-04 01:33_

> Is there an LSP PR that I can look at in tandem to understand how the API will be used?

I've opened it [here](https://github.com/astral-sh/ruff/pull/11276).

---

_@snowsignal reviewed on 2024-05-04 01:35_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/noqa.rs`:1165 on 2024-05-04 01:35_

Nope, not necessary 😅 

---

_Comment by @charliermarsh on 2024-05-04 02:11_

> @charliermarsh Sure, I can do that 😄

Thanks, ultimately it will get this merged faster if we have some test coverage.

---

_@charliermarsh reviewed on 2024-05-04 13:33_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/noqa.rs`:569 on 2024-05-04 13:33_

What about something like...
```rust
let edits = build_noqa_edits_by_line(
    comments
        .into_iter()
        .flatten()
        .fold(BTreeMap::default(), |mut map, comment| {
            map.entry(comment.line)
                .or_insert_with(Vec::default)
                .push(comment);
            map
        }),
    locator,
    line_ending,
);
```

So the sorting, grouping, etc. is handled by the map creation, rather than beforehand?

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:46 on 2024-05-06 08:51_

Cloning the Directives doesn't seem to simplify the code because `NoqaComment` requires lifetimes anyway. That's why I recommend removing the `Clone` and instead work with references

```patch
Subject: [PATCH] Remove num-cpus dependency
---
Index: crates/ruff_linter/src/noqa.rs
IDEA additional info:
Subsystem: com.intellij.openapi.diff.impl.patch.CharsetEP
<+>UTF-8
===================================================================
diff --git a/crates/ruff_linter/src/noqa.rs b/crates/ruff_linter/src/noqa.rs
--- a/crates/ruff_linter/src/noqa.rs	(revision a4e8a3948d406c40d6a31cdbfb23a9b89757eb6d)
+++ b/crates/ruff_linter/src/noqa.rs	(date 1714985389072)
@@ -43,7 +43,7 @@
 
 /// A directive to ignore a set of rules for a given line of Python source code (e.g.,
 /// `# noqa: F401, F841`).
-#[derive(Debug, Clone)]
+#[derive(Debug)]
 pub(crate) enum Directive<'a> {
     /// The `noqa` directive ignores all rules (e.g., `# noqa`).
     All(All),
@@ -232,7 +232,7 @@
     }
 }
 
-#[derive(Debug, Clone)]
+#[derive(Debug)]
 pub(crate) struct Codes<'a> {
     range: TextRange,
     codes: Vec<Code<'a>>,
@@ -634,7 +634,7 @@
             continue;
         }
         let line = locator.full_line(offset);
-        let directive = matches.first().unwrap().directive.clone();
+        let directive = matches.first().unwrap().directive;
         let rules: Vec<_> = matches
             .into_iter()
             .map(|NoqaComment { diagnostic, .. }| diagnostic.kind.rule())
@@ -656,7 +656,7 @@
 struct NoqaComment<'a> {
     line: TextSize,
     diagnostic: &'a Diagnostic,
-    directive: Option<Directive<'a>>,
+    directive: Option<&'a Directive<'a>>,
 }
 
 fn find_noqa_comments_by_line<'a>(
@@ -722,7 +722,7 @@
                         comments_by_line.push(Some(NoqaComment {
                             line: directive_line.start(),
                             diagnostic,
-                            directive: Some(directive.clone()),
+                            directive: Some(directive),
                         }));
                     }
                     continue;
@@ -742,7 +742,7 @@
 }
 
 fn generate_noqa_edit(
-    directive: Option<Directive>,
+    directive: Option<&Directive>,
     offset: TextSize,
     rules: RuleSet,
     line: &str,
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:569 on 2024-05-06 08:53_

You both for sure love inline expressions :D 

I think it would help readability by moving whatever that thing collects or folds out into a variable. We could then even use a regular loop instead of `fold`. 

I would probably even move the construction of this `comments_by_line` struct into `build_noqa_edits_by_line`, considering that it isn't used anywhere else. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:638 on 2024-05-06 09:04_


```suggestion
        let Some(first_match) = matches.first() else {
            continue;
        };
        let line = locator.full_line(offset);
        let directive = first_match.directive;
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:646 on 2024-05-06 09:07_

Nit: `RuleSet` implements `FromIter`
```suggestion
        let rules: RuleSet = matches
            .into_iter()
            .map(|NoqaComment { diagnostic, .. }| diagnostic.kind.rule())
            .collect();
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:647 on 2024-05-06 09:12_

I think I would avoid passing `line` and `line_start` to `generate_noqa_edit` and instead move the logic to determining the entire line to `generate_noqa_edit`. It reduces the number of parameters and ensures that the `generate_noqa_edit` is in full control. 

You can also avoid scanning the line multiple times by using `let full_line_range = locator.full_line_range(offset);` and then use the `locator.slice(full_line_range)` if you need access to the text

It further enables optimisations where reading the full text isn't even necessary in most code paths (you can get the length by calling full_line_range.len()).

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:656 on 2024-05-06 09:14_

I think this should be called `find_noqa_comments`. It doesn't seem to group comments by line any more. 
```suggestion
fn find_noqa_comments_by_line<'a>(
    diagnostics: &'a [Diagnostic],
    locator: &'a Locator,
    exemption: &Option<FileExemption>,
    directives: &'a NoqaDirectives,
    noqa_line_for: &NoqaMapping,
) -> Vec<Option<NoqaComment<'a>>> {
    // List of noqa comments
    let mut comments_by_line: Vec<Option<NoqaComment<'a>>> = vec![];
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:752 on 2024-05-06 09:22_


```suggestion
    let mut edit_content = String::with_capacity(line.len());
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:744 on 2024-05-06 09:23_

Should we make `NoqaEdit` a struct instead with two functions: 

* `write`: It writes the edit into a writer. This avoids allocating a string in the CLI use case
* `into_edit() -> Option<Edit>`: Returns an edit


I think this would help to closer match the performance of the CLI today where we allocate a single string to apply all edits (where this solution creates a string for each edit, and one for the merged output).

---

_@MichaReiser reviewed on 2024-05-06 09:23_

---

_@snowsignal reviewed on 2024-05-07 06:24_

---

_Review comment by @snowsignal on `crates/ruff_linter/src/noqa.rs`:744 on 2024-05-07 06:24_

I like this idea!

---

_Review requested from @MichaReiser by @snowsignal on 2024-05-07 07:34_

---

_@charliermarsh reviewed on 2024-05-08 03:12_

---

_Review comment by @charliermarsh on `crates/ruff_linter/src/noqa.rs`:591 on 2024-05-08 03:12_

Should we zip here instead? (I suppose the diagnostics could be omitted entirely?)

---

_@charliermarsh approved on 2024-05-08 03:14_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:195 on 2024-05-08 05:38_

We can undo the `Clone` change here and for `Code` as well.

---

_@MichaReiser reviewed on 2024-05-08 05:38_

---

_@MichaReiser approved on 2024-05-08 05:39_

---

_Comment by @snowsignal on 2024-05-08 17:40_

I'm holding off on merging this until I investigate the root cause of [this issue](https://github.com/astral-sh/ruff/pull/11276#issuecomment-2097773069).

---

_Comment by @snowsignal on 2024-05-10 23:10_

The root of the related issue in the follow-up PR appears to be caused by noqa mappings not generated/passed through to the edit generation function. I've added a test to show that this does work correctly when the proper `NoqaMapping` is given.


---

_Merged by @snowsignal on 2024-05-10 23:16_

---

_Closed by @snowsignal on 2024-05-10 23:16_

---

_Branch deleted on 2024-05-10 23:16_

---
