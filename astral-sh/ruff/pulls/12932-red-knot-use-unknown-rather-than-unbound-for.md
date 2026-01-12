```yaml
number: 12932
title: "[red-knot] Use `Unknown` rather than `Unbound` for unresolved imports"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - ty
assignees: []
merged: true
base: main
head: alex/unresolved-imports
created_at: 2024-08-16T14:12:31Z
updated_at: 2024-08-16T19:10:35Z
url: https://github.com/astral-sh/ruff/pull/12932
synced_at: 2026-01-12T15:55:42Z
```

# [red-knot] Use `Unknown` rather than `Unbound` for unresolved imports

---

_@AlexWaygood_

## Summary

If an import statement succeeds, it always binds a symbol. Symbols introduced by import statements can always be used successfully by other statements and expressions; you'll never get a `NameError` from trying to use a symbol introduced by an import statement. As such, it doesn't make sense to think of unresolved imports as "unbound": an unbound symbol causes a runtime `NameError` when you try to use it elsewhere, but here the runtime error (providing our type analysis is correct) will occur at the import statement itself, not when the symbol introduced by the runtime statement is used elsewhere.

This PR therefore updates red-knot to infer `Unknown` rather than `Unbound` in these cases.

## Test Plan

`cargo test`


---

_Label `red-knot` added by @AlexWaygood on 2024-08-16 14:12_

---

_Review requested from @carljm by @AlexWaygood on 2024-08-16 14:12_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-08-16 14:12_

---

_Comment by @github-actions[bot] on 2024-08-16 14:27_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review request for @MichaReiser removed by @MichaReiser on 2024-08-16 14:27_

---

_Comment by @MichaReiser on 2024-08-16 14:28_

Makes sense to me but I rather have carl review it ;)

---

_@carljm approved on 2024-08-16 17:34_

Yeah, I think this is correct, thanks for the fix!

The awkwardness is that it makes the "unresolved imports" lint rule slightly less clearly correct, because is it now possible that type Unknown on an import doesn't mean unresolved import, but rather successful import of a value of type Unknown in the module we imported from? One way to clarify this would be to clarify our use of Unknown to say that it _always_ originates from a type error (never from just lack-of-annotation), which is currently true. In this case we could reasonably say that Unknown type on an import always means the import failed to resolve. In any case I don't think this is a critical issue for this PR, since we're focusing on building a type checker right now, not a type-aware linter. So as long as we issue the right diagnostic at the moment of failing to resolve an import, it's not currently critical that we can later revisit and draw the same conclusion just from the type. If it later becomes critical, it may be an option to literally look at the presence or absence of the diagnostic, or in some other way store more metadata.

Relatedly, I think if we want to be consistent about this, then in `infer_import_from_definition`, where we call `.member` on the module type, we should call `.replace_unbound_with(Type::Unknown)` on the resulting type. Because importing an unbound name from an actually-existing module is also just another kind of unresolved import, and should also result in `Type::Unknown` (not `Type::Unbound`) in the importing module. I think it would make sense to add that (and a test for it) in this PR; what do you think?

---

_Comment by @AlexWaygood on 2024-08-16 17:49_

> The awkwardness is that it makes the "unresolved imports" lint rule slightly less clearly correct, because is it now possible that type Unknown on an import doesn't mean unresolved import, but rather successful import of a value of type Unknown in the module we imported from?

Yup. And I _assume_ this is the reason why the number of diagnostics in the benchmark is going up slightly? I want to check before merging.

> or in some other way store more metadata.

Yeah... I think I've mentioned to you before that mypy tracks quite a few different subkinds of `Any` so that it knows whether it's a result of an error, something being untyped, an explicit `Any` annotation, or something else: <https://github.com/python/mypy/blob/fe15ee69b9225f808f8ed735671b73c31ae1bed8/mypy/types.py#L187-L215>. We could consider doing something similar.

> Relatedly, I think if we want to be consistent about this, then in `infer_import_from_definition`, where we call `.member` on the module type, we should call `.replace_unbound_with(Type::Unknown)` on the resulting type. Because importing an unbound name from an actually-existing module is also just another kind of unresolved import, and should also result in `Type::Unknown` (not `Type::Unbound`) in the importing module. I think it would make sense to add that (and a test for it) in this PR; what do you think?

Yes, that makes sense to me!

---

_Comment by @AlexWaygood on 2024-08-16 18:27_

So on `main` the diagnostics are:

<details>

```
[crates/ruff_benchmark/benches/red_knot.rs:91:17] &result = List(
    [
        "Line 69 is too long (89 characters)",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "/src/tomllib/_parser.py:7:29: Unresolved import 'Iterable'",
        "/src/tomllib/_parser.py:153:22: Name 'key' used when not defined.",
        "/src/tomllib/_parser.py:153:27: Name 'flag' used when not defined.",
        "/src/tomllib/_parser.py:159:16: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:161:25: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:168:16: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:169:22: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:170:25: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:180:16: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:182:31: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:206:16: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:207:22: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:208:25: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:232:43: Name 'Iterable' used when not defined.",
        "/src/tomllib/_parser.py:330:32: Name 'header' used when not defined.",
        "/src/tomllib/_parser.py:330:41: Name 'key' used when not defined.",
        "/src/tomllib/_parser.py:333:26: Name 'cont_key' used when not defined.",
        "/src/tomllib/_parser.py:334:71: Name 'cont_key' used when not defined.",
        "/src/tomllib/_parser.py:337:31: Name 'cont_key' used when not defined.",
        "/src/tomllib/_parser.py:628:75: Name 'e' used when not defined.",
        "/src/tomllib/_parser.py:686:23: Name 'parse_float' used when not defined.",
    ],
)
```
</details>

And with this PR the diagnostics are:

<details>

```
[crates/ruff_benchmark/benches/red_knot.rs:107:17] &result = List(
    [
        "Line 69 is too long (89 characters)",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "Use double quotes for strings",
        "/src/tomllib/_parser.py:7:29: Unresolved import 'Iterable'",
        "/src/tomllib/_parser.py:10:20: Unresolved import 'Any'",
        "/src/tomllib/_parser.py:13:5: Unresolved import 'RE_DATETIME'",
        "/src/tomllib/_parser.py:14:5: Unresolved import 'RE_LOCALTIME'",
        "/src/tomllib/_parser.py:15:5: Unresolved import 'RE_NUMBER'",
        "/src/tomllib/_parser.py:20:21: Unresolved import 'Key'",
        "/src/tomllib/_parser.py:20:26: Unresolved import 'ParseFloat'",
        "/src/tomllib/_parser.py:153:22: Name 'key' used when not defined.",
        "/src/tomllib/_parser.py:153:27: Name 'flag' used when not defined.",
        "/src/tomllib/_parser.py:159:16: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:161:25: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:168:16: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:169:22: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:170:25: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:180:16: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:182:31: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:206:16: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:207:22: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:208:25: Name 'k' used when not defined.",
        "/src/tomllib/_parser.py:330:32: Name 'header' used when not defined.",
        "/src/tomllib/_parser.py:330:41: Name 'key' used when not defined.",
        "/src/tomllib/_parser.py:333:26: Name 'cont_key' used when not defined.",
        "/src/tomllib/_parser.py:334:71: Name 'cont_key' used when not defined.",
        "/src/tomllib/_parser.py:337:31: Name 'cont_key' used when not defined.",
        "/src/tomllib/_parser.py:628:75: Name 'e' used when not defined.",
        "/src/tomllib/_parser.py:686:23: Name 'parse_float' used when not defined.",
    ],
)
```
</details>

So, as expected, this PR means that the "unresolved import" lint becomes less accurate: existing symbols imported from other modules that are `Type::Unknown` in other modules are now misunderstood by that lint as "unresolved imports"; this causes six new errors from that lint. But the "name used when not defined" lint becomes more accurate: this error (which wasn't correct) goes away:

```
        "/src/tomllib/_parser.py:232:43: Name 'Iterable' used when not defined.",
```

(`Iterable` _was_ defined -- via an `import` statement -- it was just that red-knot was failing to follow that import to the definition.)

This looks okay to me (we'll just need to add more metadata in the future to track exactly what the cause of the `Unknown` was), so for now I'll add a TODO noting the inaccuracy of the import lint and adjust the assertion in the benchmark code.

---

_Review requested from @carljm by @AlexWaygood on 2024-08-16 18:33_

---

_@carljm approved on 2024-08-16 19:09_

Looks great, thank you!!

---

_Merged by @AlexWaygood on 2024-08-16 19:10_

---

_Closed by @AlexWaygood on 2024-08-16 19:10_

---

_Branch deleted on 2024-08-16 19:10_

---
