```yaml
number: 14184
title: "Add support for `--prefix` and `--with` installations in `find_uv_bin`"
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/find-prefix
created_at: 2025-06-21T12:33:49Z
updated_at: 2025-08-08T22:50:19Z
url: https://github.com/astral-sh/uv/pull/14184
synced_at: 2026-01-10T06:44:33Z
```

# Add support for `--prefix` and `--with` installations in `find_uv_bin`

---

_Pull request opened by @zanieb on 2025-06-21 12:33_

Follows #14182

Adds support for the case described at https://github.com/astral-sh/uv/issues/10194#issuecomment-2993544346

This also happens to fix both `--with` requirement test cases, which should close https://github.com/tox-dev/pre-commit-uv/issues/70


---

_Label `enhancement` added by @zanieb on 2025-06-21 12:33_

---

_@Flamefire reviewed on 2025-06-21 17:53_

---

_Review comment by @Flamefire on `python/uv/_find_uv.py`:25 on 2025-06-21 17:53_

```suggestion
        _join(_parents(_module_path(), 4), "bin"),
```
or you'll get `prefix/lib/bin`

And I think this should go first before checking the default script paths in case there are multiple uv versions installed in different locations/levels. The one next to the current module should always take precedence, doesn't it?

---

_@zanieb reviewed on 2025-06-21 18:34_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:25 on 2025-06-21 18:34_

Thanks!

On the ordering, I agree — but since it's a departure from the existing ordering I think we should do that in a separate pull request.

---

_@zanieb reviewed on 2025-06-21 18:41_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:25 on 2025-06-21 18:41_

https://github.com/astral-sh/uv/pull/14191

---

_Marked ready for review by @zanieb on 2025-06-26 12:53_

---

_Review requested from @konstin by @zanieb on 2025-06-27 17:46_

---

_Assigned to @konstin by @zanieb on 2025-06-27 17:46_

---

_Review comment by @konstin on `python/uv/_find_uv.py`:25 on 2025-06-30 12:58_

Can we check that this looks like `lib` `[pP]ython.*` `site-packages` `uv` to avoid reading a different bin dir?

---

_Review comment by @konstin on `python/uv/_find_uv.py`:22 on 2025-06-30 12:59_

Should we do this last? This could easily pick up a different, global uv installation.

---

_Review comment by @konstin on `python/uv/_find_uv.py`:22 on 2025-06-30 13:05_

This actually happens for me:

```
$ uv pip install --prefix foo_prefix --find-links target/wheels/ --no-index uv
$ PYTHONPATH="foo_prefix/lib/python3.12/site-packages/" python -S -c "import sys; print(sys.path); from uv import find_uv_bin; print(find_uv_bin())"
['', '/home/konsti/projects/uv/bar', '/usr/lib/python312.zip', '/usr/lib/python3.12', '/usr/lib/python3.12/lib-dynload']
/home/konsti/.local/bin/uv
```

---

_Review comment by @konstin on `python/uv/_find_uv.py`:28 on 2025-06-30 13:16_

Can you add tests for the prefix and the target case? We have to emulate this by copying our python files and the built test binary manually, but it catches cases such as https://github.com/astral-sh/uv/pull/14184#discussion_r2160101023 and the user scheme preference.

---

_@konstin reviewed on 2025-06-30 13:17_

---

_@zanieb reviewed on 2025-06-30 13:20_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:22 on 2025-06-30 13:20_

Yeah that's what we're discussing at https://github.com/astral-sh/uv/pull/14184#discussion_r2160115413

---

_@zanieb reviewed on 2025-06-30 13:22_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:25 on 2025-06-30 13:22_

I considered that (see also https://github.com/astral-sh/uv/pull/14191#issuecomment-3008756208) and actually started implementing it but I'm hesitant since the matching isn't just literals. I can add something again.

---

_@zanieb reviewed on 2025-06-30 13:23_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:28 on 2025-06-30 13:23_

I'm wary of adding a new test suite for this file, it's a big increase in scope for this task.

---

_@zanieb reviewed on 2025-06-30 13:35_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:28 on 2025-06-30 13:35_

(I'll see how painful it is though, especially with https://github.com/astral-sh/uv/pull/14184#discussion_r2175069753 this gets complicated to trust without tests)

---

_@Flamefire reviewed on 2025-07-01 07:40_

---

_Review comment by @Flamefire on `python/uv/_find_uv.py`:28 on 2025-07-01 07:40_

I think `fnmatch.fnmatch('/opt/prefix/lib/python3.13/site-packages/uv', '**/lib/python*/site-packages/uv')` should be easy and reliable enough to match against `_module_path()` before adding the prefix case

---

_@konstin reviewed on 2025-07-01 08:24_

---

_Review comment by @konstin on `python/uv/_find_uv.py`:28 on 2025-07-01 08:24_

We need this to work on Windows, so we need to be lenient in the matching

---

_Renamed from "Add support for `--prefix` installations in `find_uv_bin`" to "Add support for `--prefix` and `--with` installations in `find_uv_bin`" by @zanieb on 2025-08-07 20:20_

---

_@zanieb reviewed on 2025-08-07 20:21_

---

_Review comment by @zanieb on `python/uv/_find_uv.py`:22 on 2025-08-07 20:21_

I consider this a distinct change, will be addressed by #14191 

---

_@woodruffw approved on 2025-08-07 20:41_

LGTM -- confirming my understanding that this is intentionally overly permissive in what it'll discover, and #14191 will ratchet it down?

---

_Comment by @zanieb on 2025-08-07 20:44_

It depends on what you mean by overly permissive. https://github.com/astral-sh/uv/pull/14191 just changes the _ordering_ so we don't search in the user script directory before checking everywhere else that could be associated with the actual module.

---

_Merged by @zanieb on 2025-08-07 21:48_

---

_Closed by @zanieb on 2025-08-07 21:48_

---

_Branch deleted on 2025-08-07 21:48_

---

_@henryiii reviewed on 2025-08-08 22:50_

---

_Review comment by @henryiii on `python/uv/_find_uv.py`:80 on 2025-08-08 22:50_

This doesn't support Python 3.8 and 3.9. `strict=` was introduced in 3.10.

---
