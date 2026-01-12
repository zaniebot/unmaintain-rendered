```yaml
number: 4887
title: "md5 hashes in pip compile *.txt are not supported by pip"
type: issue
state: open
author: angerrp
labels:
  - compatibility
assignees: []
created_at: 2024-07-08T09:08:31Z
updated_at: 2024-12-16T10:30:31Z
url: https://github.com/astral-sh/uv/issues/4887
synced_at: 2026-01-12T15:58:52Z
```

# md5 hashes in pip compile *.txt are not supported by pip

---

_@angerrp_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->
Hello there!

**Issue:** 
When compiling dependencies with `uv pip compile requirements.in` we noticed that `uv` is putting `md5` hashes instead of `sha` hashes in the resulting `requirements.txt`. This is a problem as `pip` only supports `sha256`, `sha384`, `sha512` hashes.

**When does this happen:**
This happens only for our private [Gemfury](https://gemfury.com/) pypi index so my suspicion is that Gemfury responds with a `md5` hash as default instead of `sha`. 


Could `uv` check if there is also a `sha` hash in the index's response or calculate the `sha` from the package? 

**Example:**
```
#requirements.txt
our-pkg==3.39.0 \
    --hash=md5:2427c69da75124df182f48e6d8fa7ccb
```

```
$ pip install --require-hashes -r requirements.txt

ERROR: Invalid requirement: our-pkg==3.39.0     --hash=md5:2427c69da75124df182f48e6d8fa7ccb
pip: error: Allowed hash algorithms for --hash are sha256, sha384, sha512.
```

Version: `uv 0.2.22 (f5e84bbba 2024-07-07)`

---

_Label `compatibility` added by @zanieb on 2024-07-08 13:43_

---

_Comment by @charliermarsh on 2024-09-24 00:55_

We should probably remove MD5 support.

---

_Comment by @bcmyguest on 2024-11-07 02:41_

Hi! This issue has been open for a while, and I can second that it's likely mostly for private repositories. 

md5 support was added [here](https://github.com/astral-sh/uv/issues/1547). It seems we might need to revert that PR (I know it's not really the _source_ of the problem) or provide a set of hash algorithms that are acceptable as an option with some reasonable default. FWIW [pip doesn't allow md5 in their secure installs](https://pip.pypa.io/en/stable/topics/secure-installs/#hash-algorithms) which was the source of the original issue. 

Would suggest using `sha256, sha384, sha512` as 'acceptable' values as they seem to be the allowed values for pip, which as I understand it is still meant to remain compatible with uv.

In the meantime, for those that are having this issue can use regular `pip-compile` on problem packages temporarily, but it means keeping `uv.lock` up to date which is at best a hack.

---

_Comment by @exhuma on 2024-12-16 10:30_

> We should probably remove MD5 support.

Removing may be a bit harsh although there are very valid arguments to do so.

Making md5 support "opt-in" when generating a `requirements.txt` file would be sufficient.

I just tried integrating `uv` into our toolchain (we maintain about 100 Python projects) and a lot of CI jobs still use `pip install [...]` and using `uv` to generate the file made all those jobs fail because of the md5 hashes.

The support added in #1547 is still valid. But generating md5 hashes in requirements files... not so much :)

---
