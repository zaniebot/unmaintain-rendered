```yaml
number: 7335
title: "Support pre-releases in Python version requests - `command --python <major.minor.pre-release>`"
type: pull_request
state: merged
author: mikeleppane
labels: []
assignees: []
merged: true
base: main
head: feat(pre-release)/support-pre-releases-in-python-version-requests
created_at: 2024-09-12T16:06:44Z
updated_at: 2024-09-17T14:46:47Z
url: https://github.com/astral-sh/uv/pull/7335
synced_at: 2026-01-12T16:07:47Z
```

# Support pre-releases in Python version requests - `command --python <major.minor.pre-release>`

---

_@mikeleppane_

## Summary

This PR adds support to include Python pre-releases when requesting versions. 
Check out the docs for commands that support the `Python` option: 
```text
--python, -p python
The Python interpreter to use for the virtual environment.
```
At least the following scenarios are supported:
```bash
3.13.0a1
3.13b2
3.13rc4
313rc1
```

## Test Plan

I added a basic unit test to `uv/crates/uv-python/src/discovery.rs`. I could have added more, but I have not discovered any relevant places.

CI passes

Note: I was unable to execute the entire test set locally. There were at least some timeout issues (some tests took over 60 seconds). 

========== output ===========
beta version
```bash
cargo run -- venv --python 3.13.0b3                                                                                                           ░▒▓ 94% 󰁹
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.20s
     Running `target/debug/uv venv --python 3.13.0b3`
Using Python 3.13.0b3 interpreter at: /home/mikko/.pyenv/versions/3.13.0b3/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
````
release candidate
```bash
 cargo run -- venv --python 3.13.0rc2                                                                                                          ░▒▓ 94% 󰁹
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.83s
     Running `target/debug/uv venv --python 3.13.0rc2`
Using Python 3.13.0rc2 interpreter at: /home/mikko/.pyenv/versions/3.13.0rc2/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```

```bash
cargo run -- venv --python 313rc2                                                                                                             ░▒▓ 94% 󰁹
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 0.31s
     Running `target/debug/uv venv --python 313rc2`
Using Python 3.13.0rc2 interpreter at: /home/mikko/.pyenv/versions/3.13.0rc2/bin/python3
Creating virtualenv at: .venv
Activate with: source .venv/bin/activate
```



---

_Renamed from "Support pre-releases in Python version requests" to "Draft: Support pre-releases in Python version requests" by @mikeleppane on 2024-09-12 16:07_

---

_@zanieb reviewed on 2024-09-12 16:49_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1560 on 2024-09-12 16:49_

Should we show the pre-release as part of the "was requested" bit here?

---

_@mikeleppane reviewed on 2024-09-12 17:28_

---

_Review comment by @mikeleppane on `crates/uv-python/src/discovery.rs`:1560 on 2024-09-12 17:28_

Absolutely! 

---

_Renamed from "Draft: Support pre-releases in Python version requests" to "Support pre-releases in Python version requests - `command --python <major.minor.pre-release>`" by @mikeleppane on 2024-09-12 17:39_

---

_Review comment by @zanieb on `crates/pep440-rs/src/version.rs`:1430 on 2024-09-12 19:22_

This feels a little weird. I don't think this function should support leading content. Maybe we should just implement this directly in the relevant version request parsing section instead of implementing `FromStr`? cc @BurntSushi 

---

_@zanieb reviewed on 2024-09-12 19:22_

---

_@zanieb reviewed on 2024-09-12 19:24_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1747 on 2024-09-12 19:24_

If we're going to bother parsing the version as a `Version` above we might want to just use that result here?

I worry about pulling in the full `Version` parser but it seems okay. I think we'll want to avoid matching this case if extra sections, e.g.,`dev` and `post, are present? 

---

_@mikeleppane reviewed on 2024-09-13 06:17_

---

_Review comment by @mikeleppane on `crates/pep440-rs/src/version.rs`:1430 on 2024-09-13 06:17_

Yep, I now removed this.

---

_@mikeleppane reviewed on 2024-09-13 06:20_

---

_Review comment by @mikeleppane on `crates/uv-python/src/discovery.rs`:1747 on 2024-09-13 06:20_

I made some modifications to this, using `Version` directly. However, I don't know if it's any better because the `Version` type uses u64 and `VersionRequest` u8, so it requires some conversion logic. 

Edit: Hmm...this starts to feel too hacky, and another ingredient is this `VersionSpecifiers`. The `Version` type is not able to handle versions with a specifier (at least to my knowledge).

---

_@mikeleppane reviewed on 2024-09-13 06:56_

---

_Review comment by @mikeleppane on `crates/uv-python/src/discovery.rs`:1747 on 2024-09-13 06:56_

I think this whole `ìmpl FromStr for VersionRequest` implementation is starting to become too unwieldy. Somehow, I feel we are now lacking separation of concerns. Some of the logic here could be separated somewhere else. For example, the `Version` type could handle some of the stuff here. 

---

_Review comment by @BurntSushi on `crates/uv-python/src/discovery.rs`:1722 on 2024-09-16 13:34_

This seems pretty hacky to me, and I think isn't guaranteed to work. For example, `1.0b1+b1`. It's perhaps a contrived example, but it's a valid version string that will cause this code to choke. I think in general, the flow of parsing a full version to get the prerelease, throw away that version except for the pre-release and then try to re-parse the version is pretty tortured.

I don't know the original motivation for avoiding full `Version` parsing here, but how about this:

1. In this `FromStr` impl, just try to parse a full `Version`.
2. Get rid of the ad hoc `.` splitting.
3. Inspect the `Version` directly to determine which request to return.
4. For `VersionSpecifiers`, it looks like you're specifically only trying to parse the part of the version without a prelease? Is that right? If so, you can use the mutating methods on the parsed `Version` to strip what you don't want, serialize it to a string and then re-parse.

If `Version` fails to parse, then I think you can skip directly to trying to parse as a `VersionSpecifiers`.

Splitting some of this into helper functions might help.

---

_@BurntSushi had review dismissed on 2024-09-16 13:34_

---

_@mikeleppane reviewed on 2024-09-17 11:29_

---

_Review comment by @mikeleppane on `crates/uv-python/src/discovery.rs`:1722 on 2024-09-17 11:29_

@BurntSushi, thanks for the excellent comment! So that you know- I've made changes following your guidelines. Hopefully, it is now more concise and readable. 
I don't know why it was avoided initially to construct the `Version` type. Perf?

---

_Review requested from @BurntSushi by @mikeleppane on 2024-09-17 12:04_

---

_Comment by @zanieb on 2024-09-17 14:39_

I did a pass on this, you can see my changes in https://github.com/astral-sh/uv/pull/7335/commits/15f6ba28ecb9b25b5211eb5e228efada75429f30

---

_Comment by @zanieb on 2024-09-17 14:44_

I also tested this with a few `uv venv` invocations and it looks good!

---

_@zanieb approved on 2024-09-17 14:44_

---

_Merged by @zanieb on 2024-09-17 14:46_

---

_Closed by @zanieb on 2024-09-17 14:46_

---
