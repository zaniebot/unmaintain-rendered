```yaml
number: 16468
title: uv remove followed by uv add leaves package in incomplete state with missing RECORD file and package files
type: issue
state: open
author: rajav99
labels:
  - needs-mre
assignees: []
created_at: 2025-10-27T10:09:18Z
updated_at: 2025-11-04T19:03:54Z
url: https://github.com/astral-sh/uv/issues/16468
synced_at: 2026-01-12T16:02:32Z
```

# uv remove followed by uv add leaves package in incomplete state with missing RECORD file and package files

---

_@rajav99_

### Summary

### Description
When removing a package with `uv remove` and then immediately re-adding it with `uv add`, UV creates the package's `.dist-info` directory but fails to install the actual package files. This leaves the environment in an inconsistent state where the package metadata exists but the package itself cannot be imported.

### Environment
- **OS**: macOS Tahoe 26.0.1 (Darwin 25.0.0 arm64)
- **UV version**: uv 0.9.5 (Homebrew 2025-10-21)
- **Python version**: 3.9.6
- **Link mode**: `clone` (default on macOS)

### Minimal Reproducible Example

```bash
# Create a new project
uv init test_bug
cd test_bug

# Add requests package
uv add requests
# ---------------------- Successfully installs requests and dependencies (5 packages) ----------------------
# Creating virtual environment at: .venv
# Resolved 6 packages in 97ms
# Installed 5 packages in 14ms
#  + certifi==2025.10.5
#  + charset-normalizer==3.4.4
#  + idna==3.11
#  + requests==2.32.5
#  + urllib3==2.5.0

# Remove requests package
uv remove requests
# ---------------------- Successfully uninstalls requests and dependencies (5 packages) ----------------------
# Resolved 1 package in 25ms
# Uninstalled 5 packages in 7ms
#  - certifi==2025.10.5
#  - charset-normalizer==3.4.4
#  - idna==3.11
#  - requests==2.32.5
#  - urllib3==2.5.0

# Re-add requests package
uv add requests
# ---------------------- Claims to install only 4 packages (certifi, charset-normalizer, idna, urllib3) ----------------------
# ---------------------- requests is NOT listed in the output but shows in pyproject.toml ----------------------
# Resolved 6 packages in 13ms
# Installed 4 packages in 6ms
#  + certifi==2025.10.5
#  + charset-normalizer==3.4.4
#  + idna==3.11
#  + urllib3==2.5.0

# Verify the broken state
uv run python -c "import requests"
# ---------------------- Error: ModuleNotFoundError: No module named 'requests' ----------------------
# Traceback (most recent call last):
#   File "<string>", line 1, in <module>
# ModuleNotFoundError: No module named 'requests'
```

### Observed Behavior

After the second `uv add requests`:

1. **UV output is misleading**: Shows only 4 packages installed, doesn't mention `requests`

2. **Package files are missing**: The `requests/` folder is not present in `.venv/lib/python3.9/site-packages/`

3. **Metadata exists but is corrupted**: The `requests-2.32.5.dist-info/` directory exists but is missing the `RECORD` file

4. **pyproject.toml is correct**: Still shows `"requests>=2.32.5"` in dependencies

5. **Import fails**: `import requests` raises `ModuleNotFoundError`

### Workaround

Running `uv sync --reinstall` fixes the issue but produces a warning:

```
warning: Failed to uninstall package at .venv/lib/python3.9/site-packages/requests-2.32.5.dist-info due to missing `RECORD` file. Installation may result in an incomplete environment.
```

This confirms that the `RECORD` file (which lists all files belonging to the package) was never created during the incomplete installation.

### Expected Behavior

After `uv add requests`, the package should be:
- Fully installed with all package files in `site-packages/requests/`
- Listed in UV's installation output
- Have a complete `.dist-info/RECORD` file

### Reproduction Rate
Reproduced consistently across multiple fresh project directories.

### Platform

Darwin 25.0.0 arm64

### Version

uv 0.9.5 (Homebrew 2025-10-21)

### Python version

Python 3.9.6

---

_Label `bug` added by @rajav99 on 2025-10-27 10:09_

---

_Comment by @charliermarsh on 2025-10-28 23:42_

I'm unable to reproduce this:
```
❯ uv init test_bug
Initialized project `test-bug` at `/Users/crmarsh/workspace/registry-proxy/test_bug`
registry-proxy on  main [$?]
❯ cd test_bug

❯ uv add requests
Using CPython 3.14.0
Creating virtual environment at: .venv
Resolved 6 packages in 79ms
Prepared 1 package in 101ms
Installed 5 packages in 5ms
 + certifi==2025.10.5
 + charset-normalizer==3.4.4
 + idna==3.11
 + requests==2.32.5
 + urllib3==2.5.0

❯ uv remove requests
Resolved 1 package in 3ms
Uninstalled 5 packages in 6ms
 - certifi==2025.10.5
 - charset-normalizer==3.4.4
 - idna==3.11
 - requests==2.32.5
 - urllib3==2.5.0

❯ uv add requests
Resolved 6 packages in 4ms
Installed 5 packages in 2ms
 + certifi==2025.10.5
 + charset-normalizer==3.4.4
 + idna==3.11
 + requests==2.32.5
 + urllib3==2.5.0

❯ uv run python -c "import requests"
```

Can you try running `uv cache clean` first? I suspect that your cache is maybe in some kind of bad state?

---

_Label `bug` removed by @charliermarsh on 2025-10-28 23:42_

---

_Label `needs-mre` added by @charliermarsh on 2025-10-28 23:42_

---

_Comment by @rajav99 on 2025-11-04 19:03_

Ran the `uv cache clean` command, and tried multiple times, still gave the same error.
Had to do `uv sync --reinstall` to make it work.

---
