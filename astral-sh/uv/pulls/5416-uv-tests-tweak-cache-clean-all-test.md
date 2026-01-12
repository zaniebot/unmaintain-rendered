```yaml
number: 5416
title: "uv/tests: tweak cache clean all test"
type: pull_request
state: merged
author: BurntSushi
labels: []
assignees: []
merged: true
base: main
head: ag/cache-clear-tweak
created_at: 2024-07-24T16:03:53Z
updated_at: 2024-07-24T17:41:36Z
url: https://github.com/astral-sh/uv/pull/5416
synced_at: 2026-01-12T16:06:48Z
```

# uv/tests: tweak cache clean all test

---

_@BurntSushi_

The test asserts that 28 files were removed. But on my system, 27 files
are removed.

This PR is first about debugging what the difference is (since CI
presumably passes with the status quo snapshot). And then I'm thinking
the right way to fix the test failure is with a filter that replaces the
specific number of files removed (limited to what we know to be
correct) with a placeholder.


---

_Comment by @BurntSushi on 2024-07-24 16:04_

This is the snapshot diff I get locally:

```
    4     4 │ ----- stderr -----
    5     5 │ DEBUG uv [VERSION] ([COMMIT] DATE)
    6     6 │ Clearing cache at: [CACHE_DIR]/
    7       │-Removed 28 files ([SIZE])
          7 │+removing: [CACHE_DIR]/CACHEDIR.TAG
          8 │+removing: [CACHE_DIR]/.gitignore
          9 │+removing: [CACHE_DIR]/built-wheels-v3/.gitignore
         10 │+removing: [CACHE_DIR]/built-wheels-v3/.git
         11 │+removing: [CACHE_DIR]/interpreter-v2/7a5aff7824aeedfc.msgpack
         12 │+removing: [CACHE_DIR]/simple-v10/pypi/iniconfig.rkyv
         13 │+removing: [CACHE_DIR]/simple-v10/pypi/typing-extensions.rkyv
         14 │+removing: [CACHE_DIR]/wheels-v1/pypi/typing-extensions/typing_extensions-4.10.0-py3-none-any.msgpack
         15 │+removing: [CACHE_DIR]/wheels-v1/pypi/typing-extensions/typing_extensions-4.10.0-py3-none-any
         16 │+removing: [CACHE_DIR]/wheels-v1/pypi/typing-extensions/typing_extensions-4.10.0-py3-none-any.http
         17 │+removing: [CACHE_DIR]/wheels-v1/pypi/iniconfig/iniconfig-2.0.0-py3-none-any.msgpack
         18 │+removing: [CACHE_DIR]/wheels-v1/pypi/iniconfig/iniconfig-2.0.0-py3-none-any
         19 │+removing: [CACHE_DIR]/wheels-v1/pypi/iniconfig/iniconfig-2.0.0-py3-none-any.http
         20 │+removing: [CACHE_DIR]/archive-v0/T3uloWtwm_ZtcdGnmuDlJ/typing_extensions.py
         21 │+removing: [CACHE_DIR]/archive-v0/T3uloWtwm_ZtcdGnmuDlJ/typing_extensions-4.10.0.dist-info/LICENSE
         22 │+removing: [CACHE_DIR]/archive-v0/T3uloWtwm_ZtcdGnmuDlJ/typing_extensions-4.10.0.dist-info/WHEEL
         23 │+removing: [CACHE_DIR]/archive-v0/T3uloWtwm_ZtcdGnmuDlJ/typing_extensions-4.10.0.dist-info/METADATA
         24 │+removing: [CACHE_DIR]/archive-v0/T3uloWtwm_ZtcdGnmuDlJ/typing_extensions-4.10.0.dist-info/RECORD
         25 │+removing: [CACHE_DIR]/archive-v0/G0Jg7jbCWZGxwbrQJgZDf/iniconfig/__init__.py
         26 │+removing: [CACHE_DIR]/archive-v0/G0Jg7jbCWZGxwbrQJgZDf/iniconfig/_parse.py
         27 │+removing: [CACHE_DIR]/archive-v0/G0Jg7jbCWZGxwbrQJgZDf/iniconfig/_version.py
         28 │+removing: [CACHE_DIR]/archive-v0/G0Jg7jbCWZGxwbrQJgZDf/iniconfig/exceptions.py
         29 │+removing: [CACHE_DIR]/archive-v0/G0Jg7jbCWZGxwbrQJgZDf/iniconfig/py.typed
         30 │+removing: [CACHE_DIR]/archive-v0/G0Jg7jbCWZGxwbrQJgZDf/iniconfig-2.0.0.dist-info/METADATA
         31 │+removing: [CACHE_DIR]/archive-v0/G0Jg7jbCWZGxwbrQJgZDf/iniconfig-2.0.0.dist-info/WHEEL
         32 │+removing: [CACHE_DIR]/archive-v0/G0Jg7jbCWZGxwbrQJgZDf/iniconfig-2.0.0.dist-info/licenses/LICENSE
         33 │+removing: [CACHE_DIR]/archive-v0/G0Jg7jbCWZGxwbrQJgZDf/iniconfig-2.0.0.dist-info/RECORD
         34 │+Removed 27 files ([SIZE])
```

So there are 27 file removals there. I'm thinking that CI will show 28. I want to see what that extra one is and make sure it's "okay" to have 27 files removed. (e.g., Due to a platform difference.)

---

_Comment by @BurntSushi on 2024-07-24 16:12_

So the actual diff here is that on my system, I have this deletion:

```
[CACHE_DIR]/interpreter-v2/7a5aff7824aeedfc.msgpack
```

But in CI, there are two very similar deletions:

```
[CACHE_DIR]/interpreter-v2/5337828be59ddeb0.msgpack
[CACHE_DIR]/interpreter-v2/bd5cee70c7371a8d.msgpack
```

My guess is that this is related to the Python interpreter installations on my system? So I'd guess that 27 files being removed is OK here.

---

_Comment by @BurntSushi on 2024-07-24 16:14_

And there is indeed only one such cached file on my system, so removing just 1 of them is correct I think:

```
[andrew@duff uv]$ l ~/.cache/uv/interpreter-v2/
total 4.0K
-rw------- 1 andrew users 981 Jul 24 12:14 0cdf864954bf544e.msgpack
```

---

_Marked ready for review by @BurntSushi on 2024-07-24 16:17_

---

_Review requested from @charliermarsh by @BurntSushi on 2024-07-24 16:17_

---

_@charliermarsh reviewed on 2024-07-24 17:03_

---

_Review comment by @charliermarsh on `crates/uv/tests/cache_clean.rs`:31 on 2024-07-24 17:03_

Can you try using `context.with_filtered_counts()`?

---

_@charliermarsh approved on 2024-07-24 17:03_

Approved with one tip.

---

_@BurntSushi reviewed on 2024-07-24 17:29_

---

_Review comment by @BurntSushi on `crates/uv/tests/cache_clean.rs`:31 on 2024-07-24 17:29_

Ah yeah nice tip. I had thought about doing that same thing (re-implementing it I mean, since I didn't know about that routine) but thought that it might be taking too much away from what we're trying to test here. That is, if this test said, "Removed 0 files," then we'd probably want that to result in a failure.

---

_Merged by @BurntSushi on 2024-07-24 17:41_

---

_Closed by @BurntSushi on 2024-07-24 17:41_

---

_Branch deleted on 2024-07-24 17:41_

---
