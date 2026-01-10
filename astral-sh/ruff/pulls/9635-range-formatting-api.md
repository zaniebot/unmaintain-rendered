```yaml
number: 9635
title: Range formatting API
type: pull_request
state: merged
author: MichaReiser
labels:
  - internal
  - formatter
assignees: []
merged: true
base: main
head: formatter-range-formatting
created_at: 2024-01-24T15:16:11Z
updated_at: 2024-01-31T10:13:38Z
url: https://github.com/astral-sh/ruff/pull/9635
synced_at: 2026-01-10T22:57:09Z
```

# Range formatting API

---

_Pull request opened by @MichaReiser on 2024-01-24 15:16_

## Summary

This PR adds an API for range formatting Python code. It is not within the scope of this PR to expose range formatting to the CLI or use it in the LSP.  Part of https://github.com/astral-sh/ruff/issues/7233

The implementation is inspired by Prettier's and Biome's functionality, but I adopted it for better and correct results when formatting Python code and extended it with support for suppression comments. 

The range formatting performs the following steps:

1. ~~Trim `range` to ensure the start and end are non-whitespace characters (helps to format the smallest possible range)~~
1. Find the deepest node that fully covers `range` (ideally a single logical line statement). 
1. Try to narrow the range of the node identified by step 2: For example, the deepest node might be an `if` statement but it's only necessary to format two sibling statements in the `if` body. This step narrows the range of the `if` statement to the range between the two sibling statements.
1. Format the deepest node and use the generated source map to map the range identified in step 3. to the range in the formatted output. It takes the relevant subslice from the formatted code.


The range-formatting only selects logical-line nodes because a design goal is that formatting a range results in the same formatting (for that range) as when formatting the entire file. This property would be violated when supporting formatting sub-expressions because a) Adding parentheses around enclosing expressions can toggle an expression from non-splittable to splittable, b) formatting a sub-expression has fewer split points than formatting the entire expressions.

Note: Our implementation no longer trims the range as the first step. It proved to be an issue when used in IDEs where you want Ruff to remove newlines after you delete a set of statements or when you insert unnecessary newlines between statements. In this case, the sent range is whitespace only and we should format the whitespace to the right amount of blank lines.


This implementation differs from Prettier and Biome by:

**Indentation**

Python's whitespace sensitivity complicates range formatting in case the actual indentation in the source code doesn't match the desired indentation (specified in the configuration):

```python
0| def test(a):
1|   b = 10
2|   return a + 10
```

The above example uses a 2-space indentation, and let's assume the configured desired indentation is 4-spaces. It isn't possible to format only `b = 10` because it would change the statement indentation from 2 spaces to 4 spaces, but `return a + 10` still uses a 2-space indentation. This situation is rare enough that it isn't worth handling in non-whitespace-sensitive languages where the result looks odd, but the program remains correct. However, it must be handled when formatting Python code. This PR implements this by increasing (or not narrowing) the formatting range to cover all "incorrectly" indented statements.

I had an earlier implementation that did allow the ancestor node to use a *nonstandard* indentation. However, it complicated the implementation and violates the property that formatting a range results in the same formatting for the given range as when formatting the entire document.

**Suppressions**

The passed range may fall into a suppressed range. To my understanding, neither Biome nor Prettier handles the case where one of the ancestor nodes is suppressed.

```python
def test():
	# fmt: off
	if True:
		<RANGE_START>[a, b, c,]<RANGE_END>
``` 

The array `[a, b, c,]` should not get formatted because the enclosing `if` statement is suppressed. 

**Clause headers**

```python
<RANGE_START>def test(a, b, c): <RANGE_END>
	print("Don't format this")
```

Step 2. selects the function definition statement as the deepest node that fully encloses the formatting range. Prettier and Biome format the entire function definition statement rather than just the header. This results in formatting significantly more statements than specified by the range (it looks harmless here but what if it is a class with many methods). This is because Biome and Prettier only allow sibling nodes in the range narrowing step 3. 

Our implementation lifts the restriction that the start and end in step 3 need to be sibling nodes and supports:

* start: The start or end of any *child* node or any leading or trailing comment.
* end: The start or end of any *child* node, any leading or trailing comment, or the end of the `:` after a clause header.

## Printer changes

This PR includes a fix to the `Printer`s source map generation. The formatter emits source map markers at the beginning and end of each node. For example:

```python
def test():
	pass
```

the generated IR is:

```
[
  source_position(0),
  "def test",
  group([source_position(8), group(["()"]), source_position(10)]),
  ":",
  source_position(11),
  indent([hard_line_break, source_position(13), "pass", source_position(17)]),
  hard_line_break,
  source_position(17),
  hard_line_break,
  source_position(17)
]
```

The *start* source position marker of `pass` is emitted inside of `indent`. However, the printer mapped position 13 to the position before the indent in the formatted output (that's offset `9`). This was due to the Printer deferring the printing of indentations until it prints the next text, but it emitted the source position immediately when seeing the `FormatElement::SourcePosition`. Which semantically was equal to:

```
source_position(11),
hard_line_break
source_position(13)
indent([ "pass", source_position(17)]),
```



## Test Plan

Added tests. 


---

_Comment by @MichaReiser on 2024-01-27 20:44_

Oh my, I'm bad at making small PRs :scream: 

---

_@MichaReiser reviewed on 2024-01-27 20:47_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/builders.rs`:331 on 2024-01-27 20:47_

In many situations, nodes start and end exactly at the same position. For example:

```python
print("a")
```

The Module, the expression statement, and the call expression all start and end at the same position. This check prevents that we push the same source marker multiple times to:

* Reduce the IR size (write less data)
* Reduce the work during printing (less IR to read, also applies to humans debugging the IR)

---

_@MichaReiser reviewed on 2024-01-27 20:49_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/printer/mod.rs`:51 on 2024-01-27 20:49_

I had to change the printer to accept an arbitrary indentation to support `<space><tab><space>` indentation (and not just `<indent><space>`

---

_@MichaReiser reviewed on 2024-01-27 20:51_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/printer/mod.rs`:155 on 2024-01-27 20:51_

This change fixes the source map offsets for nodes. 

We emit a source map entry at the start of every node. For example:

```python
def a():
    pass
```

We emit a source map entry at the start of `def a` (0) and another for `pass` (13). 

**Before**:

The source map entry started at position 9 (before the indentation) which is unexpected because it is wider than the node's range.

**After**:

The source map begins after the indent at position 13.




---

_Label `formatter` added by @MichaReiser on 2024-01-27 20:57_

---

_Added to milestone `Formatter: Stable` by @MichaReiser on 2024-01-27 20:57_

---

_Comment by @github-actions[bot] on 2024-01-27 21:02_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/dalle/Image_generations_edits_and_variations_with_DALL-E.ipynb:3:7:8: Unexpected token 'prompt'
```

</p>
</details>




---

_@MichaReiser reviewed on 2024-01-28 12:29_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/printer/mod.rs`:64 on 2024-01-28 12:29_

This fixes the issue where the base indentation was missing for the first node. 

---

_@MichaReiser reviewed on 2024-01-28 12:30_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/black_compatibility@cases__line_ranges_diff_edge_case.py.snap`:29 on 2024-01-28 12:30_

This Black test documents a limitation of Black. Ruff does not have this limitation

---

_Label `internal` added by @MichaReiser on 2024-01-29 08:25_

---

_Review comment by @MichaReiser on `crates/ruff_wasm/src/lib.rs`:297 on 2024-01-29 09:27_

Seeing source position helps debugging sourcce map issues.

---

_@MichaReiser reviewed on 2024-01-29 09:27_

---

_Marked ready for review by @MichaReiser on 2024-01-29 09:59_

---

_Review requested from @konstin by @MichaReiser on 2024-01-29 09:59_

---

_Review requested from @BurntSushi by @MichaReiser on 2024-01-29 09:59_

---

_Review requested from @charliermarsh by @MichaReiser on 2024-01-29 09:59_

---

_Review comment by @charliermarsh on `crates/ruff_formatter/src/builders.rs`:331 on 2024-01-29 14:57_

Gah, how does the call expression start and end at the same position? Doesn't it start before `p` and end after `)`? I think I'm revealing a misunderstanding of `SourcePosition`.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/comments/mod.rs`:368 on 2024-01-29 15:03_

To avoid monomorphization? (Or whatever the right term would be for: generating lots of implementations now that it's generic.)

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/fixtures.rs`:59 on 2024-01-29 15:07_

Good news, you already had to write the argument-parsing code.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/tests/fixtures.rs`:103 on 2024-01-29 15:07_

Nit: should we move this about the `let extension` line, so it sits with the stability check?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/range.rs`:28 on 2024-01-29 15:09_

What's "single line body"? Like `foo()` in `while True: foo()`?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/range.rs`:331 on 2024-01-29 15:25_

Nit: ragne

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/range.rs`:353 on 2024-01-29 15:26_

Nit: two spaces after "and"

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/range.rs`:497 on 2024-01-29 15:27_

👍 👍 

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/range.rs`:512 on 2024-01-29 15:28_

Yeah this is actually the reason we end up using LibCST in the linter when adjusting indentation of bodies for fixes.

---

_@charliermarsh approved on 2024-01-29 15:29_

This is so impressive, and so well-documented!

---

_@MichaReiser reviewed on 2024-01-29 15:31_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/range.rs`:28 on 2024-01-29 15:31_

Yes. Do you know a more formal term for this? 

---

_@MichaReiser reviewed on 2024-01-29 15:42_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/builders.rs`:331 on 2024-01-29 15:42_

I was confused why you think that the call expression should start and end at the same position, but my poor phrasing was the reason. 

It's not that the call expression starts and ends at the same position, but that different nodes can start or end at the same positions

```
[
  source_position(0),
  source_position(0),
  source_position(0),
  source_position(0),
  "print",
  source_position(5),
  source_position(5),
  "(",
  group([
    indent([
      soft_line_break,
      group([source_position(6), "\"a\"", source_position(9)])
    ]),
    soft_line_break
  ]),
  ")",
  source_position(10),
  source_position(10),
  source_position(10),
  hard_line_break,
  source_position(10)
]
```

This is the IR generated for the above example **without** this change. There are a lot of duplicated positions. 

* position(0): The module, expression statement, call expression, and the identifier `print` start at position 0
* position(5): The identifier ends at position 5 and the arguments start at position 5
* position(10): The call arguments, call expression, expression statement, and module all end at position 10

---

_@charliermarsh reviewed on 2024-01-29 15:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/range.rs`:28 on 2024-01-29 15:50_

Unfortunately no. I just know that this is a compound statement (https://docs.python.org/3/reference/compound_stmts.html). Maybe "single-line compound statement"?

---

_@charliermarsh reviewed on 2024-01-29 15:51_

---

_Review comment by @charliermarsh on `crates/ruff_formatter/src/builders.rs`:331 on 2024-01-29 15:51_

Oh right, this makes sense, thank you.

---

_@MichaReiser reviewed on 2024-01-29 16:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/range.rs`:28 on 2024-01-29 16:17_

I guess we could say a compound statement with a simple statement body (or non-suite body)

---

_Review comment by @BurntSushi on `crates/ruff_formatter/src/lib.rs`:449 on 2024-01-29 18:48_

What is `source` here? Is it the unformatted/original version of `self.code`?

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/range_formatting/docstring_code_examples.py`:79 on 2024-01-29 18:59_

Glad you found this little test useful. :P 

I think Murphy was watching Madagascar while I wrote this originally...

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/comments/mod.rs`:368 on 2024-01-29 19:11_

That would be my guess. See: https://nnethercote.github.io/perf-book/compile-times.html#llvm-ir

I'm not sure "avoid monomorhpization" is the right phrasing, but I actually don't know if there is a specific word for it. I think I would probably say something like, "ensure there is only one copy of the function by writing an internal monomorphic helper." Where "monomorphic" is synonymous with "not polymorphic."

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/range.rs`:20 on 2024-01-29 19:12_

Out of curiosity, why not `use crate::{comments::Comments, prelude::*, format_module_source, ...};` instead? (And similar above.)

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/range.rs`:190 on 2024-01-29 19:33_

Do you know why we use visitors instead of "regular" recursion? (Is it to avoid recursion causing stack overflows?)

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/range.rs`:795 on 2024-01-29 19:38_

I might just call this `indent_level` or `source_indent_level`.

---

_@BurntSushi approved on 2024-01-29 19:43_

Amazing! I tried looking for bugs in the string manipulation part, but none stood out to me. :)

One thing I might suggest is trying a commit-by-commit breakdown of big PRs like this. It would let you keep it to a single PR, but might make it easier to review. The commit messages can be used to contextualize each change.

---

_@MichaReiser reviewed on 2024-01-29 20:51_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/range.rs`:20 on 2024-01-29 20:51_

I rarely write the use statements by hand. This is what RustRover generates (after running Organize imports. I would need to look into the settings to see if I can fine tune the behavior 

I personally would prefer one use per line (and sorted) because I find merging use conflicts rather annoying and my IDE collapses the use statements anyway. 

To sum it up: it's not intentional. I don't write use statements by hand

---

_@MichaReiser reviewed on 2024-01-29 20:52_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/range_formatting/docstring_code_examples.py`:79 on 2024-01-29 20:52_

Oh it was. It saved me a good amount of time to specify the edge cases

Did you notice that you wrote zeba... or is it intentional?

---

_@MichaReiser reviewed on 2024-01-29 20:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/range.rs`:190 on 2024-01-29 20:55_

How do you mean? The visitors use recursion internally. I doubt that the visitor pattern (as used here) prevents stack overflows.

How would you write this with recursion without using a visitor? Would you use a queue with the nodes to visit and loop over it until it's empty (and push the node's children to the front while visiting?)


---

_Comment by @MichaReiser on 2024-01-29 20:58_


> One thing I might suggest is trying a commit-by-commit breakdown of big PRs like this. It would let you keep it to a single PR, but might make it easier to review. The commit messages can be used to contextualize each change.

I fear that I don't have the git superpowers to do it 😕 But I could have done an interactive rebase at the end to move e.g the printer changes to their own commit (make it the first commit)


---

_@konstin approved on 2024-01-30 09:55_

---

_@BurntSushi reviewed on 2024-01-30 14:20_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/range_formatting/docstring_code_examples.py`:79 on 2024-01-30 14:20_

Hah no it wasn't. Whoops. Although the way DuBois (the villain in Madagascar 3) pronounces "zebra" does actually sound like "zeba." Hehe.

---

_@MichaReiser reviewed on 2024-01-31 09:44_

---

_Review comment by @MichaReiser on `crates/ruff_formatter/src/lib.rs`:449 on 2024-01-31 09:44_

That's it but it's gone now ;)

---

_@MichaReiser reviewed on 2024-01-31 09:48_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/range.rs`:353 on 2024-01-31 09:48_

Code pilot is no good add spaces

---

_Renamed from "Range formatting" to "Range formatting API" by @MichaReiser on 2024-01-31 10:13_

---

_Merged by @MichaReiser on 2024-01-31 10:13_

---

_Closed by @MichaReiser on 2024-01-31 10:13_

---

_Branch deleted on 2024-01-31 10:13_

---
