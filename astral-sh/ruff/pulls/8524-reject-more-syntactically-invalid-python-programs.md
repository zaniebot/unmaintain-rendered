```yaml
number: 8524
title: Reject more syntactically invalid Python programs
type: pull_request
state: merged
author: BurntSushi
labels:
  - bug
assignees: []
merged: true
base: main
head: ag/fix-6895
created_at: 2023-11-06T19:15:53Z
updated_at: 2023-11-07T12:16:07Z
url: https://github.com/astral-sh/ruff/pull/8524
synced_at: 2026-01-10T23:40:55Z
```

# Reject more syntactically invalid Python programs

---

_Pull request opened by @BurntSushi on 2023-11-06 19:15_

## Summary

This commit adds some additional error checking to the parser such that assignments that are invalid syntax are rejected. This covers the obvious cases like `5 = 3` and some not so obvious cases like `x + y = 42`.

This does add an additional recursive call to the parser for the cases handling assignments. I had initially been concerned about doing this, but `set_context` is already doing recursion during assignments, so I didn't feel as though this was changing any fundamental performance characteristics of the parser. (Also, in practice, I would expect any such recursion here to be quite shallow since the recursion is done on the target of an assignment. Such things are rarely nested much in practice.)

Fixes #6895

## Test Plan

I've added unit tests covering every case that is detected as invalid on an `Expr`.


---

_Review requested from @charliermarsh by @BurntSushi on 2023-11-06 19:15_

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/invalid.rs`:102 on 2023-11-06 19:31_

I think it's safe to return an error here too.

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/invalid.rs`:52 on 2023-11-06 19:32_

Would it be reasonable to create a new `LexicalErrorType` for this?

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/invalid.rs`:44 on 2023-11-06 19:37_

I played around with it a bit locally because I wasn't sure -- but it looks like these can all just be `Result<(), LexicalError>` without the `LalrpopError` wrapper, if you do something like this:

```diff
diff --git a/crates/ruff_python_parser/src/invalid.rs b/crates/ruff_python_parser/src/invalid.rs
index 99dfc0140..0f53cc144 100644
--- a/crates/ruff_python_parser/src/invalid.rs
+++ b/crates/ruff_python_parser/src/invalid.rs
@@ -22,9 +22,7 @@ use crate::{
 /// or contain an expression that is invalid on the left hand side of an
 /// assignment. For example, all literal expressions are invalid assignment
 /// targets.
-pub(crate) fn assignment_targets(
-    targets: &[Expr],
-) -> Result<(), LalrpopError<TextSize, Tok, LexicalError>> {
+pub(crate) fn assignment_targets(targets: &[Expr]) -> Result<(), LexicalError> {
     for t in targets {
         assignment_target(t)?;
     }
@@ -39,20 +37,17 @@ pub(crate) fn assignment_targets(
 /// This returns an error when the given expression is itself or contains an
 /// expression that is invalid on the left hand side of an assignment. For
 /// example, all literal expressions are invalid assignment targets.
-pub(crate) fn assignment_target(
-    target: &Expr,
-) -> Result<(), LalrpopError<TextSize, Tok, LexicalError>> {
+pub(crate) fn assignment_target(target: &Expr) -> Result<(), LexicalError> {
     // Allowing a glob import here because of its limited scope.
     #[allow(clippy::enum_glob_use)]
     use self::Expr::*;
 
-    let err = |location: TextSize| -> LalrpopError<TextSize, Tok, LexicalError> {
+    let err = |location: TextSize| -> LexicalError {
         let msg = "invalid assignment target";
-        let error = LexicalError {
+        LexicalError {
             error: LexicalErrorType::OtherError(msg.to_string()),
             location,
-        };
-        LalrpopError::User { error }
+        }
     };
     match *target {
         BoolOp(ref e) => return Err(err(e.range.start())),
@@ -84,24 +79,23 @@ pub(crate) fn assignment_target(
         EllipsisLiteral(ref e) => return Err(err(e.range.start())),
         // The only nested expressions allowed as an assignment target
         // are star exprs, lists and tuples.
-        Starred(ref e) => assignment_target(&e.value)?,
-        List(ref e) => assignment_targets(&e.elts)?,
-        Tuple(ref e) => assignment_targets(&e.elts)?,
+        Starred(ref e) => assignment_target(&e.value),
+        List(ref e) => assignment_targets(&e.elts),
+        Tuple(ref e) => assignment_targets(&e.elts),
         // Subscript is recursive and can be invalid, but aren't syntax errors.
         // For example, `5[1] = 42` is a type error.
-        Subscript(_) => {}
+        Subscript(_) => Ok(()),
         // Similar to Subscript, e.g., `5[1:2] = [42]` is a type error.
-        Slice(_) => {}
+        Slice(_) => Ok(()),
         // Similar to Subscript, e.g., `"foo".y = 42` is an attribute error.
-        Attribute(_) => {}
+        Attribute(_) => Ok(()),
         // These are always valid as assignment targets.
-        Name(_) => {}
+        Name(_) => Ok(()),
         // This isn't in the Python grammar but is Jupyter notebook specific.
         // It seems like this should be an error, but I'm not certain about it
         // at atime of writing. So we allow it. ---AG
-        IpyEscapeCommand(_) => {}
+        IpyEscapeCommand(_) => Ok(()),
     }
-    Ok(())
 }
```

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/invalid.rs`:44 on 2023-11-06 19:39_

Can I ask why you do `match *target` and then `BoolOp(ref e)`, instead of `match target` and `BoolOp(e)`? I'm curious :)

---

_@charliermarsh reviewed on 2023-11-06 19:39_

This is excellent! Welcome to the repo :)

---

_Label `bug` added by @charliermarsh on 2023-11-06 19:40_

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/invalid.rs`:44 on 2023-11-06 19:57_

Yes! I had written Rust for years before "[match ergonomics](https://rust-lang.github.io/rfcs/2005-match-ergonomics.html)" landed. That RFC essentially absolved you of needing to worry about using `ref` or `ref mut` or whatever. But I had always found the `ref` and `ref mut` bindings to be helpful for knowing when something was borrowed versus when ownership had been transferred.

It's a small matter overall. Do you want me to drop the `ref`? (I wonder if there is a Clippy lint for it.)

---

_@BurntSushi reviewed on 2023-11-06 19:57_

---

_@charliermarsh approved on 2023-11-06 19:57_

Nice work.

---

_Comment by @BurntSushi on 2023-11-06 19:58_

@charliermarsh Can you check out the two commits I've just pushed? In particular, the stricter parser caused two tests to fail. I think my changes to those tests are correct, but I'd like to be cautious here!

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/invalid.rs`:44 on 2023-11-06 19:58_

No I don't mind it, I was just curious why, but that makes sense. Unless I really can't help it, I try to avoid asking for changes like this that we can't enforce automatically via tooling.

(Relatedly we often talk about how we want the box match thing from nightly.)

---

_@charliermarsh reviewed on 2023-11-06 19:58_

---

_Comment by @charliermarsh on 2023-11-06 19:58_

Taking a closer look at those changes...

---

_Comment by @charliermarsh on 2023-11-06 20:01_

@BurntSushi - Those both look correct to me. I actually don't fully understand why `app_label=\"core\", model=\"user\"",` was previously marked as valid code, what was the "target" and the "value"?

---

_Comment by @BurntSushi on 2023-11-06 20:05_

> was previously marked as valid code, what was the "target" and the "value"?

For `x="a", y="b"`, it looks like it parsed as two targets, `x` and `("a", y)` with a _single_ value of `b`. It's not totally clear to me how it gets that particular parse. That first equals sign, I'd imagine, would inhibit picking up the second target.

---

_Review comment by @BurntSushi on `crates/ruff_python_parser/src/invalid.rs`:44 on 2023-11-06 20:06_

> (Relatedly we often talk about how we want the box match thing from nightly.)

Yeah, that one has been unstable since the beginning sadly. I don't think think it's landing any time soon unfortunately. :-(

---

_@BurntSushi reviewed on 2023-11-06 20:06_

---

_Comment by @charliermarsh on 2023-11-06 20:06_

Ahh ok, so it's like `x = ("a", y) = "b"`. Makes sense-ish.

---

_Review comment by @charliermarsh on `crates/ruff_python_parser/src/invalid.rs`:44 on 2023-11-06 20:08_

We mostly want it in case we choose to `Box` our large AST variants, since we pattern match a lot!

---

_@charliermarsh reviewed on 2023-11-06 20:08_

---

_Comment by @github-actions[bot] on 2023-11-06 20:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -27 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -8 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/config_templates/default_webserver_config.py#L84'>airflow/config_templates/default_webserver_config.py:84:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/example_dags/tutorial.py#L63'>airflow/example_dags/tutorial.py:63:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/serialization/serialized_objects.py#L1195'>airflow/serialization/serialized_objects.py:1195:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/serialization/serialized_objects.py#L1211'>airflow/serialization/serialized_objects.py:1211:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/settings.py#L633'>airflow/settings.py:633:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/utils/log/secrets_masker.py#L174'>airflow/utils/log/secrets_masker.py:174:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/system/providers/amazon/aws/example_eks_templated.py#L45'>tests/system/providers/amazon/aws/example_eks_templated.py:45:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/system/providers/amazon/aws/example_eks_templated.py#L49'>tests/system/providers/amazon/aws/example_eks_templated.py:49:1:</a> ERA001 Found commented-out code
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/examples/integration/layout/plot_fixed_frame_size.py#L40'>examples/integration/layout/plot_fixed_frame_size.py:40:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/integration/widgets/tables/test_source_updates.py#L114'>tests/integration/widgets/tables/test_source_updates.py:114:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/integration/widgets/tables/test_source_updates.py#L167'>tests/integration/widgets/tables/test_source_updates.py:167:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/integration/widgets/tables/test_source_updates.py#L221'>tests/integration/widgets/tables/test_source_updates.py:221:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/integration/widgets/tables/test_source_updates.py#L269'>tests/integration/widgets/tables/test_source_updates.py:269:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/plotting/test__renderer.py#L125'>tests/unit/bokeh/plotting/test__renderer.py:125:1:</a> ERA001 Found commented-out code
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/data_import/slack.py#L581'>zerver/data_import/slack.py:581:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/digest.py#L89'>zerver/lib/digest.py:89:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/display_recipient.py#L106'>zerver/lib/display_recipient.py:106:5:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/email_notifications.py#L309'>zerver/lib/email_notifications.py:309:5:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/email_notifications.py#L317'>zerver/lib/email_notifications.py:317:5:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/email_notifications.py#L321'>zerver/lib/email_notifications.py:321:5:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/middleware.py#L152'>zerver/middleware.py:152:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/middleware.py#L175'>zerver/middleware.py:175:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/tests/test_events.py#L548'>zerver/tests/test_events.py:548:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/tests/test_events.py#L550'>zerver/tests/test_events.py:550:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/tests/test_events.py#L556'>zerver/tests/test_events.py:556:17:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/tests/test_events.py#L727'>zerver/tests/test_events.py:727:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zproject/prod_settings_template.py#L264'>zproject/prod_settings_template.py:264:1:</a> ERA001 Found commented-out code
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ERA001 | 27 | 0 | 27 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -25 violations, +0 -0 fixes in 41 projects)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/config_templates/default_webserver_config.py#L84'>airflow/config_templates/default_webserver_config.py:84:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/example_dags/tutorial.py#L63'>airflow/example_dags/tutorial.py:63:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/settings.py#L633'>airflow/settings.py:633:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/airflow/utils/log/secrets_masker.py#L174'>airflow/utils/log/secrets_masker.py:174:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/system/providers/amazon/aws/example_eks_templated.py#L45'>tests/system/providers/amazon/aws/example_eks_templated.py:45:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/apache/airflow/blob/59b32dc0a0bcdffd124b82d92428f334646cd8cd/tests/system/providers/amazon/aws/example_eks_templated.py#L49'>tests/system/providers/amazon/aws/example_eks_templated.py:49:1:</a> ERA001 Found commented-out code
</pre>

</p>
</details>
<details><summary><a href="https://github.com/bokeh/bokeh">bokeh/bokeh</a> (+0 -6 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/examples/integration/layout/plot_fixed_frame_size.py#L40'>examples/integration/layout/plot_fixed_frame_size.py:40:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/integration/widgets/tables/test_source_updates.py#L114'>tests/integration/widgets/tables/test_source_updates.py:114:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/integration/widgets/tables/test_source_updates.py#L167'>tests/integration/widgets/tables/test_source_updates.py:167:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/integration/widgets/tables/test_source_updates.py#L221'>tests/integration/widgets/tables/test_source_updates.py:221:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/integration/widgets/tables/test_source_updates.py#L269'>tests/integration/widgets/tables/test_source_updates.py:269:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/bokeh/bokeh/blob/eeafcf1f0fadfcb6ad111612b669c3fc2692b21f/tests/unit/bokeh/plotting/test__renderer.py#L125'>tests/unit/bokeh/plotting/test__renderer.py:125:1:</a> ERA001 Found commented-out code
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+0 -13 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --select ALL --preview</pre>
</p>
<p>

<pre>
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/data_import/slack.py#L581'>zerver/data_import/slack.py:581:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/digest.py#L89'>zerver/lib/digest.py:89:1:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/display_recipient.py#L106'>zerver/lib/display_recipient.py:106:5:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/email_notifications.py#L309'>zerver/lib/email_notifications.py:309:5:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/email_notifications.py#L317'>zerver/lib/email_notifications.py:317:5:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/lib/email_notifications.py#L321'>zerver/lib/email_notifications.py:321:5:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/middleware.py#L152'>zerver/middleware.py:152:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/middleware.py#L175'>zerver/middleware.py:175:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/tests/test_events.py#L548'>zerver/tests/test_events.py:548:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/tests/test_events.py#L550'>zerver/tests/test_events.py:550:13:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/tests/test_events.py#L556'>zerver/tests/test_events.py:556:17:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zerver/tests/test_events.py#L727'>zerver/tests/test_events.py:727:9:</a> ERA001 Found commented-out code
- <a href='https://github.com/zulip/zulip/blob/b59e90d100cc09e195b56ae185fcfa78c209a16f/zproject/prod_settings_template.py#L264'>zproject/prod_settings_template.py:264:1:</a> ERA001 Found commented-out code
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| ERA001 | 25 | 0 | 25 | 0 | 0 |

</p>
</details>

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @charliermarsh on 2023-11-06 20:16_

Those ecosystem changes look like strict improvements to me.

---

_@zanieb approved on 2023-11-06 22:44_

Nice :)

---

_@dhruvmanila approved on 2023-11-07 02:37_

Thanks for writing such a thorough test suite! :)

---

_Merged by @BurntSushi on 2023-11-07 12:16_

---

_Closed by @BurntSushi on 2023-11-07 12:16_

---

_Branch deleted on 2023-11-07 12:16_

---
