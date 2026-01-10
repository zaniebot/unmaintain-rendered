```yaml
number: 660
title: Handle conflicting autofix instructions
type: issue
state: closed
author: charliermarsh
labels: []
assignees: []
created_at: 2022-11-08T14:43:30Z
updated_at: 2022-11-22T21:38:19Z
url: https://github.com/astral-sh/ruff/issues/660
synced_at: 2026-01-10T12:09:58Z
```

# Handle conflicting autofix instructions

---

_Issue opened by @charliermarsh on 2022-11-08 14:43_

If we have a line like:

```py
x: Union[int, Option[str]]
```

Then if the relevant rules and Python version permit it, we'd like to autofix that line to:

```py
x: int | str | None
```

However, as-is, that requires two overlapping fixes, which we don't support. So Ruff will fix the outer rule (to `x: int | Option[str]`), then fix the inner rule if you re-run it (`x: int | str | None`).

We need to implement an autofix strategy that's amenable to multiple "concurrent" fixes.

This will also apply to import sorting: we want to be able to remove unused imports _and_ re-sort imports in a single pass.


---

_Label `enhancement` added by @charliermarsh on 2022-11-08 14:43_

---

_Comment by @charliermarsh on 2022-11-08 14:47_

I don't know how to implement this yet. I need to do some research. But a few ideas come to mind:

1. Structure it as a CRDT-like concurrent editing problem. Use CRDTs or similar to enable us to reconcile these kinds of concurrent edits.
2. Re-write rules to batch these changes. For example, above, maybe we just surface a _single_ error with the autofix for the entire expression, rather than emitting an error for `Union` and an error for `Option`. This is pretty limiting, as it wouldn't work for (e.g.) the unused import + re-order import case.
3. Make the fix API more AST-aware? Enable some kind of composition semantics, such that if we know we're replacing a list of Statements with a list of Statements, then we should allow any edits of the form Statement -> Statement.


---

_Renamed from "Implement conflicting autofixes" to "Handle conflicting autofix instructions" by @charliermarsh on 2022-11-08 20:01_

---

_Comment by @ljodal on 2022-11-08 21:53_

Have you worked with LibCST? I like how that handles modification to the ast/cst. Basically you implement a visitor that takes two nodes as input, `node` and `updated_node`, and returns a node. The `node` parameter is the parsed node from source and the `updated_node` parameter is the output from the previous visitor. So depending on the rule you can decide to consider previous modifications or not completely ignore them.

I also think that would work fine for the example above here. You could have two visitors; one that does `Optional[X]` -> `X | None` and a one that does `Union[A, B]` to `A | B`. It wouldn’t really matter what order these are run in, the output should always be the same. Either `Union[A, Optional[B]]` -> `A | Optional[B]` -> `A | B | None` or `Union[A, Optional[B]` -> `Union[A, B | None]` -> `A | B | None`

This would probably not fix the remove unused import issue, though that could maybe be considered a separate issue? If I’m not mistaken libcst handles that through a special hook where you can mark imports as unused/potentially unused after a modification has been made. I assume those imports are then removed in a second pass, which could run before the sorting runs

---

_Comment by @charliermarsh on 2022-11-08 22:09_

Yeah! We use LibCST internally to power some (not all) of our autofix operations. However, I think that LibCST's visitor API is entirely defined on the Python side, so I haven't used it before (we just use the CST parser and the ability to generate code from a modified CST struct)...

I do like what you're describing here. The main challenge, as-is, is that we don't actually create a CST for every module -- we create them eagerly, since CST generation is quite expensive. When we want to autofix something via LibCST, we extract the source code (using the delimiters defined by the AST), pass it to LibCST, generate the modified source, and then treat that source-to-source replacement as the "fix".

In other words, as-implemented, the individual fix operations have access to a CST, but there's no concept of passing the modified code or CST from check to check. (That's not to say we can't use this approach; rather, that it would take work.)

I do think we probably need to do something AST-aware (or CST-aware) in order to solve this problem.


---

_Comment by @charliermarsh on 2022-11-08 22:12_

In the `Union[A, Optional[B]]`, another way to look at it would be as a series of sequential edits.

So, to get rid of the `Union`:

- "Remove `Union[`
- "Retain `A`"
- "Remove `,`"
- "Insert ` |`"
- "Retain ` Option[B]`"
- "Remove `]`"

Then, to get rid of the `Option`, you do a similar thing.

If fixes were defined as those kinds of atomic operations, I believe you could reconcile them using CRDT-like operations. But it may not apply as cleanly to over kinds of collisions.


---

_Comment by @ljodal on 2022-11-08 22:24_

Ah, then I'd misunderstood how ruff works. I naively assumed it built up a cst of the source code and then traversed that. As a developer that's at least an interface that is great to work with and one benefit is that you can share a lot of code between linting and fixing.

The visitor pattern in libcst is indeed implemented in Python. It's documented [here](https://libcst.readthedocs.io/en/latest/tutorial.html#Build-Visitor-or-Transformer) and basically mirrors the visitors from the `ast` library with extensions for modifying the tree.

---

_Comment by @charliermarsh on 2022-11-09 00:08_

Yeah, I did a bunch of exploration around using the CST everywhere, but it was ~10x slower which brought execution time into the multiple seconds range.


---

_Comment by @charliermarsh on 2022-11-09 00:09_

On CRDTs -- what I'm trying to describe is something like this:

```rust
use operational_transform::OperationSeq;

pub fn main() {
    let s = "Union[int, Option[str]]";
    let mut a = OperationSeq::default();
    a.delete("Union[".len() as u64);
    a.retain("int".len() as u64);
    a.delete(",".len() as u64);
    a.insert(" |");
    a.retain(" Option[str]".len() as u64);
    a.delete("]".len() as u64);

    let mut b = OperationSeq::default();
    b.retain("Union[int, ".len() as u64);
    b.delete("Option[".len() as u64);
    b.retain("str".len() as u64);
    b.insert(" | None");
    b.delete("]".len() as u64);
    b.retain("]".len() as u64);

    let (_, b_prime) = a.transform(&b).unwrap();
    let ab_prime = a.compose(&b_prime).unwrap();
    let s = ab_prime.apply(s).unwrap();
    println!("{}", s); // int | str | None
}
```

I'm not convinced that this is a good idea since we could easily start producing invalid Python.


---

_Comment by @charliermarsh on 2022-11-09 01:49_

The way that `pyupgrade` handles this is somewhat interesting. They have a roundtrip tokenizer, and they implement all fixes as lazy transformations on the token stream. I don't see how that's completely safe, but it does work for the cases they implement.

---

_Comment by @ljodal on 2022-11-09 07:48_

Hmm, does it have to be lazy?

My (new) understanding is that ruff builds up an ast of the file being checked, if that's correct it would be great to be able to build both checkers and fixers on top of the ast.

If we can keep both the tokens and the ast in memory and link them, then it should based on my naive understanding be possible to use the ast to detect where changes are needed, get the tokens for that ast and update them, and then based on the updated tokens update the ast representation again.

I'm a bit rusty when it comes to compiler theory, but I assume the ast is built up from the tokens, so we should have everything available. The hard part here would probably be to deal with certain things that don't map direly to ast nodes like white space, commas etc

---

_Comment by @andersk on 2022-11-20 08:02_

Maybe we just want to apply a non-conflicting subset of the fixes, then repeatedly rerun the entire linter on the file until it converges?

I doubt we’re ever going to be able to guarantee that enough operations commute to make any other strategy work correctly.

---

_Comment by @charliermarsh on 2022-11-20 15:07_

Yeah, I think that's very likely what we'll have to do.


---

_Comment by @charliermarsh on 2022-11-22 21:38_

Should be fixed by https://github.com/charliermarsh/ruff/pull/875.

---

_Closed by @charliermarsh on 2022-11-22 21:38_

---
