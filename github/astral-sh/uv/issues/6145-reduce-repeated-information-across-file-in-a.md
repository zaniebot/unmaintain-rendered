---
number: 6145
title: "Reduce repeated information across `File` in a single distribution"
type: issue
state: open
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2024-08-16T13:26:33Z
updated_at: 2024-08-16T14:55:19Z
url: https://github.com/astral-sh/uv/issues/6145
synced_at: 2026-01-07T13:12:17-06:00
---

# Reduce repeated information across `File` in a single distribution

---

_Issue opened by @charliermarsh on 2024-08-16 13:26_

`File` looks like this:

```rust
#[derive(
    Debug, Clone, Hash, Serialize, Deserialize, rkyv::Archive, rkyv::Deserialize, rkyv::Serialize,
)]
#[archive(check_bytes)]
#[archive_attr(derive(Debug))]
pub struct File {
    pub dist_info_metadata: bool,
    pub filename: String,
    pub hashes: Vec<HashDigest>,
    pub requires_python: Option<VersionSpecifiers>,
    pub size: Option<u64>,
    // N.B. We don't use a chrono DateTime<Utc> here because it's a little
    // annoying to do so with rkyv. Since we only use this field for doing
    // comparisons in testing, we just store it as a UTC timestamp in
    // milliseconds.
    pub upload_time_utc_ms: Option<i64>,
    pub url: FileLocation,
    pub yanked: Option<Yanked>,
}
```

IIRC `requires_python` and `yanked` are guaranteed to be the same across all files in a single package-version. URL almost always contains a lot of repeated information too, like the host (e.g., for PyPI, it always starts with `https://files.pythonhosted.org/packages/`).

I think we should be able to deduplicate a lot of this. It doesn't really matter for compressed data, but we store these with rkyv, so in theory savings should directly translate to improved performance?


---

_Label `performance` added by @charliermarsh on 2024-08-16 13:26_

---

_Comment by @notatallshaw on 2024-08-16 14:40_

> ... `yanked` are guaranteed to be the same across all files in a single package-version

FYI, definitely not "guaranteed", the [yank PEP](https://peps.python.org/pep-0592/) specifies only that a files are yanked, it mentions nothing about entire releases, e.g.

> The presence of a data-yanked attribute SHOULD be interpreted as indicating that the file pointed to by this particular link has been “Yanked”, and should not generally be selected by an installer, except under specific scenarios.

However PyPI has only ever implemented being able to yank entire releases, not individual files. I think though uv has made this assumption in its behaviour of dealing with yank, that yank will always be applied to an entire release.

But do be aware, currently nothing is stopping PyPI from updating to individual files, nor other indexes from implementing it on a per-file basis.

 


---

_Comment by @charliermarsh on 2024-08-16 14:45_

Thanks, you're right!

---

_Comment by @charliermarsh on 2024-08-16 14:55_

`Yanked` is the least impactful here anyway so we can always leave that as-is. I do think we _assume_ it's the same across distributions though, so might be better even to just encode that assumption for now.

---
