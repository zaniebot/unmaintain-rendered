---
number: 8731
title: File paths of ZIP contents are not sanitized
type: issue
state: closed
author: SLeitgeb
labels:
  - security
assignees: []
created_at: 2024-10-31T17:35:16Z
updated_at: 2024-10-31T19:19:01Z
url: https://github.com/astral-sh/uv/issues/8731
synced_at: 2026-01-10T01:24:32Z
---

# File paths of ZIP contents are not sanitized

---

_Issue opened by @SLeitgeb on 2024-10-31 17:35_

All files from a package WHL are extracted to the path resolved from the current directory. The ZIP file path is not sanitized first (as suggested in the [async_zip](https://docs.rs/async_zip/latest/async_zip/struct.ZipEntry.html#method.filename) package), which could lead to directory traversal attacks, e.g. by a malicious package.

The demo below uses relative filepaths, but this would presumably resolve absolute paths as well (e.g. `~/.cache/uv/archive-v0` + `/etc/passwd` → `/etc/passwd`).

Tested on Arch Linux with uv 0.4.28, Python 3.10. This should also affect Windows.

An example of the issue using an empty payload file in a parent directory.
```
touch payload
uv init payload-package
cd payload-package
uv build
cd dist/
zip payload_package-0.1.0-py3-none-any.whl ../../payload
cd ../..
uv init payload-package-importer
cd payload-package-importer
uv add ../payload-package/dist/payload_package-0.1.0-py3-none-any.whl
ls -1 ~/.cache | grep payload
```

You should observe a `payload` file created two directories up relative to the directory from which the paths are resolved (should be the `$HOME/.config` directory).

---

_Comment by @charliermarsh on 2024-10-31 17:39_

Thanks, I'll fix this (though PRs are also welcome).

---

_Label `security` added by @charliermarsh on 2024-10-31 17:39_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-10-31 17:40_

---

_Referenced in [astral-sh/uv#8732](../../astral-sh/uv/pulls/8732.md) on 2024-10-31 17:54_

---

_Comment by @BurntSushi on 2024-10-31 17:56_

@SLeitgeb Can you say more about the threat model you're using here?

---

_Comment by @SLeitgeb on 2024-10-31 18:19_

The scenario that comes to mind involves a malicious package that would get installed as a user dependency. This package doesn't need to be a direct dependency, it could also be a dependency of another package.

The WHL archive of the malicious package could be manipulated to include arbitrary files (SSH keys, binaries, etc.) that uv would then extract. Running uv as a privileged user in a production environment could then be quite bad.

I don't think this scenario is too likely, and the offending packages would be easy to discover, but there could also be other implications I'm not seeing (maybe package sources outside of pypi?).

---

_Comment by @BurntSushi on 2024-10-31 18:23_

I think you also need to stipulate that you've configured `uv` to never build anything. If you're building sdists, then in that context, arbitrary code can run.

But if you're only installing wheels with `--no-build`, then yeah, you might have the expectation that builds are isolated from doing these kinds of shenanigans.

---

_Comment by @charliermarsh on 2024-10-31 18:31_

Are you sure the example from the summary works? For local wheels, we use the `zip` crate, not `async_zip`.

---

_Comment by @SLeitgeb on 2024-10-31 18:33_

@BurntSushi That seems right. I don't think it's a big issue, just something I wanted you to be aware of.

---

_Comment by @SLeitgeb on 2024-10-31 18:36_

@charliermarsh I tested this several times to be sure, once also with the package installed from a local index, with the same results. 

---

_Closed by @charliermarsh on 2024-10-31 19:12_

---

_Comment by @charliermarsh on 2024-10-31 19:19_

Thanks for catching this @SLeitgeb, appreciate it.

---
