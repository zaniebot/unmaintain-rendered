```yaml
number: 15954
title: "Add a \"storage\" reference document"
type: pull_request
state: merged
author: zsol
labels:
  - documentation
assignees: []
merged: true
base: main
head: zsol/storage-docs
created_at: 2025-09-19T20:22:11Z
updated_at: 2025-11-17T14:38:16Z
url: https://github.com/astral-sh/uv/pull/15954
synced_at: 2026-01-12T16:12:02Z
```

# Add a "storage" reference document

---

_@zsol_

This is based on, and super**s**edes @zanieb's #13976 

---

_Review requested from @zanieb by @zsol on 2025-09-19 20:22_

---

_Marked ready for review by @zsol on 2025-09-19 20:28_

---

_Label `documentation` added by @zsol on 2025-09-19 20:28_

---

_Converted to draft by @zsol on 2025-09-19 20:29_

---

_Marked ready for review by @zsol on 2025-09-19 20:34_

---

_Assigned to @zanieb by @zanieb on 2025-09-20 02:15_

---

_Review comment by @geofft on `docs/reference/storage.md`:43 on 2025-10-03 13:37_

We use XDG for everything on macOS, no? (Like this sentence implies that we otherwise put things in ~/Library/sh.astral.uv or whatever, which I don't think we do.)

---

_Review comment by @geofft on `docs/reference/storage.md`:59 on 2025-10-03 13:38_

Probably worth not having two headings titled "Cache," especially since this one links to the other one.

---

_Review comment by @geofft on `docs/reference/storage.md`:89 on 2025-10-03 13:39_

I thought this is out of preview now, at least for the explicit-installation form e.g. `uv python install 3.14`.

---

_Review comment by @geofft on `docs/reference/storage.md`:46 on 2025-10-03 13:40_

It's probably worth adding something somewhere about how already-created virtual environments symlink to this directory, and so if you change this directory after having started to use uv, it will break venvs and require a sync, or something. 

---

_Review comment by @geofft on `docs/reference/storage.md`:195 on 2025-10-13 17:07_

I would combine these into one paragraph because that link is where the documentation for the format of this variable is (e.g. relative / absolute paths).

---

_@geofft reviewed on 2025-10-13 17:07_

---

_@konstin reviewed on 2025-10-14 10:24_

---

_Review comment by @konstin on `docs/reference/storage.md`:10 on 2025-10-14 10:24_

What about adding the table from https://github.com/astral-sh/uv/issues/11360#issuecomment-3228297622 as quick overview so you don't need to read the whole doc?

---

_@zanieb reviewed on 2025-10-17 14:55_

---

_Review comment by @zanieb on `docs/reference/storage.md`:43 on 2025-10-17 14:55_

Yeah we don't respect the system native macOS directories

---

_@zanieb reviewed on 2025-10-17 14:55_

---

_Review comment by @zanieb on `docs/reference/storage.md`:89 on 2025-10-17 14:55_

Yeah this is out of preview

---

_Comment by @zsol on 2025-11-02 04:03_

Thanks for all the feedback. I've rewritten the first section (directory strategies) based on it, including the excellent table @konstin mentioned, extending it a bit. I think the page is now more concise, less repetitive.

After some digging, I now think that `.uv` in the current directory is technically a fallback [in the code](https://github.com/astral-sh/uv/blob/ab186137e57f9b37298a73915c08f547f9ec0831/crates/uv-state/src/lib.rs#L95), I think that branch is unreachable because `uv_dirs::user_state_dir()` always returns an `Ok` value, so I removed that bit from the docs. We should get rid of it from the code, too, IMO.

---

_Comment by @konstin on 2025-11-04 16:37_

> After some digging, I now think that `.uv` in the current directory is technically a fallback [in the code](https://github.com/astral-sh/uv/blob/ab186137e57f9b37298a73915c08f547f9ec0831/crates/uv-state/src/lib.rs#L95), I think that branch is unreachable because `uv_dirs::user_state_dir()` always returns an `Ok` value, so I removed that bit from the docs. We should get rid of it from the code, too, IMO.

The code can fail if there's a `HomeDirError` cause the home dir function is fallible. From my experience there's always some minimal Docker or some Windows entreprise configuration where it fails, so a `.uv` fallback is convenient. 

---

_Review comment by @konstin on `docs/reference/storage.md`:10 on 2025-11-04 16:44_

```suggestion
For determining where to store different types of data, uv follows the [XDG](https://specifications.freedesktop.org/basedir-spec/latest/) conventions on Linux and macOS and the platform defaults on Windows, with XDG paths where Windows does not have defaults. Generally, it's best to configure these rather than each uv-specific storage location.
```

It kind of an awkward sentence cause XDG is kinda the platform default on Linux, except
* it's not defined by Linux but something people aligned on,
* we're using XDG on macOS over the macOS platform APIs because we want to treat it more like a Unix that follows XDG, which aligns better with dev usage,
* we're following the actual platform conventions on Windows, except where non exist (user-level binaries), there we follow the XDG spec. We also had a thread about respecting XDG on Windows when set, and falling back to the native APIs, but that didn't happen (yet).

---

_Review comment by @konstin on `docs/reference/storage.md`:16 on 2025-11-04 16:46_

I wouldn't call it fallback, `XDG_CACHE_HOME` is often not set, to me `~/.cache/uv` feels more like the default and `XDG_CACHE_HOME` like an override, but I would just document the order.

---

_Review comment by @konstin on `docs/reference/storage.md`:17 on 2025-11-04 19:10_

On a fresh install, it uses `%APPDATA%\uv` to e.g. install Pythons, we should document that as default location, the `%APPDATA%\uv\data` exists only not to break legacy paths.

---

_Review comment by @konstin on `docs/reference/storage.md`:34 on 2025-11-04 19:13_

```suggestion
[virtual environments](#project-environments) uv operates on.
```

---

_@konstin reviewed on 2025-11-04 19:16_

---

_@zsol reviewed on 2025-11-05 13:42_

---

_Review comment by @zsol on `docs/reference/storage.md`:17 on 2025-11-05 13:42_

Agreed. Do you think it's important to emphasize this nuance here? I think what the PR has currently is correct.

---

_Comment by @zsol on 2025-11-05 13:43_

Thanks! I addressed all of your feedback except for the one I replied to

---

_@konstin reviewed on 2025-11-05 14:05_

---

_Review comment by @konstin on `docs/reference/storage.md`:17 on 2025-11-05 14:05_

I'd document where a user can expect their data to go, and which levers they have to change this. So I'd put `%APPDATA%\uv`centrally: It's the location to find uv data, and changing `APPDATA` can change the location of uv data. `%APPDATA%\uv\data` should be more akin to a footnote, it something that may be used on a legacy setup, but never when setting something new up, or when changing the location manually. Currently, it reads to me as if `%APPDATA%\uv\data` is the main one, and `%APPDATA%\uv` is a fallback.

---

_@zsol reviewed on 2025-11-05 14:42_

---

_Review comment by @zsol on `docs/reference/storage.md`:17 on 2025-11-05 14:42_

OK, changed it. PTAL

---

_@konstin approved on 2025-11-05 15:02_

Thanks!

---

_Comment by @samypr100 on 2025-11-05 17:22_

Should we pull in the changes from https://github.com/astral-sh/uv/pull/16590 here?

---

_@zanieb reviewed on 2025-11-14 14:07_

---

_Review comment by @zanieb on `docs/reference/installer.md`:7 on 2025-11-14 14:07_

```suggestion
`XDG_DATA_HOME/../bin`. See [storage reference](./storage.md#executables) for details on these
```

---

_@zanieb reviewed on 2025-11-14 18:45_

---

_Review comment by @zanieb on `docs/reference/storage.md`:17 on 2025-11-14 18:45_

I omitted the legacy location entirely, that's really old functionality.

---

_Review comment by @zsol on `docs/reference/storage.md`:28 on 2025-11-17 11:04_

IMO these should be numbered to make the order of preference clear.

---

_@zsol reviewed on 2025-11-17 11:07_

---

_Merged by @zsol on 2025-11-17 14:38_

---

_Closed by @zsol on 2025-11-17 14:38_

---

_Branch deleted on 2025-11-17 14:38_

---
