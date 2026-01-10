```yaml
number: 14659
title: "[`ruff`] Implement `unnecessary-regular-expression` (`RUF055`)"
type: pull_request
state: merged
author: ntBre
labels:
  - rule
  - preview
  - great writeup
assignees: []
merged: true
base: main
head: brent/unnecessary-re
created_at: 2024-11-28T15:52:24Z
updated_at: 2024-12-01T16:21:08Z
url: https://github.com/astral-sh/ruff/pull/14659
synced_at: 2026-01-10T20:50:57Z
```

# [`ruff`] Implement `unnecessary-regular-expression` (`RUF055`)

---

_Pull request opened by @ntBre on 2024-11-28 15:52_

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This is a limited implementation of the rule idea from #12283 to replace some uses of the `re` module with `str` method calls. A few of the examples given there:

```python
re.sub("abc", "", s)  # => s.replace("abc", "")
re.match("abc", s)  # => s.startswith("abc")
re.search("abc", s)  # => "abc" in s
re.fullmatch("abc", s) # => "abc" == s
re.split("abc", s)  # => s.split("abc")
```

For this initial implementation, I've restricted the rule to string literals in the `pattern` argument to each of the `re` functions and further restricted these string literals to exclude any `re` metacharacters. Each of the `re` functions takes additional kwargs that change their behavior, so the rule doesn't apply when these are present either. [re.sub](https://docs.python.org/3/library/re.html#re.sub) can also take a function as the replacement argument (unlike `str.replace`, which expects another `str`), so the rule is also restricted to cases where that argument is also a string literal. Finally, `match`, `search`, and `fullmatch` return `Match` objects unlike the proposed fixes, so the rule only applies when these are used in a boolean test for their truth values. For example,

```python
if re.match("abc", s):
    pass
```
would trigger the rule, but the plain `re.match("abc", s)` call above would not because the returned `Match` could be used. I think this is probably a fairly common use case, so the rule can still be useful even with these restrictions.

The limitations around `Match` seem necessary, but some of the other restrictions can probably be loosened. For example, the `sub` replacement doesn't have to be a string *literal*, but it does need to be a string or at the very least not a function. Similarly, the patterns themselves could be plain `str` variables, but we need to inspect them for regex metacharacters. I didn't find a way to do that for non-literal strings, but if I missed it, that would be an easy improvement.

I think these checks can also be directly extended to the `regex` package. I saw `unraw-re-pattern` (`RUF039`), for example, handles both `re` and `regex`, but I only handled `re` for now.

## Test Plan

<!-- How was it tested? -->
`cargo test` with new `RUF055.py` snapshot test.

## Possible related rule
Right before submitting this, I tried running `RUF055.py` with python, and it crashed with a `ValueError: cannot use LOCALE flag with a str pattern`. That would be an easy thing to check with very similar code to what I have here.

---

_Comment by @Skylion007 on 2024-11-28 15:53_

@dosisod This seems like it would be a good 'refurb' rule for your linter

---

_Comment by @github-actions[bot] on 2024-11-28 15:58_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+50 -0 violations, +0 -0 fixes in 3 projects; 52 projects unchanged)

<details><summary><a href="https://github.com/apache/airflow">apache/airflow</a> (+6 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/apache/airflow/blob/ab2bd2d4a9d5154f9d1e9e65d30c4716eca7c4b1/helm_tests/airflow_aux/test_pod_template_file.py#L358'>helm_tests/airflow_aux/test_pod_template_file.py:358:16:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/apache/airflow/blob/ab2bd2d4a9d5154f9d1e9e65d30c4716eca7c4b1/helm_tests/airflow_aux/test_pod_template_file.py#L370'>helm_tests/airflow_aux/test_pod_template_file.py:370:16:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/apache/airflow/blob/ab2bd2d4a9d5154f9d1e9e65d30c4716eca7c4b1/helm_tests/airflow_aux/test_pod_template_file.py#L407'>helm_tests/airflow_aux/test_pod_template_file.py:407:16:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/apache/airflow/blob/ab2bd2d4a9d5154f9d1e9e65d30c4716eca7c4b1/helm_tests/airflow_aux/test_pod_template_file.py#L59'>helm_tests/airflow_aux/test_pod_template_file.py:59:16:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/apache/airflow/blob/ab2bd2d4a9d5154f9d1e9e65d30c4716eca7c4b1/helm_tests/airflow_aux/test_pod_template_file.py#L97'>helm_tests/airflow_aux/test_pod_template_file.py:97:16:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/apache/airflow/blob/ab2bd2d4a9d5154f9d1e9e65d30c4716eca7c4b1/providers/tests/cncf/kubernetes/log_handlers/test_log_handlers.py#L158'>providers/tests/cncf/kubernetes/log_handlers/test_log_handlers.py:158:16:</a> RUF055 [*] Plain string pattern passed to `re` function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+40 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/lib/test_classes.py#L2208'>zerver/lib/test_classes.py:2208:20:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_delete_unclaimed_attachments.py#L36'>zerver/tests/test_delete_unclaimed_attachments.py:36:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_markdown_thumbnail.py#L140'>zerver/tests/test_markdown_thumbnail.py:140:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_realm.py#L2401'>zerver/tests/test_realm.py:2401:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_realm.py#L2476'>zerver/tests/test_realm.py:2476:64:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_scheduled_messages.py#L655'>zerver/tests/test_scheduled_messages.py:655:20:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_scheduled_messages.py#L661'>zerver/tests/test_scheduled_messages.py:661:20:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_thumbnail.py#L315'>zerver/tests/test_thumbnail.py:315:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_thumbnail.py#L414'>zerver/tests/test_thumbnail.py:414:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_thumbnail.py#L465'>zerver/tests/test_thumbnail.py:465:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_thumbnail.py#L480'>zerver/tests/test_thumbnail.py:480:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_thumbnail.py#L494'>zerver/tests/test_thumbnail.py:494:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_thumbnail.py#L601'>zerver/tests/test_thumbnail.py:601:27:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_thumbnail.py#L651'>zerver/tests/test_thumbnail.py:651:32:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_thumbnail.py#L706'>zerver/tests/test_thumbnail.py:706:27:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_thumbnail.py#L747'>zerver/tests/test_thumbnail.py:747:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L509'>zerver/tests/test_upload.py:509:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L528'>zerver/tests/test_upload.py:528:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L588'>zerver/tests/test_upload.py:588:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L592'>zerver/tests/test_upload.py:592:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L603'>zerver/tests/test_upload.py:603:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L691'>zerver/tests/test_upload.py:691:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L751'>zerver/tests/test_upload.py:751:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L797'>zerver/tests/test_upload.py:797:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L849'>zerver/tests/test_upload.py:849:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L918'>zerver/tests/test_upload.py:918:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L963'>zerver/tests/test_upload.py:963:22:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload.py#L988'>zerver/tests/test_upload.py:988:26:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_local.py#L115'>zerver/tests/test_upload_local.py:115:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_local.py#L39'>zerver/tests/test_upload_local.py:39:19:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_local.py#L52'>zerver/tests/test_upload_local.py:52:19:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_local.py#L62'>zerver/tests/test_upload_local.py:62:19:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_local.py#L96'>zerver/tests/test_upload_local.py:96:19:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_s3.py#L126'>zerver/tests/test_upload_s3.py:126:19:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_s3.py#L140'>zerver/tests/test_upload_s3.py:140:23:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_s3.py#L173'>zerver/tests/test_upload_s3.py:173:29:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_s3.py#L180'>zerver/tests/test_upload_s3.py:180:29:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_s3.py#L62'>zerver/tests/test_upload_s3.py:62:19:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_s3.py#L79'>zerver/tests/test_upload_s3.py:79:19:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/zulip/zulip/blob/1bf3bc0f18c7863727641666033ecbfd7fe648d6/zerver/tests/test_upload_s3.py#L91'>zerver/tests/test_upload_s3.py:91:19:</a> RUF055 [*] Plain string pattern passed to `re` function
</pre>

</p>
</details>
<details><summary><a href="https://github.com/astropy/astropy">astropy/astropy</a> (+4 -0 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview</pre>
</p>
<p>

<pre>
+ <a href='https://github.com/astropy/astropy/blob/5d51416f4687c51cfc6d034ce95e2838469bcc61/astropy/coordinates/tests/test_frames.py#L364'>astropy/coordinates/tests/test_frames.py:364:11:</a> RUF055 Plain string pattern passed to `re` function
+ <a href='https://github.com/astropy/astropy/blob/5d51416f4687c51cfc6d034ce95e2838469bcc61/astropy/io/fits/card.py#L771'>astropy/io/fits/card.py:771:21:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/astropy/astropy/blob/5d51416f4687c51cfc6d034ce95e2838469bcc61/astropy/io/registry/base.py#L446'>astropy/io/registry/base.py:446:25:</a> RUF055 [*] Plain string pattern passed to `re` function
+ <a href='https://github.com/astropy/astropy/blob/5d51416f4687c51cfc6d034ce95e2838469bcc61/astropy/time/tests/test_fast_parser.py#L16'>astropy/time/tests/test_fast_parser.py:16:15:</a> RUF055 [*] Plain string pattern passed to `re` function
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| RUF055 | 50 | 50 | 0 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:218 on 2024-11-28 16:15_

```suggestion
    let has_metacharacters = string_lit.value.to_str().contains(['.',  '^', '$', '*', '+', '?', '{', '}', '[', ']', '\\', '|', '(', ')']);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:240 on 2024-11-28 16:16_

```suggestion
    diagnostic.set_fix(Fix::safe_edit(Edit::range_replacement(
        repl,
        call.range
    )));
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF055.py`:47 on 2024-11-28 16:18_

It would be great to test a few more meta characters rather than only testing the different methods. For example, demonstrate that `\n`, `(`, etc work

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:218 on 2024-11-28 16:20_

I like how you intentionally excluded meta-characters. So consider this an extension, and I think it's totally fine to do this as a follow-up pr (or not at all). 

It would be nice if the rule only skips replacement for characters that are different between regex expressions and regular strings. For example, `\n` matches `\n` in a regex and a string. 

---

_Review comment by @MichaReiser on `crates/ruff_linter/resources/test/fixtures/ruff/RUF055.py`:67 on 2024-11-28 16:21_

Commonly, regex patterns are written using raw-strings because it reduces the need for escaping. There's even a ruff rule for this. That's why I think it would be good to add a few raw-string examples

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:82 on 2024-11-28 16:23_

Nit: I personally found the alias more confusing than helpful. Calling `call.arguments.find_argument` isn't much longer. You could consider storing `arguments` in a variable, if the length annoys you

```
let arguments = &call.arguments;
arguments.find_argument("pattern", 0)?
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:204 on 2024-11-28 16:25_

Could we retrieve the `pattern` from `re_func` instead?

---

_Label `great writeup` added by @AlexWaygood on 2024-11-28 16:27_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:236 on 2024-11-28 16:27_

We should mark the fix as unsafe if the `call.range` contains any comments. You can use `checker.comment_ranges.has_comments` for that. 

We should then extend the documentation to explain the rules fixability

---

_@MichaReiser reviewed on 2024-11-28 16:30_

This overall looks great. You made this look simple. 

The only thing that I notice we miss is raw-string support (or, at least, tests for it). Raw strings are the recommended way to write regex patterns in python because it avoids the need for double escaping.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:24 on 2024-11-28 16:31_

docs nit: we prefer for this section to be a 1-2 sentence summary of the antipatterns the rule is looking for. These details are great, but should be put in a later section, in my opinion (maybe `## Details`, below the `## Why is this bad?` section?).

Here, I think the simple answer to "What it does?" is

```rs
/// Checks for uses of the `re` module that can be replaced with builtin `str` methods.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:178 on 2024-11-28 16:35_

nit: our usual style is to put the routine that actually implements the rule quite high up in the module, usually directly below the struct for the diagnostic itself

---

_@ntBre reviewed on 2024-11-28 16:37_

---

_Review comment by @ntBre on `crates/ruff_linter/resources/test/fixtures/ruff/RUF055.py`:67 on 2024-11-28 16:37_

Good call. I was pleasantly surprised to see that it handled the raw-strings in the zulip ecosystem check, but I agree that this should be part of the test.

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:77 on 2024-11-28 16:43_

nit: it looks like the only thing you need from `Checker` is actually the semantic model -- which you've already assigned to a variable in the function you call this from. I think you could simplify this function a little bit like this:

```diff
-use ruff_python_semantic::Modules;
+use ruff_python_semantic::{Modules, SemanticModel};
 use ruff_text_size::TextRange;
 
 use crate::checkers::ast::Checker;
@@ -74,7 +74,7 @@ struct ReFunc<'a> {
 
 impl<'a> ReFunc<'a> {
     fn from_call_expr(
-        checker: &'a mut Checker,
+        semantic: &SemanticModel,
         call: &'a ExprCall,
         func_name: &str,
     ) -> Option<Self> {
@@ -83,7 +83,7 @@ impl<'a> ReFunc<'a> {
 
         // the proposed fixes for match, search, and fullmatch rely on the
         // return value only being used for its truth value
-        let in_if_context = checker.semantic().in_boolean_test();
+        let in_if_context = semantic.in_boolean_test();
 
         match (func_name, nargs) {
             // `split` is the safest of these to fix, as long as metacharacters
@@ -193,7 +193,7 @@ pub(crate) fn unnecessary_regular_expression(checker: &mut Checker, call: &ExprC
 
     // skip calls with more than `pattern` and `string` arguments (and `repl`
     // for `sub`)
-    let Some(re_func) = ReFunc::from_call_expr(checker, call, func) else {
+    let Some(re_func) = ReFunc::from_call_expr(semantic, call, func) else {
         return;
     };
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:186 on 2024-11-28 16:44_

```suggestion
    let Some(qualified_name) = semantic.resolve_qualified_name(&call.func) else {
```

---

_@AlexWaygood reviewed on 2024-11-28 16:48_

Thanks for the excellent PR writeup -- it made reviewing this really easy! This looks great overall.

> The limitations around Match seem necessary, but some of the other restrictions can probably be loosened. For example, the sub replacement doesn't have to be a string literal, but it does need to be a string or at the very least not a function. Similarly, the patterns themselves could be plain str variables, but we need to inspect them for regex metacharacters. I didn't find a way to do that for non-literal strings, but if I missed it, that would be an easy improvement.

We don't have an out-of-the-box way of doing this for strings right now, so I wouldn't try to tackle it in this PR. But if you're interested, a followup might be to add an `is_str()` function to `ruff_python_semantic::analyze::typing` that looks similar to this `is_list` function: https://github.com/astral-sh/ruff/blob/d9cbf2fe44e3f13b28295377a41ae541cab6b54f/crates/ruff_python_semantic/src/analyze/typing.rs#L739-L745

And then you could use that in this rule for stronger type inference

---

_Label `rule` added by @AlexWaygood on 2024-11-28 16:53_

---

_Label `preview` added by @AlexWaygood on 2024-11-28 16:53_

---

_Comment by @ntBre on 2024-11-28 17:37_

Thank you both for the great reviews! I think I've incorporated all of the suggestions, with the exception of handling simple escapes like `\n`. I want to search around a bit for how escapes are handled elsewhere in the code, but if nothing else, it shouldn't be that hard to allow a few common escapes like `\n` at least. I'm also happy to leave that as a follow-up though.

Similarly, I'm quite interested in the `is_str` idea, but I agree that that should be separate. I'll plan to look into that next.

---

_@dscorbett reviewed on 2024-11-28 17:39_

---

_Review comment by @dscorbett on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:98 on 2024-11-28 17:39_

`]` and `}` can be removed from this list. They are only metacharacters when they follow `[` and `{`, which are already rejected here, so we can assume `]` and `}` are always matched literally.

---

_@sbrugman reviewed on 2024-11-28 18:01_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:50 on 2024-11-28 18:01_

```suggestion
/// the fix can be applied safely.
/// 
/// ## References
/// - [Python Regular Expression HOWTO: Common Problems - Use String Methods](https://docs.python.org/3/howto/regex.html#use-string-methods)
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:45 on 2024-11-28 18:02_

```suggestion
/// The rule reports the following calls when the first argument to the call is
/// a plain string literals, and no additional flags are passed:
///
/// - `re.sub`
/// - `re.match`
/// - `re.search`
/// - `re.fullmatch`
/// - `re.split`
///
/// For `re.sub`, the `repl` (replacement) argument must also be a string literal,
/// not a function. For `re.match`, `re.search`, and `re.fullmatch`, the return
/// value must also be used only for its truth value.
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:129 on 2024-11-28 18:03_

```suggestion
    checker.diagnostics.push(diagnostic.with_fix(fix));
```

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:161 on 2024-11-28 18:04_

nit: I'd just inline `nargs`

```suggestion
        // the proposed fixes for match, search, and fullmatch rely on the
        // return value only being used for its truth value
        let in_if_context = semantic.in_boolean_test();

        match (func_name, call.arguments.len()) {
```

---

_@AlexWaygood approved on 2024-11-28 18:04_

This is great, thanks!

---

_@sbrugman reviewed on 2024-11-28 18:07_

---

_Review comment by @sbrugman on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:125 on 2024-11-28 18:07_

```suggestion
let fix = Fix::applicable_edit(
    Edit::range_replacement(repl, call.range),
    if checker.comment_ranges().has_comments(call, checker.source() {
        Applicability::Unsafe
    } else {
        Applicability::Safe
    }
);
```

---

_Review comment by @ntBre on `crates/ruff_linter/src/rules/ruff/rules/unnecessary_regular_expression.rs`:98 on 2024-11-28 18:17_

Fixed! I also removed `)`, which I think should behave the same way.

---

_@ntBre reviewed on 2024-11-28 18:17_

---

_Merged by @AlexWaygood on 2024-11-28 18:29_

---

_Closed by @AlexWaygood on 2024-11-28 18:29_

---

_@zanieb reviewed on 2024-12-01 16:21_

---

_Review comment by @zanieb on `crates/ruff_linter/src/rules/ruff/snapshots/ruff_linter__rules__ruff__tests__preview__RUF055_RUF055.py.snap`:79 on 2024-12-01 16:21_

As a minor note, I think this should be `s == "abc"`. It's a minor stylistic difference (in which I think `x == VALUE` is more idiomatic), but it can cause actual differences due to type checking implementations, e.g. https://github.com/microsoft/pyright/issues/9093

---
