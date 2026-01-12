```yaml
number: 716
title: "Implement U013: Unnecessary TypedDict syntactic form"
type: pull_request
state: merged
author: martinlehoux
labels: []
assignees: []
merged: true
base: main
head: main
created_at: 2022-11-13T09:14:45Z
updated_at: 2022-11-18T17:10:48Z
url: https://github.com/astral-sh/ruff/pull/716
synced_at: 2026-01-12T15:55:05Z
```

# Implement U013: Unnecessary TypedDict syntactic form

---

_@martinlehoux_

- https://github.com/charliermarsh/ruff/issues/351

---

_Renamed from "feat: U013 Rewrite TypedDict Assign" to "Implement U013: Unnecessary TypedDict syntactic form" by @martinlehoux on 2022-11-13 21:32_

---

_Comment by @martinlehoux on 2022-11-13 21:34_

@charliermarsh I think I'm almost done here, might be interesting to have your feedback.

I also have to implement: Do not convert when keys are not valid identifiers
// MyType9 = TypedDict('MyType9', {'in': int, 'x-y': int})


---

_Comment by @charliermarsh on 2022-11-13 21:36_

(We have `IDENTIFIER_REGEX` in `src/flake8_bugbear/constants.rs` that I _think_ can help with that.)

---

_@andersk reviewed on 2022-11-13 21:44_

---

_Review comment by @andersk on `src/checks.rs`:1659 on 2022-11-13 21:44_

“Unnecessary” implies that something could be deleted; both forms are “syntactic”.

I suggest something like `` Convert `TypedDict` functional syntax to class syntax ``.

Also, this should all apply equally to `NamedTuple`.

---

_Review comment by @charliermarsh on `src/checks.rs`:1659 on 2022-11-15 21:21_

Yeah agreed, nice suggestion.

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_typed_dict_syntactic_form.rs`:27 on 2022-11-15 21:27_

I think you can avoid some clones by returning references here:

```rust
--- a/src/pyupgrade/plugins/unnecessary_typed_dict_syntactic_form.rs
+++ b/src/pyupgrade/plugins/unnecessary_typed_dict_syntactic_form.rs
@@ -10,10 +10,10 @@ use crate::{
 };

 // Returns (class_name, args, keywords)
-fn match_typed_dict_assign(
-    targets: &[Expr],
-    value: &Expr,
-) -> Option<(String, Vec<Expr>, Vec<Keyword>)> {
+fn match_typed_dict_assign<'a>(
+    targets: &'a [Expr],
+    value: &'a Expr,
+) -> Option<(&'a str, &'a [Expr], &'a [Keyword])> {
     if let Some(target) = targets.get(0) {
         if let ExprKind::Name { id: class_name, .. } = &target.node {
             if let ExprKind::Call {
@@ -24,7 +24,7 @@ fn match_typed_dict_assign(
             {
                 if let ExprKind::Name { id: func_name, .. } = &func.node {
                     if func_name == "TypedDict" {
-                        return Some((class_name.clone(), args.clone(), keywords.clone()));
+                        return Some((class_name, args, keywords));
                     }
                 }
             }
@@ -61,7 +61,7 @@ fn create_pass_stmt() -> Stmt {
     Stmt::new(Default::default(), Default::default(), StmtKind::Pass)
 }

-fn create_classdef_stmt(class_name: &String, body: Vec<Stmt>, total: Option<KeywordData>) -> Stmt {
+fn create_classdef_stmt(class_name: &str, body: Vec<Stmt>, total: Option<KeywordData>) -> Stmt {
     let keywords = match total {
         Some(keyword) => vec![Keyword::new(
             Default::default(),
@@ -139,9 +139,9 @@ fn get_properties_from_keywords(keywords: &[Keyword]) -> Result<Vec<Stmt>> {
 fn try_to_fix(
     check: &mut Check,
     stmt: &Stmt,
-    class_name: &String,
+    class_name: &str,
     args: &[Expr],
-    keywords: &Vec<Keyword>,
+    keywords: &[Keyword],
 ) -> Result<()> {
     let mut total: Option<KeywordData> = None;
     let body = if let Some(dict) = args.get(1) {
```

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_typed_dict_syntactic_form.rs`:190 on 2022-11-15 21:29_

I think it would be clearer to have `try_to_fix` return `Result<Fix>`, then do something like:

```rust
match try_to_fix(stmt, &class_name, &args, &keywords) {
    Ok(fix) => {
        check.amend(fix)
    }
    Err(e) => {
        error!("Failed to convert TypedDict: {}", e);
    }
}
```

That also removes the dependency on `check` in `try_to_fix`, _and_ we'd no longer be silently dropping the error.

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_typed_dict_syntactic_form.rs`:148 on 2022-11-15 21:31_

Can we pull this assignment into a separate series of `if` statements, or return a tuple from these branches? I think it'd be a bit clearer than using this branch to assign to `total` as a side-effect.

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_typed_dict_syntactic_form.rs`:160 on 2022-11-15 21:34_

Can we add a comment noting that the following is illegal?

```py
MyType = TypedDict('MyType', {'a': int, 'b': str}, a=int, b=str)
```


---

_@charliermarsh reviewed on 2022-11-15 21:34_

---

_@martinlehoux reviewed on 2022-11-16 08:24_

---

_Review comment by @martinlehoux on `src/pyupgrade/plugins/unnecessary_typed_dict_syntactic_form.rs`:190 on 2022-11-16 08:24_

Ok, I guess I didn't really know how ruff would handle errors like that

What does `error!(...)` do? 

---

_@martinlehoux reviewed on 2022-11-16 08:26_

---

_Review comment by @martinlehoux on `src/pyupgrade/plugins/unnecessary_typed_dict_syntactic_form.rs`:148 on 2022-11-16 08:26_

Yes you're right I didn't think of tuples. I think I did this to avoid double checking conditions

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/unnecessary_typed_dict_syntactic_form.rs`:190 on 2022-11-16 17:03_

`error!(...)` is just a logging macro -- so it doesn't return anything, it just logs that at "error" level.

---

_@charliermarsh reviewed on 2022-11-16 17:03_

---

_Comment by @martinlehoux on 2022-11-16 18:02_

Two question:
1. Where should I locate `IDENTIFIER_REGEX`? Seems wrong to import from flake8
2. When there is one non valid identifier, we must not return any check, because it is a valid reason to use the functional form. Do you have an idea on how to structure that? It seems to me complicated to juggle between fix errors (meaning we failed to fix something fixable) and check errors (meaning that there is no check to raise). Of course I can duplicate the parsing in each phase, but it seems like a waste

---

_Comment by @charliermarsh on 2022-11-17 16:50_

Let's put `IDENTIFIER_REGEX` in... `src/python/identifiers.rs` for now.

I think we should _just_ do that validation when we run the check, and in the fix, _assume_ that each string is a valid identifier. That's generally the pattern we've followed elsewhere: the check is responsible for all validation, and then the fix can make assumptions about the input. Does that make sense?

---

_Marked ready for review by @martinlehoux on 2022-11-18 08:18_

---

_Comment by @martinlehoux on 2022-11-18 08:18_

I think this one is OK now. I guess I'll be quicker to do `NamedTuple` with this experience

---

_Review comment by @martinlehoux on `src/pyupgrade/plugins/convert_typed_dict_functional_to_class.rs`:195 on 2022-11-18 08:20_

This should be moved at L169

---

_@martinlehoux reviewed on 2022-11-18 08:20_

---

_Comment by @charliermarsh on 2022-11-18 14:55_

Great! I'll try to get this merged today.

---

_Merged by @charliermarsh on 2022-11-18 17:10_

---

_Closed by @charliermarsh on 2022-11-18 17:10_

---
