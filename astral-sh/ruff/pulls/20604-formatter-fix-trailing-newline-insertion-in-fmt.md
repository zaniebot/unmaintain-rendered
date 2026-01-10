```yaml
number: 20604
title: "[`formatter`] Fix trailing newline insertion in `fmt: off` regions"
type: pull_request
state: closed
author: danparizher
labels:
  - formatter
assignees: []
base: main
head: fix-19492
created_at: 2025-09-27T21:11:09Z
updated_at: 2025-11-21T15:56:27Z
url: https://github.com/astral-sh/ruff/pull/20604
synced_at: 2026-01-10T16:48:01Z
```

# [`formatter`] Fix trailing newline insertion in `fmt: off` regions

---

_Pull request opened by @danparizher on 2025-09-27 21:11_

## Summary

Fixes #19492


---

_Review requested from @MichaReiser by @danparizher on 2025-09-27 21:11_

---

_Comment by @github-actions[bot] on 2025-09-27 21:24_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `formatter` added by @MichaReiser on 2025-09-29 07:00_

---

_@MichaReiser requested changes on 2025-09-29 07:02_

Thank you. 

This will need some tests. Testing whether the source contains any `#fmt: off` is also not strict enough. E.g. 

```py
# fmt: off
a = 10
# fmt: on
```

The new line should be inserted for the case above

This might require integration into `fmt_suite` where we track the suppression state.

---

_Comment by @danparizher on 2025-09-29 22:38_

I've implemented proper suppression state tracking by using `SuppressionKind`—the behavior works correctly with real files—it adds trailing newlines for non-suppressed regions and preserves formatting for suppressed regions.

What I couldn't figure out was that the test snapshot for Test Case 3 isn't showing the expected trailing newline, even though the actual formatter behavior is correct. Wondering if this is an issue with the fixture or intended behavior.

---

_Review requested from @MichaReiser by @danparizher on 2025-09-29 22:46_

---

_@MichaReiser requested changes on 2025-09-30 06:36_

This looks better, but I think is still incorrect for:

```py
def test():
	# fmt: off
	...
```

This should only disable formatting inside the `test` function but not at the module level. 




---

_Review requested from @MichaReiser by @danparizher on 2025-09-30 22:49_

---

_@MichaReiser reviewed on 2025-10-01 06:05_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/tests/snapshots/format@fmt_on_off__function_level_suppression.py.snap`:1 on 2025-10-01 06:05_

You'll need to create different test files or you only test whatever `fmt: off` pattern comes last

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/module/mod_module.rs`:53 on 2025-10-01 06:09_

Unfortunatelly, it's more complicated than this:

```py
a = b
    # fmt: off
b = 10
```

This is a module level `fmt: off` comment

But this isn't

```py
if True:
	a = b
  # fmt: off
```


We should also support `yapf: disable` comments

https://github.com/astral-sh/ruff/blob/32be31d2f9dce7a5e23b406c5b8a338dc4d21755/crates/ruff_python_formatter/src/comments/mod.rs#L169-L171

We should also add a test for

```py
# fmt: off
```

I think the right approach is to iterate over all statements and, for each statement, get its comments (using `context.comments.leading_trailing(statement)`). Then test if the last comment is a `fmt: off` comment

---

_@MichaReiser reviewed on 2025-10-01 06:09_

---

_Review requested from @MichaReiser by @danparizher on 2025-10-01 20:47_

---

_@MichaReiser reviewed on 2025-10-02 06:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/module/mod_module.rs`:44 on 2025-10-02 06:14_

We should avoid doing this check if the source ends with a `\n`. 

---

_@MichaReiser reviewed on 2025-10-02 06:16_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/module/mod_module.rs`:32 on 2025-10-02 06:16_

I suggest restructuring the entire code to

```rust
		body.format.with_options(SuiteKind::TopLevel).fmt(f)?;

		if source.ends_width('\n') || !has_unclosed_fmt_off(f.context()) {
			hard_line_break().fmt(f)?;
		}
```

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/resources/test/fixtures/ruff/fmt_on_off/yapf_disable_module.py`:4 on 2025-10-02 06:17_

Is it intentional that this test case and `module_level_trailing_off` have a trailing newline?

---

_@MichaReiser reviewed on 2025-10-02 06:17_

---

_Comment by @danparizher on 2025-10-02 21:47_

I refactored the modules and extracted a helper. Feel free to commit directly if there's still some missing logic; I know we've been going back and forth for a bit on this one!

---

_Review requested from @MichaReiser by @danparizher on 2025-10-02 21:47_

---

_@MichaReiser reviewed on 2025-10-03 06:14_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/module/mod_module.rs`:35 on 2025-10-03 06:14_

Hmm, I'm not sure what's happening. You have to do some debugging but I noticed that none of the tests are failing when I change this to (when I revert your changes)

```suggestion
                hard_line_break().fmt(f)?;

```

It seems we're still always inserting a trailing newline

---

_@MichaReiser reviewed on 2025-10-03 06:15_

Thanks. I updated the fixture files to remove all trailing new lines to ensure they test the right thing. I also "reverted" your changes locally to verify that the tests start failing, which they don't. I'm not sure why. Can you take a look.

We should also add a test to verify that we preserve the correct number of blank lines for

```py
test
# fmt: off
# many blank lines follow


<eof>
```

or 

```py
# empty file with many blank lines
# fmt: off


<eof>
```

Hmm... it seems that Github truncates the trailing blank lines. So I added a <EOF> marker to prevent this

---

_Comment by @danparizher on 2025-10-03 20:09_

I've spent some time debugging but wasn't able to make much headway. Not too sure what's going on either; I've tried multiple iterations of the same solution.

Might take another look later.

---

_Comment by @MichaReiser on 2025-10-06 06:38_

I suspect that the statement formatting might insert a new line

---

_Comment by @MichaReiser on 2025-11-21 07:58_

Thanks for looking into this.

I'll close this PR as there hasn't been any progress in a while. Happy to review a re-submission that address the PR feedback

---

_Closed by @MichaReiser on 2025-11-21 07:58_

---

_Branch deleted on 2025-11-21 15:56_

---
