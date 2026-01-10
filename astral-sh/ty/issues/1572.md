```yaml
number: 1572
title: Typed AST and/or Rust library?
type: issue
state: closed
author: samuelcolvin
labels:
  - question
assignees: []
created_at: 2025-11-15T19:58:01Z
updated_at: 2025-11-24T08:18:36Z
url: https://github.com/astral-sh/ty/issues/1572
synced_at: 2026-01-10T01:58:59Z
```

# Typed AST and/or Rust library?

---

_Issue opened by @samuelcolvin on 2025-11-15 19:58_

(Apologies if this question has already been answered, I searched but couldn't find it.)

Is there a way to get a typed AST from `ty`? Ideally from a Rust API using ty as a library.

Similarly, is there any plan to release ty as a crate? Or any examples of people using that way today?

---

_Label `question` added by @sharkdp on 2025-11-15 20:52_

---

_Comment by @sharkdp on 2025-11-15 21:03_

> Is there a way to get a typed AST from `ty`? Ideally from a Rust API using ty as a library.

There is currently no way to get a typed AST, no. ty does not build a typed AST internally, so this would need to be built as a new public interface.

> Similarly, is there any plan to release ty as a crate? Or any examples of people using that way today?

There is currently no plan to release any of the crates that ty uses, and I'm not aware of any projects that use ty's crates, except for the [parser](https://github.com/astral-sh/ruff/tree/main/crates/ruff_python_parser) and the [untyped AST](https://github.com/astral-sh/ruff/tree/main/crates/ruff_python_ast) which ty shares with ruff (see e.g. here how to use them as a [Git dependency](https://github.com/facebook/pyrefly/blob/03131fd1c7fea035d07a219d00c26ac793efd27b/pyrefly/Cargo.toml#L46-L47)).

---

_Comment by @carljm on 2025-11-15 21:44_

> ty does not build a typed AST internally

We do have an API (`SemanticModel`) for querying a type for any given AST node, which is sort of the same thing.

But yeah, none of this is supported for external use, or planned for release as a crate.

---

_Comment by @samuelcolvin on 2025-11-16 04:05_

Thanks both, this is very helpful. I'm fine with git dependencies.

If I wanted to use ty as a Rust library but get type errors back as text (ultimately to send to an LLM), any idea how hard that would be, or if there's any prior art?



---

_Comment by @MichaReiser on 2025-11-16 07:39_

To check an entire file, you can call `check_types`:

https://github.com/astral-sh/ruff/blob/6e4b15b6ca72285a2f509076f3871adca90e923a/crates/ty_python_semantic/src/types/infer/tests.rs#L50-L56

This is how you render diagnostics

https://github.com/astral-sh/ruff/blob/6e4b15b6ca72285a2f509076f3871adca90e923a/crates/ty/src/lib.rs#L345-L351

Note that none of these APIs are intended for external use and may change or be removed at any time.

---

_Closed by @MichaReiser on 2025-11-24 08:18_

---
