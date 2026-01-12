```yaml
number: 13022
title: Omit Python 3.7 downloads from managed versions
type: pull_request
state: merged
author: zanieb
labels:
  - breaking
assignees: []
merged: true
base: release/070
head: zb/37-down
created_at: 2025-04-21T19:56:20Z
updated_at: 2025-05-03T14:23:41Z
url: https://github.com/astral-sh/uv/pull/13022
synced_at: 2026-01-12T16:10:31Z
```

# Omit Python 3.7 downloads from managed versions

---

_@zanieb_

Removes Python 3.7 installation support.

The following are available today and would no longer be available

```
# uv python list --all-versions --all-platforms | grep 3.7
cpython-3.7.9-windows-x86_64-none                      <download available>
cpython-3.7.9-windows-x86-none                         <download available>
cpython-3.7.9-macos-x86_64-none                        <download available>
cpython-3.7.9-linux-x86_64-gnu                         <download available>
cpython-3.7.7-windows-x86_64-none                      <download available>
cpython-3.7.7-windows-x86-none                         <download available>
cpython-3.7.7-macos-x86_64-none                        <download available>
cpython-3.7.7-linux-x86_64-gnu                         <download available>
cpython-3.7.6-windows-x86_64-none                      <download available>
cpython-3.7.6-windows-x86-none                         <download available>
pypy-3.7.13-windows-x86_64-none                        <download available>
pypy-3.7.13-macos-x86_64-none                          <download available>
pypy-3.7.13-linux-x86_64-gnu                           <download available>
pypy-3.7.13-linux-x86-gnu                              <download available>
pypy-3.7.13-linux-s390x-gnu                            <download available>
pypy-3.7.13-linux-aarch64-gnu                          <download available>
pypy-3.7.12-windows-x86_64-none                        <download available>
pypy-3.7.12-macos-x86_64-none                          <download available>
pypy-3.7.12-linux-x86_64-gnu                           <download available>
pypy-3.7.12-linux-x86-gnu                              <download available>
pypy-3.7.12-linux-s390x-gnu                            <download available>
pypy-3.7.12-linux-aarch64-gnu                          <download available>
pypy-3.7.10-windows-x86_64-none                        <download available>
pypy-3.7.10-macos-x86_64-none                          <download available>
pypy-3.7.10-linux-x86_64-gnu                           <download available>
pypy-3.7.10-linux-x86-gnu                              <download available>
pypy-3.7.10-linux-s390x-gnu                            <download available>
pypy-3.7.10-linux-aarch64-gnu                          <download available>
pypy-3.7.9-windows-x86-none                            <download available>
pypy-3.7.9-macos-x86_64-none                           <download available>
pypy-3.7.9-linux-x86_64-gnu                            <download available>
pypy-3.7.9-linux-x86-gnu                               <download available>
pypy-3.7.9-linux-s390x-gnu                             <download available>
pypy-3.7.9-linux-aarch64-gnu                           <download available>
```

All the CPython ones should absolutely not be available, as they're not even the last security patch release. I'm on the fence about PyPy?

---

_Label `breaking` added by @zanieb on 2025-04-21 19:56_

---

_Renamed from "Omit Python 3.7 downloads" to "Omit Python 3.7 downloads from managed versions" by @zanieb on 2025-04-21 19:56_

---

_Comment by @zanieb on 2025-04-21 20:01_

As a minor note, would help with https://github.com/astral-sh/uv/pull/12381#issuecomment-2744326955 — though I already fixed that by using a subset.

---

_Added to milestone `v0.7.0` by @zanieb on 2025-04-21 21:08_

---

_Comment by @j178 on 2025-04-22 00:15_

You might also need to run `minify-download-metadata.py` to create the bundled JSON file.

---

_Comment by @MeitarR on 2025-04-22 18:41_

> You might also need to run `minify-download-metadata.py` to create the bundled JSON file.

This problem will be eliminated with the first commit of https://github.com/astral-sh/uv/pull/12967

---

_Comment by @konstin on 2025-04-23 11:16_

Following https://downloads.python.org/pypy/, recent PyPy releases support 2.7, 3.10 and 3.11, so I'm fine with dropping PyPy Python 3.7 builds, too.

---

_@konstin approved on 2025-04-23 11:16_

---

_Merged by @zanieb on 2025-04-25 17:56_

---

_Closed by @zanieb on 2025-04-25 17:56_

---

_Branch deleted on 2025-04-25 17:56_

---

_Comment by @inoa-jboliveira on 2025-04-30 01:12_

Hi everyone, what is the point of removing the ability to download older versions?
What does uv gain from this?

You could warn or hide the versions by default, but remove the ability to download? 

You know uv is able to install outdated and insecure packages from PyPI, so why should it not be able to install old Python versions. This makes uv less appealing for zero reason. It is like Apple removing ports from your devices and chargers. No user benefit from this change.

The file is there for grabs. There is no effort for the install to happen. Also this adds precedence for newer versions to be removed as well once they reach EOL.

I hope you can reconsider.

---

_Comment by @zanieb on 2025-04-30 02:03_

As noted above

> All the CPython ones should absolutely not be available, as they're not even the last security patch release. 

These distributions were not built until the final EOL version. They are far outdated. This version of Python was poorly supported in `python-build-standalone` and isn't even available on most platforms, which is also confusing. Even if they were the final patch version, I'm not sure uv should continue supporting installing them indefinitely, but I think we'll need to revisit that in the future.

Every feature of uv incurs _some_ maintenance burden for us. Please don't use hyperboles like "zero reason".

It'd be helpful if you talked more about concrete use-cases you have.

---

_Comment by @zanieb on 2025-04-30 02:06_

(It's also worth noting that uv was released _after_ 3.7 was EOL. We have _never_ officially supported it.)

---

_Comment by @inoa-jboliveira on 2025-04-30 02:25_

Hi @zanieb, thanks for the detailed reply. I get that every feature adds some maintenance overhead, and I didn’t mean to downplay that. In this case it does not seem you drop or implement any feature regarding this removal, which for me seems low maintenance.

The main reason I’m bringing this up is that being able to spin up old projects is a real and recurring need. It's already tricky enough sometimes, and losing access to older Python versions just adds friction. Not everyone can or will upgrade, especially in legacy systems or internal company tooling that hasn’t been touched in a while.

I get that 3.7 isn’t officially supported and that there are good reasons for cleaning things up. Still, maybe there’s a middle ground, like hiding older versions behind a flag or marking them clearly as unsupported. It also raises questions about what will happen when 3.8 and others eventually hit EOL.

Appreciate the work you do

---

_Comment by @michalc on 2025-05-03 13:29_

> The main reason I’m bringing this up is that being able to spin up old projects is a real and recurring need.

In case it's helpful - so far pinning to an older uv, e.g. 0.6.17, works for me in terms of installing Python 3.7

---

_Comment by @zanieb on 2025-05-03 14:23_

Appreciate the kind words. 

As a note, you can use `uvx uv@0.6.17 python install 3.7`. I understand that still adds friction though. 

We'll keep an eye on feedback here. I don't think we'll drop support for downloading other versions in the same way (e.g., 3.8), since we've actually been maintaining the distributions for all common platforms.



---
