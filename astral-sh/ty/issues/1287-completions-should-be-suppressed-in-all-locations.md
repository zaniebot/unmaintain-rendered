```yaml
number: 1287
title: Completions should be suppressed in all locations that are unambiguously definitions
type: issue
state: closed
author: pyscripter
labels:
  - server
  - completions
assignees: []
created_at: 2025-09-30T15:45:16Z
updated_at: 2025-11-14T21:42:59Z
url: https://github.com/astral-sh/ty/issues/1287
synced_at: 2026-01-10T02:06:25Z
```

# Completions should be suppressed in all locations that are unambiguously definitions

---

_Issue opened by @pyscripter on 2025-09-30 15:45_

### Summary

The following is based on my experience with using the ty playground. (^ is the cursor position)

In
```python
import numpy as ^
```
I get a list of modules on the python path.  Instead I should be getting an empty list as for example with Jedi.


### Version

_No response_

---

_Renamed from "Completion of from/import statements  after the as keyword" to "Completion of import statements  after the as keyword" by @pyscripter on 2025-09-30 15:45_

---

_Label `server` added by @AlexWaygood on 2025-09-30 15:47_

---

_Label `completions` added by @AlexWaygood on 2025-09-30 15:48_

---

_Renamed from "Completion of import statements  after the as keyword" to "Completions should be suppressed for `import typing as <CURSOR>`" by @AlexWaygood on 2025-11-13 13:23_

---

_Renamed from "Completions should be suppressed for `import typing as <CURSOR>`" to "Completions should be suppressed after `import typing as <CURSOR>`" by @AlexWaygood on 2025-11-13 13:23_

---

_Comment by @AlexWaygood on 2025-11-13 13:37_

By same principle:

- `for x in range(42)` creates a definition for the `x` symbol, so there shouldn't be any completion suggestions at the point `for x<CURSOR>`. Currently there are. 
- Typing a parameter name in a function declaration (`def f(xx<CURSOR>)`) should not trigger any autocomplete suggestions
- Typing a type parameter name in a function declaration (`def f[T<CURSOR>]`) should not trigger any autocomplete suggestions
- `with <EXPR> as b<CURSOR>` should not trigger any autocomplete suggestions
- `case <PATTERN> as b<CURSOR>` should not trigger any autocomplete suggestions
- `except <EXPRESSION> as b<CURSOR>` should not trigger any autocomplete suggestions

<details>
<summary>Screenshots</summary>

<img width="1138" height="664" alt="Image" src="https://github.com/user-attachments/assets/c58f4260-5688-47b2-aaf5-b15bc0bd9d6c" />

<img width="1340" height="842" alt="Image" src="https://github.com/user-attachments/assets/f140ec37-2e4f-4e20-be6e-f297713f6c64" />

<img width="1278" height="310" alt="Image" src="https://github.com/user-attachments/assets/4a11be21-8b8f-47cd-9bdc-933cc0ee07dc" />

<img width="1574" height="354" alt="Image" src="https://github.com/user-attachments/assets/8d26726d-88f6-4773-95ae-de79c9814cc0" />

<img width="1202" height="682" alt="Image" src="https://github.com/user-attachments/assets/a01d5a42-9b2f-46a7-a0ce-7c690dc02a72" />

<img width="1552" height="724" alt="Image" src="https://github.com/user-attachments/assets/0d44815b-23cf-4823-8f7a-a81d926ea8e2" />

</details>

---

_Renamed from "Completions should be suppressed after `import typing as <CURSOR>`" to "Completions should be suppressed in all locations that are unambiguously definitions" by @AlexWaygood on 2025-11-13 13:37_

---

_Comment by @pyscripter on 2025-11-13 13:43_

Also  

import numpy ^  (offer as as the only option)
for x ^  (offer in as the only option) 

---

_Added to milestone `Stable` by @MichaReiser on 2025-11-14 08:09_

---

_Assigned to @BurntSushi by @BurntSushi on 2025-11-14 17:44_

---

_Closed by @BurntSushi on 2025-11-14 19:21_

---

_Comment by @pyscripter on 2025-11-14 21:32_

Nice but it does not cover all cases discussed here:

- import numpy ^  (offer as as the only option)
-  for ^ (offer no definition)
- for x ^  (offer in as the only option) 
- Typing a parameter name in a function declaration (def f(xx<CURSOR>)) should not trigger any autocomplete suggestions
- Typing a type parameter name in a function declaration (def f[T<CURSOR>]) should not trigger any autocomplete suggestions

Shouldn't 

```rust
fn is_in_definition_place(db: &dyn Db, file: File, tokens: &[Token], typed: Option<&str>) -> bool {
    fn is_definition_token(token: &Token) -> bool {
        matches!(
            token.kind(),
            TokenKind::Def | TokenKind::Class | TokenKind::Type | TokenKind::As
        )
    }
```

include TokenKind::For?

---

_Comment by @BurntSushi on 2025-11-14 21:42_

I opened https://github.com/astral-sh/ty/issues/1563 to track those.

Context aware keyword suggestions are a separate thing. I opened https://github.com/astral-sh/ty/issues/1568 to track that.

---

_Comment by @pyscripter on 2025-11-14 21:42_

But for would fit nicely in the this commit.

---
