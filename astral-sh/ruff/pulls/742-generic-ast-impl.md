```yaml
number: 742
title: Generic AST impl
type: pull_request
state: closed
author: Seamooo
labels: []
assignees: []
base: main
head: feature/generic-ast-impl
created_at: 2022-11-14T19:08:13Z
updated_at: 2023-01-06T15:50:01Z
url: https://github.com/astral-sh/ruff/pull/742
synced_at: 2026-01-12T15:55:05Z
```

# Generic AST impl

---

_@Seamooo_

This PR is intended for discussion with a supporting example

## Intent

The purpose of this PR is to provide the basis for making the AST generic. This is particularly useful for cases where a different parser, and different intermediate representation - read AST nodes - may be more appropriate (e.g. static parsing vs incremental parsing)

Included is the trait set for python 3.10 grammar (admittedly with an extra bound of `Located` for some traits), making heavy use of bounding associated types to ensure a correct interface. Additionally an implementation of these traits is included for the rustpython_parser. Integration of this has been shown in the helper function: `ruff::ast::helpers::compose_code_path`.

This shows incremental adoption is possible, with a final goal of the `check_ast` method being exposed and able to take a type implementing `ruff::ast::nodes::Ast`

## Discussion

A few questions that would be useful to discuss:
- Is this appropriate to keep inside ruff or should it be a seperate crate?
- Is there sufficient motivation to get this change in (ie potential pitfalls of introducing this vs use cases)?
- Would this introduce performance concerns?
- Are the current associated type bounds sufficient, or should all associated types be bound with inner associated types where possible (e.g. `type Alias: Alias<'a>` vs `type Alias: Alias<'a, Ident = Self::Ident>` 

---

_Marked ready for review by @Seamooo on 2022-11-15 03:03_

---

_Comment by @charliermarsh on 2022-11-15 21:46_

Wow this is really interesting (and very impressive). To repeat my understanding: you've developed a `RustPython`-agnostic AST interface based on traits, and then implemented that interface for the existing `RustPython` AST. This would allow us to write code that consumes the AST in a way that's agnostic to the underlying AST implementation. For example, we could implement the same traits atop tree-sitter, and plug the resulting AST into the same code downstream without changes. Is that correct?

> Is this appropriate to keep inside ruff or should it be a seperate crate?

Happy to keep this inside ruff.

> Is there sufficient motivation to get this change in (ie potential pitfalls of introducing this vs use cases)?

I think what this needs is a second implementation of the trait interface, or a concrete proposal for what that second interface would be. That would give us sufficient motivation since it'd anchor us in concrete use-cases. What would this interface unlock?

> Would this introduce performance concerns?

Only way to know is try! But if it's just function calls and not allocations... maybe it's fine? We'd have to do some benchmarking.


---

_Comment by @Seamooo on 2022-11-16 08:15_

> To repeat my understanding: you've developed a RustPython-agnostic AST interface based on traits, and then implemented that interface for the existing RustPython AST. This would allow us to write code that consumes the AST in a way that's agnostic to the underlying AST implementation. For example, we could implement the same traits atop tree-sitter, and plug the resulting AST into the same code downstream without changes. Is that correct?

That's exactly it.

> I think what this needs is a second implementation of the trait interface, or a concrete proposal for what that second interface would be. That would give us sufficient motivation since it'd anchor us in concrete use-cases. What would this interface unlock?

Over at `ruffd`, there's nothing currently public but I've been messing around with creating an incremental parser. Implementing these traits could be that second implementation.
Another candidate is LibCST. I'm unsure of how much use it's receiving in the codebase but if its parser is being utilised, this would be an opportunity to only require a single pass rather than multiple for a cst vs ast.

> Only way to know is try! But if it's just function calls and not allocations... maybe it's fine? We'd have to do some benchmarking.

I suspect the diff for the full implementation will be truly gargantuan, hence the incremental approach. Setting up a benchmark for a subset of rules would probably be the best first step. Are there any recommendations on what that subset should look like / if such a benchmark already exists?

Also, as a side note many of the function calls can be completely inlined so it should be exactly equivalent in those cases. Where there's potential overhead added, admittedly very small, is the use of `ruff::ast::nodes` enums both in the transform into these types and unwrapping them for the underlying value. Given these enums are sized as a pointer + discriminator, my expectation is the performance impact would be within margin of error.

---

_Comment by @Seamooo on 2022-11-18 06:09_

I've been implementing this on top of tree-sitter-python, as there was previously some work towards getting this to interface with ruff. The main design that will likely change from this work is replacing slice references with generic associated iterators (thank you 1.65), mainly due to similarly typed nodes being stored contiguously being a large assumption.

---

_Comment by @charliermarsh on 2022-11-18 14:56_

Sweet! The other thing I can do to push this forward is spend some time myself trying to port parts of Ruff to this new generic interface, which will help me build intuition and opinions on the questions above.

---

_Comment by @Seamooo on 2022-11-25 21:02_

There's been a bit of radio silence here but it's been for a good reason. There are essentially 2 unsolved problems remaining. one is the representation of underlying values (e.g. strings) and the second is integrating stricter lifetime requirements within ruff.

## Generic, lifetime-bound, associated types

The most recent changes have introduced a generic way of specifying return types, as there are cases, particularly when using the FFI, where it is not appropriate to return a ref as the underlying data cannot be borrowed. The reason for compilation failure is that ruff currently assumes everything in the AST to be recursively borrowable, with the same lifetime.
This assumption is excellent for performance, but cannot be relied upon when data needs to be owned when querying the FFI. Having said that, it is also true that data produced from using the FFI has its implicit lifetime for data owned underneath itself, it is therefore not possible (even with allocations) to return nodes underneath intermediary nodes as those intermediary nodes were borrowed to create these new nodes and have lifetime restrictions corresponding to that borrow.
This technically would be possible to get around with implementing an IntoAst trait with corresponding Ast nodes (ie &self becomes self), but this has currently not been explored.

## Underlying literal reprs

Various implementations have different ways of representing the underlying string literals. A particularly nasty case is that of tree-sitter where location is the only information about the literal for various string literal cst nodes. As such a type must be created to store both the Node and the string to which it refers to. Unfortunately this now presents a potential problem for the implementation. Thusfar ruff has assumed that the string literal is readily available as `&str`. Although `String` is always available potentially using a character iterator is a better approach. My main concern is the devex side of things as working with rust string primitives is far easier than playing around with character iterators, even if functionally they're behaving very similar for almost every op (that doesn't care about bytes).. There's potentially a nicer set of bounds that could be guaranteed forall parsers and enables equivalent functionality but I'm hestitant to just implement Iterator traits unless there's a use case.

---

_Comment by @charliermarsh on 2023-01-06 15:50_

Closing for now to keep the PR list up-to-date.

---

_Closed by @charliermarsh on 2023-01-06 15:50_

---
