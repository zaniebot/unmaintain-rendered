```yaml
number: 2073
title: "feat: Implement TRY201"
type: pull_request
state: merged
author: alonme
labels: []
assignees: []
merged: true
base: main
head: feat-try201
created_at: 2023-01-21T22:11:41Z
updated_at: 2023-01-22T22:08:58Z
url: https://github.com/astral-sh/ruff/pull/2073
synced_at: 2026-01-12T04:52:00Z
```

# feat: Implement TRY201

---

_Pull request opened by @alonme on 2023-01-21 22:11_

I'm new to rust - so i hope this looks good enough.

Would love any feedback

See: #2056

---

_Comment by @charliermarsh on 2023-01-21 22:23_

Awesome, will take a look in a sec. Thanks for getting involved!

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:16 on 2023-01-21 22:25_

Nit: we prefer to use backticks around raise, like:

```
Use `raise` ...
```

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:27 on 2023-01-21 22:53_

I think we need to walk the tree recursively here. As-is, we'll miss this case:

```py
def bad():
    try:
        process()
    except MyException as e:
        logger.exception("process failed")
        if True:
            raise e
```

Since `stmt` here would be the `StmtKind::If`. We have to go one level further.

Here's how I'd recommend doing it...

If you look in `src/rules/flake8_annotations/rules.rs`, we have a struct called `ReturnStatementVisitor`. It just iterates over a bunch of statements recursively and collects all the `StmtKind::Return` statements. Here, we can add a `RaiseStatementVisitor` that does the same thing. Then you can iterate over the raises, and keep the logic below.


---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:33 on 2023-01-21 22:54_

I think the cast here is unnecessary -- you should be able to do this directly:

```diff
diff --git a/src/rules/tryceratops/rules/verbose_raise.rs b/src/rules/tryceratops/rules/verbose_raise.rs
index 84e1de38..9bd4ada0 100644
--- a/src/rules/tryceratops/rules/verbose_raise.rs
+++ b/src/rules/tryceratops/rules/verbose_raise.rs
@@ -26,11 +26,9 @@ pub fn verbose_raise(checker: &mut Checker, handlers: &[Excepthandler]) {
             // look for `raise` statements in the body
             for stmt in body {
                 if let StmtKind::Raise {
-                    exc: Some(box_expr),
-                    ..
+                    exc: Some(expr), ..
                 } = &stmt.node
                 {
-                    let expr: &Located<ExprKind> = box_expr;
                     // if the the raised object is a name - check if its the same name that was
                     // assigned to the exception
                     if let ExprKind::Name { id, .. } = &expr.node {
```

That compiles for me without an error. Rust handles the dereferences for you!

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:40 on 2023-01-21 22:55_

It looks like the original plugin uses `Range::from_located(stmt)` (the location of the `raise`). Not sure which is better.

---

_@charliermarsh requested changes on 2023-01-21 22:55_

Awesome! Some feedback, let me know if any of it is unclear.

---

_Review comment by @alonme on `src/rules/tryceratops/rules/verbose_raise.rs`:40 on 2023-01-22 06:04_

I think the fix here is to delete the name, so pointing to the name makes more sense to me,
but both are probably good.

---

_@alonme reviewed on 2023-01-22 06:04_

---

_Review comment by @alonme on `src/rules/tryceratops/rules/verbose_raise.rs`:27 on 2023-01-22 06:11_

Cool!

any chance that we are missing `ClassDef` there?
https://github.com/charliermarsh/ruff/blob/main/src/rules/flake8_annotations/rules.rs#L26

---

_@alonme reviewed on 2023-01-22 06:11_

---

_@charliermarsh reviewed on 2023-01-22 17:55_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:27 on 2023-01-22 17:55_

I think we could add `ClassDef` there. However it's probably just an optimization in practice... If a `return` is inside a `ClassDef`, but _not_ inside a `method`, you get a syntax error anyway.

---

_@charliermarsh reviewed on 2023-01-22 17:59_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:40 on 2023-01-22 17:59_

Yeah that's fine with me!

---

_@alonme reviewed on 2023-01-22 21:21_

---

_Review comment by @alonme on `src/rules/tryceratops/rules/verbose_raise.rs`:27 on 2023-01-22 21:21_

yea makes sense

---

_Review requested from @charliermarsh by @alonme on 2023-01-22 21:42_

---

_Comment by @alonme on 2023-01-22 21:43_

@charliermarsh  - should be good now,
Thanks for the tips!

---

_@charliermarsh reviewed on 2023-01-22 21:46_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:34 on 2023-01-22 21:46_

This will end up raising twice for this case:

```py
try:
    1/0
except ZeroDivisionError as e:
    try:
        foo
    except Exception as e:
        raise e
```

But it looks like the original plugin does that too, so, oh well.

---

_@charliermarsh reviewed on 2023-01-22 21:46_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:34 on 2023-01-22 21:46_

It'll also raise here, but, that's fine:

```py
try:
    1/0
except ZeroDivisionError as e:
    e = 1
    raise e
```

---

_Review comment by @alonme on `src/rules/tryceratops/rules/verbose_raise.rs`:34 on 2023-01-22 21:48_

yes, i didn't like the fact that the second one raises, but the original didn't handle shadowing of the name, so i guess that is ok.

is there anything where something like that is handled in ruff?

---

_@alonme reviewed on 2023-01-22 21:48_

---

_@charliermarsh reviewed on 2023-01-22 21:48_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:34 on 2023-01-22 21:48_

It's possible for us to handle these cases correctly by making this rule run on `Raise` statements rather than over `&[Excepthandler]`... We could see if the raise references a name, then do `checker.find_binding(name)`, and check if that binding comes from an except handler. It's a bit more involved, but it'd be more robust and probably a bit faster. If you're interested in trying that, cool! If not, we can still merge with this structure.

---

_@alonme reviewed on 2023-01-22 21:50_

---

_Review comment by @alonme on `src/rules/tryceratops/rules/verbose_raise.rs`:34 on 2023-01-22 21:50_

hmm sounds interesting,
I wouldn't be able to try that today - but i would like to try,
so up to you, if you want to merge this and i will get to improving it later

---

_@charliermarsh reviewed on 2023-01-22 21:54_

---

_Review comment by @charliermarsh on `src/rules/tryceratops/rules/verbose_raise.rs`:34 on 2023-01-22 21:54_

Yeah that's fine too. We can merge as-is, especially since (AFAICT) it has parity with the existing plugin.

---

_Merged by @charliermarsh on 2023-01-22 22:08_

---

_Closed by @charliermarsh on 2023-01-22 22:08_

---
