```yaml
number: 11941
title: Specify custom pragmas to be ignored for line-too-long by the formatter 
type: issue
state: open
author: getzze
labels:
  - configuration
  - formatter
assignees: []
created_at: 2024-06-19T16:30:01Z
updated_at: 2024-06-20T06:01:05Z
url: https://github.com/astral-sh/ruff/issues/11941
synced_at: 2026-01-12T15:54:51Z
```

# Specify custom pragmas to be ignored for line-too-long by the formatter 

---

_@getzze_

Some pragma comments (for instance "noqa", "type", "pylint") when put at the end of a long line do not trigger a reformatting of the line when running `ruff format`

```python
a = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  # noqa: F401
```
with `line-length=80` this line does not get reformatted.

From what I could guess, the list of ignored pragmas is hard-coded in this file:
https://github.com/astral-sh/ruff/blob/main/crates/ruff_python_trivia/src/pragmas.rs

It would be great if we could specify our own pragmas to be ignored, for instance I would need to ignore `# pragma: no cover` and `# spellchecker`.

This line will be reformatted
```python
a = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  # pragma: no cover
```

For the linter, there is the `task-tags` option in combination with `ignore-overlong-task-comments=true`, but the name is not explicit enough and it is ignored by the formatter anyway.

I currently have to add an ignored pragma before to prevent reformatting:

```python
a = "xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"  # pylint: line-too-long  # pragma: no cover
```


---

_Label `formatter` added by @AlexWaygood on 2024-06-19 17:04_

---

_Label `configuration` added by @zanieb on 2024-06-19 17:35_

---

_Comment by @MichaReiser on 2024-06-20 06:01_

Yeah I think I'm open to this kind of configuration as it doesn't change the formatting itself, it just allows customizing what the formatter considers a pragma

---
