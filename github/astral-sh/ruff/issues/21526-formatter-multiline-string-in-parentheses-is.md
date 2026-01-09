---
number: 21526
title: "Formatter: Multiline string in parentheses is formatted in one line when fitting line length (Black does not)"
type: issue
state: closed
author: KerberosMorphy
labels: []
assignees: []
created_at: 2025-11-19T14:28:59Z
updated_at: 2025-11-19T16:13:40Z
url: https://github.com/astral-sh/ruff/issues/21526
synced_at: 2026-01-07T13:12:16-06:00
---

# Formatter: Multiline string in parentheses is formatted in one line when fitting line length (Black does not)

---

_Issue opened by @KerberosMorphy on 2025-11-19 14:28_

### Summary

For better visibility, my teams often write string in multiline by putting them inside parentheses. We are currently switching from black to ruff format but ruff have a different comportment than blacks on that matter and we lose in liability.

## Configuration

```toml
[tool.ruff]
line-length = 110
required-version = "0.14.5"
```

## Example 1

### Before formatting

```python
test_xml = (
    "<parent>"
    "<element1>1</element1>"
    "<element1>12</element1>"
    "<element2>2</element2>"
    "</parent>"
)
```

### Black

```python
test_xml = (
    "<parent>"
    "<element1>1</element1>"
    "<element1>12</element1>"
    "<element2>2</element2>"
    "</parent>"
)
```

### Ruff Format

```python
test_xml = "<parent><element1>1</element1><element1>12</element1><element2>2</element2></parent>"
```

## Example 2

### Before formatting

```python
first_name = "John"
last_name = "Wick"
sequence = 42

nameid = (
    f"{first_name.replace(" ", "").lower()[:2]}"
    f"{lase_name.replace(" ", "").lower()[:3]}"
    f"{sequence[:3]}"
)
```

### Black

```python
first_name = "John"
last_name = "Wick"
sequence = 42

nameid = (
    f"{first_name.replace(" ", "").lower()[:2]}"
    f"{lase_name.replace(" ", "").lower()[:3]}"
    f"{sequence[:3]}"
)
```

### Ruff Format

```python
first_name = "John"
last_name = "Wick"
sequence = 42

nameid = f"{first_name.replace(' ', '').lower()[:2]}{lase_name.replace(' ', '').lower()[:3]}{sequence[:3]}"
```

### Version

ruff 0.14.5

---

_Comment by @MichaReiser on 2025-11-19 16:13_

Thanks for the detailed write up. I think this is the same as https://github.com/astral-sh/ruff/issues/18436

Unfortunately, in your case, the heuristic of testing for a trailing `\n` doesn't work because you want everything on a single line at runtime; you just want to write it over multiple lines in your code. 

For now, the only work-arounds are to:

Add a trailing comment to any string literal

```py
a = (
	"test" # don't join!
	" more"
)
```

or to use `# fmt: skip`

```py
a = (
	"test"
	"more"
) # fmt: skip
```

If this doesn't work for you, I suggest upvoting https://github.com/astral-sh/ruff/issues/18436. It will help us prioritize the option


---

_Closed by @MichaReiser on 2025-11-19 16:13_

---
