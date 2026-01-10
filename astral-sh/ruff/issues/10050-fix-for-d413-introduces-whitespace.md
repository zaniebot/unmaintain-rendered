```yaml
number: 10050
title: "Fix for `D413` introduces whitespace"
type: issue
state: closed
author: jhossbach
labels:
  - bug
  - docstring
  - fixes
assignees: []
created_at: 2024-02-19T13:22:05Z
updated_at: 2024-02-29T13:29:41Z
url: https://github.com/astral-sh/ruff/issues/10050
synced_at: 2026-01-10T11:09:52Z
```

# Fix for `D413` introduces whitespace

---

_Issue opened by @jhossbach on 2024-02-19 13:22_

Minimal `test.py`:
```python
def test_func():
    """Some test function.

    Returns
    -------
    None
    """
    pass
```

Fixing with `ruff --isolated --select="D413" --fix test.py` adds unnecessary whitespace (`W293`)
```diff
--- a/test.py
+++ b/test.py
@@ -4,5 +4,6 @@ def test_func():
     Returns
     -------
     None
+    
     """
     pass
```


---

_Label `bug` added by @AlexWaygood on 2024-02-19 14:22_

---

_Label `docstring` added by @AlexWaygood on 2024-02-19 14:22_

---

_Label `fixes` added by @AlexWaygood on 2024-02-19 14:22_

---

_Comment by @jusexton on 2024-02-21 09:18_

I took a quick look at this and the culprit seems to be here:
https://github.com/astral-sh/ruff/blob/175c266de30c159b69308346cb84e51134ac529e/crates/ruff_linter/src/rules/pydocstyle/rules/sections.rs#L1667-L1683

Instead of the fix "introducing" whitespace it is more so leaving behind the original indentation space which obviously violates ([W293](https://www.flake8rules.com/rules/W293.html)). In this scenario I would imagine the original indentation will need to be deleted before inserting the new line and new indentation. Below is an initial attempt at addressing this problem. Let me know if this is a good solution and I would be glad to submit a pull request.

```rust
let fix = match num_blank_lines {
    0 => Fix::safe_edit(Edit::insertion(
        format!("{}{}", line_end.repeat(2), docstring.indentation),
        context.end(),
    )),
    1 => {
        let del_start = context.end() - TextSize::from(docstring.indentation.len() as u32);
        let rest = [Edit::insertion(
            format!("{}{}", line_end, docstring.indentation),
            context.end(),
        )];
        Fix::safe_edits(Edit::deletion(del_start, context.end()), rest)
    }
    _ => unreachable!(),
};
diagnostic.set_fix(fix);
```

---

_Comment by @MichaReiser on 2024-02-22 15:24_

@jusexton this looks about right, although it would be nice if we don't have to branch on the existing empty lines. I also think that this won't work if the line has more whitespace than just the indent?

```python
def test_func():
    """Some test function.

    Returns
    -------
    None
           """
    pass
```

Could we maybe use the range information in `context.following_lines()` to determine the offset?



---

_Comment by @jusexton on 2024-02-29 07:20_

@MichaReiser Thanks for the help. A PR has been created making use of `context.following_lines()` to determine the deletion length instead of `docstring.indentation`. I also attempted removing all branching but a check still seems to be required around whether the last line is whitespace or not. Let me know if there is a better way! 

---

_Closed by @MichaReiser on 2024-02-29 13:29_

---
