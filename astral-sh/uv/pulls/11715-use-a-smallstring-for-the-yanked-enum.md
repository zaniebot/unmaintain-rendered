```yaml
number: 11715
title: "Use a `SmallString` for the Yanked enum"
type: pull_request
state: merged
author: charliermarsh
labels:
  - performance
assignees: []
merged: true
base: main
head: charlie/p-6
created_at: 2025-02-22T22:14:19Z
updated_at: 2025-02-24T19:04:02Z
url: https://github.com/astral-sh/uv/pull/11715
synced_at: 2026-01-12T16:09:57Z
```

# Use a `SmallString` for the Yanked enum

---

_@charliermarsh_

## Summary

This is stored on `File`, which we create extensively. Easy way to reduce size.


---

_Label `performance` added by @charliermarsh on 2025-02-22 22:14_

---

_@konstin approved on 2025-02-24 16:10_

Should we use a `Option<Box<Yanked>>` on `File`? This should be even smaller (`usize`) and yanked versions are rare.

---

_Comment by @charliermarsh on 2025-02-24 18:45_

We should validate that that’s smaller, since SmallString is also a single word (but Yanked might be two now with discriminant).

---

_Comment by @charliermarsh on 2025-02-24 18:59_

I can check and change if necessary before merging; no need for you to spend more time on it.

---

_Comment by @konstin on 2025-02-24 19:00_

I've confirmed that that's the case on x86_64 with a test (below). The background is that even though `Arc` (and its variant such as `ArcStr`) are a pointer, and an x86_64 pointer only uses 48 bits of its 64 bits (leaving us 16 bit), rustc only sees a `NonNull` with a single niche (Null). `Yanked` needs at least two niches to represent `Yanked::Bool`, so it doesn't fit and we get padded to two words. In the experiments I made, all instances of enums where one variant contained a pointer and there was more than a single other variant without (like `Option<T>`), the enum would get padded to two words.

I don't feel strongly about what we do with `File` here, its 432 vs 424 bytes, this is for context.

```rust
    #[test]
    fn yanked_size() {
        assert_eq!(size_of::<Option<Yanked>>(), size_of::<usize>() * 2);
        assert_eq!(size_of::<Option<Box<Yanked>>>(), size_of::<usize>());
    }

    #[test]
    #[cfg(target_pointer_width = "64")]
    fn file_size() {
        assert_eq!(size_of::<Option<crate::File>>(), 432);
        #[derive(Debug, Clone, Deserialize)]
        #[serde(rename_all = "kebab-case")]
        pub struct File2 {
            // PEP 714-renamed field, followed by PEP 691-compliant field, followed by non-PEP 691-compliant
            // alias used by PyPI.
            pub core_metadata: Option<CoreMetadata>,
            pub dist_info_metadata: Option<CoreMetadata>,
            pub data_dist_info_metadata: Option<CoreMetadata>,
            pub filename: String,
            pub hashes: Hashes,
            /// There are a number of invalid specifiers on PyPI, so we first try to parse it into a
            /// [`VersionSpecifiers`] according to spec (PEP 440), then a [`LenientVersionSpecifiers`] with
            /// fixup for some common problems and if this still fails, we skip the file when creating a
            /// version map.
            #[serde(
                default,
                deserialize_with = "crate::simple_json::deserialize_version_specifiers_lenient"
            )]
            pub requires_python: Option<Result<VersionSpecifiers, VersionSpecifiersParseError>>,
            pub size: Option<u64>,
            pub upload_time: Option<Timestamp>,
            pub url: String,
            pub yanked: Option<Box<Yanked>>,
        }
        assert_eq!(size_of::<File2>(), 424);
    }
```

---

_Comment by @charliermarsh on 2025-02-24 19:03_

I'll change separately.

---

_Merged by @charliermarsh on 2025-02-24 19:03_

---

_Closed by @charliermarsh on 2025-02-24 19:03_

---

_Branch deleted on 2025-02-24 19:04_

---
