---
number: 16764
title: run does not quite work right with __main__.py
type: issue
state: open
author: larkost
labels:
  - bug
assignees: []
created_at: 2025-11-17T22:21:32Z
updated_at: 2025-12-03T18:01:52Z
url: https://github.com/astral-sh/uv/issues/16764
synced_at: 2026-01-07T13:12:19-06:00
---

# run does not quite work right with __main__.py

---

_Issue opened by @larkost on 2025-11-17 22:21_

### Summary

After #7281 the behavior around running \_\_main__.py is not quite right. Namely it does not import the code as a package, so things like module-relative imports do not work. I am on uv 0.9.9 (4fac4cb7e) on a M1 MacBook running Sequoia 15.7.

This is pretty easy to reproduce with a trivial example. In this case you need 3 files in a folder:
| = red  (a folder)
| === \_\_init__.py  (empty file)
| === \_\_main__.py  (see below)
| === alpha.py (see below)

alpha.py
``` python
foo = "This worked"
```

__main__.py
``` python
from . import alpha

if __name__ == '__main__':
    print(alpha.foo)
```

If you run this as expected from Python as a module (from the directory enclosing `red`), you get the expected "This worked"
``` bash
python -m ./red
```

However trying this with `uv` wrongly just runs `__main__.py` directly, without using it as part of the package (which is the main point of __main__.py):
``` bash
uv run ./red
```

The results of that last are:
```
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/private/tmp/alpha/red/./red/__main__.py", line 1, in <module>
    from . import alpha
ImportError: attempted relative import with no known parent package
```

There was a mention in #7281 of using `-m`, but I think the author there missed this subtlety of the `-m` command (and \_\_main__.py).

### Platform

MacOS (Darwin 24.6.0 arm64)

### Version

uv 0.9.9 (4fac4cb7e 2025-11-12)

### Python version

Python 3.12.6

Note: edits for formating

---

_Label `bug` added by @larkost on 2025-11-17 22:21_

---

_Comment by @nooscraft on 2025-11-21 05:14_

I can reproduce this. The issue is that when uv detects a package with `__main__.py`, it's running the file directly instead of executing it as a module, which breaks relative imports.

Here's what's happening:

When you run `uv run ./red`, uv detects the `__main__.py` file and creates a `PythonPackage` command, but then executes it incorrectly. Instead of running `python -m red` (which would import `red` as a package and execute `red.__main__`), it's running something like `python ./red/__main__.py` directly, which loses the package context.

You can see this in the error traceback - it shows the file path being executed directly:
```
File "/personal/play_area/./red/__main__.py", line 1, in <module>
    from . import alpha
ImportError: attempted relative import with no known parent package
```

The workaround is to explicitly use the `-m` flag:
```bash
uv run python -m red
```

This works because it bypasses uv's package detection and directly invokes Python's module execution, which properly sets up the package context.

The bug is in `crates/uv/src/commands/project/run.rs` around line 1478. When handling `PythonPackage`, it should use `python -m <package_name>` instead of `python <path>`. The fix needs to:
1. Extract the package name from the path (e.g., "red" from "./red")
2. Use `-m` flag: `process.arg("-m")` followed by `process.arg(package_name)`
3. Ensure the parent directory is in `sys.path` so Python can find the package

I've reproduced this locally with your exact test case, and the workaround works. This is definitely a bug we should fix - packages with `__main__.py` should work the same way as `python -m` does.



---

_Comment by @nooscraft on 2025-11-21 06:58_

@zanieb, @konstin! I've been poking around this issue and think I found the problem. The `PythonPackage` handler is passing the directory path directly to Python instead of using `-m` to run it as a module, which breaks relative imports.

The fix is pretty straightforward - in `crates/uv/src/commands/project/run.rs` around line 1476-1480, instead of:

```rust
} else {
    let mut process = Command::new(interpreter.sys_executable());
    process.arg(path);  // Wrong: runs file directly
    process.args(args);
    process
}
```

We should do:

```rust
} else {
    let mut process = Command::new(interpreter.sys_executable());
    process.arg("-m");
    // Extract package name from path (e.g., "red" from "./red" or "red/")
    let package_name = path
        .file_name()
        .and_then(|n| n.to_str())
        .unwrap_or_else(|| {
            // Fallback: try to get from target string
            target.to_str().unwrap_or("")
        });
    process.arg(package_name);
    process.args(args);
    process
}
```

This makes it run `python -m red` instead of `python ./red/__main__.py`, which properly sets up the package context.

I've reproduced it locally with the exact test case from the issue, and the workaround (`uv run python -m red`) works as expected. Mind if I take a shot at implementing this? I can add a test case with relative imports to prevent regressions.



---

_Comment by @konstin on 2025-11-21 09:18_

I'm not following with the example here, for me all the cases fail:

```console
$ python -m ./red
/home/konsti/projects/red/.venv/bin/python: Relative module names not supported
$ python ./red
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/konsti/projects/red/src/./red/__main__.py", line 1, in <module>
    from . import alpha
ImportError: attempted relative import with no known parent package
$ uv run ./red/
Traceback (most recent call last):
  File "<frozen runpy>", line 198, in _run_module_as_main
  File "<frozen runpy>", line 88, in _run_code
  File "/home/konsti/projects/red/src/./red/__main__.py", line 1, in <module>
    from . import alpha
ImportError: attempted relative import with no known parent package
```

`python -m red` works, and so does `uv run -m red`, but those are different commands. Python with and without `-m` are two very different ways to launch code. Each has their specific characteristic order of operations and files they ignore which users rely on, `uv run` is limited in how much magic it can apply without causing more confusion than `python` vs `python -m` already does.

---

_Comment by @nooscraft on 2025-11-21 10:09_

I see the confusion about the `python -m ./red` syntax in the original example - Python's `-m` flag doesn't accept relative paths like that, it needs just the module name.

I actually misinterpreted the example too when I first read it. I assumed `python -m ./red` meant "run the red package as a module" and just used `python -m red` directly when reproducing (from the parent directory). Looking back, I should have caught that the syntax was wrong.

The correct setup is:
```
parent_dir/
  red/
    __init__.py
    __main__.py
    alpha.py
```

From `parent_dir/`:
```bash
python -m red        # Works - prints "This worked"
uv run ./red        # Fails - relative import error
```

The question is whether `uv run ./red` should extract "red" from the path and execute `python -m red` (which would preserve package context for relative imports), or if the current behavior of running the `__main__.py` file directly is intentional.



---

_Comment by @konstin on 2025-11-24 13:54_

The current behavior is intentional, we don't want to switch between module and path modes automatically. The current split is confusing, but trying to be smarter than that in uv would only add to the confusion.

---

_Comment by @larkost on 2025-12-03 16:23_

But by running `__main__.py` `uv` is already switching into "module" mode, it is just doing so in a way that breaks relative imports. So you are already doing that, and not doing so completely is the confusing bit.

I would argue that `uv` has three choices here, and has chosen the worst of the three:
1. When presented with a folder, try running `__main__.py`, but do so in non-module mode (current behavior)
2. Fail when presented with a folder, possibly suggest `-m`
3. Check if there is a `__main__.py` and run it as a module, from the enclosing directory.

I would argue that #3 is the most user-friendly of the three, and is (close to) never going to not be what the user is trying to do.

---

_Comment by @zanieb on 2025-12-03 17:58_

Per https://github.com/astral-sh/uv/issues/7275 we're just matching the behavior of the `python` binary which supports `python ./dir` to execute `python ./dir/__main__.py`? (that's what motivated https://github.com/astral-sh/uv/pull/7281 adding the behavior)

---

_Comment by @zanieb on 2025-12-03 18:01_

I can see why (3) would be preferable, but breaking from the convention of the `python` binary will require a strong justification and I disagree that we've chosen "the worst option" by matching its behavior 1:1.

---
