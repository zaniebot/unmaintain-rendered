```yaml
number: 16047
title: "[`flake8-type-checking`] Allow f-strings, printf strings, and formatted strings for `runtime-cast-value` (`TC006`)"
type: pull_request
state: closed
author: dylwil3
labels:
  - bug
  - rule
  - preview
assignees: []
base: main
head: parser-freeze
created_at: 2025-02-08T23:25:30Z
updated_at: 2025-02-09T16:10:03Z
url: https://github.com/astral-sh/ruff/pull/16047
synced_at: 2026-01-12T15:55:53Z
```

# [`flake8-type-checking`] Allow f-strings, printf strings, and formatted strings for `runtime-cast-value` (`TC006`)

---

_@dylwil3_

Just as we allow string arguments for `cast` in [runtime-cast-value (TC006)](https://docs.astral.sh/ruff/rules/runtime-cast-value/#runtime-cast-value-tc006), we should allow f-strings, printf-style strings, and formatted strings, since these all evaluate to strings.

Closes #16037 


---

_Label `bug` added by @dylwil3 on 2025-02-08 23:25_

---

_Label `rule` added by @dylwil3 on 2025-02-08 23:25_

---

_Label `preview` added by @dylwil3 on 2025-02-08 23:25_

---

_Comment by @github-actions[bot] on 2025-02-08 23:31_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Renamed from "[`flake8-type-checking`] Allow f-strings for `runtime-cast-value` (`TC006`)" to "[`flake8-type-checking`] Allow f-strings, printf strings, and formatted strings for `runtime-cast-value` (`TC006`)" by @dylwil3 on 2025-02-09 02:43_

---

_Comment by @Daverball on 2025-02-09 07:27_

I wonder if it would be better to introduce a generic violation for invalid type expressions, since none of these kinds of strings are actually allowed in type expressions (outside of `Annotated`, but we remove the type definition flag for those parts of the expression).

That would in theory catch all such problems in the future, rather than having to play whack-a-mole on a rule-by-rule basis, having to check for things that aren't actually valid.

In this case I think it makes more sense to disable the affected UP rules inside type definitions.

---

_Comment by @AlexWaygood on 2025-02-09 12:30_

> That would in theory catch all such problems in the future, rather than having to play whack-a-mole on a rule-by-rule basis, having to check for things that aren't actually valid.

What if the user disabled the other rule? We'd still have infinite recursion under some combinations of settings, and that's not an acceptable outcome for us unfortunately.

---

_Comment by @Daverball on 2025-02-09 13:01_

> What if the user disabled the other rule? We'd still have infinite recursion under some combinations of settings, and that's not an acceptable outcome for us unfortunately.

Fair point, ideally the way the way this would work is to make it a special type of diagnostic that silences all other diagnostics contained in the target range, it is emitted even when it is disabled, just silently.

---

_Comment by @AlexWaygood on 2025-02-09 13:06_

@Daverball's correct that f-strings are invalid in annotation expressions, though. Since RUF027 is looking for strings that seem like they have f-string syntax but are missing the `f` prefix, a better fix here might be to just not run RUF027 at all on expressions that we know are meant to be annotation expressions, such as the first argument to `cast()` or the second argument to the `TypeAliasType` constructor. (These are almost never "actually meant to be f-strings".) I think we set a flag on the semantic model when we're visiting a type definition? We can probably just return early from RUF027 if we see that flag's been set.

---

_Comment by @Daverball on 2025-02-09 13:38_

> We can probably just return early from RUF027 if we see that flag's been set.

Yup, the flag you're looking for is `TYPE_DEFINITION` or the method `semantic.in_type_definition()`.

---

_Comment by @dylwil3 on 2025-02-09 16:10_

Makes sense to me, thanks @AlexWaygood  and @Daverball ! Closing in favor of #16054 

---

_Closed by @dylwil3 on 2025-02-09 16:10_

---
