```yaml
number: 15156
title: "Bug: Cached Script Dependencies Not Properly Invalidated"
type: issue
state: open
author: Alchemyst0x
labels:
  - question
assignees: []
created_at: 2025-08-08T02:15:53Z
updated_at: 2025-08-17T10:43:27Z
url: https://github.com/astral-sh/uv/issues/15156
synced_at: 2026-01-12T16:02:05Z
```

# Bug: Cached Script Dependencies Not Properly Invalidated

---

_@Alchemyst0x_

### Summary

Script dependency resolution fails to properly invalidate/update when using the `--force-reinstall` or `--reinstall` flags (even when combined with `--refresh`), but works when the same script is renamed to a different filename. This suggests that the cache key might be filename-dependent in a way that bypasses the flags, and that the flags themselves are seemingly ineffective for their intended purposes.

### Steps to Reproduce / Minimal Reproducible Example

Save and run this bash script:

```bash
#!/usr/bin/env bash

# Arbitrary Python version 
PY_VERSION="3.13"

# We will intentionally install the wrong dependency first to demonstrate
INCORRECT_DEP="crypto"
CORRECT_DEP="pycryptodome"

SCRIPT_FILE="example.py"
SCRIPT_CONTENT='#!/usr/bin/env uv run --script

try:
    from Crypto.Cipher import AES
except Exception:
    print("Failed to import from 'Crypto' namespace")
else:
    print("It works!")'

DEMO_DIR="$(mktemp -d)/bug_example"
mkdir -p "$DEMO_DIR" && cd "$DEMO_DIR" || exit

echo -e "\n-- Creating script file: '$SCRIPT_FILE'"
echo "$SCRIPT_CONTENT" > "$SCRIPT_FILE"

echo -e "\n-- Installing Python $PY_VERSION..."
uv python install "$PY_VERSION"

echo -e "\n-- Adding '$INCORRECT_DEP' to '$SCRIPT_FILE'"
uv add --script "$SCRIPT_FILE" --python="$PY_VERSION" "$INCORRECT_DEP"

echo -e "\n-- Running '$SCRIPT_FILE' (expected to fail)"
chmod +x "$SCRIPT_FILE"
"./$SCRIPT_FILE"

echo -e "\n-- Adding '$CORRECT_DEP' to '$SCRIPT_FILE' and removing '$INCORRECT_DEP'"
echo "$SCRIPT_CONTENT" > "$SCRIPT_FILE"
uv add --script "$SCRIPT_FILE" --python="$PY_VERSION" "$CORRECT_DEP"

echo -e "\n-- Running '$SCRIPT_FILE' a second time (should work but will fail)"
"./$SCRIPT_FILE"
echo -e "\n  ...trying again with 'uv run --script --refresh --force-reinstall'"
uv run --script --refresh --force-reinstall "$SCRIPT_FILE"
echo -e "\n  ...trying again with 'uv run --script  --refresh --reinstall'"
uv run --script --refresh --reinstall example.py

echo -e "\n-- Renaming '$SCRIPT_FILE' to 'renamed.py'"
mv "$SCRIPT_FILE" 'renamed.py'

echo -e "\n-- Running 'renamed.py' (script will run as expected now)"
"./renamed.py"
```

#### Expected Behavior

- Dependencies should resolve correctly after adding `pycryptodome`, regardless of filename.
- `--reinstall` and `--refresh` flags should properly invalidate the cache.

#### Actual Behavior

When run locally, the script produces the following output:

```sh
bash ~/demo_uv_bug.sh

-- Creating script file: 'example.py'

-- Installing Python 3.13...

-- Adding 'crypto' to 'example.py'
Updated `example.py`

-- Running 'example.py' (expected to fail)
Installed 9 packages in 15ms
Failed to import from Crypto namespace

-- Adding 'pycryptodome' to 'example.py' and removing 'crypto'
Updated `example.py`

-- Running 'example.py' a second time (should work but will fail)
Installed 1 package in 7ms
Failed to import from Crypto namespace

  ...trying again with 'uv run --script --refresh --force-reinstall'
Uninstalled 1 package in 22ms
Installed 1 package in 9ms
Failed to import from Crypto namespace

  ...trying again with 'uv run --script  --refresh --reinstall'
Uninstalled 1 package in 23ms
Installed 1 package in 7ms
Failed to import from Crypto namespace

-- Renaming 'example.py' to 'renamed.py'

-- Running 'renamed.py' (script will run as expected now)
Installed 1 package in 5ms
It works!
```

As shown in the output:

- Initial import fails (expected with wrong dependency).
- Import continues to fail after adding correct dependency.
- Import continues failing despite the use of the `--reinstall` and `--refresh` flags.
- Only after renaming the file does the import succeed.

### Platform

Darwin 24.6.0 arm64

### Version

uv 0.8.5 (Homebrew 2025-08-05)

### Python version

Python 3.13.4

---

_Label `bug` added by @Alchemyst0x on 2025-08-08 02:15_

---

_Comment by @charliermarsh on 2025-08-16 22:45_

I looked into this and... it's a pretty weird case that I'm not sure we can solve.

When you run `uv add crypto`, we install the `crypto` package, which creates a module named `crypto` (lowercase) in the virtual environment.

When you then run `uv add pycryptodome`, we install the `pycryptodome` package, which _wants_ to create a module named `Crypto` (uppercase) in the virtual environment. But `crypto` (lowercase) already exists, and macOS is case-insensitive (but case-preserving). So the filesystem treats those as "the same", and overwrites the contents of `crypto` with those of `pycryptodome` (rather then creating `Crypto`). (Other Python installers have the same behavior.)

Python is not case-insensitive, though. So if you try to `import Crypto`, and the folder is `crypto`, Python will throw an error.

I'm not really sure how to avoid this. If we made `uv add` and `uv remove` automatically sync the script environment, and you'd run `uv remove crypto`, then that might've worked. Alternatively, running `uv sync --script example.py` after removing `"crypto"` also would've worked, since we'd remove that directory, and `uv add pycryptodome` would then create `Crypto` instead of `crypto`.

---

_Comment by @charliermarsh on 2025-08-16 22:47_

Actually, this would've worked: `uv run --exact --reinstall -v --script script.py` (assuming you removed `"crypto"` as a dependency). Because then we'd remove the `crypto` directory and create `Crypto`.

---

_Label `bug` removed by @charliermarsh on 2025-08-16 22:47_

---

_Label `question` added by @charliermarsh on 2025-08-16 22:47_

---

_Comment by @Alchemyst0x on 2025-08-17 10:43_

Wow, that's incredibly subtle! That makes perfect sense - I completely overlooked the case-sensitivity filesystem issue.

Thanks for digging into this. The `--exact --reinstall` workaround (after removing the dependency) is good to know.

Feel free to close this if you think it's not actionable.

---
