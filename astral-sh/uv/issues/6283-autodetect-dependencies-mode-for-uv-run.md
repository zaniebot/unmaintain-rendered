---
number: 6283
title: "Autodetect dependencies mode for `uv run`"
type: issue
state: open
author: mgaitan
labels:
  - enhancement
  - wish
assignees: []
created_at: 2024-08-20T23:19:18Z
updated_at: 2025-07-25T13:14:23Z
url: https://github.com/astral-sh/uv/issues/6283
synced_at: 2026-01-10T01:23:58Z
---

# Autodetect dependencies mode for `uv run`

---

_Issue opened by @mgaitan on 2024-08-20 23:19_

I propose a  `--with-auto` (or `--auto` directly) flag  that does its best attempt to satisfy missing dependencies in a script/package being executed by `uv tool`. For example, [this lib](https://github.com/bndr/pipreqs) does that instrospection to generate a 
`requirements.txt`. 

From my [tweet](https://x.com/tin_nqn_/status/1825970796478738940)


---

_Comment by @zanieb on 2024-08-20 23:36_

I worry about security and that we won't be able to deliver a good enough user experience, but it sounds cool.

---

_Label `wish` added by @zanieb on 2024-08-20 23:37_

---

_Label `enhancement` added by @zanieb on 2024-08-20 23:37_

---

_Referenced in [astral-sh/uv#6375](../../astral-sh/uv/pulls/6375.md) on 2024-08-23 18:53_

---

_Comment by @manzt on 2024-08-23 19:17_

What about an option for generating/populating PEP 723 metadata automatically?

Right now there is the option to add deps explicitly:

```sh
uv add --script example.py --python 3.9 'requests==2.32.3'
```

but some flag to have uv solve the top-level dependencies and pin exact versions as inline metadata. Even without the versions, it would just be useful to generate the inline meta as a starting point (which could be manually edited with versions).


---

_Comment by @fowles on 2025-03-05 22:33_

Just going so far as to make:

```
#!/usr/bin/env -S uv run --script
# /// script
import mechanize
import re
import sys
from bs4 import BeautifulSoup
# ///
```

mean

```
#!/usr/bin/env -S uv run --script
# /// script
# requires-python = ">=3.13"
# dependencies = [
#     "bs4",
#     "mechanize",
# ]
# ///

import mechanize
import re
import sys
from bs4 import BeautifulSoup
```

would be really convenient.

---

_Comment by @zanieb on 2025-03-05 22:37_

I think 

```
# /// script
import mechanize
import re
import sys
from bs4 import BeautifulSoup
# ///
```

is explicitly banned by the PEP 723 specification, i.e., we can't make up how the `script` tag is handled.

---

_Comment by @fowles on 2025-03-05 22:39_

Ah well, thanks for the quick response!

---

_Comment by @fowles on 2025-03-05 22:57_

Would you be open to a `uv` specific thing like

```
# /// uv-imports
import mechanize
import re
import sys
from bs4 import BeautifulSoup
# ///
```

I totally understand why you might not want to fragment the ecosystem, but I just like the convenience and it should be quite easy to provide an escape hatch that would transform it back to the PEP 723 version if anyone needs it.  If you are open to it, I am happy to try my hand at it and send a PR.

---

_Comment by @zanieb on 2025-03-05 23:05_

That's.. also technically not allowed 😬 they say tools cannot invent their own tags (in that `///` format).

Anyway, I'm not sure why we'd need them to be in a header like that. I think, if we added support for this, we'd just scan the full AST for imports?

---

_Comment by @fowles on 2025-03-05 23:07_

The full AST gets into interesting corner cases like conditional imports based on versions or platforms that the explicit top level ones just duck

---

_Comment by @zanieb on 2025-03-05 23:14_

But we do know your platform and type checkers already need to perform that kind of inference (which we happen to be building over on the Ruff side).

---

_Comment by @fowles on 2025-03-05 23:29_

I am happy to take a crack at it if you can point me in the correct area of code.

---

_Comment by @zanieb on 2025-03-05 23:34_

I just think we're not there yet on a foundation to build the solution we'd be excited to deliver to people around this. You're welcome to look at https://github.com/astral-sh/ruff/tree/main/crates/red_knot#red-knot but we won't be able to guide a contribution for this. It'd probably make sense as some sort of prototype first.

---

_Comment by @vvolhejn on 2025-04-03 13:30_

Here is a (mostly Claude-generated) script that figures out what the imports are and uses `uv add ... --script` to add them. Good enough for me. Won't work for packages where the import name of the library is different than the PyPI name, e.g. `sklearn`.


```python
#!/usr/bin/env python3
import ast
import os
import pkgutil
import sys


def get_builtin_modules():
    """Get a set of all built-in modules in Python."""
    return set(sys.builtin_module_names) | {mod.name for mod in pkgutil.iter_modules()}


def get_third_party_imports(file_path):
    """Parse a Python file and analyze its imports.

    Args:
        file_path (str): Path to the Python file to analyze

    Returns:
        tuple: (all_imports, builtin_imports, third_party_imports)
    """
    with open(file_path, "r", encoding="utf-8") as file:
        content = file.read()

    # Parse the file into an AST
    tree = ast.parse(content)

    # Get all built-in modules
    builtin_modules = get_builtin_modules()

    # Lists to store imports
    all_imports = []

    # Visit all import nodes in the AST
    for node in ast.walk(tree):
        # Handle regular imports (import x, import x.y)
        if isinstance(node, ast.Import):
            for name in node.names:
                module_name = name.name.split(".")[0]  # Get the top-level module
                all_imports.append(module_name)

        # Handle from imports (from x import y, from x.y import z)
        elif isinstance(node, ast.ImportFrom):
            if node.module:
                module_name = node.module.split(".")[0]  # Get the top-level module
                all_imports.append(module_name)

    # Remove duplicates and sort
    all_imports = sorted(set(all_imports))

    # Separate built-in from third-party imports
    third_party_imports = [imp for imp in all_imports if imp not in builtin_modules]

    return third_party_imports


def main():
    if len(sys.argv) != 2:
        print("Usage: python import_analyzer.py <python_file_path>")
        sys.exit(1)

    file_path = sys.argv[1]

    if not os.path.exists(file_path):
        print(f"Error: File '{file_path}' does not exist.")
        sys.exit(1)

    if not file_path.endswith(".py"):
        print(f"Warning: File '{file_path}' does not have a .py extension.")

    third_party_imports = get_third_party_imports(file_path)

    command = ["uv", "add"] + third_party_imports + ["--script", file_path]

    print(" ".join(command))
    if input("Do it? (y/N): ").strip().lower() == "y":
        os.execvp(command[0], command)


if __name__ == "__main__":
    main()
```

---

_Referenced in [mgaitan/autopep723#5](../../mgaitan/autopep723/pulls/5.md) on 2025-07-20 01:03_

---

_Comment by @mgaitan on 2025-07-22 21:13_

We (me and my llm friend) crafted something here https://github.com/mgaitan/autopep723 .   Update: related [blog post](https://mgaitan.github.io/en/posts/automatic-dependencies-management-python-scripts-autopep723/).

---
