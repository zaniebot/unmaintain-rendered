---
number: 6733
title: "`uv run` with a different root directory"
type: issue
state: closed
author: haarisr
labels:
  - question
assignees: []
created_at: 2024-08-28T00:07:46Z
updated_at: 2025-02-13T20:34:04Z
url: https://github.com/astral-sh/uv/issues/6733
synced_at: 2026-01-10T01:24:05Z
---

# `uv run` with a different root directory

---

_Issue opened by @haarisr on 2024-08-28 00:07_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

```bash
uv init bird
echo "import bird\n print(bird.hello())" > bird/main.py
uv run bird/main.py
```

```console
Traceback (most recent call last):
  File "/home/dev/bird/main.py", line 3, in <module>
    print(bird.hello())
AttributeError: module 'bird' has no attribute 'hello'
```

I would like to run `bird/main.py` from any directory that the user is in. 

If the user could specify the package root or `uv` could start searching for the `pyproject.toml` from the script path towards the root directory, that would help fix the issue.

uv version: `uv 0.3.2`






---

_Comment by @JamesMTSloan on 2024-08-28 08:47_

Something like `uv -C bird/ run` would be useful. Missing this was one of the only slight pain-points I had migrating from Poetry. git also provides an equivalent `-C` option.

Although, maybe `uvx --from <path-to-project> <entry-point>` comes close.

---

_Comment by @charliermarsh on 2024-08-28 12:54_

`uv --directory` exists already -- is this what you're looking for?

---

_Label `question` added by @charliermarsh on 2024-08-28 12:54_

---

_Comment by @bluss on 2024-08-28 13:14_

uv --directory is hidden, I guess because it is not yet ready?

---

_Comment by @zanieb on 2024-08-28 13:25_

Oh.. I want this too. Why do we hide it? haha

---

_Comment by @charliermarsh on 2024-08-28 13:26_

I think we hide it because we wanted to change the semantics such that relative paths were resolved relative to the CWD rather than `--directory`.

---

_Comment by @charliermarsh on 2024-08-28 13:28_

See: https://github.com/astral-sh/uv/issues/5613. We should unhide it though. I think I originally added it to facilitate benchmarking, and there wasn't consensus on the team at the time.

---

_Comment by @zanieb on 2024-08-28 13:34_

Oh.. hm.. I guess it might make sense to sort that out since it'll be a breaking change.

---

_Comment by @haarisr on 2024-08-28 17:54_

`--directory` works great. Didn't know that existed, thanks!

> I think we hide it because we wanted to change the semantics such that relative paths were resolved relative to the CWD rather than `--directory`.

This makes more sense to me. You can use shell autocomplete to find the file instead of knowing the script name relative to the project directory

---

_Comment by @zanieb on 2024-08-28 17:56_

I think it makes sense but it seems non-trivial and I can see people getting confused either way.

---

_Comment by @bluss on 2024-08-28 18:10_

I would feature request that it should work like this: https://github.com/astral-sh/uv/pull/5579#issuecomment-2257207224

But it doesn't have to be --directory that does it. It could be some other flag on `uv run` with those requested semantics? It is not just about convenience, if the user can't control the current working directory (user "can't control it" here because the --directory has to be equal to the project directory with the uv project) then some commands can't be used correctly at all.

But the current rules for --directory have precedent in cargo -C and git -C.

---

_Comment by @zanieb on 2024-08-28 18:16_

@bluss to clarify, `cargo -C` and `git -C` work like our current behavior?

---

_Comment by @bluss on 2024-08-28 18:20_

@zanieb Yep, at least for git I'm sure of that, git says "Run as if git was started in `<path>` instead of the current working directory"

---

_Referenced in [astral-sh/ruff#13074](../../astral-sh/ruff/pulls/13074.md) on 2024-08-28 18:46_

---

_Referenced in [astral-sh/uv#5613](../../astral-sh/uv/issues/5613.md) on 2024-09-13 01:57_

---

_Comment by @charliermarsh on 2024-09-24 00:54_

We now support `--project` which accepts a path to a directory like this.

---

_Closed by @charliermarsh on 2024-09-24 00:54_

---

_Assigned to @charliermarsh by @charliermarsh on 2024-09-24 00:54_

---

_Comment by @bgolinvaux on 2024-10-08 11:21_

Thank your for this `--project` flag and, more generally, for `uv` :smile:  
I am in love with uv. It's been a while since a two-letter CLI tool gave me so much joy.

---

_Referenced in [modelcontextprotocol/docs#121](../../modelcontextprotocol/docs/issues/121.md) on 2025-01-28 02:23_

---

_Comment by @mdengler on 2025-02-13 19:14_

> We now support `--project` which accepts a path to a directory like this.

Can I help to point to some documentation on how to use this?  As a poetry user, it's unclear how `--project` (or `--directory`) is helping here:

```
$ uv --version
uv 0.5.25

$ mkdir bird-uv-6733
$ cd bird-uv-6733/
$ mkdir bird
$ echo -e 'def hello():\n  print("hello bird")' > bird/__init__.py
$ mkdir examples
$ echo -e "import bird\nprint(bird.hello())" > examples/example1.py
$ tree .
.
├── bird
│   └── __init__.py
└── examples
    └── example1.py

3 directories, 2 files

$ uv run examples/example1.py 
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'


$ uv --project=$PWD run examples/example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'

$ uv --project=. run examples/example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'

$ uv --directory=$PWD run examples/example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'

$ uv --directory=. run examples/example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'

$ uv --directory=examples run examples/example1.py
error: Failed to spawn: `examples/example1.py`
  Caused by: No such file or directory (os error 2)

  $ uv --directory=examples run example1.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733/examples/example1.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'

```

I can file a new issue, but it seems something like what I've tried should actually work; that is, changes including:

1. add `$PWD` (or `--project` argument) to `PYTHONPATH` (this is not done; the script directory gets added to `PYTHONPATH`, regardless of the value of `--project` or `--directory`)
2. run `examples/example1.py`

...like this:

```
$ PYTHONPATH=. uv run examples/example1.py 
hello bird
None
```

---

_Comment by @charliermarsh on 2025-02-13 19:27_

@mdengler -- please open a new issue articulating your problem rather than commenting on a closed issue.

---

_Referenced in [astral-sh/uv#11490](../../astral-sh/uv/issues/11490.md) on 2025-02-13 19:56_

---

_Comment by @mdengler on 2025-02-13 19:58_

@charliermarsh sure; I opened #11490.

> > > We now support --project which accepts a path to a directory like this.

> > Can I help to point to some documentation on how to use this? 

> please open a new issue

I asked for help documenting how the new feature was going to be used, though.  Also, the original report does not seem to be resolved with either `--project` or `--directory`:

```
$ mkdir bird-uv-6733-orig
$ cd bird-uv-6733-orig
$ mkdir bird
$ uv init bird
$ echo -e "import bird\nprint(bird.hello())" > bird/main.py
$ uv run bird/main.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733-orig/bird/main.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'
$ uv --project=. run bird/main.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733-orig/bird/main.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'
$ uv --directory=. run bird/main.py
Traceback (most recent call last):
  File "/home/martin/src/bird-uv-6733-orig/bird/main.py", line 1, in <module>
    import bird
ModuleNotFoundError: No module named 'bird'
```


Should this issue be re-opened?  Or could you point me to where in the documentation it describes how to use `--project` to address this (#6733) issue?

---

_Comment by @mdengler on 2025-02-13 20:00_

Happy to add to documentation.  I was surprised to find no examples of `uv run bird/main.py` that worked, including with `--project=.`.  I'd just like to help avoid other people getting stuck like I did/am.

---

_Comment by @charliermarsh on 2025-02-13 20:17_

Thanks @mdengler -- I appreciate it. I just find it hard to keep track of outstanding work when there's discussion in closed issues since there's so much activity in the repo. I'm happy to discuss this all in https://github.com/astral-sh/uv/issues/11490!

---

_Comment by @mdengler on 2025-02-13 20:34_

Understood -- you all are doing tons.  Thanks for `uv` and the great work.

---

_Referenced in [astral-sh/uv#14585](../../astral-sh/uv/issues/14585.md) on 2025-07-13 10:22_

---
