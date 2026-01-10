```yaml
number: 12259
title: Disallow writing symlinks outside the source distribution target directory
type: pull_request
state: merged
author: charliermarsh
labels:
  - compatibility
assignees: []
merged: true
base: main
head: charlie/ext
created_at: 2025-03-18T02:27:28Z
updated_at: 2025-07-22T13:31:33Z
url: https://github.com/astral-sh/uv/pull/12259
synced_at: 2026-01-10T06:53:01Z
```

# Disallow writing symlinks outside the source distribution target directory

---

_Pull request opened by @charliermarsh on 2025-03-18 02:27_

## Summary

Closes #12163.

## Test Plan

Created an offending source distribution with this script:

```python
import io
import tarfile
import textwrap
import time

PKG_NAME  = "badpkg"
VERSION   = "0.1"
DIST_NAME = f"{PKG_NAME}-{VERSION}"
ARCHIVE   = f"{DIST_NAME}.tar.gz"


def _bytes(data: str) -> io.BytesIO:
    """Helper: wrap a text blob as a BytesIO for tarfile.addfile()."""
    return io.BytesIO(data.encode())


def main(out_path: str = ARCHIVE) -> None:
    now = int(time.time())

    with tarfile.open(out_path, mode="w:gz") as tar:

        def add_file(path: str, data: str, mode: int = 0o644) -> None:
            """Add a regular file whose *content* is supplied as a string."""
            buf  = _bytes(data)
            info = tarfile.TarInfo(path)
            info.size   = len(buf.getbuffer())
            info.mtime  = now
            info.mode   = mode
            tar.addfile(info, buf)

        # ── top‑level setup.py ───────────────────────────────────────────────
        setup_py = textwrap.dedent(f"""\
            from setuptools import setup, find_packages
            setup(
                name="{PKG_NAME}",
                version="{VERSION}",
                packages=find_packages(),
            )
        """)
        add_file(f"{DIST_NAME}/setup.py", setup_py)

        # ── minimal package code ─────────────────────────────────────────────
        add_file(f"{DIST_NAME}/{PKG_NAME}/__init__.py", "# placeholder\\n")

        # ── the malicious symlink ────────────────────────────────────────────
        link = tarfile.TarInfo(f"{DIST_NAME}/{PKG_NAME}/evil_link")
        link.type     = tarfile.SYMTYPE
        link.mtime    = now
        link.mode     = 0o777
        link.linkname = "../../../outside.txt"
        tar.addfile(link)

    print(f"Created {out_path}")


if __name__ == "__main__":
    main()
```

Verified that both `pip install` and `uv pip install` rejected it.

I also changed `link.linkname = "../../../outside.txt"` to `link.linkname = "/etc/outside"`, and verified that the absolute path was rejected too.

---

_Label `compatibility` added by @charliermarsh on 2025-03-18 02:27_

---

_@charliermarsh reviewed on 2025-07-21 17:14_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/build.rs`:1803 on 2025-07-21 17:14_

@konstin -- It looks like in this case, we're including the `.venv` in the sdist. I'm not sure why. Doesn't hatchling typically filter out `.gitignore`?

---

_@charliermarsh reviewed on 2025-07-21 17:15_

---

_Review comment by @charliermarsh on `crates/uv/tests/it/build.rs`:1757 on 2025-07-21 17:15_

We should probably reject this when building?

---

_Review requested from @konstin by @charliermarsh on 2025-07-21 17:15_

---

_Marked ready for review by @charliermarsh on 2025-07-21 17:15_

---

_Review comment by @konstin on `crates/uv/tests/it/build.rs`:1757 on 2025-07-22 12:29_

At least with the uv build backend we support it by adding the real file instead, it's useful when you have e.g. a shared readme or license file in a monorepo.

---

_Review comment by @konstin on `crates/uv/tests/it/build.rs`:1803 on 2025-07-22 12:31_

It's usually respecting the git configuration, maybe it's something about how we're setting up our tests, possibly because we don't have git repo? I'd have to dig a bit to figure that out.

---

_@konstin approved on 2025-07-22 12:32_

---

_Merged by @charliermarsh on 2025-07-22 13:20_

---

_Closed by @charliermarsh on 2025-07-22 13:20_

---

_Branch deleted on 2025-07-22 13:20_

---
