---
number: 15544
title: "`--bounds` has no effect for `--script`"
type: issue
state: closed
author: nik-sm
labels:
  - error messages
assignees: []
created_at: 2025-08-27T06:03:58Z
updated_at: 2025-12-03T13:37:05Z
url: https://github.com/astral-sh/uv/issues/15544
synced_at: 2026-01-10T01:25:57Z
---

# `--bounds` has no effect for `--script`

---

_Issue opened by @nik-sm on 2025-08-27 06:03_

### Summary

It seems that `uv add --bounds exact` has no effect when adding dependencies to a script. See example below. If this is intended behavior, I believe the CLI should warn about this.

```shell
$ uv init --script example.py                      
Initialized script at `example.py`
$ uv add --script example.py --bounds exact ipython
warning: The `bounds` option is in preview and may change in any future release. Pass `--preview-features add-bounds` to disable this warning.
Updated `example.py`

$ cat example.py 
───────┬─────────────────────────────────────
       │ File: example.py
───────┼─────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = [
   4   │ #     "ipython",
   5   │ # ]
   6   │ # ///
   7   │ 
   8   │ 
   9   │ def main() -> None:
  10   │     print("Hello from example.py!")
  11   │ 
  12   │ 
  13   │ if __name__ == "__main__":
  14   │     main()
───────┴─────────────────────────────────────



$ uv init --script example2.py                     
Initialized script at `example2.py`

$ uv add --script example2.py --bounds major ipython
warning: The `bounds` option is in preview and may change in any future release. Pass `--preview-features add-bounds` to disable this warning.
Updated `example2.py`
$ cat example2  
[bat error]: 'example2': No such file or directory (os error 2)

$ cat example2.py 
───────┬──────────────────────────────────────
       │ File: example2.py
───────┼──────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = [
   4   │ #     "ipython",
   5   │ # ]
   6   │ # ///
   7   │ 
   8   │ 
   9   │ def main() -> None:
  10   │     print("Hello from example2.py!")
  11   │ 
  12   │ 
  13   │ if __name__ == "__main__":
  14   │     main()
───────┴──────────────────────────────────────
```

P.S. loving the `--script` workflow - keep up the great work!

### Platform

Darwin 24.5.0 arm64

### Version

uv 0.8.13 (ede75fe62 2025-08-21)

### Python version

Python 3.12.11

---

_Label `bug` added by @nik-sm on 2025-08-27 06:03_

---

_Comment by @charliermarsh on 2025-08-28 00:47_

I suspect this works if you run `uv lock --script example.py` first. Without a lockfile, `uv add --script` just adds the verbatim dependency. Agree we should warn.

---

_Label `bug` removed by @charliermarsh on 2025-08-28 00:47_

---

_Label `error messages` added by @charliermarsh on 2025-08-28 00:47_

---

_Comment by @konstin on 2025-08-28 11:21_

For my use cases, I'd like to have e.g. `uv add httpx --script foo.py --bounds major` add numpy with bounds. There are cases where you don't want or can't use a lockfile, but you still want some bounds to avoid sudden breakage. It feels odd to ignore an explicit option depending on whether a lockfile exists. The same applies to `uv add pandas --bounds major --frozen`.

---

_Comment by @zanieb on 2025-08-28 14:48_

Huh why aren't we using bounds by default in scripts unless there's a lockfile? That seems surprising.

---

_Comment by @charliermarsh on 2025-08-28 14:57_

Because it would require performing a resolution, and we don't perform any resolution if there isn't a lockfile.

---

_Comment by @zanieb on 2025-08-28 14:59_

I think that's probably wrong in general, e.g., we allow you to add conflicting dependencies? (or am I missing something here?)

---

_Comment by @nik-sm on 2025-08-28 22:16_

For me, the single-file simplicity is part of the appeal of the `--script` workflow. That being said, I don't know how much of a challenge it is to resolve versions without a lockfile or whether it leads to other problems.

Just testing a bit more, is seems this feature may not be working either way. All I changed below is doing `uv lock --script file.py` before `uv add --bounds exact numpy --script file.py` but it still doesn't seem to work. Maybe there is another option I'm supposed to pass to ensure lockfile is discovered/used?



<details>

<summary> Example using 0.8.13 </summary>

```shell
✓ Desktop % uv init --script example.py
Initialized script at `example.py`
✓ Desktop % cat example.py
───────┬─────────────────────────────────────────────────────────────
       │ File: example.py
───────┼─────────────────────────────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = []
   4   │ # ///
   5   │
   6   │
   7   │ def main() -> None:
   8   │     print("Hello from example.py!")
   9   │
  10   │
  11   │ if __name__ == "__main__":
  12   │     main()
───────┴─────────────────────────────────────────────────────────────
✓ Desktop % uv add --bounds exact numpy --script example.py
warning: The `bounds` option is in preview and may change in any future release. Pass `--preview-features add-bounds` to disable this warning.
Updated `example.py`
✓ Desktop % cat example.py
───────┬─────────────────────────────────────────────────────────────
       │ File: example.py
───────┼─────────────────────────────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = [
   4   │ #     "numpy",
   5   │ # ]
   6   │ # ///
   7   │
   8   │
   9   │ def main() -> None:
  10   │     print("Hello from example.py!")
  11   │
  12   │
  13   │ if __name__ == "__main__":
  14   │     main()
───────┴─────────────────────────────────────────────────────────────
✓ Desktop % uv lock --script example.py
Resolved 1 package in 6ms
✓ Desktop % cat example.py
───────┬─────────────────────────────────────────────────────────────
       │ File: example.py
───────┼─────────────────────────────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = [
   4   │ #     "numpy",
   5   │ # ]
   6   │ # ///
   7   │
   8   │
   9   │ def main() -> None:
  10   │     print("Hello from example.py!")
  11   │
  12   │
  13   │ if __name__ == "__main__":
  14   │     main()
───────┴─────────────────────────────────────────────────────────────
✓ Desktop % wc -l example.py.lock
      69 example.py.lock
✓ Desktop % uv add --bounds exact numpy --script example.py
warning: The `bounds` option is in preview and may change in any future release. Pass `--preview-features add-bounds` to disable this warning.
Resolved 1 package in 2ms
✓ Desktop % cat example.py
───────┬─────────────────────────────────────────────────────────────
       │ File: example.py
───────┼─────────────────────────────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = [
   4   │ #     "numpy",
   5   │ # ]
   6   │ # ///
   7   │
   8   │
   9   │ def main() -> None:
  10   │     print("Hello from example.py!")
  11   │
  12   │
  13   │ if __name__ == "__main__":
  14   │     main()
───────┴─────────────────────────────────────────────────────────────
✓ Desktop %
```

</details>


<details>

<summary>
Example using 0.8.14
</summary>

```shell
✓ tmp % uv init --script example.py
Initialized script at `example.py`
✓ tmp % cat example.py
───────┬─────────────────────────────────────────────────────────────
       │ File: example.py
───────┼─────────────────────────────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = []
   4   │ # ///
   5   │
   6   │
   7   │ def main() -> None:
   8   │     print("Hello from example.py!")
   9   │
  10   │
  11   │ if __name__ == "__main__":
  12   │     main()
───────┴─────────────────────────────────────────────────────────────
✓ tmp % uv add --bounds exact numpy --script example.py
warning: The `bounds` option is in preview and may change in any future release. Pass `--preview-features add-bounds` to disable this warning.
Updated `example.py`
✓ tmp % cat example.py
───────┬─────────────────────────────────────────────────────────────
       │ File: example.py
───────┼─────────────────────────────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = [
   4   │ #     "numpy",
   5   │ # ]
   6   │ # ///
   7   │
   8   │
   9   │ def main() -> None:
  10   │     print("Hello from example.py!")
  11   │
  12   │
  13   │ if __name__ == "__main__":
  14   │     main()
───────┴─────────────────────────────────────────────────────────────
✓ tmp % uv lock --script example.py
Resolved 1 package in 4ms
✓ tmp % cat example.py
───────┬─────────────────────────────────────────────────────────────
       │ File: example.py
───────┼─────────────────────────────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = [
   4   │ #     "numpy",
   5   │ # ]
   6   │ # ///
   7   │
   8   │
   9   │ def main() -> None:
  10   │     print("Hello from example.py!")
  11   │
  12   │
  13   │ if __name__ == "__main__":
  14   │     main()
───────┴─────────────────────────────────────────────────────────────
✓ tmp % wc -l example.py.lock
      69 example.py.lock
✓ tmp % uv add --bounds exact numpy --script example.py
warning: The `bounds` option is in preview and may change in any future release. Pass `--preview-features add-bounds` to disable this warning.
Resolved 1 package in 2ms
✓ tmp % cat example.py
───────┬─────────────────────────────────────────────────────────────
       │ File: example.py
───────┼─────────────────────────────────────────────────────────────
   1   │ # /// script
   2   │ # requires-python = ">=3.12"
   3   │ # dependencies = [
   4   │ #     "numpy",
   5   │ # ]
   6   │ # ///
   7   │
   8   │
   9   │ def main() -> None:
  10   │     print("Hello from example.py!")
  11   │
  12   │
  13   │ if __name__ == "__main__":
  14   │     main()
───────┴─────────────────────────────────────────────────────────────
```

</details>

---

_Comment by @nik-sm on 2025-09-10 23:35_

Just a ping with more info about `--bounds`. In version 0.8.14 and in latest (0.8.17), the behavior is like this:

```shell
uv init --script file.py
uv add --script file.py --bounds exact numpy    # does not include bounds
uv lock --script file.py
uv add --script file.py --bounds exact loguru   # this works (add NEW package with bounds)
uv add --script file.py --bounds exact numpy    # this does nothing (add EXISTING package with bounds)
```

The behavior seems to be the same for `--script` and for regular uv projects (i.e. the result of plain `uv init`). 

In both situations (a `uv init` project, or a `uv init --script; uv lock --script`), uv does resolve dependencies, but then exits 0 without making changes. 

Not sure whether this is intentional vs bug, but I think it's unintuitive. (`uv add numpy==2.3.3` will always try to apply the requested version logic as you expect, but `uv add numpy --bounds exact` sometimes does NOT try to apply the requested version logic...)

---

_Assigned to @charliermarsh by @charliermarsh on 2025-12-03 04:05_

---

_Referenced in [astral-sh/uv#16954](../../astral-sh/uv/pulls/16954.md) on 2025-12-03 05:39_

---

_Closed by @zanieb on 2025-12-03 13:37_

---
