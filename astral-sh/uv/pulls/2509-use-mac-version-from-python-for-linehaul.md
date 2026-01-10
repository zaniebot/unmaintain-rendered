```yaml
number: 2509
title: Use mac version from python for linehaul information
type: pull_request
state: merged
author: konstin
labels:
  - internal
assignees: []
merged: true
base: main
head: konsti/delete-mac-version
created_at: 2024-03-18T13:27:48Z
updated_at: 2024-03-20T09:55:51Z
url: https://github.com/astral-sh/uv/pull/2509
synced_at: 2026-01-10T14:49:08Z
```

# Use mac version from python for linehaul information

---

_Pull request opened by @konstin on 2024-03-18 13:27_

See https://github.com/astral-sh/uv/pull/2493#pullrequestreview-1942899151.

---

_Label `internal` added by @konstin on 2024-03-18 13:27_

---

_Comment by @samypr100 on 2024-03-18 13:43_

Note, it's not quite just major.minor, it should include the product version from the plist as-is based on my testing with pip. os_info got this right, hence why I used it for testing 😅

---

_Comment by @konstin on 2024-03-18 13:59_

Oh interesting, i'd need some mac user to test this then

---

_Comment by @trag1c on 2024-03-18 14:02_

🤔
```
λ ~ :: python -q
>>> import platform
>>> platform.mac_ver()[0]
'14.4'
```

---

_Comment by @samypr100 on 2024-03-18 14:04_

@trag1c platform.mac version can include more. You're in 14.4 which got released, so its only major.minor (currently) Once 14.4.1 gets released you'll get 14.4.1.

---

_Comment by @trag1c on 2024-03-18 14:06_

Yeah I'm aware (I just recently updated from 14.2.1, what a shame 😢), but I thought you were referring to even more data than just `major.minor[.patch]` 👍

---

_Comment by @samypr100 on 2024-03-18 14:12_

@konstin If we want to use python and be compatible, we should just use the raw output of platform.mac_ver()[0]. That should take care of the test failing right now.

---

_Comment by @trag1c on 2024-03-18 14:17_

> Yeah I'm aware (I just recently updated from 14.2.1, what a shame 😢), but I thought you were referring to even more data than just `major.minor[.patch]` 👍

I'm guessing beta releases could be different 🤔

---

_Comment by @konstin on 2024-03-18 14:32_

>  If we want to use python and be compatible, we should just use the raw output of platform.mac_ver()[0]. That should take care of the test failing right now.

Sounds good, though it needs a mac user as reviewer


---

_Comment by @zanieb on 2024-03-18 14:36_

What do you want me to test?

---

_Comment by @konstin on 2024-03-18 14:47_

As @samypr100 said, we should pass `platform.mac_ver()[0]` verbatim next to the parsed major/minor and use it in the linehaul information, but i don't have reference devices for `platform.mac_ver()[0]`

---

_Comment by @zanieb on 2024-03-18 14:52_

I get
```
❯ python3.12 -c "import platform; print(platform.mac_ver())"
('14.0', ('', '', ''), 'arm64')
```

---

_Comment by @samypr100 on 2024-03-18 15:07_

Here's what I get on an 14.3.1 mac

```
>  python -c 'import platform; print(platform.mac_ver())'
 ('14.3.1', ('', '', ''), 'arm64')
```

---

_Comment by @zanieb on 2024-03-18 17:41_

Let's just send `platform.mac_ver()[0]` for now and address problems if they come up?

---

_Comment by @samypr100 on 2024-03-18 23:01_

Agreed, that's exactly what Pip does too https://github.com/pypa/pip/blob/24.0/src/pip/_internal/network/session.py#L159 

---

_Comment by @zanieb on 2024-03-18 23:40_

I guess this isn't trivial because we're not currently storing patch versions on `Platform`. We'd need to add `patch: Option<u16>` at https://github.com/astral-sh/uv/blob/c296da34bffd19d41b895bd5aa06e78d79c71bd4/crates/platform-tags/src/platform.rs#L46 and populate the value at in our retrieval script at https://github.com/astral-sh/uv/blob/ecc46c5412128df8c01409481f6f8a4b63d855d1/crates/uv-interpreter/python/get_interpreter_info.py#L473

I'm not sure linehaul information justifies further changes here... patch macOS versions are not used afaik. We generate platform tags based on a major minor tuple (which is why we don't bother storing the optional patch version now).

I think we should consider documenting this caveat and moving on?

---

_Comment by @samypr100 on 2024-03-19 00:04_

@zanieb the code currently behaves as expected per #2493 and it's fully compatible with linehaul. No more changes are technically needed.
I believe this PR was to see if we could remove `mac_version.rs` introduced in #2493 and leverage the python value of `platform.mac_ver()[0]` instead (assuming it was already there retrieved in a compatible way) per question in https://github.com/astral-sh/uv/pull/2493#discussion_r1528491592.

Since it seems its not, I'd proposed we don't merge this change at this time to avoid sending invalid linehaul statistics which could be undesired.

---

_Comment by @zanieb on 2024-03-19 01:09_

I'd prefer to use all the information we're already getting from Python (as we do here). I feel like we should see if the patch version is actually important to them. It seems weird that they'd have e.g. `14.4` imply `14.4.0` in their statistics.

---

_Assigned to @zanieb by @zanieb on 2024-03-19 01:21_

---

_Comment by @samypr100 on 2024-03-19 01:38_

Fair, I don't think their parser cares and will use the value as-is, so I think it should be ok from a parser perspective.

My original concern was more that it could inflate statistics a bit biased towards versions without `patch`.

For example, say you have a 100 users using pip and uv where 50 of them have OSX version `14.3` and 50 have OSX version `14.3.1`. The download stats would capture this subtle difference with pip where as with uv it would show 100 users on 14.3.

For what it's worth, they do test with osx patch versions https://github.com/pypi/linehaul-cloud-function/blob/main/tests/unit/ua/fixtures/pip.yml.

Worth asking @pradyunsg, as the original issue creator, does having the patch version matter in OSX in distro?

---

_Comment by @pradyunsg on 2024-03-19 14:22_

I think it's OK to not have the patch version, but I'm also not a MacOS expert. 

---

_Comment by @zanieb on 2024-03-20 01:04_

I hope you don't mind I pushed https://github.com/astral-sh/uv/pull/2509/commits/637a9cee7643dfde27de520fa019a596fd5d4a7f in an attempt to resolve the error you were encountering.

We really ought to use snapshots for these lineinfo tests though.

---

_Comment by @konstin on 2024-03-20 09:55_

Thanks

I'm merging it now with the `<major>.<minor>` formatting for mac, it's easy enough to change later.

---

_Merged by @konstin on 2024-03-20 09:55_

---

_Closed by @konstin on 2024-03-20 09:55_

---

_Branch deleted on 2024-03-20 09:55_

---
