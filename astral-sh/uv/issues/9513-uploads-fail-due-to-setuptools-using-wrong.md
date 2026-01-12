```yaml
number: 9513
title: Uploads fail due to setuptools using wrong metadata version for license-file
type: issue
state: closed
author: franzhaas
labels:
  - external
assignees: []
created_at: 2024-11-28T18:47:10Z
updated_at: 2025-11-27T15:24:25Z
url: https://github.com/astral-sh/uv/issues/9513
synced_at: 2026-01-12T15:59:52Z
```

# Uploads fail due to setuptools using wrong metadata version for license-file

---

_@franzhaas_

Dear all,

I got this error when building and uploading my wheel with 0.5.5 to pypi.org.:

```
Upload failed with status code 400 Bad Request. Server says: 400 license-file introduced in metadata version 2.4, not 2.1. See https://packaging.python.org/specifications/core-metadata for more information.
```

But it does work for me with 0.5.4.

Accoridng to the release notes, with 0.5.5 uv started to use license-file, however I did not see a mention of moving the metadata version...

Any ideas?

Thanks in advance,
Franz



---

_Comment by @konstin on 2024-11-28 19:03_

This is probably #9442: It's correct that you can only use `license-file` with metadata of at least version 2.4, but we were missing that field in the upload payload previously so pypi's validation didn't work. What build backend are you using? 

---

_Comment by @franzhaas on 2024-11-28 19:06_

Hi Konstin,

I use.:
```toml
[build-system]
requires = ["setuptools", "wheel", "setuptools-scm"]
build-backend = "setuptools.build_meta"
```
If it helps, the full project is located here.: https://github.com/MarketSquare/robotframework-construct

---

_Comment by @FHU-yezi on 2024-11-29 03:24_

I got the same error with `uv build` used in building my project.

Here is the repo: https://github.com/FHU-yezi/sshared

The failed publish GitHub Action flow: https://github.com/FHU-yezi/sshared/actions/runs/12078435589/job/33682841905

It is a pure Python package and I got this error after updated to the latest uv version.

---

_Comment by @konstin on 2024-11-29 10:41_

This is a bug in setuptools: https://github.com/pypa/setuptools/issues/4759

uv unfortunately can't do anything about this.

---

_Label `upstream` added by @konstin on 2024-11-29 10:41_

---

_Comment by @franzhaas on 2024-11-29 10:49_

Ok thanks. Any ideas how to have dynamic versioning without setuptools?

konsti ***@***.***> schrieb am Fr., 29. Nov. 2024, 11:42:

> This is a bug in setuptools: pypa/setuptools#4759
> <https://github.com/pypa/setuptools/issues/4759>
>
> uv unfortunately can't do anything about this.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/9513#issuecomment-2507543694>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/ACXSBILVY5IYIXHUHCKUNST2DBAH5AVCNFSM6AAAAABSVWJU6KVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDKMBXGU2DGNRZGQ>
> .
> You are receiving this because you authored the thread.Message ID:
> ***@***.***>
>


---

_Comment by @my1e5 on 2024-11-29 11:51_

@franzhaas [versioningit](https://github.com/jwodder/versioningit) works with setuptools and also hatchling.

---

_Comment by @franzhaas on 2024-11-29 19:12_

I did not try to upload, but when building with hatchling, I ended up using metadata 2.3... for both uv 0.5.4 and 0.5.5...

this might be related to this one.:
https://github.com/pypa/hatch/issues/1819


---

_Comment by @SermetPekin on 2024-12-02 20:06_

```toml

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
fixed my issue. as @kbrgl suggested

---

_Comment by @senges on 2024-12-05 08:09_

If you still want to use setuptools as build backend, a simple workaround is to explicitly provide an empty value for `licence-files` in your `pyproject.toml`:
```toml
[tool.setuptools]
license-files = []
```

---

_Comment by @hauntsaninja on 2024-12-17 06:50_

Okay, here's what I don't understand...
There is is a setuptools bug, but there's also something funky going on with uv.
I removed all mentions of license from my pyproject.toml and `rm -rf build dist; uv build; uv publish` gave me the same error. I then did `rm -rf build dist; python -m build . ; python -m twine upload dist/*` and everything works just fine. What gives?

Does uv build do any caching of metadata somewhere?

---

_Comment by @FHU-yezi on 2024-12-17 07:47_

> Okay, here's what I don't understand...
> Maybe this is a setuptools bug, but there's something funky going on with uv.
> I removed all mentions of license from my pyproject.toml and `rm -rf build dist; uv build; uv publish` gave me the same error. I then did `rm -rf build dist; python -m build . ; python -m twine upload dist/*` and everything works just fine. What gives?
> 
> Does uv build do any caching of metadata somewhere?

Use hatchling instead of setuptools should solve this problem, you can find guide about this in uv documentation.

---

_Comment by @hauntsaninja on 2024-12-17 07:55_

Thanks, I understand that's a valid workaround :-) Just based on the sequence of commands I ran, it appears there's also some issue in uv (maybe just a caching issue?) in addition to the one in setuptools

---

_Comment by @konstin on 2024-12-17 08:25_

@hauntsaninja:

PEP 639 adds two new fields to the wheel metadata, `License-Expression` and `License-File`, with the corresponding `pyproject.toml` fields `license = "..."` and `license-files = [...]`. These fields are added in the new metadata version 2.4.

When uploading a wheel, the uploader reads the `METADATA` file from the wheel and transforms it to formdata. PyPI has recently started validating the two new metadata fields from PEP 639 in the formdata, thought PyPI does not validate the `METADATA`. Twine hasn't implemented that PEP yet (https://github.com/pypa/twine/pull/1180), so it does not set the new license fields in the formdata, and PyPI does not catch the invalid metadata (declaring metadata version 2.1 while using version 2.4 fields).

---

_Comment by @cdce8p on 2024-12-17 12:21_

> When uploading a wheel, the uploader reads the `METADATA` file from the wheel and transforms it to formdata. PyPI has recently started validating the two new metadata fields from PEP 639 in the formdata, thought PyPI does not validate the `METADATA`. Twine hasn't implemented that PEP yet ([pypa/twine#1180](https://github.com/pypa/twine/pull/1180)), so it does not set the new license fields in the formdata, and PyPI does not catch the invalid metadata (declaring metadata version 2.1 while using version 2.4 fields).

The twine PR was just merged. It also added a carveout for setuptools to exclude `License-File` from the metadata if the version is `<2.4` as to not break all projects which use setuptools. Maybe this is something which could be added to `uv` as well? https://github.com/pypa/twine/blob/main/twine/package.py#L225-L234

The setuptools behavior has been in place for so long at this point that it's probably not worth changing until PEP 639 is fully implemented. I've PRs open for that but those depend on some other work which need to happen first and will likely still need some time.

> If you still want to use setuptools as build backend, a simple workaround is to explicitly provide an empty value for `licence-files` in your `pyproject.toml`:
> 
> [tool.setuptools]
> license-files = []

Yes, this is a workaround. However, I'd advise against it. It will not only remove the `License-File` metadata but also the license files from the distribution. You can include them manually via `MANIFEST.in` but they will still be excluded from the wheel (unless placed inside the project package itself).

The ideal case would really be a workaround added to `uv` just like in `twine`.

---

_Comment by @PeriniM on 2025-01-03 12:46_

I had [semantic-release](https://www.npmjs.com/package/@semantic-release/npm) set up and suddently it stopped working. The problem was that semrel does not support Metadata v2.4 and therefore the fix was to bump the hatchling version:

```bash
[build-system]
requires = ["hatchling==1.26.3"]
build-backend = "hatchling.build"

```

Error screenshot:
![Image](https://github.com/user-attachments/assets/090561f1-3864-4361-b397-b146aa05ced6)

Hope it helps!

---

_Renamed from "metadata version missmatch in uv 0.5.5" to "Uploads fail due to setuptools using wrong metadata version for license-file" by @konstin on 2025-08-21 08:00_

---

_Comment by @khaeru on 2025-11-27 15:19_

> This is a bug in setuptools: [pypa/setuptools#4759](https://github.com/pypa/setuptools/issues/4759)
> 
> uv unfortunately can't do anything about this.

The upstream bug has been fixed in a new release (see https://github.com/pypa/setuptools/issues/4759#issuecomment-3557496614), so perhaps this issue can be closed.

---

_Closed by @konstin on 2025-11-27 15:24_

---
