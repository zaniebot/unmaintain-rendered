```yaml
number: 8203
title: "Publish: Warn about unnormalized filenames"
type: pull_request
state: closed
author: konstin
labels:
  - enhancement
  - preview
assignees: []
base: main
head: konsti/warn-unnormalized-wheel-filenames
created_at: 2024-10-15T09:00:51Z
updated_at: 2025-03-10T23:37:27Z
url: https://github.com/astral-sh/uv/pull/8203
synced_at: 2026-01-12T16:08:12Z
```

# Publish: Warn about unnormalized filenames

---

_@konstin_

The spec requires normalizing the name and the version in distribution filenames (https://packaging.python.org/en/latest/specifications/binary-distribution-format/#escaping-and-unicode). Missing normalization caused #8030, so we warn about it. Ideally, we would error, but this would break setuptools (https://github.com/pypa/setuptools/issues/3777)

---

_Label `enhancement` added by @konstin on 2024-10-15 09:00_

---

_Label `preview` added by @konstin on 2024-10-15 09:00_

---

_Comment by @charliermarsh on 2024-10-15 12:04_

I'm torn on this because as far as I can tell there isn't any remedy available to the user?

---

_Comment by @konstin on 2024-10-15 12:22_

I believe it's the only responsible course of action: It would be irresponsible to publish invalid wheels without comment, but given the setuptools bug, we can't make it an error yet.

---

_Comment by @charliermarsh on 2024-10-15 12:25_

So what's the exact course of events here. The user has a package with a dot in the name, `setuptools` creates a wheel with an invalid name (includes the dot), PyPI _accepts_ it, and then what?

---

_Comment by @Avasam on 2024-10-15 12:32_

What about incorrect capitalization? https://github.com/astral-sh/uv/issues/8030#issuecomment-2403294235

Should that also be tested for for completeness ?

---

_Comment by @Avasam on 2024-10-15 12:34_

> I'm torn on this because as far as I can tell there isn't any remedy available to the user?

When I first hit the issue, my publish command failed and I was able to manually fix the folder name in the whl file to be normalized.


---

_Comment by @konstin on 2024-10-15 12:41_

Currently, PyPI accepts the legacy form with unnormalized wheel filenames. It seems to just split wheel filenames on dashes (https://github.com/pypi/warehouse/blob/50a58f3081e693a3772c0283050a275e350004bf/warehouse/forklift/legacy.py#L1133-L1155). This is a gap in PyPI's wheel checking. PyPI regularly tightens their checks to follow the standards more closely, e.g. https://github.com/pypi/warehouse/pull/15795 fixed the check for the version in source dist filenames. We're a bit ahead of the curve, but I'd rather emit warnings now than to quietly publish spec-breaking wheels and/or wait until PyPI starts rejecting such packages. I'd hope that setuptools fixes this on their end, which should unblock PyPI from adding such a check.

> Should that also be tested for for completeness ?

Good point, I've added a dedicated test case.

> My publish command failed and I was able to manually fix the folder name in the whl file to be normalized.

I've added a workaround in #8204 that should fix publishing without manual edits. This PR is basically adding a mechanism of complaining that what #8204 does is a hack around missing standards compliance in the build backend.

---

_Comment by @charliermarsh on 2024-10-15 12:42_

> We're a bit ahead of the curve, but I'd rather emit warnings now than to quietly publish spec-breaking wheels and/or wait until PyPI starts rejecting such packages. I'd hope that setuptools fixes this on their end, which should unblock PyPI from adding such a check.

But when PyPI accepts such a wheel, what's the impact on consumers of the package?

---

_Comment by @konstin on 2024-10-15 12:52_

Currently, there is no impact that I'd know of, since all resolver and installers I'm aware of accept the legacy style, too (there are packages that were publish before these normlization rules were written, and the spec says tools need to take care of this legacy style). I had hoped that following the spec would be sufficient grounds to add a warning.

---

_Comment by @konstin on 2025-03-10 23:03_

setuptools is now [PEP 625](https://peps.python.org/pep-0625/) compliant.

---

_Closed by @konstin on 2025-03-10 23:03_

---

_Comment by @Avasam on 2025-03-10 23:37_

If I'm not mistaken, it's specifically since [v75.8.1](https://setuptools.pypa.io/en/latest/history.html#v75-8-1) thanks to https://github.com/pypa/setuptools/pull/4766

---
