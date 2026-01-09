---
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
synced_at: 2026-01-07T13:12:18-06:00
---

# Uploads fail due to setuptools using wrong metadata version for license-file

---

_Issue opened by @franzhaas on 2024-11-28 18:47_

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

_Referenced in [yehoshuadimarsky/bcpandas#219](../../yehoshuadimarsky/bcpandas/issues/219.md) on 2024-12-02 01:31_

---

_Comment by @SermetPekin on 2024-12-02 20:06_

```toml

[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
```
fixed my issue. as @kbrgl suggested

---

_Referenced in [replit/river-python#130](../../replit/river-python/pulls/130.md) on 2024-12-04 00:38_

---

_Comment by @senges on 2024-12-05 08:09_

If you still want to use setuptools as build backend, a simple workaround is to explicitly provide an empty value for `licence-files` in your `pyproject.toml`:
```toml
[tool.setuptools]
license-files = []
```

---

_Referenced in [pypa/setuptools#4759](../../pypa/setuptools/issues/4759.md) on 2024-12-17 06:35_

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

_Referenced in [tpgillam/mt2#71](../../tpgillam/mt2/pulls/71.md) on 2024-12-19 08:12_

---

_Referenced in [rst2pdf/rst2pdf#1257](../../rst2pdf/rst2pdf/pulls/1257.md) on 2024-12-24 17:09_

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

_Referenced in [gabrieldemarmiesse/python-on-whales#666](../../gabrieldemarmiesse/python-on-whales/pulls/666.md) on 2025-01-10 17:08_

---

_Referenced in [iiasa/ixmp#555](../../iiasa/ixmp/pulls/555.md) on 2025-01-16 10:42_

---

_Referenced in [rmorshea/python-copier-template#16](../../rmorshea/python-copier-template/pulls/16.md) on 2025-01-17 18:25_

---

_Referenced in [astral-sh/uv#10733](../../astral-sh/uv/issues/10733.md) on 2025-01-18 13:49_

---

_Referenced in [beancount/beangrow#38](../../beancount/beangrow/pulls/38.md) on 2025-01-26 20:24_

---

_Referenced in [astral-sh/uv#7839](../../astral-sh/uv/issues/7839.md) on 2025-01-28 15:45_

---

_Referenced in [astral-sh/uv#11032](../../astral-sh/uv/pulls/11032.md) on 2025-01-28 18:41_

---

_Referenced in [scalableminds/webknossos-libs#1254](../../scalableminds/webknossos-libs/pulls/1254.md) on 2025-02-04 10:31_

---

_Referenced in [BfArM-MVH/grz-tools#19](../../BfArM-MVH/grz-tools/pulls/19.md) on 2025-02-21 13:27_

---

_Referenced in [fpgmaas/cookiecutter-uv#42](../../fpgmaas/cookiecutter-uv/pulls/42.md) on 2025-02-23 12:34_

---

_Referenced in [corydolphin/flask-cors#380](../../corydolphin/flask-cors/pulls/380.md) on 2025-02-24 03:39_

---

_Referenced in [scikit-rf/scikit-rf#1260](../../scikit-rf/scikit-rf/pulls/1260.md) on 2025-02-26 19:59_

---

_Referenced in [elementsinteractive/twyn#192](../../elementsinteractive/twyn/pulls/192.md) on 2025-02-28 11:54_

---

_Referenced in [corydolphin/flask-cors#382](../../corydolphin/flask-cors/issues/382.md) on 2025-03-01 17:58_

---

_Referenced in [openclimatefix/ocf-data-sampler#205](../../openclimatefix/ocf-data-sampler/pulls/205.md) on 2025-03-11 15:36_

---

_Referenced in [astral-sh/uv#13631](../../astral-sh/uv/pulls/13631.md) on 2025-05-27 20:03_

---

_Renamed from "metadata version missmatch in uv 0.5.5" to "Uploads fail due to setuptools using wrong metadata version for license-file" by @konstin on 2025-08-21 08:00_

---

_Comment by @khaeru on 2025-11-27 15:19_

> This is a bug in setuptools: [pypa/setuptools#4759](https://github.com/pypa/setuptools/issues/4759)
> 
> uv unfortunately can't do anything about this.

The upstream bug has been fixed in a new release (see https://github.com/pypa/setuptools/issues/4759#issuecomment-3557496614), so perhaps this issue can be closed.

---

_Referenced in [iiasa/ixmp#614](../../iiasa/ixmp/pulls/614.md) on 2025-11-27 15:20_

---

_Closed by @konstin on 2025-11-27 15:24_

---
