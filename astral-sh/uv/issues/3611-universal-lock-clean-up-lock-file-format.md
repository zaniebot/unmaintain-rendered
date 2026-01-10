```yaml
number: 3611
title: "universal-lock: clean up lock file format"
type: issue
state: closed
author: BurntSushi
labels:
  - preview
assignees: []
created_at: 2024-05-15T16:43:52Z
updated_at: 2024-07-17T23:03:25Z
url: https://github.com/astral-sh/uv/issues/3611
synced_at: 2026-01-10T05:31:37Z
```

# universal-lock: clean up lock file format

---

_Issue opened by @BurntSushi on 2024-05-15 16:43_

In #3314, an initial version of a universal lock file format was added. It came with a lot of TODOs in the code and some uncertainties with the data model. Before we declare the lock file ready to use by users, we should take another pass over it and smooth things out. Here is a non-exhaustive list of things:

* [x] #4924
* [x] There are some redundancies in the current format where we repeat information. For example, if we have a path dependency to a wheel, the path is encoded in the source (part of the distribution's ID) and then again in a `wheel` table. The same happens for sdists I believe. I think we would ideally not have `wheel` or `sdist` tables for distributions that have a `path` or `directory` or `git` source. I think the rule should be that an `sdist` and `wheel` table are only present if it's _possible_ for more than one of them to exist. I think that only happens with registry dependencies.
* [x] Don't store absolute paths for path dependencies (https://github.com/astral-sh/uv/issues/3506)
* [x] The actual TOML serialization format is a bit bulky right now. I think it uses table arrays a bit too much. I'd love to, for example, squash wheels down into an inline table or an array of strings.
* [x] Consider indenting `[distribution.dependencies]` to make the file easier to scan.
* [x] Improve the formatting of source, specifically paths, e.g. we currently have `source = "editable+."` which looks bad.
* [x] The git source could use an audit and a potential refactor. We need to decide on how we want to encode the various git information (revision, tag, branch, sub-directory, URL).
* [x] We should be able to drop the `source` (and possibly also `version`) from a `distribution.dependency` entry when there is only one distribution with that package name.
* [x] Explore whether merge conflicts are created in basic cases. For example, if two different PRs add different dependencies and one gets merged, does the other PR wind up with conflicts that must be merged by hand?

---

_Label `preview` added by @BurntSushi on 2024-05-15 16:43_

---

_Comment by @charliermarsh on 2024-05-18 13:48_

Another piece here: we currently lose the file size (reported by the registry), which is used in downloading to facilitate prioritization (i.e., we start larger downloads earlier).

(Done in #3652.)

---

_Comment by @BurntSushi on 2024-06-27 13:23_

For testing whether conflicts happen with independent updates to the lock file, I did something like this. To start:

```
$ cat > pyproject.toml <<EOF
[project]
name = 'project'
version = '0.1.0'
requires-python = '>=3.12'
dependencies = [
  'anyio<4.4',
  'click',
  'Flask<3.0.3',
]
EOF
$ uv lock
```

Then I made two branches from this point: one where I changed `anyio<4.4` to `anyio<5` and re-locked, and another where I changed `Flask<3.0.3` to `Flask<4` and re-locked. In both cases, the lock file is updated to new versions of packages. I then went back to `master` and tried to merge each of the branches, one after the other (in both orders). The result is that they both merge cleanly.

I then tried this same example with Poetry. Starting with:

```
$ cat > pyproject.toml <<EOF
[tool.poetry]
name = "project"
version = "0.1.0"
description = ""
authors = ["someone"]

[tool.poetry.dependencies]
python = "^3.12"
anyio = "<4.4"
click = "*"
Flask = "<3.0.3"

[build-system]
requires = ["poetry-core"]
build-backend = "poetry.core.masonry.api"
$ poetry lock
```

I then repeated the same process as above: creating two different branches from master, updating `anyio` in one and `Flask` in another and then re-locking. I then went back to master and tried to merge the anyio update branch, which worked, but then trying to merge the Flask update branch results in a merge conflict:

```
$ git merge --no-ff ag/update-flask
Auto-merging poetry.lock
CONFLICT (content): Merge conflict in poetry.lock
Auto-merging pyproject.toml
Recorded preimage for 'poetry.lock'
Automatic merge failed; fix conflicts and then commit the result.

$ git diff
diff --cc poetry.lock
index d5a5b19,acd9943..0000000
--- a/poetry.lock
+++ b/poetry.lock
@@@ -217,4 -217,4 +217,8 @@@ watchdog = ["watchdog (>=2.3)"
  [metadata]
  lock-version = "2.0"
  python-versions = "^3.12"
++<<<<<<< HEAD
 +content-hash = "a84aedf401179b44c4e263992f3be48a231859fbe39edb2d8e315db13cc7f2ce"
++=======
+ content-hash = "5ebe45498202bf09fe0279c1096870f7f6e766ff0b09cd255f80d4aa31f723a4"
++>>>>>>> ag/update-flask
```

---

_Comment by @sbidoul on 2024-07-13 14:11_

Hi, do you have any plan regarding locking of build dependencies (build-system.requires) for sdists and source trees ?

---

_Comment by @charliermarsh on 2024-07-13 16:46_

Not currently, but it would be a natural thing for us to support... It's somewhat expensive because it means we have to download the source distribution for all packages regardless of whether we can extract the metadata from a wheel alone.

---

_Comment by @sbidoul on 2024-07-13 17:48_

And will there be a mean for the sync/install command to abort if it has to download a build dependency that is not locked?

---

_Comment by @konstin on 2024-07-15 10:56_

@sbidoul Could you add a separate issue with some background on the motivation?

---

_Comment by @charliermarsh on 2024-07-17 23:03_

All the issues above have been crossed off. Lets close this out for now and we can track new issues separately as they come up.

---

_Closed by @charliermarsh on 2024-07-17 23:03_

---
