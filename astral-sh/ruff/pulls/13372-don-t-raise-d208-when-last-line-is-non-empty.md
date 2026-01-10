```yaml
number: 13372
title: "Don't raise `D208` when last line is non-empty"
type: pull_request
state: merged
author: ukyen8
labels:
  - bug
  - rule
assignees: []
merged: true
base: main
head: fix-D208-indent
created_at: 2024-09-16T21:31:43Z
updated_at: 2024-09-26T12:53:21Z
url: https://github.com/astral-sh/ruff/pull/13372
synced_at: 2026-01-10T20:59:36Z
```

# Don't raise `D208` when last line is non-empty

---

_Pull request opened by @ukyen8 on 2024-09-16 21:31_

…D208)

<!--
Thank you for contributing to Ruff! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

This PR is to fix an unexpected change of the D208 rule when trailing quotes are not on a separate line.

Related issue: https://github.com/astral-sh/ruff/issues/13260

## Test Plan

Add more test cases to D208.py and update the snapshot file.

---

_Comment by @github-actions[bot] on 2024-09-16 22:11_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --no-preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/0fdcd8b27e6d6ba70f8f38b4a033c61555af92bd/scripts/cancel_github_workflows.py#L69'>scripts/cancel_github_workflows.py:69:1:</a> D208 [*] Docstring is over-indented
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D208 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>

### Linter (preview)
ℹ️ ecosystem check **detected linter changes**. (+0 -1 violations, +0 -0 fixes in 1 projects; 53 projects unchanged)

<details><summary><a href="https://github.com/apache/superset">apache/superset</a> (+0 -1 violations, +0 -0 fixes)</summary>
<p>
<pre>ruff check --no-cache --exit-zero --ignore RUF9 --output-format concise --preview --select ALL</pre>
</p>
<p>

<pre>
- <a href='https://github.com/apache/superset/blob/0fdcd8b27e6d6ba70f8f38b4a033c61555af92bd/scripts/cancel_github_workflows.py#L69'>scripts/cancel_github_workflows.py:69:1:</a> D208 [*] Docstring is over-indented
</pre>

</p>
</details>
<details><summary>Changes by rule (1 rules affected)</summary>
<p>

| code | total | + violation | - violation | + fix | - fix |
| ---- | ------- | --------- | -------- | ----- | ---- |
| D208 | 1 | 0 | 1 | 0 | 0 |

</p>
</details>




---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:298 on 2024-09-17 08:45_

I think this might panic if there are fewer than 3 lines because it then results in an out of bound access.

We should also consider the case where there's a number of blank lines between the last and previous line


```python
def right_indent_quotes_same_line(a: int) -> None:
    """Foo.
    Parameters
    ----------
    a : int





        A parameter."""
    pass
```


---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:297 on 2024-09-17 08:48_

I think we can simplify this by testing if the `line_indent.text_len() == `last.text_len()`. If the condition is true, then the last line is either all whitespace or empty 

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:303 on 2024-09-17 08:53_

Not sure about this, but could we possibly use `over_indented_size`? 

---

_@MichaReiser requested changes on 2024-09-17 08:53_

Thanks. There are a few extra cases that we should consider but this is heading in a good direction

---

_Label `bug` added by @MichaReiser on 2024-09-17 08:54_

---

_Label `rule` added by @MichaReiser on 2024-09-17 08:54_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:300 on 2024-09-20 08:46_

Let's avoid overriding the outer variable and instead shadow it
```suggestion
                    let over_indented_size =
                        std::cmp::min(line_indent_size - docstring_indent_size, over_indented_size);
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:311 on 2024-09-20 08:48_

Nit: We can avoid mutating the variable by using let
```suggestion
                // The trailing quotes are not on a separate line.
                let offset_len = if line_indent.text_len() != last.text_len() {
                    over_indented_size =
                        std::cmp::min(line_indent_size - docstring_indent_size, over_indented_size);
                    match over_indented_size.cmp(&docstring_indent_size) {
                        Ordering::Equal => {
                            return;
                        }
                        Ordering::Less => over_indented_size,
                        Ordering::Greater => line_indent_size - over_indented_size - docstring_indent_size
                    }
                } else {
                	line_indent_size
              	};
```

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:312 on 2024-09-20 08:53_

Could you add a comment here that explains when the indent sizes are equal and why we return in that case? I'm having a hard time understanding what's happening, and a comment might also be useful for future readers.

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:316 on 2024-09-20 08:54_

The use of `offset_len` here as a byte offset is incorrect because `line_indent_size` and `docstring_indent_size` are both character offsets. 

We'll need to do something similar to 

https://github.com/astral-sh/ruff/blob/14dcae1e9a68332daab459792bab2aaa84445f69/crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs#L275-L281

---

_@MichaReiser requested changes on 2024-09-20 08:55_

There's one more subtle bug that needs fixing. But we're almost there :)

---

_@ukyen8 reviewed on 2024-09-23 21:43_

---

_Review comment by @ukyen8 on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:311 on 2024-09-23 21:43_

This is helpful, thanks!

---

_@ukyen8 reviewed on 2024-09-23 21:44_

---

_Review comment by @ukyen8 on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:316 on 2024-09-23 21:44_

Thanks! I was struggling to find a way to convert it to character offsets.

---

_Review requested from @MichaReiser by @ukyen8 on 2024-09-24 07:47_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:312 on 2024-09-24 11:51_

Nice thank you. The examples are very helpful!

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:316 on 2024-09-24 11:55_

The documentation here doesn't match the code's behavior. It isn't returning `docstring.indentation` and 
I'm uncertain if this case is also about over indent, considering that `Ordering::Greater` also handles an over indent.

---

_@MichaReiser reviewed on 2024-09-24 11:57_

---

_@ukyen8 reviewed on 2024-09-24 21:46_

---

_Review comment by @ukyen8 on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:316 on 2024-09-24 21:46_

Thanks, I've rephrased the comments. I think it is still an over-indentation case, we return `over_indented_size ` here, which is the result of `line_indent_size - docstring_indent_size` and `line_indent_size > docstring_indent_size`.

---

_Review requested from @MichaReiser by @ukyen8 on 2024-09-24 21:52_

---

_@MichaReiser reviewed on 2024-09-25 08:48_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:301 on 2024-09-25 08:48_

I'm sorry but I'm still confused (about the comments and the code). It can very well be that it's just me not understanding the code.

I'm confused about the comparison of `over_indent_size` with `docstring_indent_size` in the case when `over_indent_size = line_indent_size - docstring_indent_size`. 

```python
def test():
    """
    Abcd
        line"""
```

In this case:

* `line_indent_size`:  8
* `docstring_indent_size`: 4
* outer `over_indented_size`: `usize::MAX`?
* local `over_indented_size: 4 (`8 - 4`)

That means the code now takes the equal branch because `over_indent_size == docstring_indent_size` but this code should be flagged. 

 


---

_@ukyen8 reviewed on 2024-09-25 09:40_

---

_Review comment by @ukyen8 on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:301 on 2024-09-25 09:40_

You are correct that this case should be flagged, but my current approach will overlook it. I am not pretty sure how we can handle both cases if the trailing quotes are on the same line unless we have a way to know the pattern of the previous line. Do you have any idea?

```
def right_indent_quotes_same_line(a: int) -> None:
    """Foo.

    Parameters
    ----------
    a : int
        A parameter."""
    pass
```
```
def test():
    """
    Abcd
        line"""
```        

---

_@MichaReiser reviewed on 2024-09-26 08:10_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:246 on 2024-09-26 08:10_

This is the main fix where we only exclude the last line if it is all empty

---

_@MichaReiser reviewed on 2024-09-26 08:11_

---

_Review comment by @MichaReiser on `crates/ruff_linter/src/rules/pydocstyle/rules/indent.rs`:325 on 2024-09-26 08:11_

This requires the same logic. Only apply the last line logic when it is the last line and it only contains the quotes

---

_Renamed from "Fix indentation error when trailing quotes are not on separate line (…" to "Don't raise `D208` when last line is non-empty" by @MichaReiser on 2024-09-26 08:11_

---

_Comment by @MichaReiser on 2024-09-26 08:12_

I pushed a commit that now removes the `D208` if the last line is not all empty and at least one docstring line is correctly indented. This aligns the behavior with when the closing quotes are on their own line. 

I added a test that demonstrates the behavior. 

I also took this as a chance to remove the, IMO, unnecessary `lines.collect` call (resulting in 1% perf improvement)

---

_Review requested from @charliermarsh by @MichaReiser on 2024-09-26 08:19_

---

_@charliermarsh approved on 2024-09-26 12:51_

---

_Merged by @MichaReiser on 2024-09-26 12:53_

---

_Closed by @MichaReiser on 2024-09-26 12:53_

---
