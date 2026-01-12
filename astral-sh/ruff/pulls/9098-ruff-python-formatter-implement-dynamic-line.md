```yaml
number: 9098
title: "ruff_python_formatter: implement \"dynamic\" line width mode for docstring code formatting"
type: pull_request
state: merged
author: BurntSushi
labels:
  - formatter
assignees: []
merged: true
base: main
head: ag/fmt/dynamic
created_at: 2023-12-11T18:57:06Z
updated_at: 2023-12-12T14:58:44Z
url: https://github.com/astral-sh/ruff/pull/9098
synced_at: 2026-01-10T23:40:55Z
```

# ruff_python_formatter: implement "dynamic" line width mode for docstring code formatting

---

_Pull request opened by @BurntSushi on 2023-12-11 18:57_

## Summary

This PR changes the internal `docstring-code-line-width` setting to additionally accept a string value `dynamic`. When `dynamic` is set, the line width is dynamically adjusted when reformatting code snippets in docstrings based on the indent level of the docstring. The result is that the reformatted lines from the code snippet should not exceed the "global" line width configuration for the surrounding source.

This PR does not change the default behavior, although I suspect the default should probably be `dynamic`.

Closes #8855

## Test Plan

I added a new configuration to the existing docstring code tests and also added a new set of tests dedicated to the new `dynamic` mode.


---

_Review requested from @MichaReiser by @BurntSushi on 2023-12-11 18:57_

---

_Review requested from @charliermarsh by @BurntSushi on 2023-12-11 18:57_

---

_Comment by @github-actions[bot] on 2023-12-11 19:10_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Label `formatter` added by @charliermarsh on 2023-12-11 20:48_

---

_@charliermarsh reviewed on 2023-12-11 23:24_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples_dynamic_line_width.py`:6 on 2023-12-11 23:24_

I think we should add a test for the case in which the docstring contents are under-indented, like:

```python
def repeated():
    """
    First line.
```py
class Abcdefghijklmopqrstuvwxyz(Abc, Def, Ghi, Jkl, Mno, Pqr, Stu, Vwx, Yz, A1, A2, A3, A4, A5):
    def abcdefghijklmnopqrstuvwxyz(self, abc, ddef, ghi, jkl, mno, pqr, stu, vwx, yz, a1, a2, a3, a4):
        def abcdefghijklmnopqrstuvwxyz(abc, ddef, ghi, jkl, mno, pqr, stu, vwx, yz, a1, a2, a3, a4):
            # For 4 space indents, this is just one character shy of
            # tripping the default line width of 88. So it should not be
            # wrapped.
            print(abc, ddef, ghi, jkl, mno, pqr, stu, vwx, yz, a1, a2, a3, a4, a567)
            return 5
        self.x = doit( 5 )
    """
```

---

_@charliermarsh approved on 2023-12-11 23:25_

Looks great!

---

_@charliermarsh reviewed on 2023-12-11 23:26_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples_dynamic_line_width.py`:6 on 2023-12-11 23:26_

In this case, we end up formatting like:

```python
def repeated():
    """
        First line.
    ```py
    class Abcdefghijklmopqrstuvwxyz(Abc, Def, Ghi, Jkl, Mno, Pqr, Stu, Vwx, Yz, A1, A2, A3, A4, A5):
        def abcdefghijklmnopqrstuvwxyz(self, abc, ddef, ghi, jkl, mno, pqr, stu, vwx, yz, a1, a2, a3, a4):
            def abcdefghijklmnopqrstuvwxyz(abc, ddef, ghi, jkl, mno, pqr, stu, vwx, yz, a1, a2, a3, a4):
                # For 4 space indents, this is just one character shy of
                # tripping the default line width of 88. So it should not be
                # wrapped.
                print(abc, ddef, ghi, jkl, mno, pqr, stu, vwx, yz, a1, a2, a3, a4, a567)
                return 5
            self.x = doit( 5 )
    """
```

That is, we indent relative to the minimally-indented line.

---

_@charliermarsh reviewed on 2023-12-11 23:26_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples_dynamic_line_width.py`:6 on 2023-12-11 23:26_

(I suspect the code handles this correctly, but it's worth testing.)

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/context.rs`:228 on 2023-12-11 23:27_

The "number of ASCII space characters" is, I guess, a little less clear when talking about tabs. Like, to determine the number of tabs, it'd be the number of ASCII space characters divided by the tab width? But IIRC we don't support tabs within docstrings anyway, so perhaps moot here.

---

_@charliermarsh reviewed on 2023-12-11 23:27_

---

_@BurntSushi reviewed on 2023-12-12 00:15_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/context.rs`:228 on 2023-12-12 00:15_

> But IIRC we don't support tabs within docstrings anyway,

Yeah that's exactly why I used the naming I did. I felt like if I used more generic naming, it might be too easy to accidentally use this method in an inappropriate way.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/context.rs`:238 on 2023-12-12 02:37_

Nit: We can probably use a `u32` here considering that we don't support files larger than 32 bytes or even `u16` because that's the most the formatter `Printer` supports.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/context.rs`:248 on 2023-12-12 02:38_

Nit: To me the semantic of `next` for an indent level isn't immediately clear. I would find`increment` and `decrement` easier to understand.

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/statement/suite.rs`:75 on 2023-12-12 02:40_

That looks too easy. 


---

_@MichaReiser approved on 2023-12-12 02:42_

---

_@BurntSushi reviewed on 2023-12-12 13:36_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/statement/suite.rs`:75 on 2023-12-12 13:36_

That's what I thought. Hah.

And I had to do a little trickery to make this work. In particular, asking for a `DerefMut<Target=impl Buffer>` instead of an `impl Buffer` in order to make it composable with `WithNodeLevel`. I pondered for a bit on a better design, but there were multiple choices and it wasn't obvious to me which path to go.

---

_@BurntSushi reviewed on 2023-12-12 13:42_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/src/context.rs`:238 on 2023-12-12 13:42_

Yeah I was trying to figure out what type to use here. I noticed other similar types use `u32` or `u16` or whatever.

I think I do favor using fixed size types here because it provides more of a speed-bump to make sure you're careful about how they're used. On the other hand, these values do ultimately seem to get used for indexing memory, so I also find `usize` to be reasonable as well.

I'll switch this over to a `u16`. Given that it needs to do arithmetic with other `u16` values, I think that seems fine.

---

_@BurntSushi reviewed on 2023-12-12 14:12_

---

_Review comment by @BurntSushi on `crates/ruff_python_formatter/resources/test/fixtures/ruff/docstring_code_examples_dynamic_line_width.py`:6 on 2023-12-12 14:12_

Ah yeah nice test. I added that and another one where a line, _after its indentation has been fixed_, just barely exceeds the limit. It wraps as we might expect.

---

_Merged by @BurntSushi on 2023-12-12 14:58_

---

_Closed by @BurntSushi on 2023-12-12 14:58_

---

_Branch deleted on 2023-12-12 14:58_

---
