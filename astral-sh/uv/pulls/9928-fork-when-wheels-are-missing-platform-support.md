```yaml
number: 9928
title: Fork when wheels are missing platform support
type: pull_request
state: closed
author: charliermarsh
labels:
  - bug
  - resolver
assignees: []
base: main
head: charlie/incomplete
created_at: 2024-12-16T03:23:55Z
updated_at: 2024-12-20T19:14:12Z
url: https://github.com/astral-sh/uv/pull/9928
synced_at: 2026-01-12T16:09:02Z
```

# Fork when wheels are missing platform support

---

_@charliermarsh_

## Summary

See https://github.com/astral-sh/uv/issues/9711 for an extensive explanation of the problem at play here.

This PR attempts to improve the resolver for cases in which a source distribution isn't available. In such cases, we assume that the available wheels cover all necessary platforms... So, taking PyTorch as an example, we always pick `2.5.1+cpu`, even though it has no macOS wheels.

The approach taken here is: we require at least one wheel for each "platform" (macOS, Windows, and Linux). If a distribution is missing any of those platforms, then we fork and backtrack to a version that _does_ have the wheels we need (or a source distribution).

The impact of this change is a little nuanced, so I'll talk through each example issue:

- This approach _does_ fix PyTorch, which is the most common failure case here (i.e., you can remove all the `sys_platform != 'darwin'` gating that we have in the docs and that we recommend to users).

- It also fixes this [`pyqt5`](https://github.com/astral-sh/uv/issues/7005) case, which was reported twice (https://github.com/astral-sh/uv/issues/8603).

- It helps a little with this [`markupsafe`](https://github.com/astral-sh/uv/issues/9647) case... But doesn't completely solve it. If you're on a Linux platform that doesn't match the wheel in the PyTorch index, you'll still _only_ get that wheel. (This case is maybe best solved with an explicit index.)

- It does _not_ fix this [`odrive`](https://github.com/astral-sh/uv/issues/8536) issue. In that case, the user wants a wheel that supports an old version of macOS. I don't know how to solve this one without imposing much stricter requirements on the resolver (i.e., we'd have to require that the resolver _always_ find a wheel to support all versions of macOS, which would fail in many cases, e.g., PyTorch).

Ref: https://github.com/astral-sh/uv/issues/9711.

Closes: https://github.com/astral-sh/uv/issues/7005.

Closes: https://github.com/astral-sh/uv/issues/9646.


---

_Review requested from @BurntSushi by @charliermarsh on 2024-12-16 03:23_

---

_Review requested from @zanieb by @charliermarsh on 2024-12-16 03:23_

---

_Review requested from @konstin by @charliermarsh on 2024-12-16 03:23_

---

_Label `bug` added by @charliermarsh on 2024-12-16 03:24_

---

_@charliermarsh reviewed on 2024-12-16 03:24_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1232 on 2024-12-16 03:24_

We only go down this path for distributions that lack a source distribution (which is comparatively quite rare).

---

_@charliermarsh reviewed on 2024-12-16 03:26_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/prioritized_distribution.rs`:692 on 2024-12-16 03:26_

We basically have control over how granular we want to make this (and the forking logic). For example... we could easily include architecture here too, so that we ensure we always get an x86 and an ARM wheel for macOS. However, we'd then need to _require_ (in the resolver) that we always find an x86 wheel somewhere.

So the tradeoff here is: we can get more complete resolutions (making it less likely that we fail at install time due to a missing wheel), but we make the resolver stricter (making it more likely that resolution fails entirely).

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/prioritized_distribution.rs`:692 on 2024-12-16 03:28_

Another way to think of this: we can make sure that there's _always_ an x86 wheel available for macOS, but that requires that we can always _find_ an x86 wheel for macOS.

---

_@charliermarsh reviewed on 2024-12-16 03:28_

---

_@charliermarsh reviewed on 2024-12-16 03:28_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1273 on 2024-12-16 03:28_

Note that we don't enforce these if the current environment is disjoint. So, like, if a user _needs_ to resolve a requirement that only has Windows wheels, they can set `environments = ["sys_platform == 'win32'"]`.

---

_Comment by @zanieb on 2024-12-16 03:33_

Casually posting a solution to this very hard problem on a Sunday! Can't wait to give it a look.

---

_@charliermarsh reviewed on 2024-12-16 03:49_

---

_Review comment by @charliermarsh on `crates/uv-resolver/src/resolver/mod.rs`:1289 on 2024-12-16 03:49_

Oh sorry, this is a TODO -- I'll fix it up tomorrow (going to bed soon). I need to do proper error messages for this.

---

_Comment by @charliermarsh on 2024-12-16 03:50_

Hah :) I think this pretty much a strict improvement, and it fixes PyTorch which is the most common case. But it won't fix every case, and I think there's an inherent tension between fixing the install-time failures vs. forcing more resolve-time failures :(

---

_Review comment by @konstin on `crates/uv-distribution-types/src/prioritized_distribution.rs`:688 on 2024-12-16 10:51_

Nit: combine into a single branch with `||`

---

_Review comment by @konstin on `crates/uv-resolver/src/resolver/environment.rs`:573 on 2024-12-16 10:53_

```suggestion
    // For example, given `python_version >= '3.10'` and the split marker `sys_platform == 'linux'`,
```

---

_@konstin approved on 2024-12-16 10:56_

---

_Comment by @charliermarsh on 2024-12-16 22:56_

Ok, I think what I have here could potentially work...

The basic idea is that while solving, we track the superset of markers for which a dependency was included. And then we avoid applying this logic if a dependency has only been requested _only_ on disjoint platforms.

This is basically a heuristic to continue delaying aggressive forking. So if a dependency is requested with `python-magic-bin>=0.4.14,<0.5.0 ; sys_platform == 'win32'`, we note `python-magic-bin: sys_platform == 'win32'` globally (in `ForkState`); then when we look at the new forking logic in this PR, we skip platforms that are disjoint with the platforms on which the dependency has been requested.

These "known markers" _will_ be wrong when we backtrack, but they'll always be a superset.

---

_Comment by @codspeed-hq[bot] on 2024-12-16 23:25_

## [CodSpeed Performance Report](https://codspeed.io/astral-sh/uv/branches/charlie%2Fincomplete)

### Merging #9928 will **not alter performance**

<sub>Comparing <code>charlie/incomplete</code> (056b61c) with <code>main</code> (e65a273)</sub>



### Summary

`✅ 14` untouched benchmarks  





---

_@charliermarsh reviewed on 2024-12-16 23:40_

---

_Review comment by @charliermarsh on `docs/guides/integration/pytorch.md`:297 on 2024-12-16 23:40_

Well, the previous version doesn't work anymore... So, is this breaking?

---

_Comment by @charliermarsh on 2024-12-17 00:23_

If anyone else is planning to review, this is now ready.

---

_Label `resolver` added by @samypr100 on 2024-12-17 02:35_

---

_Comment by @konstin on 2024-12-17 10:24_

For transformers ecosystem check, we get:

```
DEBUG Forking platform for tensorflow==2.15.0.post1 (split `python_full_version >= '3.13' and sys_platform == 'win32'`, split `python_full_version >= '3.13' and sys_platform != 'win32'`)
```

and several other forks for tensorflow, but only tensorflow. As diff we get:

```diff
 wheels = [
     { url = "https://files.pythonhosted.org/packages/9c/d3/904d5bf64305218ce19f81ff3b2cb872cf434a558443b4a9a5357924637a/tensorflow-2.15.1-cp310-cp310-macosx_10_15_x86_64.whl", hash = "sha256:91b51a507007d63a70b65be307d701088d15042a6399c0e2312b53072226e909", size = 236439313 },
     { url = "https://files.pythonhosted.org/packages/54/38/2be65dc6f47e6aa0fb0494877676774f8faa685c08a5cecf0c0040afccbc/tensorflow-2.15.1-cp310-cp310-macosx_12_0_arm64.whl", hash = "sha256:10132acc072d59696c71ce7221d2d8e0e3ff1e6bc8688dbac6d7aed8e675b710", size = 205693732 },
     { url = "https://files.pythonhosted.org/packages/51/1b/1f6eb37c97d9998010751511308058800fc3736092aac64c3fee23cf0b35/tensorflow-2.15.1-cp310-cp310-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:30c5ef9c758ec9ff7ce2aff76b71c980bc5119b879071c2cc623b1591a497a1a", size = 2121 },
     { url = "https://files.pythonhosted.org/packages/4f/42/433c0c64c5d3b8bee696cde2006d15f03f0504c2f746d49f38e32e52e239/tensorflow-2.15.1-cp310-cp310-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:ea290e435464cf0794f657b48786e5fa413362abe55ed771c172c25980d070ce", size = 475215357 },
-      url = "https://files.pythonhosted.org/packages/1c/b7/604ed5e5507e3dd34b14295d5e4a762d47cc2e8cf29a23b4c20575461445/tensorflow-2.15.1-cp310-cp310-win_amd64.whl", hash = "sha256:8e5431d45ceb416c2b1b6de87378054fbac7d2ed35d45b102d89a786613fffdc", size = 2098 },
     { url = "https://files.pythonhosted.org/packages/25/72/2ede9c4b9b96650a8a7b909abf4733adf110c5907425ee252f8095385b11/tensorflow-2.15.1-cp311-cp311-macosx_10_15_x86_64.whl", hash = "sha256:6761efe511e6ee0f893f60738fefbcc51d6dc386eeaaafea59d21899ef369ffd", size = 236482723 },
     { url = "https://files.pythonhosted.org/packages/f1/31/3191cd83da2f213a3c4af5e40597a98996e9c84b56666f9595dad8a6e780/tensorflow-2.15.1-cp311-cp311-macosx_12_0_arm64.whl", hash = "sha256:aa926114d1e13ffe5b2ea59c3f195216f26646d7fe36e9e5207b291e4b7902ff", size = 205736374 },
     { url = "https://files.pythonhosted.org/packages/81/40/31b27ab3f46de305475ef5160acc8a2addb8fa9ea2179777c4e9bcc5baaf/tensorflow-2.15.1-cp311-cp311-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:e73d43dbc68d8c711e70edecc4ac70472799a25ec4ec18a84d479ee18033d3c5", size = 2121 },
     { url = "https://files.pythonhosted.org/packages/c1/2d/636471492d93b6c1bf5375b81be7105a313a8c91a07c37e43625b00ca5c3/tensorflow-2.15.1-cp311-cp311-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:bb0edd69103c154245c5f209f0507355cc68ba7e4de350084bc31edc562478e4", size = 475258663 },
-      url = "https://files.pythonhosted.org/packages/cb/c5/f5b31ee348459d6f6c54d762488aa0a8e42ff11a7395f4d158915ad43000/tensorflow-2.15.1-cp311-cp311-win_amd64.whl", hash = "sha256:a49f8755c74a89553294a99ab25aa87ab1cddbfa40fe58387e09f64f0578cedc", size = 2096 },
     { url = "https://files.pythonhosted.org/packages/62/e7/b8db69612f401f52b510163d5a04b783c3f3440e8f86e299480095e5e76f/tensorflow-2.15.1-cp39-cp39-macosx_10_15_x86_64.whl", hash = "sha256:f8e85821317c9c0fbf1256e9f721cfb1400ba1e09becb844b3ddd91f744805fc", size = 236437643 },
     { url = "https://files.pythonhosted.org/packages/91/ee/1c4db1bb82d1158fdbfe024bc2331b06272cb8f4abaf7d4e2127b21fc428/tensorflow-2.15.1-cp39-cp39-macosx_12_0_arm64.whl", hash = "sha256:b75815b6a601edad52b4181e9805c8fcd04813a6ab1d5cd8127188dfd2788e20", size = 205692241 },
     { url = "https://files.pythonhosted.org/packages/16/85/6b758898a4342b50add537e2dd50f36fe3b16451bd3a41c2dec0a1ac7548/tensorflow-2.15.1-cp39-cp39-manylinux_2_17_aarch64.manylinux2014_aarch64.whl", hash = "sha256:432788ac5d1234b9e9b7c7f73603a5655271a28c293329c52c7c0b9434a1184e", size = 2120 },
     { url = "https://files.pythonhosted.org/packages/38/03/d509cd280c07ba34f1690b5f094cd715bec3c69a08c15a8a61cf82833cf6/tensorflow-2.15.1-cp39-cp39-manylinux_2_17_x86_64.manylinux2014_x86_64.whl", hash = "sha256:89b5aa1022dec47e567512eaf4e1271b8e6c1ff1984e30d0d9127bd1093ed4c5", size = 475214135 },
-      url = "https://files.pythonhosted.org/packages/a5/ef/a9fe22fabd5e11bda4e322daec40d8798a504fd2ee5725a56ba786503452/tensorflow-2.15.1-cp39-cp39-win_amd64.whl", hash = "sha256:aaf3cfa290597ebbdf19d1a78729e3f555e459506cd58f8d7399359ac5e02a05", size = 2095 },
 ]
```

As replacement, we're adding this older tensorflow version:

```toml
[[package]]
name = "tensorflow"
version = "2.10.1"
source = { registry = "https://pypi.org/simple" }
resolution-markers = [
    "python_full_version < '3.10' and sys_platform == 'win32'",
    "python_full_version == '3.10.*' and sys_platform == 'win32'",
    "python_full_version == '3.11.*' and sys_platform == 'win32'",
    "python_full_version == '3.12.*' and sys_platform == 'win32'",
    "python_full_version >= '3.13' and sys_platform == 'win32'",
]
dependencies = [
    # ...
]
wheels = [
    { url = "https://files.pythonhosted.org/packages/ad/87/f484e0b86687c97d2dfb081e03e948b796561fc8608b409a9366e3b4a663/tensorflow-2.10.1-cp310-cp310-win_amd64.whl", hash = "sha256:a6049664f9a0d14b0a4a7e6f058be87b2d8c27be826d7dd9a870ff03683fbc0b", size = 455948187 },
    { url = "https://files.pythonhosted.org/packages/fe/7d/9114d4d155b4414578dbb30e4b61a33dee4437d1c303b73445d79891ca54/tensorflow-2.10.1-cp39-cp39-win_amd64.whl", hash = "sha256:153111af1d773033264f8591f5deffece180a1f16935b579f43edd83acb17584", size = 455882176 },
]
```

This version does not have more files, this looks like a regression to me. 

---

_Comment by @charliermarsh on 2024-12-17 13:28_

Thanks @konstin, will review.

---

_Comment by @charliermarsh on 2024-12-17 13:41_

I understand _why_ we're forking on `tensorflow==2.15.0.post1` and it roughly looks correct to do so (it only has Linux wheels), but I don't understand why it's then leading us to choose older Tensorflow versions.

---

_Comment by @charliermarsh on 2024-12-17 16:01_

On inspection, the transformers resolution is generally much worse.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/environment.rs`:601 on 2024-12-17 17:59_

Unit tests for this would be great. (Although they sadly might be annoying to write given the `ResolverEnvironment` input.)

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/markers.rs`:20 on 2024-12-17 18:08_

Is this a bit of a footgun? If you have two different `KnownMarkers` values and call `insert` on one of them, it will only mutate that one and not the other. It feels a little odd because they start by referencing the same object, and by virtue of mutation, they end up referring to different objects.

I haven't read on yet to see how this is used, but I'm guessing this is intended behavior.

---

_Review comment by @BurntSushi on `crates/uv-resolver/src/resolver/markers.rs`:20 on 2024-12-17 18:11_

Hmmm it almost seems like you might not be benefiting much from the `Arc` here?

Feel free to ignore me if you've already considered all of this. :)

---

_@BurntSushi reviewed on 2024-12-17 18:12_

I am definitely a little uneasy about this change. Just from more of a "theory" perspective, it does feel a like adding explicit knowledge of known platforms into the resolver logic is a bit worrisome. On the other hand, the way some packages are oriented does seem to demand this if we want a universal resolution, so I'm not sure I have much better ideas. (Other than the one you mentioned, which was always resolving for Windows/macOS/Linux.)

---

_Comment by @charliermarsh on 2024-12-17 18:39_

We definitely can't merge this as-is. Transformers goes from `Tried 12643 versions` to `Tried 80879 versions`.

---

_Comment by @charliermarsh on 2024-12-17 19:18_

It turns out that the changes in Transformers are actually _correct_, and our existing resolution was _worse_. Specifically, `tensorflow-text` dropped Windows support in `2.10.0`: https://github.com/tensorflow/text#a-note-about-different-operating-system-packages. Our previous version used `2.15.0` everywhere, which would fail to install on Windows due to a missing wheel...

---

_Comment by @charliermarsh on 2024-12-17 19:30_

> We definitely can't merge this as-is. Transformers goes from `Tried 12643 versions` to `Tried 80879 versions`.

Ok, this is wrong. Refining that estimate, the total visited versions goes from 89,171 to 166,359 (or 129,212 if you omit Windows).


---

_Comment by @charliermarsh on 2024-12-18 03:14_

Okay, this solves a lot of user issues, so... I do think it's worth doing. At the same time, I don't love that it's going to lead to larger markers and more "expensive" resolutions in some cases.

I've also considered making this a setting (enabled by default) to allow for opt-out. It's a bit strange, though, since opt-out already exists in a sense: you can mark a platform as not necessary by excluding it from `environments`.


---

_Comment by @charliermarsh on 2024-12-19 03:11_

I was able to get rid of `KnownMarkers` entirely which is nice.

---

_@konstin reviewed on 2024-12-19 11:24_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/prioritized_distribution.rs`:692 on 2024-12-19 11:24_

It would be ideal if we could make those very specific, that is include the arch, but give the user control over what they need. For example, if the user only want macos (any), than both the x86_64 and the aarch64 wheel should satisfy that since they are more specific than macos.

---

_@charliermarsh reviewed on 2024-12-19 12:44_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/prioritized_distribution.rs`:692 on 2024-12-19 12:44_

I don’t think I understand. Is this different then what’s in the follow-up PR?

---

_@konstin reviewed on 2024-12-19 12:46_

---

_Review comment by @konstin on `crates/uv-distribution-types/src/prioritized_distribution.rs`:692 on 2024-12-19 12:46_

Yes this is covered by #10017

---

_@charliermarsh reviewed on 2024-12-19 13:45_

---

_Review comment by @charliermarsh on `crates/uv-distribution-types/src/prioritized_distribution.rs`:692 on 2024-12-19 13:45_

Ah right, makes sense.

---

_Comment by @charliermarsh on 2024-12-20 19:14_

We merged https://github.com/astral-sh/uv/pull/10046 instead for now. We may return to this later.

---

_Closed by @charliermarsh on 2024-12-20 19:14_

---
