```yaml
number: 14325
title: "[red-knot] Handle invalid assignment targets"
type: pull_request
state: merged
author: sharkdp
labels:
  - ty
assignees: []
merged: true
base: main
head: david/fix-invalid-assignment-target-panics
created_at: 2024-11-13T18:50:23Z
updated_at: 2024-11-13T19:50:42Z
url: https://github.com/astral-sh/ruff/pull/14325
synced_at: 2026-01-12T15:55:47Z
```

# [red-knot] Handle invalid assignment targets

---

_@sharkdp_

## Summary

This fixes several panics related to invalid assignment targets. All of these led to some a crash, previously:
```py
(x.y := 1)  # only name-expressions are valid targets of named expressions
([x, y] := [1, 2])  # same
(x, y): tuple[int, int] = (2, 3)  # tuples are not valid targets for annotated assignments
(x, y) += 2  # tuples are not valid targets for augmented assignments
```

closes #14321
closes #14322

## Test Plan

I symlinked four files from `crates/ruff_python_parser/resources/{invalid,inline/err}` into the red knot corpus, as they seemed like ideal test files for this exact scenario. I can also copy them, if we prefer that (these relative symlinks are probably annoying if we move folders around). I think eventually, it might be a good idea to simply include *all* invalid-syntax examples from the parser tests into red knots corpus (I believe we're actually not too far from that goal). Or expand the scope of the corpus test to this directory. Then we can get rid of these symlinks again.

---

_Label `red-knot` added by @sharkdp on 2024-11-13 18:50_

---

_Review requested from @carljm by @sharkdp on 2024-11-13 18:50_

---

_Review requested from @MichaReiser by @sharkdp on 2024-11-13 18:50_

---

_Review requested from @AlexWaygood by @sharkdp on 2024-11-13 18:50_

---

_@sharkdp reviewed on 2024-11-13 18:53_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:709 on 2024-11-13 18:53_

I tried to search for something like `Expr::is_valid_{aug_,ann_,}assign_target` methods but couldn't find anything. And all existing code that deals with assignments could not make use of these, as it typically needs to do something specific for each branch. So it's just an inline match for now.

---

_@AlexWaygood reviewed on 2024-11-13 18:57_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/resources/test/corpus/04_assign_invalid_targets.py`:1 on 2024-11-13 18:57_

is this the test file you wanted to add?

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:708 on 2024-11-13 19:04_

I don't think `Starred` expressions are legal in `AugAssign`s:

Python's parser permits them in `Assign`s:

```pycon
>>> import ast
>>> print(ast.dump(ast.parse('*y = z'), indent=2))
Module(
  body=[
    Assign(
      targets=[
        Starred(
          value=Name(id='y', ctx=Store()),
          ctx=Store())],
      value=Name(id='z', ctx=Load()))])
```

But not `AugAssign`s:

```pycon
>>> import ast
>>> print(ast.dump(ast.parse('*y += z'), indent=2))
Traceback (most recent call last):
  File "<python-input-4>", line 1, in <module>
    print(ast.dump(ast.parse('*y += z'), indent=2))
                   ~~~~~~~~~^^^^^^^^^^^
  File "/Users/alexw/.pyenv/versions/3.13.0/lib/python3.13/ast.py", line 54, in parse
    return compile(source, filename, mode, flags,
                   _feature_version=feature_version, optimize=optimize)
  File "<unknown>", line 1
    *y += z
    ^^
SyntaxError: 'starred' is an illegal expression for augmented assignment
```

---

_@AlexWaygood reviewed on 2024-11-13 19:04_

---

_Comment by @github-actions[bot] on 2024-11-13 19:04_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_@sharkdp reviewed on 2024-11-13 19:08_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/resources/test/corpus/04_assign_invalid_targets.py`:1 on 2024-11-13 19:08_

Yes. I wanted to make sure that invalid *normal* assignments (non-annotated, non-augmented) were also handled correctly. Or what is wrong?

---

_@sharkdp reviewed on 2024-11-13 19:11_

---

_Review comment by @sharkdp on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:708 on 2024-11-13 19:11_

Ah, thanks. This means the Python docs are a bit misleading here. For `AugAssign`, they state that the *"target attribute cannot be of class [Tuple](https://docs.python.org/3/library/ast.html#ast.Tuple) or [List](https://docs.python.org/3/library/ast.html#ast.List), unlike the targets of [Assign](https://docs.python.org/3/library/ast.html#ast.Assign)."* — which I interpreted as: everything allowed for a normal assignment, but not `Tuple` or `List`.

---

_@AlexWaygood reviewed on 2024-11-13 19:12_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/resources/test/corpus/04_assign_invalid_targets.py`:1 on 2024-11-13 19:12_

It looks from the GitHub diff as though this file contains the literal text

```
../../../../ruff_python_parser/resources/invalid/statements/invalid_assignment_targets.py
```

Am I missing something -- is it a symlink or something? 😄

---

_@sharkdp reviewed on 2024-11-13 19:14_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/resources/test/corpus/04_assign_invalid_targets.py`:1 on 2024-11-13 19:14_

Yes. See PR description :smile:.



---

_@sharkdp reviewed on 2024-11-13 19:14_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/resources/test/corpus/04_assign_invalid_targets.py`:1 on 2024-11-13 19:14_

I'm waiting for the CI failure on Windows.

---

_@sharkdp reviewed on 2024-11-13 19:15_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/resources/test/corpus/04_assign_invalid_targets.py`:1 on 2024-11-13 19:15_

But my links are a bit off. Give me a minute.

---

_@AlexWaygood reviewed on 2024-11-13 19:16_

---

_Review comment by @AlexWaygood on `crates/red_knot_python_semantic/src/semantic_index/builder.rs`:708 on 2024-11-13 19:16_

I agree the docs are bad here.

I think arguably the docs for the stdlib `ast` module say too _much_ here, though. They should instead say less, and link to the canonical docs for which expressions are allowed as augmented-assignment targets, which are at https://docs.python.org/3/reference/simple_stmts.html#augmented-assignment-statements (and which are much more precise)

---

_@AlexWaygood reviewed on 2024-11-13 19:17_

---

_Review comment by @AlexWaygood on `crates/red_knot_workspace/resources/test/corpus/04_assign_invalid_targets.py`:1 on 2024-11-13 19:17_

> Yes. See PR description 😄.

Ahh sorry!!

> But my links are a bit off. Give me a minute.

Yeah, I think GitHub displays correct symlinks differently 😄

---

_@sharkdp reviewed on 2024-11-13 19:19_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/resources/test/corpus/04_assign_invalid_targets.py`:1 on 2024-11-13 19:19_

> Yeah, I think GitHub displays correct symlinks differently 😄

No I believe the links are correct on a.. filesystem level. But there is a small semantic/naming mistake. This is how GitHub shows symlinks.

---

_@sharkdp reviewed on 2024-11-13 19:26_

---

_Review comment by @sharkdp on `crates/red_knot_workspace/resources/test/corpus/04_assign_invalid_targets.py`:1 on 2024-11-13 19:26_

Okay. Now it should be fine. The names are different from the source files, but hopefully match the naming pattern of red knot's corpus.

Windows also works.

---

_Comment by @carljm on 2024-11-13 19:31_

I would support just modifying the corpus test to run on all parser examples, whenever we are at a point where that doesn't panic. If we aren't at that point yet, symlinking certain files seems like a fine temporary solution (if it works cross-platform)

---

_@carljm approved on 2024-11-13 19:47_

Looks great, thank you!

---

_Merged by @sharkdp on 2024-11-13 19:50_

---

_Closed by @sharkdp on 2024-11-13 19:50_

---

_Branch deleted on 2024-11-13 19:50_

---
