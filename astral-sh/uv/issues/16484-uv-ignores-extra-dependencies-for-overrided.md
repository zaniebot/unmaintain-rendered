```yaml
number: 16484
title: UV ignores extra dependencies for overrided library
type: issue
state: open
author: Wintreist
labels:
  - bug
assignees: []
created_at: 2025-10-28T13:55:23Z
updated_at: 2025-10-30T10:44:09Z
url: https://github.com/astral-sh/uv/issues/16484
synced_at: 2026-01-12T16:02:32Z
```

# UV ignores extra dependencies for overrided library

---

_@Wintreist_

### Summary

Hi!
I am testing the changes in the "my-testing-lib" library. It is also used by other libraries from my repository, so I can't just change the link in sources to the branch where I'm testing it, so I use override-dependencies to force download it from the branch I need. But there was a problem that UV does not install any libraries from the "gui" group of extra dependencies

MyLib pyproject.toml
```
[project]
name = "my-lib"
version = "0.1.0"
dependencies = [
    ...
]
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"
[tool.uv]
override-dependencies = [
    "my-testing-lib @ git+ssh://my-git-repo@TESTING-BRANCH#subdirectory=my_testing_lib"
]
[tool.uv.sources]
...
# my-testing-lib = { git = "ssh://my-git-repo", subdirectory = "my_testing_lib" }

[dependency-groups]
dev = [
    ...
    "my-testing-lib[gui]",
]
```
MyTestingLib pyproject.toml:
```
[project]
name = "my-testing-lib"
version = "0.1.0"
dependencies = [
    ...
]
[build-system]
requires = ["hatchling"]
build-backend = "hatchling.build"

[project.optional-dependencies]
gui = [
    ...
    "pyqt-extra-dep-lib",
]

[tool.uv.sources]
...
pyqt-extra-dep-lib = { git = "ssh://git@my-git-repo", subdirectory = "pyqt_extra_dep_lib" }
```
If I try to run the command with override-dependencies, it won't find anything.
```
(my-lib) PS D:\...\Repositories\my-lib> uv tree --package pyqt-extra-dep-lib
Resolved 232 packages in 1ms
(my-lib) PS D:\...\Repositories\my-lib> 
```
But if I return everything to the standard view and synchronize venv, the library will be installed:
```
(my-lib) PS D:\...\Repositories\my-lib> uv tree --package pyqt-extra-dep-lib --reverse
Resolved 236 packages in 1ms
pyqt-extra-dep-lib v0.1.0
└── my-testing-lib v0.1.0 (extra: gui)
    └── my-lib[gui] v0.1.0 (group: dev)
(my-lib) PS D:\...\Repositories\my-lib>
```

### Platform

Windows 10 x86_64

### Version

uv 0.9.5

### Python version

3.12.12

---

_Label `bug` added by @Wintreist on 2025-10-28 13:55_

---

_Comment by @konstin on 2025-10-28 14:26_

This behavior is by design, overrides replace all existing mentions of a package, including extras, with the new specifier. This is exists to support cases where you want to change the extra too.

> It is also used by other libraries from my repository, so I can't just change the link in sources to the branch where I'm testing it, so I use override-dependencies to force download it from the branch I need.

I'm not following, what happens if you use sources for it? Alternatively, can you use `constraint-dependencies` to set the URL? Constraints can also set a URL, but don't remove extras.

---

_Comment by @Wintreist on 2025-10-28 15:07_

@konstin 

> I'm not following, what happens if you use sources for it?

Conditionally, this will happen:
```
  × Failed to resolve dependencies for `other-lib` (v0.1.1)
  ╰─▶ Requirements contain conflicting URLs for package `my-testing-lib` in all marker environments:
      - git+ssh://git@my-git-repo@TESTING-BRANCH#subdirectory=my_testing_lib
      - git+ssh://git@my-git-repo#subdirectory=my_testing_lib
  help: `other-lib` (v0.1.1) was included because `my-lib` (v0.1.0) depends on `other-lib`
```
> This is exists to support cases where you want to change the extra too.

But if I want to override extra dependencies, I will specify them in "my-lib" pyproject.toml:
```
[tool.uv]
override-dependencies = [
    "my-testing-lib @ git+ssh://my-git-repo@TESTING-BRANCH#subdirectory=my_testing_lib"
    "pyqt-extra-dep-lib @ git+ssh://git@my-git-repo@TESTING-BRANCH#subdirectory=pyqt_extra_dep_lib"
]
```

> Alternatively, can you use constraint-dependencies to set the URL?

I didn't quite figure out the difference between override and constraint, but I replaced override with constraint and an error occurred:
```
[tool.uv]
constraint-dependencies = [
    "my-testing-lib @ git+ssh://my-git-repo@TESTING-BRANCH#subdirectory=my_testing_lib"
]
```
```
  × Failed to resolve dependencies for `my-testing-lib` (v0.1.0)
  ╰─▶ Package `pyqt-extra-dep-lib` was included as a URL dependency. URL dependencies must be expressed as direct requirements or constraints. Consider adding `pyqt-extra-dep-lib @
      git+ssh://git@my-git-repo#subdirectory=pyqt_extra_dep_lib` to your dependencies or constraints file.
  help: `my-testing-lib` (v0.1.0) was included because `my-lib:dev` (v0.1.0) depends on `my-testing-lib`
```
As I understand it, UV wants me to manually specify where to get the dependency "pyqt-extra-dep-lib"
But I don't need it. It's enough for me to override only the library under test. If I want to override something else, I'll do it on purpose.

---

_Comment by @konstin on 2025-10-28 15:55_

Sorry, I'm a but lost what's happening here, can you share a repo that reproduces the problem?



> > I'm not following, what happens if you use sources for it?
> 
> Conditionally, this will happen:
> 
> ```
>   × Failed to resolve dependencies for `other-lib` (v0.1.1)
>   ╰─▶ Requirements contain conflicting URLs for package `my-testing-lib` in all marker environments:
>       - git+ssh://git@my-git-repo@TESTING-BRANCH#subdirectory=my_testing_lib
>       - git+ssh://git@my-git-repo#subdirectory=my_testing_lib
>   help: `other-lib` (v0.1.1) was included because `my-lib` (v0.1.0) depends on `other-lib`
> ```

This should only happen when there are two conflicting sources.
 
> > Alternatively, can you use constraint-dependencies to set the URL?
> 
> 
> I didn't quite figure out the difference between override and constraint, but I replaced override with constraint and an error occurred:
> 
> ```
> [tool.uv]
> constraint-dependencies = [
>     "my-testing-lib @ git+ssh://my-git-repo@TESTING-BRANCH#subdirectory=my_testing_lib"
> ]
> ```
> 
> 
>     
>   
> 
> ```
>   × Failed to resolve dependencies for `my-testing-lib` (v0.1.0)
>   ╰─▶ Package `pyqt-extra-dep-lib` was included as a URL dependency. URL dependencies must be expressed as direct requirements or constraints. Consider adding `pyqt-extra-dep-lib @
>       git+ssh://git@my-git-repo#subdirectory=pyqt_extra_dep_lib` to your dependencies or constraints file.
>   help: `my-testing-lib` (v0.1.0) was included because `my-lib:dev` (v0.1.0) depends on `my-testing-lib`
> ```

This error should only happen when a registry dependency uses a URL, other URL dependency should not cause this error. 

---

_Comment by @Wintreist on 2025-10-28 16:42_

@konstin 
I created a repository with a structure similar to mine:
https://github.com/Wintreist/uv_issue_16484


---

_Comment by @konstin on 2025-10-29 12:12_

Thank you for the repo! In practice, are those packages in the same repo, and do they use `git = ` deps inside the repo (vs. `path = ` deps in the repo)?

---

_Comment by @Wintreist on 2025-10-29 12:24_

Hi!

> In practice, are those packages in the same repo

In practice, my-lib is located in a separate git repository. All the others are in the other one

> do they use git =  deps inside the repo (vs. path =  deps in the repo)?

They all use git= in all libraries (which are written by us, the rest we take from pypi). path= not in use


---

_Comment by @konstin on 2025-10-29 15:29_

Can you use `path =` dependencies inside the git repository? This could solve your different-branch problem. uv recognizes root -> git -> path dependencies and internally converts it to another git dependency.

---

_Comment by @Wintreist on 2025-10-30 10:44_

@konstin, Hi!
I created a [branch](https://github.com/Wintreist/uv_issue_16484/tree/test_update_git_to_path) in the same repository where I tested your proposal. In the end, there is still a problem.
As I mentioned above, my-lib is in a separate repository, so the my-testing-lib, other-lib, and pyqt-extra-dep-lib libraries still need to be accessed via git+ssh, so there is still a conflicting [URL error](https://github.com/Wintreist/uv_issue_16484/blob/test_update_git_to_path/readme.md).


---
