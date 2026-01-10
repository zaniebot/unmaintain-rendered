---
number: 6710
title: uv run script always returns status exit 1 even if the script had a different exit code
type: issue
state: closed
author: taranlu-houzz
labels:
  - bug
assignees: []
created_at: 2024-08-27T18:10:41Z
updated_at: 2024-09-04T13:33:27Z
url: https://github.com/astral-sh/uv/issues/6710
synced_at: 2026-01-10T01:24:04Z
---

# uv run script always returns status exit 1 even if the script had a different exit code

---

_Issue opened by @taranlu-houzz on 2024-08-27 18:10_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

I was actually trying to illustrate a bug in Blender's gltf importer when I ran into this. Basically, the script always returns exit code 245 (despite running properly without raising an exception), but when run via `uv run`, the returned code is always 1. Technically, I supposed that is correct, but it seems like it would be nice to have the actual exit code of the script being run returned. Here is the script for reference:

```python
#!/usr/bin/env python


# /// script
# requires-python = ">=3.11,<3.12"
# dependencies = [
#     "bpy",
#     "typer",
# ]
# ///


from pathlib import Path
from typing import (
    Any,
    Sequence,
)

import bpy
import bpy_types
import typer


app = typer.Typer()


def load_gltf(file_path: Path) -> Sequence[bpy_types.Object]:
    """Import a gltf file into the current scene.

    Parameters
    ----------
    file_path : Path
        The path to the gltf file to import.

    Returns
    -------
    Sequence[bpy_types.Object]
        The selected objects after the gltf is imported.
    """
    op_params: dict[str, Any] = {
        "filepath": str(file_path),
    }
    bpy.ops.import_scene.gltf(**op_params)

    return bpy.context.selected_objects


@app.command()
def import_gltf(file_path: Path):
    """CLI command to import a gltf file."""
    objects = load_gltf(file_path)
    typer.echo(f"Imported {len(objects)} objects.")


if __name__ == "__main__":
    app()
```

---

_Comment by @charliermarsh on 2024-08-27 18:17_

Ahh yeah. There's a TODO about this in the code right now.

---

_Label `bug` added by @charliermarsh on 2024-08-27 18:17_

---

_Comment by @charliermarsh on 2024-08-27 18:17_

I'll call it a bug though arguably just a missing feature :)

---

_Comment by @charliermarsh on 2024-08-27 18:18_

\cc @zanieb in case you disagree on changing this.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-04 03:18_

---

_Referenced in [astral-sh/uv#6994](../../astral-sh/uv/pulls/6994.md) on 2024-09-04 03:29_

---

_Closed by @charliermarsh on 2024-09-04 13:33_

---
