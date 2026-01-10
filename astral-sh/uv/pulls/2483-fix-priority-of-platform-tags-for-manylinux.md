```yaml
number: 2483
title: Fix priority of platform tags for manylinux
type: pull_request
state: merged
author: zanieb
labels:
  - bug
assignees: []
merged: true
base: main
head: zb/platform-tags-fix
created_at: 2024-03-15T20:51:08Z
updated_at: 2024-03-16T22:33:48Z
url: https://github.com/astral-sh/uv/pull/2483
synced_at: 2026-01-10T14:49:08Z
```

# Fix priority of platform tags for manylinux

---

_Pull request opened by @zanieb on 2024-03-15 20:51_

Closes https://github.com/astral-sh/uv/issues/2477

See also:
- #2489

---

_Label `bug` added by @zanieb on 2024-03-15 20:51_

---

_Review requested from @charliermarsh by @zanieb on 2024-03-15 20:53_

---

_Review requested from @konstin by @zanieb on 2024-03-15 20:53_

---

_@bdice reviewed on 2024-03-15 21:43_

---

_Review comment by @bdice on `crates/platform-tags/src/tags.rs`:553 on 2024-03-15 21:43_

Thanks so much for the quick fix.

Just to make sure I understand this -- does this ordering mean that legacy tags `manylinux1`, `manylinux2014`, and `manylinux2010` would be preferred over `manylinux_2_28`? That's not quite what I would expect.

Note that the "legacy" manylinux tags `manylinux1`, `manylinux2010`, and `manylinux2014` have corresponding glibc versions and should be prioritized like their corresponding "new glibc-style" tags.

https://peps.python.org/pep-0600/#legacy-manylinux-tags

> The existing manylinux tags are redefined as aliases for new-style tags:
>
>    manylinux1_x86_64 is now an alias for manylinux_2_5_x86_64
>    manylinux1_i686 is now an alias for manylinux_2_5_i686
>    manylinux2010_x86_64 is now an alias for manylinux_2_12_x86_64
>    manylinux2010_i686 is now an alias for manylinux_2_12_i686
>    manylinux2014_x86_64 is now an alias for manylinux_2_17_x86_64
>    manylinux2014_i686 is now an alias for manylinux_2_17_i686
>    manylinux2014_aarch64 is now an alias for manylinux_2_17_aarch64
>    manylinux2014_armv7l is now an alias for manylinux_2_17_armv7l
>    manylinux2014_ppc64 is now an alias for manylinux_2_17_ppc64
>    manylinux2014_ppc64le is now an alias for manylinux_2_17_ppc64le
>    manylinux2014_s390x is now an alias for manylinux_2_17_s390x


Concretely, we should expect this priority order:

`manylinux_2_28` > ... > `manylinux_2_17` == `manylinux2014` > `manylinux_2_16` > ... > `manylinux_2_12` == `manylinux2010` > `manylinux_2_11` > ... > `manylinux_2_5` == `manylinux1`

With the aliasing, I _think_ you want to prefer new-style over old style (which resolves any ambiguity in the `==` portions above). Typically wheels are tagged like `pyarrow-15.0.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl` and include both new-style and old-style tags if applicable.

---

_@bdice reviewed on 2024-03-15 21:47_

---

_Review comment by @bdice on `crates/platform-tags/src/tags.rs`:553 on 2024-03-15 21:47_

Please correct me if I'm reading this wrong, I'm basing this on my recollection of `pip` behavior and a quick skim of https://peps.python.org/pep-0600/#package-installers. Also I'm assuming this must be the desired behavior or else we'd be getting `pyarrow-15.0.1-cp312-cp312-manylinux_2_17_x86_64.manylinux2014_x86_64.whl` over `pyarrow-15.0.1-cp312-cp312-manylinux_2_28_x86_64.whl` due to the presence of the `manylinux2014` tag, _if_ that was supposed to take precedence.

---

_Review comment by @charliermarsh on `crates/platform-tags/src/tags.rs`:492 on 2024-03-15 22:27_

I need to read up on it, but I don't think this is quite what we want. We need to change the ordering of the `manylinux2010_` and friends statements too.

---

_@charliermarsh reviewed on 2024-03-15 22:27_

---

_@zanieb reviewed on 2024-03-15 22:37_

---

_Review comment by @zanieb on `crates/platform-tags/src/tags.rs`:553 on 2024-03-15 22:37_

Thanks for the details! I can futz with the ordering as you've described and I'll double check that we're matching the expected ordering.

---

_Renamed from "Fix platform tag ordering" to "Fix priority of platform tags for manylinux" by @zanieb on 2024-03-16 00:16_

---

_Comment by @zanieb on 2024-03-16 00:18_

I fixed the ordering of the abi tags, but I'm not sure where it's documented that `linux_x86_64` should take priority over `manylinux_<abi>_x86_64`. If someone has a link for that part of the specification I'd appreciate it — I can add it to the code.

Looks correct per https://github.com/pypa/packaging/blob/fd4f11139d1c884a637be8aa26bb60a31fbc9411/packaging/tags.py#L434-L444

---

_Comment by @charliermarsh on 2024-03-16 14:04_

I think it might be the other way around, I think higher priority tags come first in that code, so `linux_x86_64` would be last? That's at least my read of the code, but I didn't test it.

---

_Comment by @charliermarsh on 2024-03-16 14:05_

You can test by comparing to:

```python
from packaging import tags

for tag in tags.sys_tags(): print(tag)
```

---

_@charliermarsh reviewed on 2024-03-16 14:06_

---

_Review comment by @charliermarsh on `crates/platform-tags/src/tags.rs`:368 on 2024-03-16 14:06_

Doesn't `manylinux2014` need to be greater than `manylinux2010`, and also greater than `manylinux_2_12`? I think these legacy tags need to be interspersed.

---

_@zanieb reviewed on 2024-03-16 15:59_

---

_Review comment by @zanieb on `crates/platform-tags/src/tags.rs`:368 on 2024-03-16 15:59_

Eek okay this was more wrong than I thought to start..

---

_Comment by @zanieb on 2024-03-16 16:13_

Apologies for all the back and forth apparently this needed a more rigorous look :) 

I've addressed the last round of comments. I might look into a better way to test we match Python's behavior.

---

_Comment by @zanieb on 2024-03-16 16:25_

It looks like our ordering of ABI tags is wrong, i.e. Python does

```
cp311-cp311-manylinux_2_28_x86_64
...
cp311-cp311-manylinux_2_17_x86_64
cp311-cp311-manylinux2014_x86_64
cp311-cp311-manylinux_2_16_x86_64
...
cp311-cp311-manylinux_2_12_x86_64
cp311-cp311-manylinux2010_x86_64
cp311-cp311-manylinux_2_11_x86_64
...
cp311-cp311-manylinux_2_5_x86_64
cp311-cp311-manylinux1_x86_64
cp311-cp311-linux_x86_64
cp311-abi3-manylinux_2_28_x86_64
...
cp311-abi3-manylinux_2_17_x86_64
cp311-abi3-manylinux2014_x86_64
cp311-abi3-manylinux_2_16_x86_64
...
cp311-abi3-manylinux_2_12_x86_64
cp311-abi3-manylinux2010_x86_64
cp311-abi3-manylinux_2_11_x86_64
...
cp311-abi3-manylinux_2_5_x86_64
cp311-abi3-manylinux1_x86_64
cp311-abi3-linux_x86_64
cp311-none-manylinux_2_28_x86_64
...
cp311-none-manylinux_2_17_x86_64
cp311-none-manylinux2014_x86_64
cp311-none-manylinux_2_16_x86_64
...
cp311-none-manylinux_2_12_x86_64
cp311-none-manylinux2010_x86_64
cp311-none-manylinux_2_11_x86_64
...
cp311-none-manylinux_2_5_x86_64
cp311-none-manylinux1_x86_64
cp311-none-linux_x86_64
cp310-abi3-manylinux_2_28_x86_64
...
```

but we do

```
cp311_cp311_manylinux_2_28_x86_64
cp311_none_manylinux_2_28_x86_64
...
cp311_cp311_manylinux_2_17_x86_64
cp311_none_manylinux_2_17_x86_64
cp311_cp311_manylinux2014_x86_64
cp311_none_manylinux2014_x86_64
cp311_cp311_manylinux_2_16_x86_64
cp311_none_manylinux_2_16_x86_64
...
cp311_cp311_manylinux_2_12_x86_64
cp311_none_manylinux_2_12_x86_64
cp311_cp311_manylinux2010_x86_64
cp311_none_manylinux2010_x86_64
cp311_cp311_manylinux_2_11_x86_64
cp311_none_manylinux_2_11_x86_64
...
cp311_cp311_manylinux_2_5_x86_64
cp311_none_manylinux_2_5_x86_64
cp311_cp311_manylinux1_x86_64
cp311_none_manylinux1_x86_64
cp311_cp311_linux_x86_64
cp311_none_linux_x86_64
cp311_abi3_manylinux_2_28_x86_64
...
cp311_abi3_manylinux_2_17_x86_64
cp311_abi3_manylinux2014_x86_64
cp311_abi3_manylinux_2_16_x86_64
...
cp311_abi3_manylinux_2_12_x86_64
cp311_abi3_manylinux2010_x86_64
cp311_abi3_manylinux_2_11_x86_64
...
cp311_abi3_manylinux_2_5_x86_64
cp311_abi3_manylinux1_x86_64
cp311_abi3_linux_x86_64
cp310_abi3_manylinux_2_28_x86_64
...
```

This is a different part of the code though, I'll address in a second pull request.

---

_Comment by @charliermarsh on 2024-03-16 16:32_

Oh interesting… they might be mutually exclusive such that it hasn’t mattered? Hard to reason about from my phone :D Thank you for digging into this!

---

_Comment by @zanieb on 2024-03-16 17:36_

I'm no expert, just mindlessly matching Python's behavior at this point. See #2489 for the ABI changes.

I'll wait for @konstin to review both of these.

---

_@charliermarsh approved on 2024-03-16 18:05_

Great!

---

_@charliermarsh approved on 2024-03-16 18:06_

I’m comfortable signing off on this if you want to merge.

---

_@bdice approved on 2024-03-16 18:49_

Thanks, this looks right to me too.

---

_Merged by @zanieb on 2024-03-16 22:33_

---

_Closed by @zanieb on 2024-03-16 22:33_

---

_Branch deleted on 2024-03-16 22:33_

---
