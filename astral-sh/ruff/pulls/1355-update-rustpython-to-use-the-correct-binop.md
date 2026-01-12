```yaml
number: 1355
title: "Update RustPython to use the correct `BinOp` location"
type: pull_request
state: merged
author: harupy
labels: []
assignees: []
merged: true
base: main
head: update-RustPython-binop
created_at: 2022-12-24T03:22:36Z
updated_at: 2022-12-24T04:08:48Z
url: https://github.com/astral-sh/ruff/pull/1355
synced_at: 2026-01-12T15:55:06Z
```

# Update RustPython to use the correct `BinOp` location

---

_@harupy_

See https://github.com/RustPython/RustPython/pull/4348


```shell
# On main
> cargo run -- --show-source resources/test/fixtures/pyflakes/F504.py
...
resources/test/fixtures/pyflakes/F504.py:3:15: F504 '...' % ... has unused named argument(s): b
  |
3 | "%(a)s %(c)s" % {a: "?", "b": "!"}  # F504 ("b" not used)
  |               ^^^^^^^^^^^^^^^^^^^^ F504
  |
resources/test/fixtures/pyflakes/F504.py:8:9: F504 '...' % ... has unused named argument(s): b
  |
8 | "%(a)s" % {"a": 1, r"b": "!"}  # F504 ("b" not used)
  |         ^^^^^^^^^^^^^^^^^^^^^ F504
  |
resources/test/fixtures/pyflakes/F504.py:9:9: F504 '...' % ... has unused named argument(s): b
  |
9 | "%(a)s" % {'a': 1, u"b": "!"}  # F504 ("b" not used)
  |         ^^^^^^^^^^^^^^^^^^^^^ F504
  |
Found 3 error(s).
3 potentially fixable with the --fix option.
```

```shell
# On this PR branch
> cargo run -- --show-source resources/test/fixtures/pyflakes/F504.py
...
resources/test/fixtures/pyflakes/F504.py:3:1: F504 '...' % ... has unused named argument(s): b
  |
3 | "%(a)s %(c)s" % {a: "?", "b": "!"}  # F504 ("b" not used)
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ F504
  |
resources/test/fixtures/pyflakes/F504.py:8:1: F504 '...' % ... has unused named argument(s): b
  |
8 | "%(a)s" % {"a": 1, r"b": "!"}  # F504 ("b" not used)
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ F504
  |
resources/test/fixtures/pyflakes/F504.py:9:1: F504 '...' % ... has unused named argument(s): b
  |
9 | "%(a)s" % {'a': 1, u"b": "!"}  # F504 ("b" not used)
  | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ F504
  |
Found 3 error(s).
3 potentially fixable with the --fix option.
```

---

_Renamed from "Update RustPython to use the correct BinOp location" to "Update RustPython to use the correct `BinOp` location" by @harupy on 2022-12-24 03:23_

---

_Comment by @harupy on 2022-12-24 03:35_

btw do we have a script or command in this repository that can update snapshots quickly?

```shell
> cargo test
# generates new snapshots if there are failed tests

> ???
# Updates existing snapshots

```

---

_Comment by @charliermarsh on 2022-12-24 03:57_

Do you have `cargo-insta` installed? Have you tried running `cargo insta review`?

https://crates.io/crates/cargo-insta

---

_Comment by @charliermarsh on 2022-12-24 03:57_

(Apologies if misunderstanding the question.)

---

_Merged by @charliermarsh on 2022-12-24 03:58_

---

_Closed by @charliermarsh on 2022-12-24 03:58_

---

_Comment by @harupy on 2022-12-24 04:04_

@charliermarsh `cargo insta review` is what I've looking for. Thanks!

---

_Branch deleted on 2022-12-24 04:04_

---

_Comment by @charliermarsh on 2022-12-24 04:06_

@harupy - Oh no!!! I wish you'd known about it before! I had no idea you were doing it manually. Hopefully it'll save you a _ton_ of time.

---

_Comment by @harupy on 2022-12-24 04:08_

I wrote this shell script and have been using it :)

```shell
for f in $(find . -name '*.snap.new'); do
    new_f=$(echo $f | sed 's/\.snap\.new$/.snap/')
    echo $new_f
    cat $f | grep -v '^assertion_line' > $new_f
    rm $f
done
```

---
