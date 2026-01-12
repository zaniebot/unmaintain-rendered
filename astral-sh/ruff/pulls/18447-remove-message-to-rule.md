```yaml
number: 18447
title: "Remove `Message::to_rule`"
type: pull_request
state: merged
author: ntBre
labels:
  - internal
  - diagnostics
assignees: []
merged: true
base: main
head: brent/remove-to-rule
created_at: 2025-06-03T18:55:47Z
updated_at: 2025-06-05T16:48:31Z
url: https://github.com/astral-sh/ruff/pull/18447
synced_at: 2026-01-12T15:56:19Z
```

# Remove `Message::to_rule`

---

_@ntBre_

## Summary

As the title says, this PR removes the `Message::to_rule` method by replacing related uses of `Rule` with `NoqaCode` (or the rule's name in the case of the cache). Where it seemed a `Rule` was really needed, we convert back to the `Rule` by parsing either the rule name (with `str::parse`) or the `NoqaCode` (with `Rule::from_code`).

I thought this was kind of like cheating and that it might not resolve this part of Micha's [comment](https://github.com/astral-sh/ruff/pull/18391#issuecomment-2933764275):

> because we can't add Rule to Diagnostic or **have it anywhere in our shared rendering logic**

but after looking again, the only remaining `Rule` conversion in rendering code is for the SARIF output format. The other two non-test `Rule` conversions are for caching and writing a fix summary, which I don't think fall into the shared rendering logic. That leaves the SARIF format as the only real problem, but maybe we can delay that for now.

The motivation here is that we won't be able to store a `Rule` on the new `Diagnostic` type, but we should be able to store a `NoqaCode`, likely as a string.

## Test Plan

Existing tests

## [Benchmarks](https://codspeed.io/astral-sh/ruff/branches/brent%2Fremove-to-rule)

Almost no perf regression, only -1% on `linter/default-rules[large/dataset.py]`.


---

_Label `internal` added by @ntBre on 2025-06-03 18:55_

---

_Label `diagnostics` added by @ntBre on 2025-06-03 18:55_

---

_Comment by @github-actions[bot] on 2025-06-03 19:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Marked ready for review by @ntBre on 2025-06-03 19:13_

---

_Review requested from @MichaReiser by @ntBre on 2025-06-03 19:13_

---

_Converted to draft by @ntBre on 2025-06-03 19:14_

---

_Marked ready for review by @ntBre on 2025-06-03 19:16_

---

_Review comment by @MichaReiser on `crates/ruff/src/cache.rs`:443 on 2025-06-04 06:29_

Unrelated to your PR: Does that mean we drop syntax errors from the cache?

---

_Review comment by @MichaReiser on `crates/ruff/src/cache.rs`:360 on 2025-06-04 06:31_

Could we move this call to the serialization and keep storing a `Rule` in the cache? 

---

_Review comment by @MichaReiser on `crates/ruff/src/printer.rs`:507 on 2025-06-04 06:39_

Does `rule.to_string` return the rule name? I'm inclined to change `FixTable` to a map from `noqa` to `(name, usize)`. This removes the need to lookup the rule. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/mod.rs`:134 on 2025-06-04 06:46_

Hmm. This hurts readability quiet a bit but I understnad why it's necessary. Calling `noqa_code` isn't cheap. 

I think a nice solution here could be to add a `has_noqa_code(code: &str)` method to `Rule` (or make calling `noqa` cheap).

Or wait, could we compare the kebab case names instead? These can be retrieved cheaply

```
let redefined_while_unused = Rule::RedefinedWhileUnused.as_ref();
```

We could even make this more explicit by adding a `name` method to `Rule` that returns a `LintName`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/mod.rs`:228 on 2025-06-04 06:51_

Nit: Rename to `noqa_code`, now that the conversion is free.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:74 on 2025-06-04 06:56_

I think I'd prefer if we would preserve the name during the transformation instead of looking up the rule here again:

```rust
        let unique_rules: HashSet<_> = results.iter().filter_map(|result| result.code).collect();
```

This could be changed to be a hash map that maps from code to name (or HashSet with `(NoqaCode, &str)` as key). 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:69 on 2025-06-04 06:57_

It's a bit silly that `parse_code` strips the prefix again, when `NoqaCode` already has the prefix. If not too hard, would it make sense to add a `parse_noqa` method that matches on `code.prefix()`? 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/mod.rs`:781 on 2025-06-04 06:59_

Not too important but we could also consider transforming `expected` and `actual` and map them to the rule names instead. This avoids the rule lookup

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/noqa.rs`:818 on 2025-06-04 07:04_

Can you double check if we need to use a `HashSet` for `noqa_codes` in case there are multiple matches for the same code? Or does it not matter, or does some other code guarantee that there are no duplicates?

---

_Review comment by @MichaReiser on `crates/ruff_server/src/lint.rs`:172 on 2025-06-04 07:06_

Nit: maybe for another PR, but I'd prefer to use `message.is_syntax_error` over `to_noqa_code` to decide if something is a syntax error or not. Ideally, that would also allow us to remove needing both `to_lsp_diagnostic` and `syntax_error_to_lsp_diagnostic`

---

_@MichaReiser approved on 2025-06-04 07:07_

Thank you. This is great. I think we can avoid the `from_code` in a few more places, but this, overall looks good

---

_Comment by @MichaReiser on 2025-06-04 07:32_

Here a patch for adding a `rule.name` method (it also allows us to remove the `AsStrRef` derive:

<details>
<summary>Patch</summary>

```patch
diff --git a/crates/ruff/src/commands/rule.rs b/crates/ruff/src/commands/rule.rs
index 45b071d2e..adc761b3e 100644
--- a/crates/ruff/src/commands/rule.rs
+++ b/crates/ruff/src/commands/rule.rs
@@ -30,7 +30,7 @@ impl<'a> Explanation<'a> {
         let (linter, _) = Linter::parse_code(&code).unwrap();
         let fix = rule.fixable().to_string();
         Self {
-            name: rule.as_ref(),
+            name: rule.name().as_str(),
             code,
             linter: linter.name(),
             summary: rule.message_formats()[0],
@@ -44,7 +44,7 @@ impl<'a> Explanation<'a> {
 
 fn format_rule_text(rule: Rule) -> String {
     let mut output = String::new();
-    let _ = write!(&mut output, "# {} ({})", rule.as_ref(), rule.noqa_code());
+    let _ = write!(&mut output, "# {} ({})", rule.name(), rule.noqa_code());
     output.push('\n');
     output.push('\n');
 
diff --git a/crates/ruff_dev/src/generate_docs.rs b/crates/ruff_dev/src/generate_docs.rs
index d191e8264..5f2309328 100644
--- a/crates/ruff_dev/src/generate_docs.rs
+++ b/crates/ruff_dev/src/generate_docs.rs
@@ -29,7 +29,7 @@ pub(crate) fn main(args: &Args) -> Result<()> {
         if let Some(explanation) = rule.explanation() {
             let mut output = String::new();
 
-            let _ = writeln!(&mut output, "# {} ({})", rule.as_ref(), rule.noqa_code());
+            let _ = writeln!(&mut output, "# {} ({})", rule.name(), rule.noqa_code());
 
             let (linter, _) = Linter::parse_code(&rule.noqa_code().to_string()).unwrap();
             if linter.url().is_some() {
@@ -101,7 +101,7 @@ pub(crate) fn main(args: &Args) -> Result<()> {
             let filename = PathBuf::from(ROOT_DIR)
                 .join("docs")
                 .join("rules")
-                .join(rule.as_ref())
+                .join(&*rule.name())
                 .with_extension("md");
 
             if args.dry_run {
diff --git a/crates/ruff_dev/src/generate_rules_table.rs b/crates/ruff_dev/src/generate_rules_table.rs
index 48b1cdc2c..3255f8f42 100644
--- a/crates/ruff_dev/src/generate_rules_table.rs
+++ b/crates/ruff_dev/src/generate_rules_table.rs
@@ -55,7 +55,7 @@ fn generate_table(table_out: &mut String, rules: impl IntoIterator<Item = Rule>,
             FixAvailability::None => format!("<span {SYMBOL_STYLE}></span>"),
         };
 
-        let rule_name = rule.as_ref();
+        let rule_name = rule.name();
 
         // If the message ends in a bracketed expression (like: "Use {replacement}"), escape the
         // brackets. Otherwise, it'll be interpreted as an HTML attribute via the `attr_list`
diff --git a/crates/ruff_linter/src/codes.rs b/crates/ruff_linter/src/codes.rs
index fafb840a0..c31e989a4 100644
--- a/crates/ruff_linter/src/codes.rs
+++ b/crates/ruff_linter/src/codes.rs
@@ -4,7 +4,7 @@
 /// `--select`. For pylint this is e.g. C0414 and E0118 but also C and E01.
 use std::fmt::Formatter;
 
-use strum_macros::{AsRefStr, EnumIter};
+use strum_macros::EnumIter;
 
 use crate::registry::Linter;
 use crate::rule_selector::is_single_rule_selector;
diff --git a/crates/ruff_linter/src/logging.rs b/crates/ruff_linter/src/logging.rs
index dadf6ce25..b59c63737 100644
--- a/crates/ruff_linter/src/logging.rs
+++ b/crates/ruff_linter/src/logging.rs
@@ -20,7 +20,7 @@ pub static IDENTIFIERS: LazyLock<Mutex<Vec<&'static str>>> = LazyLock::new(Mutex
 /// Warn a user once, with uniqueness determined by the given ID.
 #[macro_export]
 macro_rules! warn_user_once_by_id {
-    ($id:expr, $($arg:tt)*) => {
+    ($id:expr, $($arg:tt)*) => {{
         use colored::Colorize;
         use log::warn;
 
@@ -31,7 +31,7 @@ macro_rules! warn_user_once_by_id {
                 states.push($id);
             }
         }
-    };
+    }};
 }
 
 pub static MESSAGES: LazyLock<Mutex<FxHashSet<String>>> = LazyLock::new(Mutex::default);
diff --git a/crates/ruff_linter/src/registry.rs b/crates/ruff_linter/src/registry.rs
index 9b03a8b7f..2f8a92324 100644
--- a/crates/ruff_linter/src/registry.rs
+++ b/crates/ruff_linter/src/registry.rs
@@ -1,6 +1,7 @@
 //! Remnant of the registry of all [`Rule`] implementations, now it's reexporting from codes.rs
 //! with some helper symbols
 
+use ruff_db::diagnostic::LintName;
 use strum_macros::EnumIter;
 
 pub use codes::Rule;
@@ -348,9 +349,18 @@ impl Rule {
 
     /// Return the URL for the rule documentation, if it exists.
     pub fn url(&self) -> Option<String> {
-        self.explanation()
-            .is_some()
-            .then(|| format!("{}/rules/{}", env!("CARGO_PKG_HOMEPAGE"), self.as_ref()))
+        self.explanation().is_some().then(|| {
+            format!(
+                "{}/rules/{name}",
+                env!("CARGO_PKG_HOMEPAGE"),
+                name = self.name()
+            )
+        })
+    }
+
+    pub fn name(&self) -> LintName {
+        let name: &'static str = self.into();
+        LintName::of(name)
     }
 }
 
@@ -421,7 +431,7 @@ pub mod clap_completion {
         fn possible_values(&self) -> Option<Box<dyn Iterator<Item = PossibleValue> + '_>> {
             Some(Box::new(Rule::iter().map(|rule| {
                 let name = rule.noqa_code().to_string();
-                let help = rule.as_ref().to_string();
+                let help: &'static str = rule.into();
                 PossibleValue::new(name).help(help)
             })))
         }
@@ -443,7 +453,7 @@ mod tests {
             assert!(
                 rule.explanation().is_some(),
                 "Rule {} is missing documentation",
-                rule.as_ref()
+                rule.name()
             );
         }
     }
@@ -460,10 +470,10 @@ mod tests {
             .collect();
 
         for rule in Rule::iter() {
-            let rule_name = rule.as_ref();
+            let rule_name = rule.name();
             for pattern in &patterns {
                 assert!(
-                    !pattern.matches(rule_name),
+                    !pattern.matches(&*rule_name),
                     "{rule_name} does not match naming convention, see CONTRIBUTING.md"
                 );
             }
diff --git a/crates/ruff_linter/src/registry/rule_set.rs b/crates/ruff_linter/src/registry/rule_set.rs
index d83ff0771..62601d722 100644
--- a/crates/ruff_linter/src/registry/rule_set.rs
+++ b/crates/ruff_linter/src/registry/rule_set.rs
@@ -302,9 +302,8 @@ impl Display for RuleSet {
         } else {
             writeln!(f, "[")?;
             for rule in self {
-                let name = rule.as_ref();
                 let code = rule.noqa_code();
-                writeln!(f, "\t{name} ({code}),")?;
+                writeln!(f, "\t{name} ({code}),", name = rule.name())?;
             }
             write!(f, "]")?;
         }
diff --git a/crates/ruff_linter/src/rule_selector.rs b/crates/ruff_linter/src/rule_selector.rs
index 7588aa9f6..74f069b97 100644
--- a/crates/ruff_linter/src/rule_selector.rs
+++ b/crates/ruff_linter/src/rule_selector.rs
@@ -485,8 +485,7 @@ pub mod clap_completion {
                                     prefix.linter().common_prefix(),
                                     prefix.short_code()
                                 );
-                                let name: &'static str = rule.into();
-                                return Some(PossibleValue::new(code).help(name));
+                                return Some(PossibleValue::new(code).help(rule.name().as_str()));
                             }
 
                             None
diff --git a/crates/ruff_linter/src/rules/fastapi/mod.rs b/crates/ruff_linter/src/rules/fastapi/mod.rs
index 90f52b7d6..60f180973 100644
--- a/crates/ruff_linter/src/rules/fastapi/mod.rs
+++ b/crates/ruff_linter/src/rules/fastapi/mod.rs
@@ -3,7 +3,6 @@ pub(crate) mod rules;
 
 #[cfg(test)]
 mod tests {
-    use std::convert::AsRef;
     use std::path::Path;
 
     use anyhow::Result;
@@ -18,7 +17,7 @@ mod tests {
     #[test_case(Rule::FastApiNonAnnotatedDependency, Path::new("FAST002_1.py"))]
     #[test_case(Rule::FastApiUnusedPathParameter, Path::new("FAST003.py"))]
     fn rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("fastapi").join(path).as_path(),
             &settings::LinterSettings::for_rule(rule_code),
@@ -32,7 +31,7 @@ mod tests {
     #[test_case(Rule::FastApiNonAnnotatedDependency, Path::new("FAST002_0.py"))]
     #[test_case(Rule::FastApiNonAnnotatedDependency, Path::new("FAST002_1.py"))]
     fn rules_py38(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}_py38", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}_py38", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("fastapi").join(path).as_path(),
             &settings::LinterSettings {
diff --git a/crates/ruff_linter/src/rules/flake8_fixme/mod.rs b/crates/ruff_linter/src/rules/flake8_fixme/mod.rs
index d2433fc8b..44071d1b7 100644
--- a/crates/ruff_linter/src/rules/flake8_fixme/mod.rs
+++ b/crates/ruff_linter/src/rules/flake8_fixme/mod.rs
@@ -16,7 +16,7 @@ mod tests {
     #[test_case(Rule::LineContainsTodo; "T003")]
     #[test_case(Rule::LineContainsXxx; "T004")]
     fn rules(rule_code: Rule) -> Result<()> {
-        let snapshot = format!("{}_T00.py", rule_code.as_ref());
+        let snapshot = format!("{}_T00.py", rule_code.name());
         let diagnostics = test_path(
             Path::new("flake8_fixme/T00.py"),
             &settings::LinterSettings::for_rule(rule_code),
diff --git a/crates/ruff_linter/src/rules/flake8_gettext/mod.rs b/crates/ruff_linter/src/rules/flake8_gettext/mod.rs
index 09d440526..cd385932a 100644
--- a/crates/ruff_linter/src/rules/flake8_gettext/mod.rs
+++ b/crates/ruff_linter/src/rules/flake8_gettext/mod.rs
@@ -29,7 +29,7 @@ mod tests {
     #[test_case(Rule::FormatInGetTextFuncCall, Path::new("INT002.py"))]
     #[test_case(Rule::PrintfInGetTextFuncCall, Path::new("INT003.py"))]
     fn rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_gettext").join(path).as_path(),
             &settings::LinterSettings::for_rule(rule_code),
diff --git a/crates/ruff_linter/src/rules/flake8_raise/mod.rs b/crates/ruff_linter/src/rules/flake8_raise/mod.rs
index d2d636b1e..178045453 100644
--- a/crates/ruff_linter/src/rules/flake8_raise/mod.rs
+++ b/crates/ruff_linter/src/rules/flake8_raise/mod.rs
@@ -3,7 +3,6 @@ pub(crate) mod rules;
 
 #[cfg(test)]
 mod tests {
-    use std::convert::AsRef;
     use std::path::Path;
 
     use anyhow::Result;
@@ -15,7 +14,7 @@ mod tests {
 
     #[test_case(Rule::UnnecessaryParenOnRaiseException, Path::new("RSE102.py"))]
     fn rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_raise").join(path).as_path(),
             &settings::LinterSettings::for_rule(rule_code),
diff --git a/crates/ruff_linter/src/rules/flake8_self/mod.rs b/crates/ruff_linter/src/rules/flake8_self/mod.rs
index a445b40ba..7a3f83ddd 100644
--- a/crates/ruff_linter/src/rules/flake8_self/mod.rs
+++ b/crates/ruff_linter/src/rules/flake8_self/mod.rs
@@ -4,7 +4,6 @@ pub mod settings;
 
 #[cfg(test)]
 mod tests {
-    use std::convert::AsRef;
     use std::path::Path;
 
     use crate::registry::Rule;
@@ -18,7 +17,7 @@ mod tests {
     #[test_case(Rule::PrivateMemberAccess, Path::new("SLF001.py"))]
     #[test_case(Rule::PrivateMemberAccess, Path::new("SLF001_1.py"))]
     fn rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_self").join(path).as_path(),
             &settings::LinterSettings::for_rule(rule_code),
diff --git a/crates/ruff_linter/src/rules/flake8_todos/mod.rs b/crates/ruff_linter/src/rules/flake8_todos/mod.rs
index 7a4129a78..486c46af0 100644
--- a/crates/ruff_linter/src/rules/flake8_todos/mod.rs
+++ b/crates/ruff_linter/src/rules/flake8_todos/mod.rs
@@ -2,7 +2,6 @@ pub(crate) mod rules;
 
 #[cfg(test)]
 mod tests {
-    use std::convert::AsRef;
     use std::path::Path;
 
     use anyhow::Result;
@@ -20,7 +19,7 @@ mod tests {
     #[test_case(Rule::InvalidTodoCapitalization, Path::new("TD006.py"))]
     #[test_case(Rule::MissingSpaceAfterTodoColon, Path::new("TD007.py"))]
     fn rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_todos").join(path).as_path(),
             &settings::LinterSettings::for_rule(rule_code),
diff --git a/crates/ruff_linter/src/rules/flake8_type_checking/mod.rs b/crates/ruff_linter/src/rules/flake8_type_checking/mod.rs
index f5b777ec4..acdb788f5 100644
--- a/crates/ruff_linter/src/rules/flake8_type_checking/mod.rs
+++ b/crates/ruff_linter/src/rules/flake8_type_checking/mod.rs
@@ -6,7 +6,6 @@ pub mod settings;
 
 #[cfg(test)]
 mod tests {
-    use std::convert::AsRef;
     use std::path::Path;
 
     use anyhow::Result;
@@ -55,7 +54,7 @@ mod tests {
     #[test_case(Rule::TypingOnlyThirdPartyImport, Path::new("typing_modules_1.py"))]
     #[test_case(Rule::TypingOnlyThirdPartyImport, Path::new("typing_modules_2.py"))]
     fn rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings::for_rule(rule_code),
@@ -70,7 +69,7 @@ mod tests {
     #[test_case(Rule::QuotedTypeAlias, Path::new("TC008.py"))]
     #[test_case(Rule::QuotedTypeAlias, Path::new("TC008_typing_execution_context.py"))]
     fn type_alias_rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings::for_rules(vec![
@@ -84,11 +83,7 @@ mod tests {
 
     #[test_case(Rule::QuotedTypeAlias, Path::new("TC008_union_syntax_pre_py310.py"))]
     fn type_alias_rules_pre_py310(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!(
-            "pre_py310_{}_{}",
-            rule_code.as_ref(),
-            path.to_string_lossy()
-        );
+        let snapshot = format!("pre_py310_{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings {
@@ -107,7 +102,7 @@ mod tests {
     #[test_case(Rule::RuntimeImportInTypeCheckingBlock, Path::new("quote3.py"))]
     #[test_case(Rule::TypingOnlyThirdPartyImport, Path::new("quote3.py"))]
     fn quote(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("quote_{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("quote_{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings {
@@ -126,7 +121,7 @@ mod tests {
     #[test_case(Rule::TypingOnlyStandardLibraryImport, Path::new("init_var.py"))]
     #[test_case(Rule::TypingOnlyStandardLibraryImport, Path::new("kw_only.py"))]
     fn strict(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("strict_{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("strict_{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings {
@@ -170,7 +165,7 @@ mod tests {
         Path::new("exempt_type_checking_3.py")
     )]
     fn exempt_type_checking(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings {
@@ -207,7 +202,7 @@ mod tests {
         Path::new("runtime_evaluated_base_classes_5.py")
     )]
     fn runtime_evaluated_base_classes(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings {
@@ -238,7 +233,7 @@ mod tests {
         Path::new("runtime_evaluated_decorators_3.py")
     )]
     fn runtime_evaluated_decorators(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings {
@@ -264,7 +259,7 @@ mod tests {
         Path::new("module/undefined.py")
     )]
     fn base_class_same_file(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings {
@@ -282,7 +277,7 @@ mod tests {
     #[test_case(Rule::RuntimeImportInTypeCheckingBlock, Path::new("module/app.py"))]
     #[test_case(Rule::TypingOnlyStandardLibraryImport, Path::new("module/routes.py"))]
     fn decorator_same_file(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("flake8_type_checking").join(path).as_path(),
             &settings::LinterSettings {
diff --git a/crates/ruff_linter/src/rules/numpy/mod.rs b/crates/ruff_linter/src/rules/numpy/mod.rs
index 7a313eac2..9403f2501 100644
--- a/crates/ruff_linter/src/rules/numpy/mod.rs
+++ b/crates/ruff_linter/src/rules/numpy/mod.rs
@@ -4,7 +4,6 @@ pub(crate) mod rules;
 
 #[cfg(test)]
 mod tests {
-    use std::convert::AsRef;
     use std::path::Path;
 
     use anyhow::Result;
@@ -22,7 +21,7 @@ mod tests {
     #[test_case(Rule::Numpy2Deprecation, Path::new("NPY201_2.py"))]
     #[test_case(Rule::Numpy2Deprecation, Path::new("NPY201_3.py"))]
     fn rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("numpy").join(path).as_path(),
             &settings::LinterSettings::for_rule(rule_code),
diff --git a/crates/ruff_linter/src/rules/pydoclint/mod.rs b/crates/ruff_linter/src/rules/pydoclint/mod.rs
index af4131e42..6e2ff9402 100644
--- a/crates/ruff_linter/src/rules/pydoclint/mod.rs
+++ b/crates/ruff_linter/src/rules/pydoclint/mod.rs
@@ -4,7 +4,6 @@ pub mod settings;
 
 #[cfg(test)]
 mod tests {
-    use std::convert::AsRef;
     use std::path::Path;
 
     use anyhow::Result;
@@ -20,7 +19,7 @@ mod tests {
 
     #[test_case(Rule::DocstringMissingException, Path::new("DOC501.py"))]
     fn rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("pydoclint").join(path).as_path(),
             &settings::LinterSettings::for_rule(rule_code),
@@ -36,7 +35,7 @@ mod tests {
     #[test_case(Rule::DocstringMissingException, Path::new("DOC501_google.py"))]
     #[test_case(Rule::DocstringExtraneousException, Path::new("DOC502_google.py"))]
     fn rules_google_style(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("pydoclint").join(path).as_path(),
             &settings::LinterSettings {
@@ -58,7 +57,7 @@ mod tests {
     #[test_case(Rule::DocstringMissingException, Path::new("DOC501_numpy.py"))]
     #[test_case(Rule::DocstringExtraneousException, Path::new("DOC502_numpy.py"))]
     fn rules_numpy_style(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("pydoclint").join(path).as_path(),
             &settings::LinterSettings {
@@ -79,7 +78,7 @@ mod tests {
     fn rules_google_style_ignore_one_line(rule_code: Rule, path: &Path) -> Result<()> {
         let snapshot = format!(
             "{}_{}_ignore_one_line",
-            rule_code.as_ref(),
+            rule_code.name(),
             path.to_string_lossy()
         );
         let diagnostics = test_path(
diff --git a/crates/ruff_linter/src/rules/tryceratops/mod.rs b/crates/ruff_linter/src/rules/tryceratops/mod.rs
index 21f3a0138..8d431aae6 100644
--- a/crates/ruff_linter/src/rules/tryceratops/mod.rs
+++ b/crates/ruff_linter/src/rules/tryceratops/mod.rs
@@ -4,7 +4,6 @@ pub(crate) mod rules;
 
 #[cfg(test)]
 mod tests {
-    use std::convert::AsRef;
     use std::path::Path;
 
     use anyhow::Result;
@@ -25,7 +24,7 @@ mod tests {
     #[test_case(Rule::ErrorInsteadOfException, Path::new("TRY400.py"))]
     #[test_case(Rule::VerboseLogMessage, Path::new("TRY401.py"))]
     fn rules(rule_code: Rule, path: &Path) -> Result<()> {
-        let snapshot = format!("{}_{}", rule_code.as_ref(), path.to_string_lossy());
+        let snapshot = format!("{}_{}", rule_code.name(), path.to_string_lossy());
         let diagnostics = test_path(
             Path::new("tryceratops").join(path).as_path(),
             &settings::LinterSettings::for_rule(rule_code),
diff --git a/crates/ruff_macros/src/map_codes.rs b/crates/ruff_macros/src/map_codes.rs
index 7d1ccaf02..39993af6a 100644
--- a/crates/ruff_macros/src/map_codes.rs
+++ b/crates/ruff_macros/src/map_codes.rs
@@ -174,7 +174,7 @@ pub(crate) fn map_codes(func: &ItemFn) -> syn::Result<TokenStream> {
 
         output.extend(quote! {
             impl #linter {
-                pub fn rules(&self) -> ::std::vec::IntoIter<Rule> {
+                pub(crate) fn rules(&self) -> ::std::vec::IntoIter<Rule> {
                     match self { #prefix_into_iter_match_arms }
                 }
             }
@@ -182,7 +182,7 @@ pub(crate) fn map_codes(func: &ItemFn) -> syn::Result<TokenStream> {
     }
     output.extend(quote! {
         impl RuleCodePrefix {
-            pub fn parse(linter: &Linter, code: &str) -> Result<Self, crate::registry::FromCodeError> {
+            pub(crate) fn parse(linter: &Linter, code: &str) -> Result<Self, crate::registry::FromCodeError> {
                 use std::str::FromStr;
 
                 Ok(match linter {
@@ -190,7 +190,7 @@ pub(crate) fn map_codes(func: &ItemFn) -> syn::Result<TokenStream> {
                 })
             }
 
-            pub fn rules(&self) -> ::std::vec::IntoIter<Rule> {
+            pub(crate) fn rules(&self) -> ::std::vec::IntoIter<Rule> {
                 match self {
                     #(RuleCodePrefix::#linter_idents(prefix) => prefix.clone().rules(),)*
                 }
@@ -319,7 +319,7 @@ See also https://github.com/astral-sh/ruff/issues/2186.
                 matches!(self.group(), RuleGroup::Preview)
             }
 
-            pub fn is_stable(&self) -> bool {
+            pub(crate) fn is_stable(&self) -> bool {
                 matches!(self.group(), RuleGroup::Stable)
             }
 
@@ -371,7 +371,7 @@ fn generate_iter_impl(
     quote! {
         impl Linter {
             /// Rules not in the preview.
-            pub fn rules(self: &Linter) -> ::std::vec::IntoIter<Rule> {
+            pub(crate) fn rules(self: &Linter) -> ::std::vec::IntoIter<Rule> {
                 match self {
                     #linter_rules_match_arms
                 }
@@ -385,7 +385,7 @@ fn generate_iter_impl(
         }
 
         impl RuleCodePrefix {
-            pub fn iter() -> impl Iterator<Item = RuleCodePrefix> {
+            pub(crate) fn iter() -> impl Iterator<Item = RuleCodePrefix> {
                 use strum::IntoEnumIterator;
 
                 let mut prefixes = Vec::new();
@@ -436,7 +436,6 @@ fn register_rules<'a>(input: impl Iterator<Item = &'a Rule>) -> TokenStream {
             PartialOrd,
             Ord,
             ::ruff_macros::CacheKey,
-            AsRefStr,
             ::strum_macros::IntoStaticStr,
             ::strum_macros::EnumString,
             ::serde::Serialize,
diff --git a/crates/ruff_server/src/server/api/requests/hover.rs b/crates/ruff_server/src/server/api/requests/hover.rs
index 6b391e15b..846f3654c 100644
--- a/crates/ruff_server/src/server/api/requests/hover.rs
+++ b/crates/ruff_server/src/server/api/requests/hover.rs
@@ -85,7 +85,7 @@ pub(crate) fn hover(
 
 fn format_rule_text(rule: Rule) -> String {
     let mut output = String::new();
-    let _ = write!(&mut output, "# {} ({})", rule.as_ref(), rule.noqa_code());
+    let _ = write!(&mut output, "# {} ({})", rule.name(), rule.noqa_code());
     output.push('\n');
     output.push('\n');
 
diff --git a/crates/ruff_workspace/src/configuration.rs b/crates/ruff_workspace/src/configuration.rs
index 684a2b434..ae0d23150 100644
--- a/crates/ruff_workspace/src/configuration.rs
+++ b/crates/ruff_workspace/src/configuration.rs
@@ -1098,7 +1098,7 @@ impl LintConfiguration {
         // approach to give each pair it's own `warn_user_once`.
         for (preferred, expendable, message) in INCOMPATIBLE_CODES {
             if rules.enabled(*preferred) && rules.enabled(*expendable) {
-                warn_user_once_by_id!(expendable.as_ref(), "{}", message);
+                warn_user_once_by_id!(expendable.name().as_str(), "{}", message);
                 rules.disable(*expendable);
             }
         }
```

</details>

IMO, it improves readability over the `to_string`, `as_ref` or `let name: &'static str = rule.into()` calls

---

_@ntBre reviewed on 2025-06-04 13:42_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/sarif.rs`:69 on 2025-06-04 13:42_

I looked into this a bit last week, and the tricky thing is that despite `NoqaCode` having two `&'static str`s and a `prefix` and `suffix`  method, they don't always do what you expect because in cases with multiple common prefixes (at least I think this is the cause, from what I remember) the "prefix" will actually be empty and the whole code is in the "suffix." That's why `parse_code` is implemented like this, I think.

This is also what I was trying to work around with my proc macro changes in https://github.com/astral-sh/ruff/pull/18391.

Here's the macro code for generating the `common_prefix` method:

https://github.com/astral-sh/ruff/blob/43277a1536e74379d04d249777567f2c245b829c/crates/ruff_macros/src/rule_namespace.rs#L89-L96

And then the construction of `NoqaCode`s uses it:

https://github.com/astral-sh/ruff/blob/43277a1536e74379d04d249777567f2c245b829c/crates/ruff_macros/src/map_codes.rs#L291-L293

---

_@ntBre reviewed on 2025-06-04 13:44_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/sarif.rs`:69 on 2025-06-04 13:44_

I think your idea about storing a single `&str` and a `usize` here might help, but it's still a bit tricky to get everything into a single `&'static str`. We might still need my `const` tricks from the other PR to `concat!` them here.

---

_@ntBre reviewed on 2025-06-04 13:51_

---

_Review comment by @ntBre on `crates/ruff/src/cache.rs`:443 on 2025-06-04 13:51_

I think we also avoid caching files with syntax errors, but that happens somewhere else. Here I think:

https://github.com/astral-sh/ruff/blob/43277a1536e74379d04d249777567f2c245b829c/crates/ruff/src/diagnostics.rs#L326-L331

(the comment is a bit outdated, `has_syntax_errors` includes semantic and version errors too)

So this filter shouldn't actually do anything anymore, except double check that the later `parse` call should be unwrappable.

---

_@ntBre reviewed on 2025-06-04 13:59_

---

_Review comment by @ntBre on `crates/ruff/src/cache.rs`:360 on 2025-06-04 13:59_

Yes, good idea! I also added a comment explaining the filter/syntax error handling.

---

_@ntBre reviewed on 2025-06-04 14:15_

---

_Review comment by @ntBre on `crates/ruff_linter/src/noqa.rs`:818 on 2025-06-04 14:15_

Oh good catch. The one place this is called with a non-single-element vec is here:

https://github.com/astral-sh/ruff/blob/11db567b0b493b107446124d4121c3a5257fd6c3/crates/ruff_linter/src/noqa.rs#L814-L823

so I guess it's possible that there would be duplicates if the user had duplicate noqa comments in the input, if I'm reading this correctly. I can change it to a `HashSet` to avoid that.

---

_Comment by @ntBre on 2025-06-04 14:31_

Thanks for the patch! I also got confused a couple times by the various options but hadn't tried to fix it. This should help a lot.

---

_@ntBre reviewed on 2025-06-04 14:46_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/sarif.rs`:74 on 2025-06-04 14:46_

I think we need the rule to call `message_formats`  and `explanation` anyway, right? I could be missing something here.

I previously tried a bigger refactor here where I passed the `Message::body` and `Message::suggestion` instead of `message_formats` and `explanation`, but they are quite different. `body` and `message_formats[0]` are pretty similar, and `body` is actually preferable, in my opinion, because it has the placeholders from `message_formats` filled in. But `explanation` is the full rule documentation, which I don't think we have easy access to except via the `Rule`.

---

_@ntBre reviewed on 2025-06-04 15:03_

---

_Review comment by @ntBre on `crates/ruff_linter/src/fix/mod.rs`:134 on 2025-06-04 15:03_

I feel like I'm missing something here, but how does comparing the names help with readability? If we had a `const` way of getting the rule names, we could restore the `match` approach with something like:

```rust
const REDEFINED_WHILE_UNUSED: &str = Rule::RedefinedWhileUnused.name();

match (name1, name2) {
    (REDEFINED_WHILE_UNUSED, REDEFINED_WHILE_UNUSED) => Ordering::Equal,
    /* ... */
}
```

but it looks like the derived `IntoStaticStr` impl is not const. We could generate our own const version with our `kebab_case` macro, though.

Otherwise I guess I'm just picturing swapping out the `noqa_code` calls for `name` calls, which doesn't seem to help too much. Again I could be wrong,  but I don't think calling `name` is much cheaper than `noqa_code` either. It does have to call `Linter::common_prefix`, but that's just a match on the `Linter` enum.

---

_@MichaReiser reviewed on 2025-06-04 15:06_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:74 on 2025-06-04 15:06_

Oh, I didn't notice that it uses `message_formats`. I guess yes, in this case we'll need the rule

---

_@MichaReiser reviewed on 2025-06-04 15:08_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/mod.rs`:134 on 2025-06-04 15:08_

>  but I don't think calling name is much cheaper than noqa_code e

Doesn't `noqa_code` allocate because how `noqa_code` is designed today?

I agree, that it probably doesn't help with readability. Although we could probably inline the expression and hope for the compiler to inline the static string pointer.

---

_@ntBre reviewed on 2025-06-04 15:15_

---

_Review comment by @ntBre on `crates/ruff_linter/src/fix/mod.rs`:134 on 2025-06-04 15:15_

I didn't think so since it's just two static strings:

https://github.com/astral-sh/ruff/blob/f1883d71a4d47a56f8dd8c1874db40f12bc3b105/crates/ruff_linter/src/codes.rs#L13-L14

Maybe I have too narrow a view of allocation, I was looking for a `String` to indicate that.

I can still switch it to `LintName`, one `&'static str` should still be faster than two, either way. I was hoping I missed something with the readability though, the `match` was much nicer.

---

_@ntBre reviewed on 2025-06-04 19:25_

---

_Review comment by @ntBre on `crates/ruff_server/src/lint.rs`:172 on 2025-06-04 19:25_

Yeah... we actually end up using it like that in several, if not most, places. I guess this was the benefit of the separate `DiagnosticMessage` and `SyntaxErrorMessage` enum variants. It's just a bit unfortunate to make this explicit because you end up with an `is_syntax_error` check followed by an `unwrap`/`expect` if you need the noqa code.

In this case, it does look straightforward to combine `to_lsp_diagnostic` and `syntax_error_to_lsp_diagnostic`, so maybe we can treat them uniformly in other cases too. I'm happy to follow up with that!

---

_@ntBre reviewed on 2025-06-04 19:32_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/sarif.rs`:152 on 2025-06-04 19:32_

How do you feel about moving the `cfg` attrs here just to the `uri` attribute? Or alternatively something like

```rust
let uri = cfg!(target_arch = "wasm32") {
    path.display().to_string()
} else { 
      url::Url::from_file_path(&path)
                .map_err(|()| anyhow::anyhow!("Failed to convert path to URL: {}", path.display()))?
                .to_string()
};
```

That's the only difference between these two `from_message` implementations. I guess it's pretty minor in this case, but I'm curious about your thoughts in general. I've managed to forget to update the wasm version at least twice in this PR.


---

_@MichaReiser reviewed on 2025-06-05 07:05_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/fix/mod.rs`:134 on 2025-06-05 07:05_

oh right. It only allocates when getting the full string. You're right. sorry

---

_@MichaReiser reviewed on 2025-06-05 07:07_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:152 on 2025-06-05 07:07_

I don't know if you can use the inline `cfg` but I'm definetely in favor of reducing unnecessary code duplication

---

_@ntBre reviewed on 2025-06-05 13:26_

---

_Review comment by @ntBre on `crates/ruff_linter/src/message/sarif.rs`:152 on 2025-06-05 13:26_

Ah you're right. `Url::from_file_path` isn't available on WASM. I'll just leave it alone for now, but good to know. Thanks!

---

_@MichaReiser reviewed on 2025-06-05 14:02_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/message/sarif.rs`:152 on 2025-06-05 14:02_

The easiest would be to extract the URI conversion into it's own function and gate that

---

_@MichaReiser reviewed on 2025-06-05 16:34_

---

_Review comment by @MichaReiser on `crates/ruff/src/diagnostics.rs`:1 on 2025-06-05 16:34_

Nice cleanup in this commit!

---

_@ntBre reviewed on 2025-06-05 16:36_

---

_Review comment by @ntBre on `crates/ruff/src/diagnostics.rs`:1 on 2025-06-05 16:36_

Thank you!

---

_Merged by @ntBre on 2025-06-05 16:48_

---

_Closed by @ntBre on 2025-06-05 16:48_

---

_Branch deleted on 2025-06-05 16:48_

---
