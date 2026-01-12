```yaml
number: 9030
title: "ruff_python_formatter: support reformatting Markdown code blocks"
type: pull_request
state: merged
author: BurntSushi
labels:
  - docstring
  - formatter
assignees: []
merged: true
base: main
head: ag/fmt/markdown
created_at: 2023-12-06T22:32:28Z
updated_at: 2023-12-07T21:57:05Z
url: https://github.com/astral-sh/ruff/pull/9030
synced_at: 2026-01-12T15:55:27Z
```

# ruff_python_formatter: support reformatting Markdown code blocks

---

_@BurntSushi_

(This is not possible to actually use until https://github.com/astral-sh/ruff/pull/8854 is merged.)

This commit slots in support for formatting Markdown fenced code blocks[1]. With the refactoring done for reStructuredText previously, this ended up being pretty easy to add. Markdown code blocks are also quite a bit easier to parse and recognize correctly.

One point of contention in #8860 is whether to assume that unlabeled Markdown code fences are Python or not by default. In this PR, we make such an assumption. This follows what `rustdoc` does. The mitigation here is that if an unlabeled code block isn't Python, then it probably won't parse as Python. And we'll end up skipping it. So in the vast majority of cases, the worst thing that can happen is a little bit of wasted work.

Closes #8860

[1]: https://spec.commonmark.org/0.30/#fenced-code-blocks

---

_Label `docstring` added by @BurntSushi on 2023-12-06 22:32_

---

_Label `formatter` added by @BurntSushi on 2023-12-06 22:32_

---

_Added to milestone `Formatter: Stable` by @BurntSushi on 2023-12-06 22:32_

---

_Review requested from @MichaReiser by @BurntSushi on 2023-12-06 22:35_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-06 22:35_

---

_Comment by @github-actions[bot] on 2023-12-06 22:47_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1212 on 2023-12-07 01:07_

So this is, e.g., the number of backticks?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1248 on 2023-12-07 01:08_

Is there a source for this regex or was it written by hand for these purposes?

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1248 on 2023-12-07 01:10_

Is there any sense in checking for the triple quotes / tildes out-of-band before running the regex, since the vast majority of lines won't contain them? Or would you expect (wink wink) regex to perform equivalently-or-better?

Edit: hah, nevermind, you already have this above 🤦 

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1350 on 2023-12-07 01:10_

Nit: uncloded -> unclosed

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1305 on 2023-12-07 01:12_

What is this case? The code is under-indented compared to the opening of the code fence? I actually don't know what Markdown does in that case.

---

_@charliermarsh approved on 2023-12-07 01:12_

Looks great.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:1292 on 2023-12-07 02:32_

Nice test suite and I like how you documented the cases. 

Should we add a test where the code block itself contains closing quotes at a different indentation level:

```python
def test() {
	"""
  You can use this function to render markdown, even conditionally.

	```py
	if 5 == 4:
		a = """
			It supports **code blocks** 
			```py
				print("Hello World")
			```
  	"""
```

A simplified version that triggered me to make this example is that

```python
def test() {
	"""
  Code block with incorrectly placed closing fences
	
	```python
	
		 ```
	  # The above fences should not close the code block
		a = 10
	```

	Now the code block is closed
	"""

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:4 on 2023-12-07 02:33_

all of them? Is this the start of a rebellion against our pedantic rules (I consider joining :P)

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1214 on 2023-12-07 02:35_

That's a lot of supported ticks 🤣 But makes sense, using `u32` would require some annoying casts

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1321 on 2023-12-07 02:40_

Does `starts_with` work with over intended lines


```python
def test():
	"""
	A docstring
		over intended
		```python
		print("a")
		```
	"""
	
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/normalizer.rs`:86 on 2023-12-07 02:43_

I mean... markdown is a superset of HTML where almost anything is valid....

---

_@MichaReiser approved on 2023-12-07 02:44_

LGTM.

My only fear is into how many weird edge cases we'll run where we would need a full markdown parser to parse nested structures correctly. But that's something we can figure out later.

I recommend releasing this soon under the formatter Beta where we have some more wiggle room to make breaking changes if any of our assumptions were wrong.

---

_@BurntSushi reviewed on 2023-12-07 16:55_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1212 on 2023-12-07 16:55_

Yup! I've tweaked the docs to make this clearer.

---

_@BurntSushi reviewed on 2023-12-07 16:57_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1248 on 2023-12-07 16:57_

Written by hand. And indeed, I did add a "fast path," but I actually speculate that it isn't necessary. While `Regex::captures` isn't the fastest thing in the world, what isn't fast is the path that reports a match. The path that _doesn't_ report a match should be quite fast. It only has to use a slower engine to resolve capture groups if a match is detected.

But, the fast path here is very simple.

---

_@BurntSushi reviewed on 2023-12-07 17:06_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1305 on 2023-12-07 17:06_

Yeah it's a weird case. I believe it is technically valid Markdown. But when I allowed it, it led to an idempotency bug that seemed a little tricky to fix. Basically, it seemed like a bug that was probably okay to ship with, and we can prioritize fixing it based on user feedback.

Good call out though. I added this comment:

```rust
        // When a line in a Markdown fenced closed block is indented *less*
        // than the opening indent, we treat the entire block as invalid.
        //
        // I believe that code blocks of this form are actually valid Markdown
        // in some cases, but the interplay between it and our docstring
        // whitespace normalization leads to undesirable outcomes. For example,
        // if the line here is unindented out beyond the initial indent of the
        // docstring itself, then this causes the entire docstring to have
        // its indent normalized. And, at the time of writing, a subsequent
        // formatting run undoes this indentation, thus violating idempotency.
```

---

_@BurntSushi reviewed on 2023-12-07 19:01_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples.py`:1292 on 2023-12-07 19:01_

So in the first case, the `"""` ends the docstring and makes the entire thing invalid Python. I tried playing with it a bit to massage it into a more "useful" test, but couldn't get to anything I liked.

In the second case, we do actually allow the first set of closing fences to close the block. Markdown technically allows up to three spaces of indentation before the start of the fences, but I think that's a little tricky to enforce in this context given that we're in a docstring. So I ended up just deciding that Markdown fences could be indented as much as possible. This isn't as much of an issue here, since we specifically do not support (at least, not yet anyway) the sort of Markdown code block that is inserted purely by indentation and not fences. I added this as a test case though.

---

_@BurntSushi reviewed on 2023-12-07 19:04_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:4 on 2023-12-07 19:04_

Oh my heavens no, haha. No rebellion here. I sometimes add this to the top of a file when adding a new chunk of code. I just forgot to remove it.

I do find some of our Clippy lints a touch pedantic, but no strong feelings. Yet.

---

_@BurntSushi reviewed on 2023-12-07 19:06_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1214 on 2023-12-07 19:06_

Yeah I did consider this when I wrote down the data type, but didn't see a good reason to artificially restrict it. That is, I don't any issues arising from a lot of ticks that we would be unique from the more general issue of a supremely long line.

But if we wanted to restrict this, probably `u8` would be good enough. 255 ticks ought to be enough for anyone, right?

---

_@BurntSushi reviewed on 2023-12-07 19:07_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/expression/string/docstring.rs`:1321 on 2023-12-07 19:07_

Yeah that works, because we strip all leading whitespace.

Test case added. :-)

---

_@BurntSushi reviewed on 2023-12-07 19:07_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/tests/normalizer.rs`:86 on 2023-12-07 19:07_

Yeah I just mean valid fence blocks, not all of Markdown. :-)

---

_Comment by @BurntSushi on 2023-12-07 19:09_

> My only fear is into how many weird edge cases we'll run where we would need a full markdown parser to parse nested structures correctly. But that's something we can figure out later.

Yeah right, this PR only covers naked code fence blocks. It won't work with fenced code blocks inside of quote blocks for example. I do indeed feel like that's something we can add later if there's demand for it. And yeah, that may wind up requiring a full Markdown parser (`pulldown-cmark` perhaps).

> I recommend releasing this soon under the formatter Beta where we have some more wiggle room to make breaking changes if any of our assumptions were wrong.

Yeah the plan is to get this out soon. Basically want to get this merged, get #8855 fixed up and then write a blog post. :-)

---

_Comment by @BurntSushi on 2023-12-07 19:30_

Going to bring this in. As usual, if y'all have any more feedback, I'd be happy to address it in follow-up PRs.

---

_Merged by @BurntSushi on 2023-12-07 19:30_

---

_Closed by @BurntSushi on 2023-12-07 19:30_

---

_Branch deleted on 2023-12-07 19:30_

---

_Comment by @ofek on 2023-12-07 20:23_

Is it supported to have additional text after the language marker? For example: https://github.com/pypa/hatch/blob/52a81d80cd03bfb90d99a5837c778971ea0c8c22/src/hatch/env/plugin/interface.py#L23-L41

---

_Comment by @BurntSushi on 2023-12-07 21:57_

Ah yeah good catch. I did indeed specifically add support for that, but neglected to add a test. I added one in https://github.com/astral-sh/ruff/pull/9050.

---
