```yaml
number: 2478
title: "Feature request: Signature help: add support for annotated-doc (FastAPI)"
type: issue
state: open
author: tiangolo
labels:
  - server
assignees: []
created_at: 2026-01-13T09:57:27Z
updated_at: 2026-01-13T10:15:57Z
url: https://github.com/astral-sh/ty/issues/2478
synced_at: 2026-01-13T10:29:59Z
```

# Feature request: Signature help: add support for annotated-doc (FastAPI)

---

_@tiangolo_

I love ty! 🫶 

---

I'm not sure if this is the right repo or if it should be on the main ty repo.

## Description

I would like ty to provide better support for the **Signature help** (I think that's the name?) for FastAPI's methods and classes.

## Annotated Doc

So, what I'm asking is actually support for annotated-doc.

In FastAPI, I document parameters/arguments in structured Python instead of a micro syntax in docstrings, I use: https://github.com/fastapi/annotated-doc/ instead of numpydoc, sphinx, or other things.

The code looks like this:

```Python
from typing import Annotated

from annotated_doc import Doc

def hi(name: Annotated[str, Doc("Who to say hi to")]) -> None:
    print(f"Hi, {name}!")
```

It is supported by mkdocstrings for the online docs, but currently, no editor or language server supports it.

Part of the idea with annotated-doc was that it would be relatively simple for tooling to add support for it, as all the data is in Python syntax, would be extracted from the AST, and so on, and rendered with Markdown.

## Preview

This FastAPI code:

```Python
from fastapi import FastAPI

app = FastAPI()


@app.get("/{item_id}")
async def read_item(item_id: int):
    return {"item_id": item_id, "name": f"Item {item_id}"}
```

When triggering signature help for `@app.get()`:

<img width="1164" height="337" alt="Image" src="https://github.com/user-attachments/assets/3cdbd2fe-117c-49c9-8ddf-85ba1e9fbdba" />

### Current

Currently, the signature help looks like this:

<img width="763" height="874" alt="Image" src="https://github.com/user-attachments/assets/ddabcf75-e28e-4d0e-9277-cd9a045eab72" />

### Desired

I would like it to look, for example, like this:

<img width="755" height="1335" alt="Image" src="https://github.com/user-attachments/assets/7ddfe47b-59f3-4076-97fe-f302f36c6d69" />

## Other Editors

I see this repo is the one that says "signature help", but I would suspect it would be better supported at the ty language server side so that Zed (and others) can also benefit from it.

## FastAPI support

I think this would also mean that ty and the ty language server would be able to claim having the best FastAPI support for editors. Which I think is nice.

---

_Comment by @MichaReiser on 2026-01-13 10:14_

Thanks for the kind words and the detailed write-up.

This is related to https://github.com/astral-sh/ty/issues/504, although we could maybe special case some of it in the server only

---

_Label `server` added by @MichaReiser on 2026-01-13 10:14_

---
