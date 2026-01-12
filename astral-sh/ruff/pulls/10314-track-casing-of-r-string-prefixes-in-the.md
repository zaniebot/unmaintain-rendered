```yaml
number: 10314
title: Track casing of r-string prefixes in the tokenizer and AST
type: pull_request
state: merged
author: AlexWaygood
labels:
  - internal
assignees: []
merged: true
base: main
head: r-prefix-casing
created_at: 2024-03-09T14:51:07Z
updated_at: 2024-03-18T17:27:43Z
url: https://github.com/astral-sh/ruff/pull/10314
synced_at: 2026-01-12T15:55:31Z
```

# Track casing of r-string prefixes in the tokenizer and AST

---

_@AlexWaygood_

## Summary

Following #10298, I wanted to look for places in ruff where we could simplify and/or speedup our code by using the information newly available to us in the AST nodes. One obvious place was the formatter, but when I looked at the formatter's code, I realised it was important to the formatter whether or not an uppercase or lowercase `r` prefix was used for a raw string:

https://github.com/astral-sh/ruff/blob/0c84fbb6db646550e00eed5560a692f2ea1b6224/crates/ruff_python_formatter/src/string/mod.rs#L165-L171

https://github.com/astral-sh/ruff/blob/0c84fbb6db646550e00eed5560a692f2ea1b6224/crates/ruff_python_formatter/src/string/mod.rs#L107-L118

That information isn't currently captured in the tokens or AST, so we wouldn't, right now, be able to simplify any of that formatter code. This PR makes it so the tokens and AST _do_ capture that information.

The PR should be easiest to review commit-by-commit.

## Test Plan

`cargo test`


---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-03-09 14:51_

---

_Review requested from @MichaReiser by @AlexWaygood on 2024-03-09 14:51_

---

_Renamed from "Track casing of r string prefixes in the tokenizer and AST" to "Track casing of r-string prefixes in the tokenizer and AST" by @AlexWaygood on 2024-03-09 14:51_

---

_Comment by @github-actions[bot] on 2024-03-09 15:09_

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

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1203 on 2024-03-10 02:11_

I think we should just have two separate variants instead of a boolean value: `Raw` and `RawUpper`. It's easier to type (`FStringPrefix::RawUpper` vs `FStringPrefix::RawFormat { uppercase: true }`) and it clearly separates the two variants w.r.t. an enum. What do you think?

This would apply to other `*Prefix` enums as well.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1629 on 2024-03-10 02:25_

nit: can we use `is_r_string` method if it's accessible here?

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1627 on 2024-03-10 02:27_

What's the reason for removing the default implementation? I think it's reasonable that the default implementation is to not have any prefix. But, I leave this up to you.

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1200 on 2024-03-10 02:30_

nit: maybe we can just use `Regular` ;)

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/lexer.rs`:547 on 2024-03-10 02:36_

Should this be using `FStringPrefix`?

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string_token_flags.rs`:367 on 2024-03-10 03:08_

It's unfortunate that we need to use `panic` here but I understand the reasoning behind it which is that we're going from a wide type containing all possible string prefix to a narrow one for a specific type of string (for example `StringPrefix` to `FStringPrefix`).

I think this should actually be an `unreachable` instead as our lexer will raise an error for invalid combination of prefixes used.

I'm sure you must have thought of this but if possible, I would try to find any type-safe way to do this conversion to avoid a potential panic.

---

_@dhruvmanila reviewed on 2024-03-10 03:09_

This makes sense. Thank you for writing all the documentation. It helps in understanding the code :)

There are a few instances where the documentation is in the form of a question for functions which returns a `bool`. I think my preference would be to just write it in a simple way like "Returns true if ..." or "Checks if ...". We could even top it up with an example like `Returns true if ... e.g., r"foo"`

There are a few nits which I leave at your discretion to apply or not.

---

_Comment by @MichaReiser on 2024-03-10 08:50_

Hmm I'm not sure if we should track this. It goes a bit beyond what an AST tends to capture. But it's also not clear to me what the downsides would be. 

What does the formatter use the lower prefix for? I thought we always normalize to lower case

---

_Comment by @AlexWaygood on 2024-03-10 08:53_

> What does the formatter use the lower prefix for? I thought we always normalize to lower case

The formatter normalises all prefixes to lowercase, except for raw strings, where it retains the casing in line with what black does for raw strings: https://black.readthedocs.io/en/stable/the_black_code_style/current_style.html#r-strings-and-r-strings

> Black normalizes string quotes as well as string prefixes, making them lowercase. One exception to this rule is r-strings. It turns out that the very popular [MagicPython](https://github.com/MagicStack/MagicPython/) syntax highlighter, used by default by (among others) GitHub and Visual Studio Code, differentiates between r-strings and R-strings. The former are syntax highlighted as regular expressions while the latter are treated as true raw strings with no special semantics.



---

_Comment by @MichaReiser on 2024-03-10 09:46_

I see and I guess it's fine adding it now that we already track prefixes 

The reason I'm hesitant to add such fine grained data to the ast are

* This kind of Informationen could probably not be represented if we ever migrate to a CST (or would limit us in the design options for the CST)
* Tracking the information increases the struct sizes which has a cost in memory consumption, data to write and to read (both slow operations). It can often times be faster (when considering the entire compilation pipeline) to reparse information that is needed rarely than paying the cost everywhere 



---

_Comment by @AlexWaygood on 2024-03-10 10:00_

> * Tracking the information increases the struct sizes which has a cost in memory consumption, data to write and to read (both slow operations). It can often times be faster (when considering the entire compilation pipeline) to reparse information that is needed rarely than paying the cost everywhere

Hmm, but internally we're just storing the new information on the same bitflag, no? The enum variants are only created from the bitflag "on demand" when the `prefix()` method is called. Possibly I'm misunderstanding, but I don't _think_ this should increase memory consumption significantly

---

_Comment by @AlexWaygood on 2024-03-10 10:12_

Also, I think the arguments for preserving the casing of r-strings in the formatter apply equally well to the expression generator. Being able to losslessly round-trip these kinds of details in the expression generator was the original motivation for this series of changes (https://github.com/astral-sh/ruff/issues/7799, #9663, etc)

---

_@AlexWaygood reviewed on 2024-03-10 11:34_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1627 on 2024-03-10 11:34_

I think it's a reasonable default as well. But, looking at this enum again, I pondered whether there was actually any real use-case for implementing `Default` on this enum. I can't really think of any situation where it would improve readability to use `StringLiteralPrefix::default()` rather than `StringLiteralPrefix::None`, and I think there is value in forcing people to be explicit here.

There are no places where we use `StringLiteralPrefix::default()` currently; I added it in https://github.com/astral-sh/ruff/pull/10298 more for completeness than anyone else

---

_@AlexWaygood reviewed on 2024-03-10 12:06_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1629 on 2024-03-10 12:06_

Not available to us here, unfortunately -- that method is only available on variants of the `StringLiteralPrefix` enum, and we don't know which variant applies to this instance yet without calling the `prefix()` method (which is the method that we're in right here)

---

_@AlexWaygood reviewed on 2024-03-10 13:01_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1203 on 2024-03-10 13:01_

I'm somewhat torn here. I agree with you that it's easier to type if they're two separate variants. But semantically, they're the same kind of string in Python, just represented in two different ways; having them be two flavours of the same variant feels more expressive of the semantics here. I also think that if I were matching on the prefix in a lint rule (for example), it would be more natural to do this:

```rs
match string_expr.flags.prefix() {
    StringLiteralPrefix::Regular => ...,
    StringLiteralPrefix::Raw { .. } => ...,
}
```

rather than

```rs
match string_expr.flags.prefix() {
    StringLiteralPrefix::Regular => ...,
    StringLiteralPrefix::RawLower | StringLiteralPrefix::RawUpper => ...,
}
```

(In most lint rules, I doubt you'll care much whether it's an `R` or an `r`, since they have the same semantics at runtime for Python.)

---

_@AlexWaygood reviewed on 2024-03-10 13:05_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:367 on 2024-03-10 13:05_

I agree this is somewhat unfortunate. It might be worth noting, though, that this isn't really any less type-safe than previously -- the `panic!`s in this function are really just doing the work for us that the `debug_assert!`s were previously.

The only other way I can _see_ of doing this would be to implement `TryFrom` instead of `From` here -- return an `Err` variant instead of panicking. But that feels incorrect to me. If we hit the panicking branch, I think we _should_ be immediately bailing from this function, since that indicates a logical error in our code somewhere. If we've written the tokenizer and parser correctly, it should be impossible for us to get here.

---

_Comment by @AlexWaygood on 2024-03-10 13:05_

Thanks @dhruvmanila! I've implemented many of your suggestions, in particular most of the ones relating to naming and documentation

---

_Review requested from @dhruvmanila by @AlexWaygood on 2024-03-10 13:05_

---

_Comment by @charliermarsh on 2024-03-10 16:13_

> The reason I'm hesitant to add such fine grained data to the ast are

> This kind of Informationen could probably not be represented if we ever migrate to a CST (or would limit us in the design options for the CST)
> Tracking the information increases the struct sizes which has a cost in memory consumption, data to write and to read (both slow operations). It can often times be faster (when considering the entire compilation pipeline) to reparse information that is needed rarely than paying the cost everywhere

This was my initial reaction too, but...

1. Can you explain more about why this would be a problem for a CST? I don't understand that part.
2. I don't _think_ this has a cost, since we're using a bitflag of the same size either way, though I may be mistaken.


---

_@dhruvmanila reviewed on 2024-03-11 03:06_

---

_Review comment by @dhruvmanila on `crates/ruff_python_parser/src/string_token_flags.rs`:367 on 2024-03-11 03:06_

> the `panic!`s in this function are really just doing the work for us that the `debug_assert!`s were previously.

You're correct although they differ from a user perspective. `debug_assert!` will only panic on debug builds while `panic` will bail out even on the release version.

> I think we _should_ be immediately bailing from this function, since that indicates a logical error in our code somewhere. If we've written the tokenizer and parser correctly, it should be impossible for us to get here.

Yes, this is the reason I suggest using the `unreachable` macro instead. As a developer it's hard to reason about a `panic` usage unless there's an indicator like a comment. However, if `unreachable` is used, one can say that this is indeed a logical error.

---

_@dhruvmanila reviewed on 2024-03-11 03:08_

---

_Review comment by @dhruvmanila on `crates/ruff_python_ast/src/nodes.rs`:1627 on 2024-03-11 03:08_

Thank you for the explanation; this reasoning makes sense.

---

_@AlexWaygood reviewed on 2024-03-11 07:05_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:367 on 2024-03-11 07:05_

Makes sense — I'll switch to `unreachable`, thanks

---

_Label `internal` added by @AlexWaygood on 2024-03-11 07:48_

---

_@AlexWaygood reviewed on 2024-03-11 09:21_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:367 on 2024-03-11 09:21_

> Makes sense — I'll switch to `unreachable`, thanks

Done in https://github.com/astral-sh/ruff/pull/10314/commits/3ee6454f02aa49f2ee451da19f704e280c248dc0

---

_@AlexWaygood reviewed on 2024-03-11 10:42_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:367 on 2024-03-11 10:42_

One way in which we could make this type-safe would be to emit different tokens for string literals and bytes literals. Then we would be able to have the same three-way `f-string/bytestring/everything-else` distinction at the token level as we do at the AST level, which would mean we would be able to have three different bitflags at the token level. We wouldn't need to do the type narrowing here when creating an AST node from a token because the type would already be narrowed.

But that would be quite a large change to make.

---

_Comment by @MichaReiser on 2024-03-18 12:41_

> Can you explain more about why this would be a problem for a CST? I don't understand that part.

It depends on the CST design. For example, Roslyn, rust-analyzer, Biome, and libswift all use a uniform representation for all nodes that mainly consists of the node's kind, its children and possibly some flags (single flags structure shared between all nodes). That means that node specific attributes or flags aren't possible. You can see this in [Biome's playground](https://biomejs.dev/playground/?code=ZgB1AG4AYwB0AGkAbwBuACAAdABlAHMAdAAoACkAIAB7AAoAIAAgAGMAbwBuAHMAbwBsAGUALgBsAG8AZwAoACIAYQAiACkACgB9AA%3D%3D) when opening the syntax tab and looking at the CST content. For each level, it only shows the kind followed by its children.

A uniform representation has the advantage of traversing the tree without casting to the specific AST nodes. This makes it easy to implement traversal methods like `descendants` or storing a reference to a node. 

The way we would need to model this with such a CST design is to look at the tokens directly (similar to what we used to do with Locator)

> I don't think this has a cost, since we're using a bitflag of the same size either way, though I may be mistaken.

That's correct and what I mentioned in 
> I see and I guess it's fine adding it now that we already track prefixes

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1236 on 2024-03-18 12:45_

Nit: I prefer using `f.write_str(self.as_str())` when writing a `str` without any placeholders, to avoid relying on the compiler to remove the write macro overhead.
```suggestion
        f.write_str(self.as_str())
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1268 on 2024-03-18 12:48_

The following will create an invalid prefix:

```rust
FStringFlags::default().with_prefix(FStringPrefix::Raw { uppercase_r: true }).with_prefix(FStringPrefix::Raw { uppercase_r: false })
```

Or passing regular after uppercase won't have the desired outcome:


```rust
FStringFlags::default().with_prefix(FStringPrefix::Raw { uppercase_r: true }).with_prefix(FStringPrefix::Regular)
    .prefix().is_raw() // still returns true
```


You'll need to unset the Raw flags before setting them.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1202 on 2024-03-18 12:51_

Nit:I don't think it simplifies anything, so I would probably keep what you have. An alternative flag design would be to have `R_PREFIX` and `R_UPPER_CASE` where the `R_PREFIX` is set for lower or upper case, and upper is only set when using `R` (but not for `r`). This could simplify the "has an r flag" check, depending on how it is performed, or simplify the `with_prefix` because you can always write both flags.

```rust
    #[must_use]
    pub fn with_prefix(mut self, prefix: FStringPrefix) -> Self {
        self.0.set(FStringFlagsInner::R_PREFIX_LOWER, matches!(prefix, FStringPrefix::Raw { ..}));
        self.0.set(FStringFlagsInner::R_PREFIX_UPPER, matches!(prefix, FStringPrefix::Raw { uppercase_r: true }));
        
        self
    }
```



---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1606 on 2024-03-18 12:54_

Same as for `FStringFlags`, this can lead to both lower and upper being set (or unicode) when this method is called multiple times on the same flags instance.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1307 on 2024-03-18 12:55_

Nit: Calling into the prefixe's `Debug` implementation seems more idiomatic to me rather than using the `Display` representation.
```suggestion
            .field("prefix", &self.prefix())
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1650 on 2024-03-18 12:56_

Nit: I think I would find calling into the `Debug` representation more idomatic (rather than using the `Display` representation)
```suggestion
            .field("prefix", &self.prefix())
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1693 on 2024-03-18 12:56_

Nit
```suggestion
        f.write_str(self.as_str())
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1973 on 2024-03-18 12:57_

Nit: 

```suggestion
        f.write_str(self.as_str())
```

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2006 on 2024-03-18 12:57_

Same as for other `with_prefix`: Chaining `with_prefix` calls can lead to inconsistent internal state.

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:2044 on 2024-03-18 12:58_

Nit
```suggestion
            .field("prefix", &self.prefix())
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/lexer.rs`:711 on 2024-03-18 12:59_

Nice that `prefix` now is no longer an `Option` :)

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:165 on 2024-03-18 13:01_


```suggestion
        f.write_str(self.as_str())
```

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:235 on 2024-03-18 13:03_

Nit: It could make sense to move this to `StringPrefix:from_kind` so that the method is next to `StringPrefix::as_flags`

---

_Review comment by @MichaReiser on `crates/ruff_python_parser/src/string_token_flags.rs`:327 on 2024-03-18 13:03_

Nit:
```suggestion
            .field("prefix", &self.prefix())
```

---

_@MichaReiser approved on 2024-03-18 13:04_

---

_@AlexWaygood reviewed on 2024-03-18 13:45_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1268 on 2024-03-18 13:45_

I addressed this in https://github.com/astral-sh/ruff/pull/10314/commits/bc3e406d1e564ffb256badcb2da8794884cb1ebc

But I also wondered about simply prohibiting calling `.with_prefix()` if a prefix flag had already been set (using `debug_assert!(!self.0.intersects(FStringFlagsInner::R_PREFIX_UPPER | FStringFlagsInner::R_PREFIX_LOWER), "Cannot call .with_prefix() if a prefix has already been set")`. This way is more type-safe, though :-)

---

_@AlexWaygood reviewed on 2024-03-18 15:51_

---

_Review comment by @AlexWaygood on `crates/ruff_python_parser/src/string_token_flags.rs`:235 on 2024-03-18 15:51_

Nice idea!

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1307 on 2024-03-18 16:10_

I tried this out locally... I agree with you that in general it seems correct to use the `Debug` representation of a field inside the `Debug` representation of the superstructure, but here it results in lots of snapshot diffs like this:

<details>
<summary>Screenshot</summary>

![image](https://github.com/astral-sh/ruff/assets/66076021/9187eba7-7f37-410b-b72b-c4e20c6517ed)

</details>

In my opinion, that makes the snapshots significantly less readable :/ What do you think?

---

_@AlexWaygood reviewed on 2024-03-18 16:10_

---

_@MichaReiser reviewed on 2024-03-18 16:36_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1268 on 2024-03-18 16:36_

I would find it surprising if I'm not allowed to call a method multiple times. An alternative is to have a `from_prefix` method that constructs `Self`. This way, the compiler guarantees that the method can only be called once during construction. 

---

_@MichaReiser reviewed on 2024-03-18 16:38_

---

_Review comment by @MichaReiser on `crates/ruff_python_ast/src/nodes.rs`:1307 on 2024-03-18 16:38_

I agree that it makes the snapshot less readable but it is more accurate in the sense that `prefix` isn't a string. The main use case for using `dbg!` is to understand a data structure. That's where it's important to get accurate types. 


---

_@AlexWaygood reviewed on 2024-03-18 16:39_

---

_Review comment by @AlexWaygood on `crates/ruff_python_ast/src/nodes.rs`:1307 on 2024-03-18 16:39_

fair enough!

---

_Merged by @AlexWaygood on 2024-03-18 17:18_

---

_Closed by @AlexWaygood on 2024-03-18 17:18_

---

_Branch deleted on 2024-03-18 17:20_

---
