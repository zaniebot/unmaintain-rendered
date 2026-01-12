```yaml
number: 2954
title: "globset: Support paths already present as [u8] (e.g. typed_path::Path)"
type: issue
state: closed
author: aawsome
labels:
  - rollup
assignees: []
created_at: 2024-12-24T01:08:49Z
updated_at: 2025-09-20T01:08:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2954
synced_at: 2026-01-12T16:13:25Z
```

# globset: Support paths already present as [u8] (e.g. typed_path::Path)

---

_@aawsome_

Hi!

I'd like to use `globset` to match paths - but I like to use [`typed_path::Path`](https://docs.rs/typed-path/latest/typed_path/struct.Path.html) for them. This means they are already represented as `[u8]` on windows and unix platforms. Unfortunately, matching is currently only supported for `AsRef<Path>` and `Candidate` which can itself only be created from `AsRef<Path>`.

I could convert `typed_path::Path` into a `std::pathPath`/`std::path::PathBuf` but this is ugly because it will internally convert back and forth and (more annoying) the conversation may fail (e.g. non-UTF parts on windows).

Is it possible to add support for `AsRef<[u8]>` or something similar?

Thanks in advance!

---

_Comment by @BurntSushi on 2024-12-24 14:37_

`globset` itself uses `&[u8]` internally, so in theory, this shouldn't be much of a problem. But you can't ask for either `AsRef<[u8]>` or `AsRef<Path>` in the same API without a new trait, which I have no intent of doing. Which means you need another API for it.

However... it looks like [`typed_path::Path` implements `AsRef<OsStr>`](https://docs.rs/typed-path/latest/typed_path/struct.Path.html#impl-AsRef%3COsStr%3E-for-Path%3CT%3E)? Although, I guess that's only on Unix. But at least on Unix, it looks like you can get an `&OsStr` and thus a `&Path` with zero cost.

I might revisit this in a `globset 0.5` release (no idea when that will happen), but for now, I think you might need to just deal with the conversion (which _should_ be zero cost on Unix).

---

_Comment by @aawsome on 2024-12-24 19:25_

Thanks @BurntSushi for your fast response!

I must say that I am not very satisfied with your proposal :-/ The idea of using `typed_path::Path` is to have treatment of unix-style paths under unix *and* windows without the need for platform-specific code. Also, as mentioned, using a non-valid path on windows leads to extra trouble which I don't see how to fix, actually..

About API: A `Candidate::from_bytes<P: AsRef<[u8]>(path: P) -> Self` (replace naming if you find something more suited) would be sufficient from my point of view. I agree that having traits such that the matching methods would run with both `AsRef<std::path::Path>` and `AsRef<[u8]>` would be even more elegant.

I however respect if you don't want to change your API. In that case I'd actually prefer to maintain a fork which provides that functionality (which seems to be the lesser evil to me compared to the conversion hassle when coding platform-agnostic)

Merry Christmas!

---

_Comment by @BurntSushi on 2024-12-24 19:36_

I think I'd be open to a `Candidate::from_bytes` constructor. That's pretty unobtrusive.

Note that the encoding of the bytes still has to be conventionally UTF-8, even if it isn't fully valid. So, e.g., WTF-8 would be okay. But you'll get nonsense if you pass a `&[u8]` that is UCS-2 or UTF-16.

---

_Label `rollup` added by @BurntSushi on 2025-08-17 18:15_

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
