```yaml
number: 7329
title: "Project's name interferes with resolution logic"
type: issue
state: closed
author: nhedger
labels:
  - error messages
assignees: []
created_at: 2024-09-12T13:26:21Z
updated_at: 2024-10-11T23:30:32Z
url: https://github.com/astral-sh/uv/issues/7329
synced_at: 2026-01-12T15:59:12Z
```

# Project's name interferes with resolution logic

---

_@nhedger_

When attempting to `add` or `sync` a dependency to a project, if the dependency depends on a package whose name matches the name in your pyproject.toml, resolution fails.

---

Create the following **pyproject.toml**

```toml
[project]
name = "dagster"
version = "0.0.0"
description = "Add your description here"
requires-python = ">=3.12"
```

Run `uv add dagster-webserver --verbose` (`dagster-webserver` depends on `dagster`)


[log.txt](https://github.com/user-attachments/files/16980042/log.txt) (too big to paste)


<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

Changing the name in pyproject.toml makes it work correctly.

Running on macOS 14.6.1 (23G93), with uv 0.4.9 (Homebrew 2024-09-10)

---

_Comment by @charliermarsh on 2024-09-12 13:28_

I think it's correct to fail here.

---

_Comment by @nhedger on 2024-09-12 13:31_

I wasn't sure either. I guess it wasn't immediately obvious why the resolution was failing, which led me to dig further.

---

_Comment by @zanieb on 2024-09-12 13:34_

We could add a note about this somewhere. Is the error message helpful?

---

_Comment by @nhedger on 2024-09-12 14:21_

The error got me confused at first because I did not understand why the dependency I was adding was depending on my project.

```
Because only the following versions of dagster-webserver are available:
          dagster-webserver>=1.3.14,<=1.4.12
          dagster-webserver>=1.4.13,<=1.5.13
          dagster-webserver>=1.5.14,<=1.7.1
          dagster-webserver>=1.7.2,<=1.7.8
          dagster-webserver>=1.7.9
      and dagster-webserver==1.3.14 depends on your project, we can conclude that all of:
          dagster-webserver<1.4.0
          dagster-webserver>1.4.12,<1.4.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
```
> and dagster-webserver==1.3.14 depends on your project

---

_Comment by @zanieb on 2024-09-12 15:42_

I see okay thanks! I think we can do better there.

---

_Label `error messages` added by @zanieb on 2024-09-12 15:42_

---

_Comment by @zanieb on 2024-09-16 00:12_

A possible solution here is that we'd add a "hint" after a resolution error (we have some infrastructure for this) if the project name shadows the name of a package available in the index. I'm not sure if we'll have checked if the package is available though. 

Another (perhaps easier to implement)  solution is to hint if a dependency of the project depends on the project itself e.g. here `dagster-webserver` is a dependency of the project and depends on the project.

---

_Comment by @lucab on 2024-09-18 09:24_

As a side note related to `uv add`, the dependency resolution error message is overly verbose in part because it reports details about all available versions, at once.
Manually asking the resolver to pick the "currently latest available" version results in a much a shorter and direct output:
```
$ uv add 'dagster-webserver==1.8.7'
  × No solution found when resolving dependencies:
  ╰─▶ Because dagster-webserver==1.8.7 depends on your project and your project depends
      on dagster-webserver==1.8.7, we can conclude that your project's requirements are unsatisfiable.
  help: If this is intentional, run `uv add --frozen` to skip the lock and sync steps.
```

---

_Comment by @zanieb on 2024-09-18 14:00_

Oof yeah here's the whole error message.

I think there's another problem here. Like we should collapse these and say "all versions of `dagster-webserver` depend on your project" instead of enumerating each version. I can look into that... probably hard.
```
 × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of dagster-webserver are available:
          dagster-webserver>=1.3.14,<=1.4.12
          dagster-webserver>=1.4.13,<=1.5.13
          dagster-webserver>=1.5.14,<=1.7.1
          dagster-webserver>=1.7.2,<=1.7.8
          dagster-webserver>=1.7.9
      and dagster-webserver==1.3.14 depends on your project, we can conclude that all of:
          dagster-webserver<1.4.0
          dagster-webserver>1.4.12,<1.4.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.0 depends on your project, we can conclude that all of:
          dagster-webserver<1.4.1
          dagster-webserver>1.4.12,<1.4.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.1 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.4.3
          dagster-webserver>1.4.12,<1.4.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.3 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.4.5
          dagster-webserver>1.4.12,<1.4.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.5 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.4.7
          dagster-webserver>1.4.12,<1.4.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.7 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.4.9
          dagster-webserver>1.4.12,<1.4.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.9 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.4.11
          dagster-webserver>1.4.12,<1.4.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.11 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.4.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.13 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.4.15
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.15 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.4.17
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.4.17 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.5.1
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.5.1 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.5.3
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.5.3 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.5.5
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.5.5 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.5.7
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.5.7 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.5.9
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.5.9 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.5.11
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.5.11 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.5.13
          dagster-webserver>1.5.13,<1.5.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.5.13 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.6.0
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.6.0 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.6.2
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.6.2 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.6.4
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.6.4 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.6.6
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.6.6 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.6.8
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.6.8 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.6.10
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.6.10 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.6.12
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.6.12 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.6.14
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.6.14 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.7.1
          dagster-webserver>1.7.1,<1.7.2
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.7.1 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.7.3
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.7.3 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.7.5
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.7.5 depends on your project and your project, we can conclude that all of:
          dagster-webserver<1.7.7
          dagster-webserver>1.7.8,<1.7.9
      depend on your project.
      And because dagster-webserver==1.7.7 depends on your project and your project, we can conclude that dagster-webserver<1.7.9 depends on your project.
      And because dagster-webserver==1.7.9 depends on your project and your project, we can conclude that dagster-webserver<1.7.11 depends on your project.
      And because dagster-webserver==1.7.11 depends on your project and your project, we can conclude that dagster-webserver<1.7.13 depends on your project.
      And because dagster-webserver==1.7.13 depends on your project and your project, we can conclude that dagster-webserver<1.7.15 depends on your project.
      And because dagster-webserver==1.7.15 depends on your project and your project, we can conclude that dagster-webserver<1.8.0 depends on your project.
      And because dagster-webserver==1.8.0 depends on your project and your project, we can conclude that dagster-webserver<1.8.2 depends on your project.
      And because dagster-webserver==1.8.2 depends on your project and dagster-webserver==1.8.3 was yanked, we can conclude that dagster-webserver<1.8.4 depends on your project.
      And because dagster-webserver==1.8.4 depends on your project and your project, we can conclude that dagster-webserver<1.8.6 depends on your project.
      And because dagster-webserver==1.8.6 depends on your project and your project depends on dagster-webserver, we can conclude that your project's requirements are unsatisfiable.
```

---

_Comment by @zanieb on 2024-09-18 14:02_

Here's the derivation tree

```
Resolver derivation tree after reduction
  dagster==0.0.0 depends on dagster-webserver*
    dagster-webserver==1.8.7 depends on dagster==1.8.7
      dagster-webserver==1.8.6 depends on dagster==1.8.6
        dagster-webserver==1.8.5 depends on dagster==1.8.5
          dagster-webserver==1.8.4 depends on dagster==1.8.4
            dagster-webserver==1.8.3 yanked
              dagster-webserver==1.8.2 depends on dagster==1.8.2
                dagster-webserver==1.8.1 depends on dagster==1.8.1
                  dagster-webserver==1.8.0 depends on dagster==1.8.0
                    dagster-webserver==1.7.16 depends on dagster==1.7.16
                      dagster-webserver==1.7.15 depends on dagster==1.7.15
                        dagster-webserver==1.7.14 depends on dagster==1.7.14
                          dagster-webserver==1.7.13 depends on dagster==1.7.13
                            dagster-webserver==1.7.12 depends on dagster==1.7.12
                              dagster-webserver==1.7.11 depends on dagster==1.7.11
                                dagster-webserver==1.7.10 depends on dagster==1.7.10
                                  dagster-webserver==1.7.9 depends on dagster==1.7.9
                                    dagster-webserver==1.7.8 depends on dagster==1.7.8
                                      dagster-webserver==1.7.7 depends on dagster==1.7.7
                                        dagster-webserver==1.7.6 depends on dagster==1.7.6
                                          dagster-webserver==1.7.5 depends on dagster==1.7.5
                                            dagster-webserver==1.7.4 depends on dagster==1.7.4
                                              dagster-webserver==1.7.3 depends on dagster==1.7.3
                                                dagster-webserver==1.7.2 depends on dagster==1.7.2
                                                  dagster-webserver==1.7.1 depends on dagster==1.7.1
                                                    dagster-webserver==1.7.0 depends on dagster==1.7.0
                                                      dagster-webserver==1.6.14 depends on dagster==1.6.14
                                                        dagster-webserver==1.6.13 depends on dagster==1.6.13
                                                          dagster-webserver==1.6.12 depends on dagster==1.6.12
                                                            dagster-webserver==1.6.11 depends on dagster==1.6.11
                                                              dagster-webserver==1.6.10 depends on dagster==1.6.10
                                                                dagster-webserver==1.6.9 depends on dagster==1.6.9
                                                                  dagster-webserver==1.6.8 depends on dagster==1.6.8
                                                                    dagster-webserver==1.6.7 depends on dagster==1.6.7
                                                                      dagster-webserver==1.6.6 depends on dagster==1.6.6
                                                                        dagster-webserver==1.6.5 depends on dagster==1.6.5
                                                                          dagster-webserver==1.6.4 depends on dagster==1.6.4
                                                                            dagster-webserver==1.6.3 depends on dagster==1.6.3
                                                                              dagster-webserver==1.6.2 depends on dagster==1.6.2
                                                                                dagster-webserver==1.6.1 depends on dagster==1.6.1
                                                                                  dagster-webserver==1.6.0 depends on dagster==1.6.0
                                                                                    dagster-webserver==1.5.14 depends on dagster==1.5.14
                                                                                      dagster-webserver==1.5.13 depends on dagster==1.5.13
                                                                                        dagster-webserver==1.5.12 depends on dagster==1.5.12
                                                                                          dagster-webserver==1.5.11 depends on dagster==1.5.11
                                                                                            dagster-webserver==1.5.10 depends on dagster==1.5.10
                                                                                              dagster-webserver==1.5.9 depends on dagster==1.5.9
                                                                                                dagster-webserver==1.5.8 depends on dagster==1.5.8
                                                                                                  dagster-webserver==1.5.7 depends on dagster==1.5.7
                                                                                                    dagster-webserver==1.5.6 depends on dagster==1.5.6
                                                                                                      dagster-webserver==1.5.5 depends on dagster==1.5.5
                                                                                                        dagster-webserver==1.5.4 depends on dagster==1.5.4
                                                                                                          dagster-webserver==1.5.3 depends on dagster==1.5.3
                                                                                                            dagster-webserver==1.5.2 depends on dagster==1.5.2
                                                                                                              dagster-webserver==1.5.1 depends on dagster==1.5.1
                                                                                                                dagster-webserver==1.5.0 depends on dagster==1.5.0
                                                                                                                  dagster-webserver==1.4.17 depends on dagster==1.4.17
                                                                                                                    dagster-webserver==1.4.16 depends on dagster==1.4.16
                                                                                                                      dagster-webserver==1.4.15 depends on dagster==1.4.15
                                                                                                                        dagster-webserver==1.4.14 depends on dagster==1.4.14
                                                                                                                          dagster-webserver==1.4.13 depends on dagster==1.4.13
                                                                                                                            dagster-webserver==1.4.12 depends on dagster==1.4.12
                                                                                                                              dagster-webserver==1.4.11 depends on dagster==1.4.11
                                                                                                                                dagster-webserver==1.4.10 depends on dagster==1.4.10
                                                                                                                                  dagster-webserver==1.4.9 depends on dagster==1.4.9
                                                                                                                                    dagster-webserver==1.4.8 depends on dagster==1.4.8
                                                                                                                                      dagster-webserver==1.4.7 depends on dagster==1.4.7
                                                                                                                                        dagster-webserver==1.4.6 depends on dagster==1.4.6
                                                                                                                                          dagster-webserver==1.4.5 depends on dagster==1.4.5
                                                                                                                                            dagster-webserver==1.4.4 depends on dagster==1.4.4
                                                                                                                                              dagster-webserver==1.4.3 depends on dagster==1.4.3
                                                                                                                                                dagster-webserver==1.4.2 depends on dagster==1.4.2
                                                                                                                                                  dagster-webserver==1.4.1 depends on dagster==1.4.1
                                                                                                                                                    dagster-webserver==1.4.0 depends on dagster==1.4.0
                                                                                                                                                      dagster-webserver==1.3.14 depends on dagster==1.3.14
                                                                                                                                                      no versions of dagster-webserver<1.3.14 | >1.3.14, <1.4.0 | >1.4.0, <1.4.1 | >1.4.1, <1.4.2 | >1.4.2, <1.4.3 | >1.4.3, <1.4.4 | >1.4.4, <1.4.5 | >1.4.5, <1.4.6 | >1.4.6, <1.4.7 | >1.4.7, <1.4.8 | >1.4.8, <1.4.9 | >1.4.9, <1.4.10 | >1.4.10, <1.4.11 | >1.4.11, <1.4.12 | >1.4.12, <1.4.13 | >1.4.13, <1.4.14 | >1.4.14, <1.4.15 | >1.4.15, <1.4.16 | >1.4.16, <1.4.17 | >1.4.17, <1.5.0 | >1.5.0, <1.5.1 | >1.5.1, <1.5.2 | >1.5.2, <1.5.3 | >1.5.3, <1.5.4 | >1.5.4, <1.5.5 | >1.5.5, <1.5.6 | >1.5.6, <1.5.7 | >1.5.7, <1.5.8 | >1.5.8, <1.5.9 | >1.5.9, <1.5.10 | >1.5.10, <1.5.11 | >1.5.11, <1.5.12 | >1.5.12, <1.5.13 | >1.5.13, <1.5.14 | >1.5.14, <1.6.0 | >1.6.0, <1.6.1 | >1.6.1, <1.6.2 | >1.6.2, <1.6.3 | >1.6.3, <1.6.4 | >1.6.4, <1.6.5 | >1.6.5, <1.6.6 | >1.6.6, <1.6.7 | >1.6.7, <1.6.8 | >1.6.8, <1.6.9 | >1.6.9, <1.6.10 | >1.6.10, <1.6.11 | >1.6.11, <1.6.12 | >1.6.12, <1.6.13 | >1.6.13, <1.6.14 | >1.6.14, <1.7.0 | >1.7.0, <1.7.1 | >1.7.1, <1.7.2 | >1.7.2, <1.7.3 | >1.7.3, <1.7.4 | >1.7.4, <1.7.5 | >1.7.5, <1.7.6 | >1.7.6, <1.7.7 | >1.7.7, <1.7.8 | >1.7.8, <1.7.9 | >1.7.9, <1.7.10 | >1.7.10, <1.7.11 | >1.7.11, <1.7.12 | >1.7.12, <1.7.13 | >1.7.13, <1.7.14 | >1.7.14, <1.7.15 | >1.7.15, <1.7.16 | >1.7.16, <1.8.0 | >1.8.0, <1.8.1 | >1.8.1, <1.8.2 | >1.8.2, <1.8.3 | >1.8.3, <1.8.4 | >1.8.4, <1.8.5 | >1.8.5, <1.8.6 | >1.8.6, <1.8.7 | >1.8.7
```

---

_Closed by @zanieb on 2024-09-20 18:07_

---

_Comment by @pawamoy on 2024-10-11 15:06_

Sorry, I'm not sure to understand the issue here, which I experience too.

Locally, with uv 0.4.20, I'm able to install my dependencies. Yet in CI, with the same version, 0.4.20, I'm getting this error:

```
 Using CPython 3.12.7 interpreter at: /opt/hostedtoolcache/Python/3.12.7/x64/bin/python
Creating virtual environment at: .venv
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of griffe-inherited-docstrings are
      available:
          griffe-inherited-docstrings<=1.0.0
          griffe-inherited-docstrings==1.0.1
      and griffe-inherited-docstrings==1.0.0 depends on your project, we can
      conclude that griffe-inherited-docstrings>=1.0.0,<1.0.1 depends on your
      project.
      And because griffe-inherited-docstrings==1.0.1 depends on your project,
      we can conclude that griffe-inherited-docstrings>=1.0.0 depends on your
      project.
      And because griffe:dev depends on griffe-inherited-docstrings>=1.0 and
      your project depends on griffe:dev, we can conclude that your project's
      requirements are unsatisfiable.

      hint: The package `griffe-inherited-docstrings` depends on the package
      `griffe` but the name is shadowed by your project. Consider changing the
      name of the project.
```

The only difference between local and CI is that in CI I pass `--no-editable` to `uv sync`, yet passing this same flag locally works fine (even after removing `uv.lock` and `.venv`).

Does this hint mean that cyclic dependencies are not supported :thinking:? I think it's pretty common for plugins/extensions to specify the main project as dependency (django-apps specify their accepted django versions, mkdocs plugins their accepted mkdocs versions, griffe extensions their accepted griffe versions, etc.), and then if the main project starts depending on a plugin/extension, it creates kind of a cyclic dependency (actual example: mkdocs depends on mkdocstrings to render API docs, and mkdocstrings specifies its accepted mkdocs versions), but I think it resolved fine previously.

Changing project names is not possible, and removing the `griffe` dep spec from `griffe-inherited-docstrings` is definitely not wanted either. Yet `griffe` requires `griffe-inherited-docstrings` to build its docs... So, what should I do here :melting_face:?

---

_Comment by @pawamoy on 2024-10-11 22:36_

Hmmm, looks like it's working again in CI :shrug: I'll let you know if I encounter the issue again :slightly_smiling_face: 

---

_Comment by @pawamoy on 2024-10-11 22:43_

Spoke too fast:

- https://github.com/mkdocstrings/griffe/actions/runs/11300146785/job/31432473858: OK
- https://github.com/mkdocstrings/griffe/actions/runs/11300147363/job/31432475050: OK
- https://github.com/mkdocstrings/griffe/actions/runs/11300201361/job/31432629294: Not OK

No dependency changes. Is it because the two first are on a tagged commit but not the last one O_o?

---

_Comment by @pawamoy on 2024-10-11 23:02_

Still cannot reproduce locally so none of this will be very helpful for you :bow: 

---

_Comment by @pawamoy on 2024-10-11 23:29_

Can confirm it works fine when CI is triggered by pushing a tag.

Here's what I gather:

- when installing deps (in non-editable mode) at an untagged commit
- uv builds my project `A` with version `x.y.z.devN+ghash`
- then wants to install `A@x.y.z.devN+ghash`, as well as `B` which depends on `A>=x.y.z`
- and it gets confused about the version of `A` to install :shrug: 

---
