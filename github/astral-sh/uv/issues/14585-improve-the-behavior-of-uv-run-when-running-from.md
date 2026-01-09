---
number: 14585
title: "Improve the behavior of `uv run` when running from outside the project folder"
type: issue
state: open
author: AngelEzquerra
labels:
  - enhancement
assignees: []
created_at: 2025-07-13T10:11:52Z
updated_at: 2025-07-16T18:58:55Z
url: https://github.com/astral-sh/uv/issues/14585
synced_at: 2026-01-07T13:12:18-06:00
---

# Improve the behavior of `uv run` when running from outside the project folder

---

_Issue opened by @AngelEzquerra on 2025-07-13 10:11_

### Summary

I was recently surprised when I found that using `uv run` from outside the project folder does not work as I expected. I thought that `uv run /path/to/project/script.py` would use the `pyproject.toml` found in `/path/to/project/pyproject.toml`, while running the script from the current path. Putting it another way, I expected it to behave as if I had placed a `#!/usr/env -S uv run --script` shebang at the top of the `script.py` and I had just run `/path/to/project/script.py` from the current path (having done `chmod u+x /path/to/project/script.py` first of course).

Instead I got an error telling me that certain modules were not found (since uv looked for `pyproject.toml` in the current path). After searching a bit I found that according to #6733 there are two flags that can help with this scenario but neither of them does exactly what I expected:

- `--directory`: This is the same as doing `cd /path/to/project; uv run script.py`. This is not what I want because I want the script to run in the current directory.
- `--project`: This works as I expected, but has the drawback that I need to input the `/path/to/project` twice. That is, to get the behavior I want I need to do `uv run --project /path/to/project /path/to/project/script.py` which is pretty inconvenient.

It would be great if there was a way to combine the strengths of both solutions. I would argue that this should be the default behavior of `uv run` or maybe `uv run --directory` but if that is not possible, could we have a new flag that behaves as I described above? It could either be an additional flag that modifies the behavior of `--project` to _also_ look for the run script from the project (while running from the current path) or another alternative to `--directory` and `project` that behaves as expected?

Finally I want to thank you guys for the work that you are doing in `uv`. It is fantastic. It's amazing that this is the first unexpected behavior I have found so far :)


### Example

_No response_

---

_Label `enhancement` added by @AngelEzquerra on 2025-07-13 10:11_

---

_Comment by @powercoconola on 2025-07-16 18:58_

I agree, I also assumed that `uv run` would find the project folder for me. It seems obvious to me that if I run `/path/to/project/script.py` that the project can be found one or two layers above. 

At the very least, the error message of `Module not found` wasn't helpful. Maybe this can be improved to mention the `--directory` and/or `--project` commands?

---
