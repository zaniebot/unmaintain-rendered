---
number: 17043
title: Explicitly set EntryType for file entries in tar
type: pull_request
state: closed
author: e-nomem
labels:
  - bug
assignees: []
base: main
head: tar-entry-type
created_at: 2025-12-09T08:17:08Z
updated_at: 2025-12-11T10:37:36Z
url: https://github.com/astral-sh/uv/pull/17043
synced_at: 2026-01-10T01:26:20Z
---

# Explicitly set EntryType for file entries in tar

---

_Pull request opened by @e-nomem on 2025-12-09 08:17_

## Summary
This PR explicitly sets the entry type for files in an sdist. This changes the entry type from `AREGTYPE` (the 'legacy' regular file type) to `REGTYPE` (the 'normal' regular file type) in the generated tar.

This change works around a bug in the python `tarfile` module that causes all entries after a certain point in the tar to be silently ignored if any entry matches some very specific conditions. In `maturin` this was very visible since the `PKG-INFO` was written at the very end so `twine check` would loudly complain that the `PKG-INFO` was missing and that the sdist was invalid. In `uv` the `PKG-INFO` is written at the beginning so this issue is unlikely to be caught.

Note that this change does mean that sdists created with newer versions of the uv build backend will not be byte-for-byte identical with sdists from an older version.

See https://github.com/PyO3/maturin/issues/2855#issuecomment-3546501132

## Test Plan
This is the same as the change that was made in maturin to work around the same issue

---

_Comment by @woodruffw on 2025-12-09 16:48_

> This change works around a bug in the python `tarfile` module that causes all entries after a certain point in the tar to be silently ignored if any entry matches some very specific conditions.

Out of curiosity, did you file a bug with CPython for this? I suspect it's the kind of thing they'd be interested in fixing in a patch release for each non-EOL version.

Edit: my bad, I see you filed it as https://github.com/python/cpython/issues/141707! Linking here so these get cross-referenced 🙂 

---

_Review requested from @konstin by @zanieb on 2025-12-09 17:15_

---

_Comment by @e-nomem on 2025-12-09 19:07_

There's also a related issue in `tar-rs` to initialize headers by default to `REGTYPE` instead of `AREGTYPE` because I suspect that most users are just not explicitly setting this flag rather than wanting to actually use the legacy type. It's not likely to get merged anytime soon though because it's a breaking change. https://github.com/alexcrichton/tar-rs/issues/422

---

_Comment by @konstin on 2025-12-10 10:24_

Do you have a reproducible example (ideally a standalone rust script) and a Python script that both use and CPython can use to check the problem and its fix?

---

_Comment by @e-nomem on 2025-12-10 12:23_

Here's a rust script that will create two tar files in the local directory:
```rust
#!/usr/bin/env -S cargo +nightly -Zscript
---cargo
[dependencies]
tar = "0.4.44"
---
use std::fs::File;
use std::io::Error as IoError;
use std::path::Path;

use tar::Builder;
use tar::EntryType;
use tar::Header;

fn create_tar(filename: impl AsRef<Path>, entry_type: Option<EntryType>) -> Result<(), IoError> {
	let file = File::options().write(true).create(true).truncate(true).open(filename)?;
	let mut tarfile = Builder::new(file);

	let entries = [
		"file1".into(),
		format!("{}/bbb", "a".repeat(99)),
		"file2".into(),
	];

	for entry in entries {
		let mut header = Header::new_gnu();
		header.set_mode(0o644);
		header.set_size(entry.len() as u64);
		if let Some(entry_type) = entry_type {
			header.set_entry_type(entry_type);
		}
		tarfile.append_data(&mut header, &entry, entry.as_bytes())?;
	}

	tarfile.finish()?;
	Ok(())
}

fn main() -> Result<(), IoError> {
	create_tar("good.tar", Some(EntryType::Regular))?;
	create_tar("bad.tar", None)?;
	Ok(())
}
```

And here's a python script to test the two tar files:
```python
#!/usr/bin/env -S uv run --script
# /// script
# requires-python = ">=3.9"
# dependencies = [
#   "pytest",
# ]
# ///
import tarfile
from io import BytesIO

import pytest


def get_members(filename):
	with tarfile.open(filename, 'r') as tar:
		return tar.getmembers()


def test_good_tar():
	assert len(get_members("good.tar")) == 3


@pytest.mark.xfail(strict=True)
def test_bad_tar():
	assert len(get_members("bad.tar")) == 3


@pytest.mark.parametrize('format', (
	pytest.param(tarfile.GNU_FORMAT, id="gnu"),
	pytest.param(tarfile.PAX_FORMAT, id="pax"),
))
@pytest.mark.xfail(strict=True)
def test_python_only(format):
	fp = BytesIO()
	with tarfile.open(mode='w', fileobj=fp, format=format) as tar:
		info = tarfile.TarInfo()
		info.type = tarfile.AREGTYPE
		info.name = ("a" * 99) + "/bbb"
		tar.addfile(info)

		expected = {t.name: t.type for t in tar.getmembers()}
	
	fp.seek(0)
	with tarfile.open(mode='r', fileobj=fp) as tar:
		actual = {t.name: t.type for t in tar.getmembers()}
	
	assert expected == actual


if __name__ == "__main__":
	pytest.main([__file__])
```

---

_Label `bug` added by @konstin on 2025-12-11 10:17_

---

_Referenced in [python/cpython#141707](../../python/cpython/issues/141707.md) on 2025-12-11 10:20_

---

_@konstin approved on 2025-12-11 10:24_

Thanks!

---

_Comment by @konstin on 2025-12-11 10:24_

I've added a comment explaining why we need to set that type.

---

_Merged by @konstin on 2025-12-11 10:37_

---

_Closed by @konstin on 2025-12-11 10:37_

---
