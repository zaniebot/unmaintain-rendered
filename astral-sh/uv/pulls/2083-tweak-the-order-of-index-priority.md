```yaml
number: 2083
title: tweak the order of index priority
type: pull_request
state: merged
author: BurntSushi
labels:
  - enhancement
assignees: []
merged: true
base: main
head: ag/multi-registry
created_at: 2024-02-29T15:16:33Z
updated_at: 2024-09-03T15:11:04Z
url: https://github.com/astral-sh/uv/pull/2083
synced_at: 2026-01-12T16:04:51Z
```

# tweak the order of index priority

---

_@BurntSushi_

Previously, `uv` would always prioritize the index given by
`--index-url`. It would then try any indexes after that given by zero
or more `--extra-index-url` flags. This differed from `pip` in that any
priority was given at all, where `pip` doesn't guarantee any priority
ordering of indexes.

We could go in the direction of mimicing `pip`'s behavior here, but it
at present has issues with dependency confusion attacks where packages
may get installed from indexes you don't control. More specifically,
there is an issue of different trust levels. See discussion in #171 and
[PEP-0708] for more on the security impact.

In contrast, `uv` will only select versions for a package from a single
index. That is, even if `foo` is in indexes `a` and `b`, it will
only consider the versions from the index that it checks first. This
probably helps with respect to dependency confusion attacks, but also
means that `uv` doesn't quite cover all of the same use cases as `pip`.

In this PR, we retain the notion of prioritizing indexes, but
tweak it so that PyPI is preferred last as opposed to first. Or
more precisely, the `--index-url` flag specifies a fallback index,
not the primary index, and is deprioritized beneath every index
specified by `--extra-index-url`. The ordering among indexes given by
`--extra-index-url` remains the same: earlier indexes are prioritized
over later indexes.

While this tweak likely won't hit all use cases, I believe it will
resolve some of the most common pain points without exacerbating
dependency confusion problems.

Ref #171, Fixes #1377, Fixes #1451, Fixes #1600

[PEP-0708]: https://peps.python.org/pep-0708/


---

_Review requested from @charliermarsh by @BurntSushi on 2024-02-29 15:16_

---

_Review requested from @konstin by @BurntSushi on 2024-02-29 15:16_

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_install.rs`:833 on 2024-02-29 15:17_

I feel a little bad about doing this. I'm happy to try a different approach if folks have ideas.

It feels like maybe the better longer term solution is to add multi-index support to packse? (I briefly looked to see if it had it already, and it doesn't look like it does.)

---

_@BurntSushi reviewed on 2024-02-29 15:17_

---

_@charliermarsh reviewed on 2024-02-29 15:42_

---

_Review comment by @charliermarsh on `crates/uv/tests/pip_install.rs`:833 on 2024-02-29 15:42_

Could we instead use `https://download.pytorch.org/whl`, since we already use that in other tests (so we avoid adding yet another point of failure)?

---

_@BurntSushi reviewed on 2024-02-29 16:28_

---

_Review comment by @BurntSushi on `crates/uv/tests/pip_install.rs`:833 on 2024-02-29 16:28_

Thanks for the nudge. I managed to find an example that worked with test.pypi.org.

---

_Merged by @BurntSushi on 2024-02-29 16:57_

---

_Closed by @BurntSushi on 2024-02-29 16:57_

---

_Branch deleted on 2024-02-29 16:57_

---

_Comment by @zanieb on 2024-02-29 17:04_

I believe this helps with dependency confusion attacks. If I'm providing an extra index URL that's usually the index that _I_ control. The attack would be that someone uploads a package with the same name to the primary index (i.e. PyPI) and suddenly we use that package instead of the one I intend to.

---

_Label `enhancement` added by @zanieb on 2024-02-29 17:04_

---

_Comment by @jarshwah on 2024-03-01 11:46_

> If I'm providing an extra index URL that's usually the index that I control

As a counter-point (not suggesting this is the most common) - we use extra-index-url to install a vendor library (we pay them for access), and the index is horribly slow.

---

_Comment by @adrienball on 2024-03-01 15:08_

I just ran into an issue which is caused by this change.
My company uses a private index, and thus I have some script which is run with `UV_EXTRA_INDEX_URL=<private-index-url>`. 
The following issue happened when `uv venv --seed` is run inside the aforementioned script:
```console
#15 0.746 Creating virtualenv at: /app/.venv
#15 0.775 uv::venv::seed
#15 0.775 
#15 0.775   × Failed to install seed packages
#15 0.775   ├─▶ No solution found when resolving: pip, setuptools, wheel
#15 0.775   ├─▶ Failed to download and build: pip==20.2.4
#15 0.775   ╰─▶ Building source distributions is disabled
```

After digging a bit, and trying to understand why this old`pip==20.2.4` was getting installed, I found that someone uploaded an old version of `pip` into the private index.
I don't know if the error results from an invalid pip wheel on the private index, or some version incompatibilities, but it made me realize that as soon as someone publishes a version of a public package in our private index, then it will potentially break all libs or apps which depend on a different version of the package.

Is there any plan to mitigate this issue ?

Thanks !

---

_Comment by @charliermarsh on 2024-03-01 15:25_

For clarity, you can always revert to the previous behavior by inverting the order:

```
--index-url=<private-index-url> --extra-index-url=https://pypi.org/simple
```

This would tell uv to look in PyPI first, and then only fall back to your index if a package doesn't exist on PyPI.


---

_Comment by @xnox on 2024-09-03 15:11_

How does this interact with --find-links?

---
