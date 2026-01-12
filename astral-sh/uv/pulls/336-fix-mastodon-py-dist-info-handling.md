```yaml
number: 336
title: Fix mastodon-py dist-info handling
type: pull_request
state: merged
author: konstin
labels: []
assignees: []
merged: true
base: main
head: fix-mastodon-py
created_at: 2023-11-06T16:05:05Z
updated_at: 2023-11-07T11:36:12Z
url: https://github.com/astral-sh/uv/pull/336
synced_at: 2026-01-12T16:03:53Z
```

# Fix mastodon-py dist-info handling

---

_@konstin_

mastodon-py 1.5.1 uses a dot in its dist-info dir name, which we previously didn't handle, causing home-assistant to fail. The new implementation is based on https://github.com/pypa/packaging/blob/2f83540272e79e3fe1f5d42abae8df0c14ddf4c2/src/packaging/utils.py#L146-L172.

Part of #199

```
unzip -l  Mastodon.py-1.5.1-py2.py3-none-any.whl
Archive:  Mastodon.py-1.5.1-py2.py3-none-any.whl
  Length      Date    Time    Name
---------  ---------- -----   ----
   153929  2020-02-29 17:39   mastodon/Mastodon.py
     1029  2019-10-11 19:15   mastodon/__init__.py
     7357  2019-10-11 20:24   mastodon/streaming.py
       10  2020-03-14 18:14   Mastodon.py-1.5.1.dist-info/DESCRIPTION.rst
     1398  2020-03-14 18:14   Mastodon.py-1.5.1.dist-info/metadata.json
        9  2020-03-14 18:14   Mastodon.py-1.5.1.dist-info/top_level.txt
      110  2020-03-14 18:14   Mastodon.py-1.5.1.dist-info/WHEEL
     1543  2020-03-14 18:14   Mastodon.py-1.5.1.dist-info/METADATA
      753  2020-03-14 18:14   Mastodon.py-1.5.1.dist-info/RECORD
---------                     -------
   166138                     9 files
```

---

_@charliermarsh reviewed on 2023-11-07 03:54_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:931 on 2023-11-07 03:54_

I find this much clearer as:
```rust
let dist_info_prefix = match crate::find_dist_info(filename, archive.file_names().map(|name| (name, name))) {
    Ok((_, dist_info_dir)) => dist_info_dir.to_string(),
    Err(err) => return Err(Error::InvalidWheel(err)),
};
```

---

_@charliermarsh reviewed on 2023-11-07 03:55_

---

_Review comment by @charliermarsh on `crates/puffin-resolver/src/distribution/cached_wheel.rs`:59 on 2023-11-07 03:55_

Is this `.to_string` necessary? We immediately pass this to a `format` call.

---

_@charliermarsh reviewed on 2023-11-07 03:56_

---

_Review comment by @charliermarsh on `crates/install-wheel-rs/src/wheel.rs`:1071 on 2023-11-07 03:56_

So what happened to these errors? Are they no longer ever surfaced? Should we remove them from the enum?

---

_Review comment by @konstin on `crates/install-wheel-rs/src/wheel.rs`:1071 on 2023-11-07 08:56_

There are still other cases where `Error::InvalidWheel` is used

---

_@konstin reviewed on 2023-11-07 08:56_

---

_Comment by @konstin on 2023-11-07 11:36_

Merging this since i need it for the top 8k pypi checks

---

_Merged by @konstin on 2023-11-07 11:36_

---

_Closed by @konstin on 2023-11-07 11:36_

---

_Branch deleted on 2023-11-07 11:36_

---
