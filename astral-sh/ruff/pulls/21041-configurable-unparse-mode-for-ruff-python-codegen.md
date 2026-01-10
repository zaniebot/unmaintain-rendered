```yaml
number: 21041
title: "Configurable \"unparse mode\" for `ruff_python_codegen::Generator`"
type: pull_request
state: merged
author: ShaharNaveh
labels:
  - internal
assignees: []
merged: true
base: main
head: generator-force-tup-parenth
created_at: 2025-10-23T08:30:51Z
updated_at: 2025-10-25T08:52:33Z
url: https://github.com/astral-sh/ruff/pull/21041
synced_at: 2026-01-10T16:59:49Z
```

# Configurable "unparse mode" for `ruff_python_codegen::Generator`

---

_Pull request opened by @ShaharNaveh on 2025-10-23 08:30_

## Summary

At [RustPython](https://github.com/RustPython/RustPython) we want to fully replace https://github.com/RustPython/RustPython/blob/153d0eef51ea109d8a322fd351678847b2fb8fe2/compiler/codegen/src/unparse.rs.

Currently our only regression when using Ruff's unparsing, is forcing tuple parentheses. 

## Test Plan

Added tests

---

_Comment by @github-actions[bot] on 2025-10-23 08:40_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:1633 on 2025-10-23 09:46_

The signature here becomes a bit awkward. Should we introduce a `GeneratorOptions` struct with: 

* `indentation`
* `line_ending`
* `preferred_quote`: Option
* `forced_tuple_parentheses`

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-23 09:48_

This option seems fairly low-level and I worry we'll have more conflicts in the future because we're likely to go to preserve the source formatting in more places where what you want is some fixed formatting (not sure I understand why it matters to you how tuples are formatted). 

IMO, this does raise the question if our goals are well aligned or if we'll keep patching up differences forever. Have you considerd just forking the generator?

---

_@MichaReiser reviewed on 2025-10-23 09:48_

---

_@ShaharNaveh reviewed on 2025-10-23 10:18_

---

_Review comment by @ShaharNaveh on `crates/ruff_python_codegen/src/generator.rs`:1633 on 2025-10-23 10:18_

sounds good

---

_@ShaharNaveh reviewed on 2025-10-23 10:28_

---

_Review comment by @ShaharNaveh on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-23 10:28_

> This option seems fairly low-level and I worry we'll have more conflicts in the future because we're likely to go to preserve the source formatting in more places where what you want is some fixed formatting (not sure I understand why it matters to you how tuples are formatted).

Because at RustPython we are using CPython test suite. when you do:
```python
import ast
parsed = ast.parse('x = 1,')
ast.unparse(parsed)
```
The returned value is `x = (1,)`, and our tests fails because it's getting `x = 1,` instead

> IMO, this does raise the question if our goals are well aligned or if we'll keep patching up differences forever. Have you considerd just forking the generator?

We have considered that, and agreed on try to get it upstream and in the worst case we will fork, but that's less ideal for us. obviously I get the concern with merging code that solves a very niche issue


---

_@MichaReiser reviewed on 2025-10-23 10:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-23 10:44_

> We have considered that, and agreed on try to get it upstream and in the worst case we will fork, but that's less ideal for us. obviously I get the concern with merging code that solves a very niche issue

I'm less concerned about the niche use case. My concern is that our goals aren't aligned. Our long-term goal is to preserve as much as possible about the source formatting as possible. This is in direct contrast with what you want. You want a generator that's `ast.unparse` compatible. 

---

_@youknowone reviewed on 2025-10-23 13:39_

---

_Review comment by @youknowone on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-23 13:39_

Hi! Long time no see.

I fully understand that it’s not natural for unnecessary features to be included in the Ruff repository.

However, from interpreting your explanation, it seems that @ShaharNaveh’s requirements may not actually be in direct contrast with your long-term goals.

To preserve the source formatting, the system would need to store whether the original text included parentheses. Only then it could decide whether to restore them. Currently, since the original parenthesis information isn’t preserved, Ruff can only offer two fixed options: always include parentheses or never include them.

In the long term, If ruff_python_codegen::Generator will need to include information from the original source anyway, then perhaps @ShaharNaveh could help implement this based on your suggestion how to approach consistent with the project’s goals. 
Meanwhile, RustPython could achieve the same purpose by simply using a fixed value instead of the original formatting information.

---

_@MichaReiser reviewed on 2025-10-23 13:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-23 13:45_

The issue is that, once added, other users than RustPython will start using it and it will be harder for us to remove or even remember what the use case was. 

I also don't fully understand the hesitation of just forking that one module. It's not that much code.

---

_@youknowone reviewed on 2025-10-23 14:24_

---

_Review comment by @youknowone on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-23 14:24_

My point is that if, as you say, your goal is to preserve the exact syntax of the original input, then users also can use the same feature aligned to your goal.

It’s unfortunate that this discussion was seen merely as hesitation about forking. Everyone knows the code can be forked at any time, but I don’t think that was ever the focus of this conversation.

Anyway, I understand that you’re currently not interested in this issue, and that the Ruff project is more focused on the concern that “it will be harder for us to remove or even remember what the use case was” rather than on a technical discussion about whether a similar feature might be needed in the future to restore the original syntax.

Thanks

---

_@MichaReiser reviewed on 2025-10-23 17:24_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-23 17:24_

> My point is that if, as you say, your goal is to preserve the exact syntax of the original input, then users also can use the same feature aligned to your goal.

I don't understand that part. The feature you need is to always set parentheses because that's the only formatting that's compatible with `unparse`.

> rather than on a technical discussion about whether a similar feature might be needed in the future to restore the original syntax.

I don't think that's what you need? 

The example from @ShaharNaveh shows that you don't want to preserve formatting:

```py
import ast
parsed = ast.parse('x = 1,')
ast.unparse(parsed) # should become x = (1,)
```

---

_@youknowone reviewed on 2025-10-24 04:25_

---

_Review comment by @youknowone on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-24 04:25_

To preserve the original syntax, there must be a field to store information that it had parenthesis or not before.
Once if there is a field. @ShaharNaveh can fill the field as he needs.

That's based on your description:

> ... because we're likely to go to preserve the source formatting in more places ...

> Our long-term goal is to preserve as much as possible about the source formatting as possible. 

Is the preservation already implemented? Then he might be able to look for it if it is reusable or not.
Otherwise it will be required later to preserve the original formatting.



---

_@MichaReiser reviewed on 2025-10-24 06:00_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-24 06:00_

No such field exists today because Ruff always uses the precedence. But we ran into the same issue with quotes, where Ruff used to preserve parentheses, and we had to add an extra option to allow customizing the quotes. That's where my concern comes from, the "better" we make the generator for Ruff's purpose, the more flags we'll need. 

I'd be open to removing the quote style option again and instead introducing a Mode option with two variants: `Default` and `AstUnparse`. This at least cleary documents the intent of all those options



---

_Review requested from @MichaReiser by @ShaharNaveh on 2025-10-24 10:05_

---

_Renamed from "Optionally force tuple parentheses for `ruff_python_codegen::Generator`" to "Configurable "unparse mode" for `ruff_python_codegen::Generator`" by @ShaharNaveh on 2025-10-24 10:06_

---

_@MichaReiser reviewed on 2025-10-24 10:44_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:72 on 2025-10-24 10:44_

Nit: I think just `Mode` is enough. I'd rename `Ast` to `AstUnparse` and document that the intent is to emit the same output as Python's `ast.unparse` method

---

_@MichaReiser reviewed on 2025-10-24 10:45_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:188 on 2025-10-24 10:45_

We could make this a method on `Mode` insteada of repeating it at every call site

```
impl Mode {
	const fn quote_style(self, flags: AnyStringFlags) -> QuoteStyle {
		...
	}
}
```

---

_Review comment by @ShaharNaveh on `crates/ruff_python_codegen/src/generator.rs`:72 on 2025-10-24 10:47_

np, would you prefer this documented at the enum variant, or the `Generator::with_unparse_mode`?

---

_@ShaharNaveh reviewed on 2025-10-24 10:47_

---

_@MichaReiser reviewed on 2025-10-24 10:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:72 on 2025-10-24 10:48_

I suggest documenting it at the variant level

---

_Review comment by @ShaharNaveh on `crates/ruff_python_codegen/src/generator.rs`:188 on 2025-10-24 10:49_

I guess that it would make sense to move the tuple precedence value to it's own method as well, right? although it's only used once it kind of makes more sense to me for some reason. I'll do that if you think it's a good idea

---

_@ShaharNaveh reviewed on 2025-10-24 10:49_

---

_@MichaReiser reviewed on 2025-10-24 10:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_codegen/src/generator.rs`:188 on 2025-10-24 10:55_

I'm less concerned about the tuple case because it's used exactly once.

---

_@ShaharNaveh reviewed on 2025-10-24 11:23_

---

_Review comment by @ShaharNaveh on `crates/ruff_python_codegen/src/generator.rs`:82 on 2025-10-24 11:23_

had to use generics because this gets called with either `AnyStringFlags` or `BytesLiteralFlags`

---

_@ShaharNaveh reviewed on 2025-10-24 11:24_

---

_Review comment by @ShaharNaveh on `crates/ruff_python_codegen/src/generator.rs`:1620 on 2025-10-24 11:24_

Renamed to `UnparsedMode` here because the name collides with 
```rust
use ruff_python_parser::{..., Mode, ...};
```

---

_Review requested from @MichaReiser by @ShaharNaveh on 2025-10-24 11:28_

---

_Label `internal` added by @MichaReiser on 2025-10-24 15:41_

---

_Merged by @MichaReiser on 2025-10-24 15:44_

---

_Closed by @MichaReiser on 2025-10-24 15:44_

---

_Review comment by @youknowone on `crates/ruff_python_codegen/src/generator.rs`:130 on 2025-10-25 00:44_

Thank you all for trying to find common ground.

---

_@youknowone reviewed on 2025-10-25 00:44_

---

_Comment by @ShaharNaveh on 2025-10-25 03:46_

tysm @MichaReiser <3 !

---

_Branch deleted on 2025-10-25 08:52_

---
