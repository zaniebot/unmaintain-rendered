```yaml
number: 11267
title: "Refactor the `ExprDict` node"
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: refactor-dict-node
created_at: 2024-05-03T17:44:03Z
updated_at: 2024-07-01T10:29:46Z
url: https://github.com/astral-sh/ruff/pull/11267
synced_at: 2026-01-10T21:55:59Z
```

# Refactor the `ExprDict` node

---

_Pull request opened by @AlexWaygood on 2024-05-03 17:44_

## Summary

This PR refactors the `ExprDict` node in `crates/ruff_python_ast/nodes.rs` from this:

```rs
struct ExprDict {
    keys: Vec<Option<Expr>>,
    values: Vec<Expr>,
}
```

to this:

```rs
struct DictItem {
    key: Option<Expr>,
    value: Expr,
}

struct ExprDict {
    items: Vec<DictItem>,
}
```

This has several advantages:
- In most cases where we're iterating over the elts for an `ExprDict` node, we `zip` the keys and values together to iterate over them both simultaneously. With the new representation of the AST structure, using `zip` is no longer necessary; the key and value are naturally paired together, and iteration over the two together can be done in a much more elegant way.
- All of our rules that _do_ currently iterate over the keys and values zipped together implicitly rely on the invariant that the `keys` and `values` vectors on an `ExprDict` node will be of exactly the same length. This refactoring encodes this invariant into the type itself, making it explicit.
- The size of an `ExprDict` node is reduced from 56 to 32 bytes

In addition to the above refactoring, I added two convenience methods to `ExprDict` so that we can still easily iterate over and index into the keys and values of a dictionary: `keys()` and `values()`. Both return read-only "view" objects that allow you to iterate over -- and also index directly into -- an `ExprDict` node's keys or values. Some of these implementations might be _slightly_ over the top right now. Let me know what you think!

## Test Plan

`cargo test` / `cargo insta review`


---

_Label `internal` added by @AlexWaygood on 2024-05-03 17:44_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-05-03 17:44_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-05-03 17:44_

---

_@AlexWaygood reviewed on 2024-05-03 17:47_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/pyflakes/rules/repeated_keys.rs`:198 on 2024-05-03 17:47_

I wanted to do this here:

```suggestion
                    let comparable_value: ComparableExpr = (&dict.values()[i]).into();
```

But the borrow checker wouldn't let me

---

_Comment by @github-actions[bot] on 2024-05-03 18:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/parser/recovery.rs`:54 on 2024-05-06 06:50_

We should probably change patterns in the same way to keep the AST consistent (doesn't have to be part of this PR)

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor.rs`:399 on 2024-05-06 06:53_

The semantic here is now different, but I don't know if the old iteration order was intentional. We'll have to check the python semantic to know if the evaluation order is `keys` followed by `values` or if it is for each pair, `key` then `value`. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:810 on 2024-05-06 06:54_

Nit: I think `DictKeys` would be more idiomatic (e.g. the iterator for `HashMap` is just `Keys`). It's an iterator over keys. 

Oh, I just now see that this isn't the iterator. 

I wonder if this extra struct is necessary. All methods can be implemented directly on iterator

* `first`/`last` -> Are iterator functions
* `len`/`is_empty` -> Are `ExactSizeIter` functions
* `get` -> `nth`

The only one that isn't is the implementation of `Index`. I wonder if we should just add `key(index)` and `value(index)` methods to `ExprDict` instead. But that probably heavily depends on where the `Index` methods are used.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/visitor/transformer.rs`:380 on 2024-05-06 07:01_

Same as for the `visitor`. The new implementation visits the items in a different ordering. We need to verify if this new order matches Python's evaluation order.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:177 on 2024-05-06 07:03_

Reading the code here makes me wonder if the `keys` and `values` method should be implemented on `DictItems` (a zero cost wrapper around `Vec<DictItem>`) instead of on the `ExprDict`. It's a bit awkward that it is now necessary to hold on to the `dict` and `items`. I think the alternative is to expose an `items()` method on `dict` that returns the items slice. That way, the code here can be written entirely without destructuring `dict`

* `range` -> `dict.range()`
* `items` -> `dict.items()`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pylint/rules/repeated_keyword_argument.rs`:61 on 2024-05-06 07:05_




```suggestion
            for key in dict.keys().iter().flatten() {
```

We should expose an `iter` function if it doesn't already exist (It's most common to have `iter` and `into_iter` but rare to onlyh have `into_iter`)

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/checkers/ast/mod.rs`:1204 on 2024-05-06 07:12_

Same as for the visitor. The new implementatio changes the visitation order from keys, then values to: for each pair, `key` then `value`. Does this match pythons evaluation order?

---

_@MichaReiser reviewed on 2024-05-06 07:15_

There are a few places where you changed the visitation order but where it is important that the visitation order matches the runtime evaluation order. We should double check which order is correct. 

I would consider:

* Implementing `items()` as a method on `ExprDict` (returns a `&[DictItem]`). This way, all relevant methods are directly available on the `expression` and it isn't required to mix and match `items`, with `expr.keys()`, expr.values()`
* I would consider changing `keys()` and `values()` to directly return iterators instead of `View`s and replace `Index` with `key(index)` and `value(index)` (although we may want to return `Options` and have `key_unwrap` and `value_unwrap`

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pyflakes/rules/repeated_keys.rs`:198 on 2024-05-06 07:19_

I think the problem here is that `Index::index` returns an `Expr` a lifetime of `DictKeysView` (It doesn't return a `&'a Self::Output`). We could work around this by having a `dict.value(i)` method instead because that method could return a `&'a Expr`

---

_@MichaReiser reviewed on 2024-05-06 07:19_

---

_Comment by @MichaReiser on 2024-05-06 07:21_

I really like the change. Having both key and value in a single item makes the API way more convenient to use. Thanks for doing this refactor.

---

_@AlexWaygood reviewed on 2024-05-06 07:25_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:810 on 2024-05-06 07:25_

Yep, the intermediate "view" object only gets us indexing on top of all the iterator functionality. Having `.keys()` return a view object felt appealing as it's very similar to what Python does at runtime with the `.keys()`, `.values()` and `.items()` methods on dictionaries (although the runtime `dict_keys`, `dict_values` and `dict_items` view objects aren't actually indexable in Python — they're closer to set-like containers than sequences). But I also wasn't sure whether the extra complexity of the intermediate struct was warranted here; I'm happy to get rid of them

---

_@AlexWaygood reviewed on 2024-05-06 07:30_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/visitor.rs`:399 on 2024-05-06 07:30_

I think Python evaluates dictionary literals pair-by-pair (i.e., according to the change I'm proposing here):

```pycon
>>> {a:b}
Traceback (most recent call last):
  File "<string>", line 1, in <module>
NameError: name 'a' is not defined
>>> {'a':b, c:'d'}
Traceback (most recent call last):
  File "<string>", line 1, in <module>
NameError: name 'b' is not defined
```

If it evaluated all the keys before any of the values, the second error would be "'c' is not defined" rather than "'b' is not defined". I'll see if I can verify this more

---

_Review comment by @dhruvmanila on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:302 on 2024-05-06 08:26_

I'm assuming the need to change the type of `value_elts` is because of the indirection of `DictKeyView`? I wonder if we could just use an iterator approach along with [`as_slice`](https://doc.rust-lang.org/stable/std/slice/struct.Iter.html#method.as_slice) method.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/comparable.rs`:692 on 2024-05-06 08:27_

nit: I think it could be useful to have `ComparableDictItem` but it's not strictly required, so I'm fine with this option as well

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:810 on 2024-05-06 08:29_

nit: I'd just keep the iterator and remove the intermediate struct. I like the `key(index)` approach Micha suggested. Although, I leave it up to you as you will know more about how the dictionary expression is being utilized in downstream tools.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:797 on 2024-05-06 08:33_

I wonder if you thought about an alternative design which would utilize an enum like:
```rs
pub enum DictItem {
	KeyValue(KeyValueItem),
	DoubleStarred(Expr), // or Expanded, Spread, ...
}
```

Although this approach does add in an additional indirection.

That said, I'd keep the `DictItem` approach for now.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/visitor.rs`:399 on 2024-05-06 08:36_

There's a small mention of the evaluation order in the docs although it's still unclear to me:

> If a comma-separated sequence of dict items is given, they are evaluated from left to right to define the entries of the dictionary [...]
>
> Reference: https://docs.python.org/3/reference/expressions.html#dictionary-displays

And, the following might mean that the order as in this PR is correct:
> This means that you can specify the same key multiple times in the dict item list, and the final dictionary’s value for that key will be the last one given.

---

_@dhruvmanila reviewed on 2024-05-06 08:39_

Thank you for doing this refactor!

---

_@AlexWaygood reviewed on 2024-05-06 08:43_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:797 on 2024-05-06 08:43_

Oh, very nice... I hadn't considered this at all! And I think it makes it much more readable.

---

_@dhruvmanila reviewed on 2024-05-06 08:46_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:797 on 2024-05-06 08:46_

Just to be clear, I'd keep the `DictItem` approach as is in this PR. This is just an alternative design that I had in mind while working on the parser :)

---

_@AlexWaygood reviewed on 2024-05-06 11:37_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/comparable.rs`:692 on 2024-05-06 11:37_

Implemented in https://github.com/astral-sh/ruff/pull/11267/commits/65e5b1dd4fd67d6570cf6ed1a7a9c3fa899105d4

---

_@AlexWaygood reviewed on 2024-05-06 13:46_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/parser/recovery.rs`:54 on 2024-05-06 13:46_

Done in https://github.com/astral-sh/ruff/pull/11267/commits/3ce0f6a96f0dd2a1c3d8cb8a452a4d5b7c5ad08a

---

_@AlexWaygood reviewed on 2024-05-06 13:49_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:4619 on 2024-05-06 13:49_

I have no idea if `80` is correct for Rustc < 1.76. I just did `72 + 8`.

---

_Comment by @carljm on 2024-05-06 13:56_

The evaluation order at runtime is `key, value, key, value, ...` in source order, matching the new order used in this PR.

This is perhaps simplest to observe by de-compiling some Python code using `dis`:

```
>>> import dis
>>> def f():
...     return {a: b, c: d}
...
>>> dis.dis(f)
  1           RESUME                   0

  2           LOAD_GLOBAL              0 (a)
              LOAD_GLOBAL              2 (b)
              LOAD_GLOBAL              4 (c)
              LOAD_GLOBAL              6 (d)
              BUILD_MAP                2
              RETURN_VALUE
```

---

_Comment by @AlexWaygood on 2024-05-06 14:01_

> The evaluation order at runtime is `key, value, key, value, ...` in source order, matching the new order used in this PR.
> 
> This is perhaps simplest to observe by de-compiling some Python code using `dis`:

But unfortunately, it appears that the evaluation order for mapping patterns in `case` statements may be different, so I might have to tweak some of the changes I just pushed in https://github.com/astral-sh/ruff/pull/11267/commits/3ce0f6a96f0dd2a1c3d8cb8a452a4d5b7c5ad08a:

```pycon
>>> def f():
...     match config:
...         case {'a': 'b', 'c': 'd'}:
...             pass
... 
>>> dis.dis(f)
  1           0 RESUME                   0

  2           2 LOAD_GLOBAL              0 (config)

  3          12 MATCH_MAPPING
             14 POP_JUMP_IF_FALSE       24 (to 64)
             16 GET_LEN
             18 LOAD_CONST               1 (2)
             20 COMPARE_OP              92 (>=)
             24 POP_JUMP_IF_FALSE       19 (to 64)
             26 LOAD_CONST               4 (('a', 'c'))
             28 MATCH_KEYS
             30 COPY                     1
             32 POP_JUMP_IF_NONE        13 (to 60)
             34 UNPACK_SEQUENCE          2
             38 LOAD_CONST               2 ('b')
             40 COMPARE_OP              40 (==)
             44 POP_JUMP_IF_FALSE        7 (to 60)
             46 LOAD_CONST               3 ('d')
             48 COMPARE_OP              40 (==)
             52 POP_JUMP_IF_FALSE        4 (to 62)
             54 POP_TOP
             56 POP_TOP

  4          58 RETURN_CONST             0 (None)

  3     >>   60 POP_TOP
        >>   62 POP_TOP
        >>   64 POP_TOP
             66 RETURN_CONST             0 (None)
```

(Side note: wow that's a lot of bytecode instructions!)

I'm not entirely sure if there's a situation where it would matter for a mapping pattern, but perhaps we should err on the side of caution here and match the Python runtime as far as possible anyway.

---

_@AlexWaygood reviewed on 2024-05-06 14:07_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/node.rs`:3807 on 2024-05-06 14:07_

Hmm, here I didn't change the iteration order, but according to the results of `dis.dis()` in https://github.com/astral-sh/ruff/pull/11267#issuecomment-2096090737, the existing order might be incorrect? Possibly we should be visiting all the keys in a `PatternMatchMapping` before we visit any of the values, since it seems (unlike with dictionary literals) the Python runtime loads all the keys before loading any of the values when it comes to mapping patterns

---

_@AlexWaygood reviewed on 2024-05-06 14:54_

---

_Review comment by @AlexWaygood on `crates/ruff_linter/src/rules/ruff/rules/sort_dunder_slots.rs`:302 on 2024-05-06 14:54_

the main reason to change the type is that there used to be a `Vec<Expr>` representing the `values` of the `ExprDict` node that was owned by the `ExprDict` node, and we just passed around a reference to that `Vec`. Now there isn't anymore, so we either need to construct our own `Vec` of values that we own (seems wasteful as it involves allocating) or do something like this, or pass around a reference to a container from which we could easily retrieve the `values`. I've switched to the latter approach in https://github.com/astral-sh/ruff/pull/11267/commits/4837ce49136c535ceb4ab92ffe94a8bc46c837db, to simplify the signature here.

---

_Comment by @AlexWaygood on 2024-05-06 16:48_

I've taken the changes to the pattern-matching nodes back out again, as @dhruvmanila pointed out to me that they might conflict with some changes around that node being planned in #10570 (thanks!).

> I wonder if you thought about an alternative design which would utilize an enum like:
> 
> ```rust
> pub enum DictItem {
> 	KeyValue(KeyValueItem),
> 	DoubleStarred(Expr), // or Expanded, Spread, ...
> }
> ```

I have a working prototype of this locally, and I think it's possibly an improvement. But I'm also happy to leave it for now.

The PR should be ready for re-review!

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-05-06 16:48_

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-05-06 16:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:826 on 2024-05-07 06:16_

I think we could implement `Index` for `item`, if that's even something that's commonly used. 

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:865 on 2024-05-07 06:17_


```suggestion
        self.items.size_hint()
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:908 on 2024-05-07 06:17_


```suggestion
        self.items.size_hint()
    }
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:902 on 2024-05-07 06:18_


```suggestion
        self.next_back()
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:859 on 2024-05-07 06:18_


```suggestion
        self.next_back()
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:876 on 2024-05-07 06:22_

You get `is_empty` for free by implementing `is_empty`. You may want to consider overriding `ExactSizeIterator::len()`

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:834 on 2024-05-07 06:23_


```suggestion
#[derive(Debug, Clone)]
```

It's sometimes useful to have the ability to clone an iterator.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:810 on 2024-05-07 06:27_

You could rewrite this to 
```suggestion
            ast::Expr::Dict(dict) => {
                let mut narrowed_keys = Vec::with_capacity(dict.len());
                for key in dict.iter_keys() {
                    if let Some(key) = key {
                        // This is somewhat unfortunate,
                        // *but* using a dict for __slots__ is very rare
                        narrowed_keys.push(key.to_owned());
                    } else {
                        return None;
                    }
                }
                // If `None` was present in the keys, it indicates a "** splat", .e.g
                // `__slots__ = {"foo": "bar", **other_dict}`
                // If `None` wasn't present in the keys,
                // the length of the keys should always equal the length of the values
                assert_eq!(narrowed_keys.len(), dict.len());
                Self {
                    elts: Cow::Owned(narrowed_keys),
                    range: dict.range(),
                    display_kind: DisplayKind::Dict { items: dict.items() },
                }
            }
            _ => return None,
```

If you add a `len` and `is_empty` method to `ExprDict`

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:814 on 2024-05-07 06:28_

I recommend returning a slice here. It's often easier to work with a slice than an iterator.

```suggestion
    pub fn iter_items(&self) -> &[DictItem] {
        self.items.iter()
    }

```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:43 on 2024-05-07 06:29_

Nit. You could consider passing the item to `KeyValuePair`

---

_@MichaReiser approved on 2024-05-07 06:33_

---

_Comment by @MichaReiser on 2024-05-07 06:34_

> I've taken the changes to the pattern-matching nodes back out again, as @dhruvmanila pointed out to me that they might conflict with some changes around that node being planned in https://github.com/astral-sh/ruff/issues/10570 (thanks!).

Do we know how the pattern layout will change? Are the changes compatible with the changes we made here or will we end up with a large inconsistency between the two nodes?

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:876 on 2024-05-07 06:38_

Unfortunately `is_empty` is an unstable feature of `ExactSizeIterator` currently: https://doc.rust-lang.org/std/iter/trait.ExactSizeIterator.html#method.is_empty

---

_@AlexWaygood reviewed on 2024-05-07 06:38_

---

_@AlexWaygood reviewed on 2024-05-07 06:42_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:877 on 2024-05-07 06:42_

```suggestion
#[derive(Debug, Clone)]
```

---

_@AlexWaygood reviewed on 2024-05-07 06:46_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:810 on 2024-05-07 06:46_

I think adding those methods to `ExprDict` is very worth considering, but I'd want to add them to `ExprList`, `ExprSet` and `ExprTuple` at the same time. I think it's outside the scope of this PR.

---

_Comment by @AlexWaygood on 2024-05-07 06:48_

> Do we know how the pattern layout will change? Are the changes compatible with the changes we made here or will we end up with a large inconsistency between the two nodes?

Not sure. I'll wait for @dhruvmanila to chime in before merging (I know he's out for much of today)

---

_@AlexWaygood reviewed on 2024-05-07 09:14_

---

_Review comment by @AlexWaygood on `crates/ruff_python_formatter/src/expression/expr_dict.rs`:43 on 2024-05-07 09:14_

Done in https://github.com/astral-sh/ruff/pull/11267/commits/1d6bab848d0a72571e087f52502787582a44aec6

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:814 on 2024-05-07 09:22_

I feel like I'd either prefer to keep the returned type consistent with `iter_key()` and `iter_values()`, or just remove this method entirely. If you want a `[DictItem]` slice, you can just access the `.items` field on the `ExprDict` node directly; it's a public field

---

_@AlexWaygood reviewed on 2024-05-07 09:22_

---

_@AlexWaygood reviewed on 2024-05-07 09:32_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:826 on 2024-05-07 09:32_

I'm not sure it's a common enough need for it to be worth it. But if we did do this, I think (same as https://github.com/astral-sh/ruff/pull/11267#discussion_r1591893793) we should do it for `ExprList`, `ExprSet` and `ExprTuple` at the same time

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:814 on 2024-05-07 10:50_

nit: in that case, I'd remove the `iter_items` method.

---

_@dhruvmanila approved on 2024-05-07 10:54_

Thank you for working on this refactor!

Can you add basic documentation around the new structs and methods? I'd specifically want to document the panic case for methods which index into the dictionary.

---

_@MichaReiser reviewed on 2024-05-07 11:02_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:814 on 2024-05-07 11:02_

> I feel like I'd either prefer to keep the returned type consistent with iter_key() and iter_values(), or just remove this method entirely. If you want a [DictItem] slice, you can just access the .items field on the ExprDict node directly; it's a public field

That's true. The reason why I often prefer methods is because it avoids the sometimes awkward `&` or `as_ref()`

```rust
for item in &dict.items
```

vs 

```
for item in dict.items() 
```

But yeah, I'm okay with removing the method.

---

_Comment by @AlexWaygood on 2024-05-07 11:41_

Thanks both!

---

_Merged by @AlexWaygood on 2024-05-07 11:46_

---

_Closed by @AlexWaygood on 2024-05-07 11:46_

---

_Branch deleted on 2024-07-01 10:29_

---
