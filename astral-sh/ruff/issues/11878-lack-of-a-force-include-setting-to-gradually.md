```yaml
number: 11878
title: Lack of a --force-include setting to gradually introduce ruff into matured codebase
type: issue
state: open
author: cam-laf
labels:
  - question
assignees: []
created_at: 2024-06-14T16:42:01Z
updated_at: 2024-06-21T15:48:32Z
url: https://github.com/astral-sh/ruff/issues/11878
synced_at: 2026-01-12T15:54:51Z
```

# Lack of a --force-include setting to gradually introduce ruff into matured codebase

---

_@cam-laf_

I want to gradually introduce files under checking for ruff check. I'm using pre-commit with ruff. I work on a large repository (in the thousdands of python files) and I cannot reasonably include ` # noqa` at the top of each of those files. What I want is to have the `include` setting be the single source of truth of which files would be run under ruff. Since ruff's pre-commit passes all files changed in a commit to ruff, it means that the include setting is ignored. Although ruff's pre-commit uses `--force-exclude`, I'd have to `exclude` _every_ single path that I'm not currently wanting to check. I want to be able to "opt-in" by file to using ruff. Honestly I think that the lack of this feature probably prevents many matured python codebases from using ruff. The only option that I've come up with is creating my own pre-commit script which would pass my own include list to `ruff`. Is there any better way to do what I want here?


---

_Comment by @MichaReiser on 2024-06-15 08:14_

I'm not a pre-commit user myself but could pre-commit's [`files`](https://pre-commit.com/#hooks-files) option be what you're looking for? I think it should allow you to only run Ruff on a specific set of files instead of all files.

---

_Label `question` added by @MichaReiser on 2024-06-15 08:14_

---

_Comment by @cam-laf on 2024-06-21 15:05_

> I'm not a pre-commit user myself but could pre-commit's [`files`](https://pre-commit.com/#hooks-files) option be what you're looking for? I think it should allow you to only run Ruff on a specific set of files instead of all files.

@MichaReiser It's possible but not sane for what I want to do as I would have to insert a hefty regex for every single file/directory that I want to include. I.e. I cannot just do the simple
```yaml
files: |
[ 
   "path/to/file.py",
   "path/to/dir/",
   "file.py"
]
```
nor can I do
```yaml
files: |
" path/to/file.py,
  path/to/dir/,
  file.py,
"
```

I'd have to do something like
```yaml
files: | 
"
(^path/to/file\.py$)|
(^my/dir/)
"
```
 Given the size of my repository, roughly ~3000 python files, I'm expecting to need this --include behavior until I get to a very sizable percentage of ~3000, especially uniformly across certain file directories. Now that's not so bad for 2 paths, but look what it's like with nearly two dozen:
```yaml
         files: |
          (^path/to/file\.py$)|
          (^my/dir/)|
          (^random/path1/file1\.py$)|
          (^random/path2/file2\.py$)|
          (^random/path3/subdir1/file3\.py$)|
          (^random/path4/file4\.py$)|
          (^random/path5/subdir2/file5\.py$)|
          (^random/path6/file6\.py$)|
          (^random/path7/subdir3/subsubdir1/file7\.py$)|
          (^random/path8/file8\.py$)|
          (^random/path9/subdir4/file9\.py$)|
          (^random/path10/file10\.py$)|
          (^random/path11/subdir5/file11\.py$)|
          (^random/path12/file12\.py$)|
          (^random/path13/subdir6/file13\.py$)|
          (^random/path14/file14\.py$)|
          (^random/path15/subdir7/subsubdir2/file15\.py$)|
          (^random/path16/file16\.py$)|
          (^random/path17/subdir8/file17\.py$)|
          (^random/path18/file18\.py$)|
          (^random/path19/subdir9/file19\.py$)|
          (^random/path20/file20\.py$)|
          (^random/path21/subdir10/file21\.py$)|
          (^random/path22/file22\.py$)
```
This is not nearly as manageable IMO as having

```toml
include = [
    "path/to/file.py",
    "my/dir/",
    "random/path1/file1.py",
    "random/path2/file2.py",
    "random/path3/subdir1/file3.py",
    "random/path4/file4.py",
    "random/path5/subdir2/file5.py",
    "random/path6/file6.py",
    "random/path7/subdir3/subsubdir1/file7.py",
    "random/path8/file8.py",
    "random/path9/subdir4/file9.py",
    "random/path10/file10.py",
    "random/path11/subdir5/file11.py",
    "random/path12/file12.py",
    "random/path13/subdir6/file13.py",
    "random/path14/file14.py",
    "random/path15/subdir7/subsubdir2/file15.py",
    "random/path16/file16.py",
    "random/path17/subdir8/file17.py",
    "random/path18/file18.py",
    "random/path19/subdir9/file19.py",
    "random/path20/file20.py",
    "random/path21/subdir10/file21.py",
    "random/path22/file22.py"
]


---

_Comment by @MichaReiser on 2024-06-21 15:48_

I agree that the regex syntax is less readable, maybe something worth bringing up in the pre-commit repository? Otherwise, the configuration looks very similar to me. 

---
