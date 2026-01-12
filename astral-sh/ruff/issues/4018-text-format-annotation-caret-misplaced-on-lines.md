```yaml
number: 4018
title: "Text format: Annotation Caret misplaced on lines with tabs"
type: issue
state: closed
author: MichaReiser
labels:
  - bug
assignees: []
created_at: 2023-04-19T06:40:18Z
updated_at: 2023-05-02T13:14:04Z
url: https://github.com/astral-sh/ruff/issues/4018
synced_at: 2026-01-12T15:54:44Z
```

# Text format: Annotation Caret misplaced on lines with tabs

---

_@MichaReiser_

The `TextEmitter` misplaces the position caret `^` if the output string contains any tab characters. 

```
/home/micha/astral/test/test.py:4:92: E501 Line too long (90 > 88 characters)
  |
4 |     def bind_keys(self):
5 | 		for key in self.operations:
6 | 			self.window.bind(key, lambda event, operator=key: self.append_operator_too_long(operator))
  |                                                                                         ^^ E501
  |

Found 1 error.
```

I assume this is because the rendered width of the tab character depends on the terminal. 

## Proposed solution

Replace tabs (and other invisible characters) before rendering. See https://github.com/rome/tools/blob/af2cfbe370f29baea0277c3c0814f108e22af54e/crates/rome_diagnostics/src/display/frame.rs#L394-L406 for inspiration (Rome also uses a heuristic to decide when to show invisible characters). 

## Reproduction

Input file

```py
class Test:
    def bind_keys(self):
		for key in self.operations:
			self.window.bind(key, lambda event, operator=key: self.append_operator_too_long(operator))
```

```sh
cargo run --bin ruff -- check --no-cache --show-source --select E501 ../test/test.py --isolated
```

---

_Label `bug` added by @MichaReiser on 2023-04-19 06:40_

---

_Assigned to @MichaReiser by @MichaReiser on 2023-04-22 13:43_

---

_Closed by @MichaReiser on 2023-05-02 13:14_

---
