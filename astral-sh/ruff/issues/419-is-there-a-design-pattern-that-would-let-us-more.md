```yaml
number: 419
title: "Is there a design pattern that would let us more closely couple `CheckKind` and `CheckCode`?"
type: issue
state: closed
author: charliermarsh
labels:
  - internal
assignees: []
created_at: 2022-10-13T13:31:02Z
updated_at: 2023-01-07T20:53:29Z
url: https://github.com/astral-sh/ruff/issues/419
synced_at: 2026-01-12T15:54:40Z
```

# Is there a design pattern that would let us more closely couple `CheckKind` and `CheckCode`?

---

_@charliermarsh_

Right now, we have matches in each implementation that create bidirectional links between `CheckKind` and `CheckCode`. But there's nothing enforcing this behavior, and it's really tedious to implement.

I'd love something like (made-up syntax):

```rust
CheckKind::UnusedImport(CheckCode::F401, ...)
```

...that keeps them coupled with a single definition. What's possible here?


---

_Label `internal` added by @charliermarsh on 2022-10-13 13:31_

---

_Comment by @charliermarsh on 2022-10-27 17:09_

We could have a `Check` trait and treat each error as its own struct.

---

_Comment by @squiddy on 2022-12-06 20:01_

Those bidirectional links I also struggled with (or rather, forgot to update certain parts) when implementing my first check. I've been looking at rust & clippy lately, and there are some interesting ideas. Some of which also related to that other ticket about updating `Checker`.

Short summary how this roughly works with clippy, taking the `REDUNDANT_FIELD_NAMES` lint. Declaring a lint:

```rust
declare_clippy_lint! {
    /// ### What it does
    /// Checks for fields in struct literals where shorthands
    /// could be used.
    pub REDUNDANT_FIELD_NAMES,
    style,
    "checks for fields in struct literals where shorthands could be used"
}
```

Through some macros from rust this eventually expands into something like this:

```rust
pub static REDUNDANT_FIELD_NAMES: &crate::Lint = &crate::Lint {
    name: "clippy::REDUNDANT_FIELD_NAMES",
    desc: "...snip...",
    ...snip...
};

pub(crate) static REDUNDANT_FIELD_NAMES_INFO: &'static crate::LintInfo = &crate::LintInfo {
    lint: &REDUNDANT_FIELD_NAMES,
    category: crate::LintCategory::Style,
    explanation: "... snip ...",
};
```

---

Now this in itself I find valuable already, tying together everything related to a lint (or check/checkkind). We could have:

```rust
declare_check! {
    /// What does this do? Why is this bad?
    MODULE_IMPORT_NOT_AT_TOP_OF_FILE,
    E402,
    Pycodestyle,
    "Module level import not at top of file"
}
```

Somewhere else, a global list of available checks:
```rust
pub static CHECKS: &'static [&Check] = &[MODULE_IMPORT_NOT_AT_TOP_OF_FILE, ...];
```

Using such a check (just making this up):

```rust
if self.settings.enabled.contains(&MODULE_IMPORT_NOT_AT_TOP_OF_FILE) {
   if self.seen_import_boundary && stmt.location.column() == 0 {
       self.add_check(Check::new(
           MODULE_IMPORT_NOT_AT_TOP_OF_FILE,
           Range::from_located(stmt),
       ));
   }
}
```

Making this check like this would also lower the significance of check code in ruff's code, which, if I understood that other ticket correctly, is something you see ruff moving to.

---

I'm happy to look further into this and come up with some proof-of-concept.

---

_Comment by @squiddy on 2022-12-06 20:12_

Clippy & rust have traits for "passes". `EarlyLintPass` for AST information, `LateLintPass` after type checking etc. `REDUNDANT_FIELD_NAMES` would use those like this (excerpt):

```rust
impl EarlyLintPass for RedundantFieldNames {
    fn check_expr(&mut self, cx: &EarlyContext<'_>, expr: &Expr) {
        if let ExprKind::Struct(ref se) = expr.kind {
            for field in &se.fields {
                if field.is_shorthand {
                    continue;
                }
                if let ExprKind::Path(None, path) = &field.expr.kind {
                    if path.segments.len() == 1
                        && path.segments[0].ident == field.ident
                        && path.segments[0].args.is_none()
                    {
                        span_lint_and_sugg(
                            cx,
                            REDUNDANT_FIELD_NAMES,
                            field.span,
                            "redundant field names in struct initialization",
                            "replace it with",
                            field.ident.to_string(),
                            Applicability::MachineApplicable,
                        );
                    }
                }
            }
        }
    }
}
```
Then there is a `LintStore` where all these passes are registered explicitely, much of which is automated by a script however.

---

_Comment by @charliermarsh on 2022-12-06 21:13_

Yeah I'd definitely be interested in a proposal or draft PR here, and I think something based on macros would make sense. My main hesitation is that the current system based on enums has really strong type-safety (apart from the non-enforced coupling between "code" and "kind") and plays well with autocomplete, and macros tend to make code harder to work with in the editor since it requires compile-time expansion (making a fuzzy claim here but hopefully you get what I mean).


---

_Comment by @squiddy on 2022-12-06 22:31_

Oh yeah, I get it. Initially I also wouldn't use macros but just type it out first. Syntactic sugar/shortcuts can come later. I think in the case of clippy the usage of macros also comes from it using existing infrastructure (based on macros) from the rust compiler.

---

_Comment by @squiddy on 2022-12-18 06:31_

```rust
pub struct CheckDef {
    pub name: &'static str,
    pub description: &'static str,
    pub code: CheckCode,
    pub category: CheckCategory,
    pub lint_source: LintSource,
}

pub struct Check {
    pub definition: &'static CheckDef,
    pub location: Location,
    pub end_location: Location,
    pub fix: Option<Fix>,
    pub message: String,
}

// Defining new checks (these would live in the pyflakes/pycodestyle modules).

pub static RAISE_NOT_IMPLEMENTED: &CheckDef = &CheckDef {
    name: "raise_not_implemented",
    description: "`raise NotImplemented` should be `raise NotImplementedError`",
    code: CheckCode::F901,
    category: CheckCategory::Pyflakes,
    lint_source: LintSource::AST,
};

pub static AMBIGUOUS_CLASS_NAME: &CheckDef = &CheckDef {
    name: "ambiguous_class_name",
    description: "Ambigious class name: ...",
    code: CheckCode::E742,
    category: CheckCategory::Pycodestyle,
    lint_source: LintSource::AST,
};

// Implementing new checks

pub fn ambiguous_class_name(checker: &mut Checker, name: &str, location: Range) {
    if is_ambiguous_name(name) {
        checker.add_check(Check::new(
            AMBIGUOUS_CLASS_NAME,
            format!("Ambiguous class name: `{name}`"),
            location,
        ));
    }
}

pub fn raise_not_implemented(checker: &mut Checker, expr: &Expr) {
    if let Some(expr) = match_not_implemented(expr) {
        let mut check = Check::new(
            RAISE_NOT_IMPLEMENTED,
            "`raise NotImplemented` should be `raise NotImplementedError`".to_string(),
            Range::from_located(expr),
        );
        if checker.patch(RAISE_NOT_IMPLEMENTED) {
            check.amend(Fix::replacement(
                "NotImplementedError".to_string(),
                expr.location,
                expr.end_location.unwrap(),
            ));
        }
        checker.add_check(check);
    }
}

// Check ast

if self.settings.enabled.contains(RAISE_NOT_IMPLEMENTED) {
    if let Some(expr) = exc {
        pyflakes::plugins::raise_not_implemented(self, expr);
    }
}
```

This doesn't give the same type safety guarantees as the current code though, e.g. multiple checks could map to the same error code, could reuse the same name etc. Haven't yet figured out that part.

---

_Comment by @charliermarsh on 2023-01-07 20:53_

Greatly improved by #1685!

---

_Closed by @charliermarsh on 2023-01-07 20:53_

---
