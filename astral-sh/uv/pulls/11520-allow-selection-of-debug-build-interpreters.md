```yaml
number: 11520
title: Allow selection of debug build interpreters
type: pull_request
state: merged
author: zanieb
labels:
  - enhancement
assignees: []
merged: true
base: main
head: zb/debug
created_at: 2025-02-14T20:24:36Z
updated_at: 2025-09-12T13:32:23Z
url: https://github.com/astral-sh/uv/pull/11520
synced_at: 2026-01-12T16:09:52Z
```

# Allow selection of debug build interpreters

---

_@zanieb_

Extends the `PythonVariant` logic to support interpreters with the debug flag enabled.

---

_Label `enhancement` added by @zanieb on 2025-02-14 20:24_

---

_@zanieb reviewed on 2025-02-14 20:26_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:1688 on 2025-02-14 20:26_

I need to confirm it's `td` and not `dt` still.

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:677 on 2025-02-14 20:27_

I think something is wrong with `generate-download-metadata.py` because this download should exist.

---

_@zanieb reviewed on 2025-02-14 20:27_

---

_Comment by @zanieb on 2025-02-14 20:30_

A couple notes on things to do

- `uv python list` should probably not show downloads for these by default
- Parsing needs to be implemented in download request keys e.g. `uv python install cpython-3.10.2+debug-macos-aarch64-none`
- Ensure `uv python find` works for existing interpreters with debug enabled

---

_@zanieb reviewed on 2025-02-14 20:32_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:1218 on 2025-02-14 20:32_

Copied the `freethreaded` test to start

---

_Review comment by @davidhewitt on `crates/uv/tests/it/python_install.rs`:677 on 2025-03-07 11:33_

I just did some debugging here, it looks like the regex in `_filename_re` is picking up `-debug` as being a 4th piece of the triple, e.g. `aarch64-apple-darwin-debug`

So then the build options / variant processing is not working correctly.

This happens to not be a problem for the free-threaded builds because they are all `freethreaded+pgo` or `freethreaded+pgo+lto`, and `+` is not allowed as a character in the triple portion of the regex.

---

_@davidhewitt reviewed on 2025-03-07 11:36_

Thank you very much for working on this! I would really appreciate this for making testing of debug builds in PyO3 easier; we currently manually pull `python-build-standalone` releases and set some hacks to configure them. But `uv` is such a convenient tool, being able to just do `UV_PYTHON=3.13d uv tool run nox -s test` would be fantastic ❤️ 

I just tried this branch and have found the possible bug which blocked you; I'm not sure how you'd like to fix the bug (either in the regex or in `python-build-standalone` url formats, I guess).

---

_Comment by @davidhewitt on 2025-03-07 11:40_

cc @arielb1 - if we get this functionality in `uv` working then installing and running with a debug build should hopefully be much easier (for testing the PyO3 issues with the debug build in https://github.com/PyO3/pyo3/pull/4874)

---

_Comment by @mg3146 on 2025-06-10 13:07_

hi - is there an update on this?

---

_@yan12125 reviewed on 2025-08-04 06:51_

---

_Review comment by @yan12125 on `crates/uv/tests/it/python_install.rs`:677 on 2025-08-04 06:51_

> I just did some debugging here, it looks like the regex in `_filename_re` is picking up `-debug` as being a 4th piece of the triple, e.g. `aarch64-apple-darwin-debug`

Negative lookahead may be a solution?

---

_@zanieb reviewed on 2025-09-11 16:52_

---

_Review comment by @zanieb on `crates/uv/tests/it/python_install.rs`:677 on 2025-09-11 16:52_

Thanks! Fixed this.

---

_Comment by @geofft on 2025-09-11 19:45_

Small bug in this code:

```
$ ~/.local/bin/uvx python@3.13+freethreaded
cpython-3.13.7+freethreaded-linux-aarch64-gnu (download) ------------------------------ 20.69 MiB/87.17 MiB                                                  ^C
$ /tmp/uvx python@3.13+freethreaded
error: Invalid version request: 3.13+freethreade
```

It's stripping the `d` suffix before parsing the plus syntax.

---

_Review comment by @geofft on `crates/uv-python/fetch-download-metadata.py`:1 on 2025-09-11 19:46_

This file also needs to change to learn about `freethreaded+debug` interpreters, but we can land that as a followup.

---

_Review comment by @geofft on `crates/uv-python/build.rs`:15 on 2025-09-11 19:47_

How much does this increase the `uv` file size?

---

_Review comment by @geofft on `crates/uv-python/src/discovery.rs`:1688 on 2025-09-11 19:47_

It is indeed `td`. (See configure.ac)

---

_Review comment by @geofft on `crates/uv-python/src/discovery.rs`:172 on 2025-09-11 19:49_

I think this might be nicer as a pair of bools, but I can try to clean that up in a followup.

---

_Review comment by @geofft on `crates/uv-python/src/installation.rs`:637 on 2025-09-11 19:49_

Any reason this spells out the variants and the similar code above uses `_`?

---

_@geofft approved on 2025-09-11 19:50_

Seems fine to ship.

---

_Review comment by @geofft on `crates/uv-python/build.rs`:15 on 2025-09-11 19:53_

Apparently at most 0.1 MB, seems fine!

---

_@geofft reviewed on 2025-09-11 19:53_

---

_Review comment by @zanieb on `crates/uv-python/fetch-download-metadata.py`:1 on 2025-09-11 19:53_

(done locally)

---

_@zanieb reviewed on 2025-09-11 19:53_

---

_@zanieb reviewed on 2025-09-11 19:54_

---

_Review comment by @zanieb on `crates/uv-python/build.rs`:15 on 2025-09-11 19:54_

Need to measure with release builds, probably a significant amount.

---

_@zanieb reviewed on 2025-09-11 19:54_

---

_Review comment by @zanieb on `crates/uv-python/src/discovery.rs`:172 on 2025-09-11 19:54_

I don't know if I want that, though I understand why it may be preferable. I'd rather wait until there are more relevant variants.

---

_@zanieb reviewed on 2025-09-11 19:55_

---

_Review comment by @zanieb on `crates/uv-python/build.rs`:15 on 2025-09-11 19:55_

Oh thanks!

---

_@zanieb reviewed on 2025-09-11 19:56_

---

_Review comment by @zanieb on `crates/uv-python/src/installation.rs`:637 on 2025-09-11 19:56_

I usually strongly prefer exhaustive matches so they need to revisited on change, but I guess I was on the fence here and did it two ways! I'll change them to be consistent.

---

_Marked ready for review by @zanieb on 2025-09-12 11:19_

---

_Merged by @zanieb on 2025-09-12 13:32_

---

_Closed by @zanieb on 2025-09-12 13:32_

---

_Branch deleted on 2025-09-12 13:32_

---
