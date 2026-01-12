```yaml
number: 16205
title: ruff analyze graph - empty results
type: issue
state: closed
author: vinnybod
labels:
  - question
  - analyze
assignees: []
created_at: 2025-02-17T06:37:12Z
updated_at: 2025-02-17T17:06:47Z
url: https://github.com/astral-sh/ruff/issues/16205
synced_at: 2026-01-12T15:54:55Z
```

# ruff analyze graph - empty results

---

_@vinnybod_

### Description

I created a new project with `uv init` and added python files that contain imports of each other. The `ruff analyze graph` command is finding the source files but each one has empty results. I feel like I must to be missing something obvious.

```
➜  ruff-graph uv run ruff --version
ruff 0.9.6
```

```
➜  ruff-graph uv run ruff analyze graph --preview --isolated
{
  "__init__.py": [],
  "main.py": [],
  "src/__init__.py": [],
  "src/hello.py": [],
  "src/lib_a.py": [],
  "src/lib_b.py": [],
  "src/lib_c.py": []
}
```

Minimal Example (Branch `ruff-graph`, subdirectory `ruff-graph`): https://github.com/vinnybod/blog-examples/tree/ruff-graph/ruff-graph
pyproject.toml: https://github.com/vinnybod/blog-examples/blob/ruff-graph/ruff-graph/pyproject.toml


---

_Label `analyze` added by @AlexWaygood on 2025-02-17 07:33_

---

_Comment by @MichaReiser on 2025-02-17 08:21_

Thanks for the repro. 

The output of Ruff graph seems correct to me:

* I tried running your program but even Python fails to resolve the imports when you e.g. change the import in `main.py` from `import src.lib_c as lib_c` to `import src.lib_a as lib_c` (note taht we now import the file `lib_a` instead of `c`. 
  ```
  uv run --with requests main.py
	Traceback (most recent call last):
	  File "/Users/micha/astral/blog-examples/ruff-graph/main.py", line 1, in <module>
	    import src.lib_a as lib_c
	  File "/Users/micha/astral/blog-examples/ruff-graph/src/lib_a.py", line 1, in <module>
	    import lib_b
	ModuleNotFoundError: No module named 'lib_b'
  ```
  To fix this, you have to change the imports in `lib_a` to either use absolute imports or relative imports
  * `import lib_b` -> `import src.lib_b`
  * `import lib_b` -> `from . import lib_b`
  * Changing the imports in `lib_a` to use relative imports, e.g. 

* You have to delete the `__init__.py` in the `ruff_graph` directory because main isn't a python package on its own. I do think this is a bug because ruff silently ignores the directories specified in the ``src` directory. But I'm interested in @charliermarsh's opinion on that



---

_Label `question` added by @MichaReiser on 2025-02-17 08:21_

---

_Comment by @vinnybod on 2025-02-17 16:33_

@MichaReiser thanks for the quick response! I switched the example to use absolute imports and removed the `__init__.py` from the top-level. The output now looks correct.

```
➜  ruff-graph git:(ruff-graph) ✗ uv run ruff analyze graph --preview --isolated
{
  "main.py": [
    "src/lib_c.py"
  ],
  "src/__init__.py": [],
  "src/hello.py": [],
  "src/lib_a.py": [
    "src/lib_b.py",
    "src/lib_c.py"
  ],
  "src/lib_b.py": [],
  "src/lib_c.py": []
}
```

Additional question. **Is it possible yet to have `ruff analyze graph` output the use of 3rd party deps?**
https://github.com/astral-sh/ruff/issues/13431 seems related.

---

_Comment by @MichaReiser on 2025-02-17 17:03_

Glad to hear that.

> Is it possible yet to have ruff analyze graph output the use of 3rd party deps?

No, this isn't currently possible and yes, the linked issue is related.

---

_Closed by @vinnybod on 2025-02-17 17:06_

---
