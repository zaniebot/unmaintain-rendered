```yaml
number: 12472
title: "uvx --with \"package@version\" ?"
type: issue
state: open
author: nickolay
labels:
  - enhancement
  - cli
assignees: []
created_at: 2025-03-25T22:10:05Z
updated_at: 2025-04-01T18:45:41Z
url: https://github.com/astral-sh/uv/issues/12472
synced_at: 2026-01-10T03:41:47Z
```

# uvx --with "package@version" ?

---

_Issue opened by @nickolay on 2025-03-25 22:10_

### Summary

I got confused with the docs while attempting to run a tool `with` extra dependencies. The correct incantation (`uvx --with 'jupyterlab-skip-traceback==5.0.0' --from="jupyterlab@4.0.10" jupyter-lab`) requires inconsistent version specifiers.

I learned the `uvx package@version` syntax from https://docs.astral.sh/uv/concepts/tools/#tool-versions

When trying to apply it to jupyter, I added `--from`, which also accepts the `package@version` syntax

I naturally thought it would also work in `--with "jupyterlab-skip-traceback@5.0.0"` too, but got:
```
 error: Failed to parse: `jupyterlab-skip-traceback@5.0.0`
  Caused by: Expected path (`C:\Users\Nickolay\5.0.0`) to end in a supported file extension: `.whl`, `.tar.gz`, `.zip`, `.tar.bz2`, `.tar.lz`, `.tar.lzma`, `.tar.xz`, `.tar.zst`, `.tar`, `.tbz`, `.tgz`, `.tlz`, or `.txz`
jupyterlab-skip-traceback@5.0.0
                          ^^^^^
```

(To be fair the correct syntax `uvx --with <extra-package>==<version>` is mentioned on https://docs.astral.sh/uv/concepts/tools/ , but I missed it while scanning the page.)

I turned to the CLI reference https://docs.astral.sh/uv/reference/cli/#uv-tool-run--with , but it doesn't say what format the `--with` version specifiers are supposed to use. (Same problem with `--from`.)

After searching the issue tracker I understand that the `@` notation was initially specific to `uvx`, but it's being added to other places as well (e.g. https://github.com/astral-sh/uv/issues/6535).

Would it make sense to extend `--with` to accept package@version syntax too or document its syntax instead (and maybe improve the error message)?

### Platform

Windows 10

### Version

uv 0.6.9 (3d9460278 2025-03-20)

### Python version

Python 3.10.16

---

_Label `bug` added by @nickolay on 2025-03-25 22:10_

---

_Label `bug` removed by @zanieb on 2025-04-01 18:44_

---

_Label `enhancement` added by @zanieb on 2025-04-01 18:44_

---

_Label `cli` added by @zanieb on 2025-04-01 18:44_

---

_Comment by @zanieb on 2025-04-01 18:45_

We can consider adding this. I'm not sure how hard it would be or if there are downsides, i.e., we are treating `@` as a source for the package (per the standard) rather than a version. I'm not sure if that's ambiguous ever?

---
