```yaml
number: 15657
title: Consider a more data-oriented AST representation
type: issue
state: open
author: dcreager
labels:
  - core
  - help wanted
  - needs-design
assignees: []
created_at: 2025-01-21T22:30:44Z
updated_at: 2025-01-22T07:18:58Z
url: https://github.com/astral-sh/ruff/issues/15657
synced_at: 2026-01-12T15:54:54Z
```

# Consider a more data-oriented AST representation

---

_@dcreager_

We spend a noticeable amount of time (in both Ruff and Red Knot) parsing, and a noticeable subset of _that_ is in `malloc`.  Our [current AST representation](https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_ast/src/nodes.rs) is just a straight tree of nodes, which can put a lot of pressure on the memory allocator and isn't very cache-friendly when we go to consume the AST later.

<details>
<summary>Flame graph</summary>

![flamegraph](https://github.com/user-attachments/assets/9b7baecc-5df9-4a5d-8e10-45185905d454)

</details>

In https://github.com/astral-sh/ruff/pull/12419, @MichaReiser explored using `bumpalo`.  This would reduce `malloc` overhead, since all AST nodes would be allocated from the bump allocator.  But the data structure itself was still directly storing the tree structure.  In https://github.com/astral-sh/ruff/pull/12419#issuecomment-2262727929, Micha referenced [this blog post](https://ziglang.org/download/0.8.0/release-notes.html#Reworked-Memory-Layout) about Zig's data-oriented "struct of arrays" representation.  We should consider exploring a similar approach to see if we can achieve similar speedups and memory savings.

(This would be much easier with https://github.com/astral-sh/ruff/issues/15655, since we wouldn't have to copy/paste the data representation changes in each of the syntax nodes.  But this will likely require extensive updates everywhere we consume ASTs, since all of that code currently assumes it can use direct field access.  We might consider a separate first step that updates consumers to use accessor methods, which would just read the fields directly without changing the representation.)

---

_Label `internal` added by @dcreager on 2025-01-21 22:30_

---

_Label `help wanted` added by @dcreager on 2025-01-21 22:30_

---

_Comment by @dylwil3 on 2025-01-22 02:04_

Some nicely written, relevant blog posts:

1. [Adrian Sampson](https://www.cs.cornell.edu/~asampson/blog/flattening.html)
2. [Inanna Malick](https://recursion.wtf/posts/rust_schemes/)

Something like (2) might be easier to incrementally adopt. It could also help with the issue that we use the AST in two different ways: on the one hand, it is a sort of static object, the result of parsing, which lives for the duration of the program and is then dropped. On the other hand, we sometimes hand-write mini ASTs to feed into the `Generator` when writing fixes. If we adopted the generic approach of (2), maybe we could allow both uses on equal footing. 

Unfortunately I don't think (2) applies immediately- we can't just take `T=Box<Expr>` since sometimes we refer to children via `Vec` and other times via `Box` etc.

---

_Comment by @MichaReiser on 2025-01-22 07:18_

Some other considerations when working on this

* How expensive and ergonomic is it to mutate the AST? This becomes relevant if we want to move from text based fixes to AST generated fixes (which would help protect against the many incorrect parantheses/precedence bugs we've experienced and might allow for an automated way to preserve comments).
* Does the representation support ancestor/children/descendents traversal (ideally allocation free)
* Does the representation allow for node-reuse? E.g. reuse the same `self` identifier everywhere its used in the file

Other, worth considerin, alternatives are:

* Use a different representation after parsing, e.g. one that is also location-independent for better incrementality
* Use a red/green tree similar to Roslyn/Rust analyzer/swift



---

_Label `internal` removed by @MichaReiser on 2025-01-22 07:18_

---

_Label `core` added by @MichaReiser on 2025-01-22 07:18_

---

_Label `needs-design` added by @MichaReiser on 2025-01-22 07:18_

---
