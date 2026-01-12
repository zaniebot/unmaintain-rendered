```yaml
number: 1434
title: "PyUpgrade: Turn errors into OSError"
type: pull_request
state: merged
author: colin99d
labels: []
assignees: []
merged: true
base: main
head: osalias
created_at: 2022-12-29T02:00:13Z
updated_at: 2023-01-02T04:43:49Z
url: https://github.com/astral-sh/ruff/pull/1434
synced_at: 2026-01-12T05:36:31Z
```

# PyUpgrade: Turn errors into OSError

---

_Pull request opened by @colin99d on 2022-12-29 02:00_

A part of #827. The following needs to be happen for this to be moved out of draft status:

- [x] Handle statements with multiple except statements
- [x] Handle `raise` statements
- [x] Add the rest of the tests from pyupgrade

---

_Marked ready for review by @colin99d on 2022-12-29 18:11_

---

_Comment by @charliermarsh on 2022-12-29 18:14_

@colin99d - Is this all set to review?

---

_Comment by @colin99d on 2022-12-29 18:15_

@charliermarsh I got this done and we are passing all pyupgrade tests. I intentionally left it a little messy because I figured it would need refactored, and I wanted your input on how. This one was a lot more code because there were a lot more possible situations. Also please show me how to get Located<ExprKind> out of the box, so that I only need two implement my trait for two types. I tried using (*item).downcast::<Located<ExprKind>>(), but rust was saying that Box<Located<ExprKind>> has no method downcast (same with downcast_ref).

---

_Comment by @charliermarsh on 2022-12-29 18:32_

Ok thanks, I'll try to review today!

---

_Comment by @charliermarsh on 2022-12-29 18:32_

(And will look to answer your questions too.)

---

_Comment by @colin99d on 2022-12-29 18:49_

Perfect! Also is there a Discord server or something similar for people contributing to chat on?

---

_Comment by @charliermarsh on 2022-12-29 22:27_

There's not, but I've considered creating one 🤔 

---

_Review comment by @charliermarsh on `src/ast/types.rs`:25 on 2022-12-29 22:28_

I agree we should have this. It'd be nice to fix the existing call-sites though... Not sure if you're interested in doing that :D

---

_@charliermarsh reviewed on 2022-12-29 22:28_

---

_Review comment by @colin99d on `src/ast/types.rs`:25 on 2022-12-29 23:55_

I can bust it out.

---

_@colin99d reviewed on 2022-12-29 23:55_

---

_Comment by @colin99d on 2022-12-29 23:56_

> There's not, but I've considered creating one 🤔

Would be nice just so that I can know where you plan on taking this thing and how I can help it get there. You could make it private for now if your worried about spammers and randos.

---

_@colin99d reviewed on 2022-12-30 01:00_

---

_Review comment by @colin99d on `src/ast/types.rs`:25 on 2022-12-30 01:00_

Got this done. I will fix clippy once you are good with the rest of my code.

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 01:26_

I'm wondering if this whole part (handling try, raise, etc.) could be changed to instead look at `ExprKind::Attribute` and `ExprKind::Name` (that is: in `ast.rs`, call this plugin for every `ExprKind::Attribute` and every `ExprKind::Name`).

Like, ultimately, we just want to rewrite all references. It doesn't matter if they occur in try, or raise, or wherever. And here, you're just delegating to checks on attribute and name anyway. (Within call, you're then checking name and attribute.)

Would that work? Would it simplify things?

---

_@charliermarsh reviewed on 2022-12-30 01:26_

---

_@charliermarsh reviewed on 2022-12-30 01:27_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/os_error_alias.rs`:33 on 2022-12-30 01:27_

Everywhere that we do `Located<ExprKind>`, we should instead of the RustPython type `Expr`. There are analogous types for every `Located<T>`.

---

_@colin99d reviewed on 2022-12-30 01:29_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:33 on 2022-12-30 01:29_

This is very good to know.

---

_@colin99d reviewed on 2022-12-30 01:30_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 01:30_

I think you are right here... Will take me a minute to confirm but im on it.

---

_@charliermarsh reviewed on 2022-12-30 01:31_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 01:31_

I’m guessing pyupgrade doesn’t do this, but I’d have to look. If not, I wonder why.

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 01:42_

So there is one issue with this implementation. If the exceptions are in a tuple, we lose our ability to intelligently group them if we only go by name and attribute (unless there is something I am missing here).

---

_@colin99d reviewed on 2022-12-30 01:42_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 01:53_

Ahhh interesting. We actually have a different rule, in flake8-bugbear, that detects and removes duplicate exceptions. So perhaps we can accept that limitation here, and rely on the other rule to clean up those rare errors? What do you think? It feels like we’re accepting a lot of complexity here to accommodate that.

---

_@charliermarsh reviewed on 2022-12-30 01:53_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 02:01_

I have one idea I like a little more. Let me see if I can get it to work, and if not I will do your way.

---

_@colin99d reviewed on 2022-12-30 02:01_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 02:12_

I can also give the code a closer read now that I understand that limitation.

---

_@charliermarsh reviewed on 2022-12-30 02:12_

---

_@colin99d reviewed on 2022-12-30 02:51_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 02:51_

Haha still thinking through this. The issue with the solution you provided is that if the simplifier ends up with only one option the final value will show as (OSError,) when it should be OSError per pyupgrade. This means we will then have to run a third check. So I am weighing three checks, vs how I can simplify the current code.

---

_@colin99d reviewed on 2022-12-30 02:55_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 02:55_

```
            ExprKind::Name{ .. } | ExprKind::Attribute{ .. } => {
                handle();
            }
```
Thinking about doing something like this. That way we can reduce having the same code copied everywhere.

---

_@charliermarsh reviewed on 2022-12-30 02:56_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 02:56_

It's ok with me if it ends up as `(OSError,)`.

---

_@charliermarsh reviewed on 2022-12-30 02:56_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 02:56_

If helpful.

---

_@colin99d reviewed on 2022-12-30 02:56_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 02:56_

Additionally, if we figure out how to get rid of the box I can remove one of the three implementations of OSErrorAliasChecker.

---

_@charliermarsh reviewed on 2022-12-30 02:59_

---

_Review comment by @charliermarsh on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 02:59_

```diff --git a/src/checkers/ast.rs b/src/checkers/ast.rs
index b0e48f5d..73e39976 100644
--- a/src/checkers/ast.rs
+++ b/src/checkers/ast.rs
@@ -1087,7 +1087,7 @@ where
                 }
                 if self.settings.enabled.contains(&CheckCode::UP024) {
                     if let Some(item) = exc {
-                        pyupgrade::plugins::os_error_alias(self, item);
+                        pyupgrade::plugins::os_error_alias(self, &**item);
                     }
                 }
             }
diff --git a/src/pyupgrade/plugins/os_error_alias.rs b/src/pyupgrade/plugins/os_error_alias.rs
index f6032b6a..32c9a0a8 100644
--- a/src/pyupgrade/plugins/os_error_alias.rs
+++ b/src/pyupgrade/plugins/os_error_alias.rs
@@ -1,5 +1,5 @@
 use itertools::Itertools;
-use rustpython_ast::{Excepthandler, ExcepthandlerKind, ExprKind, Located};
+use rustpython_ast::{Excepthandler, ExcepthandlerKind, Expr, ExprKind, Located};
 
 use crate::ast::helpers::match_module_member;
 use crate::ast::types::Range;
@@ -162,29 +162,7 @@ impl OSErrorAliasChecker for &Vec<Excepthandler> {
     }
 }
 
-impl OSErrorAliasChecker for &Box<Located<ExprKind>> {
-    fn check_error(&self, checker: &mut Checker) {
-        let mut replacements: Vec<String>;
-        let mut before_replace: Vec<String>;
-        match &self.node {
-            ExprKind::Name { id, .. } => {
-                (replacements, before_replace) = check_module(checker, self);
-                if replacements.is_empty() {
-                    let new_name = get_correct_name(&id);
-                    replacements.push(new_name);
-                    before_replace.push(id.to_string());
-                }
-            }
-            ExprKind::Attribute { .. } => {
-                (replacements, before_replace) = check_module(checker, self);
-            }
-            _ => return,
-        }
-        handle_making_changes(checker, self, before_replace, replacements);
-    }
-}
-
-impl OSErrorAliasChecker for &Located<ExprKind> {
+impl OSErrorAliasChecker for &Expr {
     fn check_error(&self, checker: &mut Checker) {
         let mut replacements: Vec<String>;
         let mut before_replace: Vec<String>;
```

---

_@colin99d reviewed on 2022-12-30 03:03_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 03:03_

Push that real quick!! I can add my simplified function to it.

---

_@colin99d reviewed on 2022-12-30 03:03_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 03:03_

Still more complicated, but I can add a bunch of comments so its crystal clear.

---

_@colin99d reviewed on 2022-12-30 03:28_

---

_Review comment by @colin99d on `src/pyupgrade/plugins/os_error_alias.rs`:228 on 2022-12-30 03:28_

> Push that real quick!! I can add my simplified function to it.

Nvmd I got it

---

_Converted to draft by @colin99d on 2022-12-30 03:44_

---

_Comment by @colin99d on 2022-12-30 03:45_

Switched this to draft until I get done with all our changes.

---

_Marked ready for review by @colin99d on 2022-12-30 16:53_

---

_Comment by @colin99d on 2022-12-30 16:54_

@charliermarsh I am switching this out of draft now. Unfortunately, trying to consolidate to two implementations caused tests to fail, so I did not do it. Let me know if you want anything else cleaned up or more comments anywhere else. 

---

_Review comment by @charliermarsh on `src/ast/types.rs`:25 on 2022-12-30 18:10_

Thank you! That's no small feat! Really appreciate it!

(Please don't take this as discouragement, solely advice.) In the future, for a change like that, I'd suggest breaking it out into a separate PR, which has a bunch of benefits. For example:

1. Each PR remains clearer in its responsibilities. In the future, when someone digs up this PR to understand the `pyupgrade` changes, they'll need to sift through the `Range` refactor too.
2. Each PR can be merged faster. A PR that just does the `Range` change would be easy to approve and merge, since it'd be decoupled from the `pyupgrade` changes.

Not gonna ask that in this case since it's already done, unless you're eager to try it.

---

_@charliermarsh reviewed on 2022-12-30 18:10_

---

_Comment by @charliermarsh on 2022-12-31 04:45_

Going to revisit this tomorrow!

---

_Merged by @charliermarsh on 2022-12-31 21:36_

---

_Closed by @charliermarsh on 2022-12-31 21:36_

---

_Comment by @andersk on 2023-01-02 04:43_

> Perfect! Also is there a Discord server or something similar for people contributing to chat on?

@charliermarsh If you’re interested, Zulip will happily sponsor free Zulip Cloud hosting for Ruff (as we do [for any open-source project](https://zulip.com/for/open-source/)).

---
