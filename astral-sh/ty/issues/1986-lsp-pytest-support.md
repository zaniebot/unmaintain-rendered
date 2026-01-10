---
number: 1986
title: "LSP: pytest support"
type: issue
state: open
author: MichaReiser
labels:
  - server
assignees: []
created_at: 2025-12-17T08:45:14Z
updated_at: 2026-01-01T01:51:42Z
url: https://github.com/astral-sh/ty/issues/1986
synced_at: 2026-01-10T01:51:14Z
---

# LSP: pytest support

---

_Issue opened by @MichaReiser on 2025-12-17 08:45_

* Code lense to run test
* List tests in the test explorer
* Fixtures support?

---

_Added to milestone `Stable` by @MichaReiser on 2025-12-17 08:45_

---

_Label `server` added by @MichaReiser on 2025-12-17 08:45_

---

_Comment by @maxhully on 2025-12-17 21:01_

Chiming in as a user: "go to definition" for pytest fixtures would be great. Thanks for building such an awesome tool!

---

_Comment by @olzhasar on 2025-12-29 20:57_

Second fixtures support.
"Go to definition" alone would be amazing enough, but to make things even more incredible:
- Infering fixture types in tests based on their declaration, e.g.:
```py3
@pytest.fixture
def some_number() -> int:
    return some_calculation()

def test_foo(some_number):  # -> treat some_number as int in this test 
```
- Finding fixtures [references](https://microsoft.github.io/language-server-protocol/specifications/lsp/3.17/specification/#textDocument_references) - tests using specific fixture

If implemented, this would mitigate the main pitfalls of `pytest`, at least for me :)

---

_Comment by @qdewaghe on 2026-01-01 01:51_

Some CodeLens support would be nice as well. I'd love for ty to support [inline-snapshot](https://15r10nk.github.io/inline-snapshot/latest/) the same way rust-analyzer supports insta.

<img width="401" height="65" alt="Image" src="https://github.com/user-attachments/assets/3d484080-af22-43a4-9210-734122fbef2d" />



---
