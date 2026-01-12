```yaml
number: 7219
title: "uv --color=never shows colors :)"
type: issue
state: open
author: mattiasb
labels:
  - bug
  - cli
assignees: []
created_at: 2024-09-09T15:18:42Z
updated_at: 2025-04-06T21:48:03Z
url: https://github.com/astral-sh/uv/issues/7219
synced_at: 2026-01-12T15:59:11Z
```

# uv --color=never shows colors :)

---

_@mattiasb_

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->

If I run `uv --color=never --help` the help output is still full of colors. :)

![Screenshot from 2024-09-09 17-14-25](https://github.com/user-attachments/assets/c53d396e-7159-4b70-ae18-782f08ccabd2)

This is on `uv 0.4.7` FWIW.

---

_Label `bug` added by @charliermarsh on 2024-09-09 15:20_

---

_Label `cli` added by @charliermarsh on 2024-09-09 15:20_

---

_Comment by @zanieb on 2024-09-09 15:55_

cc @blueraft 

Introduced in https://github.com/astral-sh/uv/pull/6280

---

_Comment by @blueraft on 2024-09-09 15:58_

Uh oh, sorry. I will take a look!

---

_Comment by @StSav012 on 2024-10-23 10:07_

I've stumbled upon another manifestation of the issue. Despite the `--color=never` option is provided, it's ignored when an error is displayed:

```commandline
> uv --color never                                                                                  
←[1m←[31merror:←[0m '←[33muv←[0m' requires a subcommand but one was not provided                                                
  [subcommands: ←[32mrun←[0m, ←[32minit←[0m, ←[32madd←[0m, ←[32mremove←[0m, ←[32msync←[0m, ←[32mlock←[0m, ←[32mexport←[0m, ←[32m
tree←[0m, ←[32mtool←[0m, ←[32mpython←[0m, ←[32mpip←[0m, ←[32mvenv←[0m, ←[32mvirtualenv←[0m, ←[32mv←[0m, ←[32mbuild←[0m, ←[32mpub
lish←[0m, ←[32mbuild-backend←[0m, ←[32mcache←[0m, ←[32mself←[0m, ←[32mclean←[0m, ←[32mversion←[0m, ←[32mgenerate-shell-completio
n←[0m, ←[32m--generate-shell-completion←[0m, ←[32mhelp←[0m]                                                                     

←[1m←[32mUsage:←[0m ←[1m←[36muv←[0m ←[36m[OPTIONS]←[0m ←[36m<COMMAND>←[0m                                                       

For more information, try '←[1m←[36m--help←[0m'.                                                                                

```

However, `NO_COLOR=1` works fine now, in `uv 0.4.25 (97eb6ab4a 2024-10-21)`.

---

_Comment by @blueraft on 2024-10-23 10:56_

Thanks for catching that @StSav012, I've added a fix for that in the linked PR 

<img width="431" alt="Screenshot 2024-10-23 at 12 56 27" src="https://github.com/user-attachments/assets/49c8b6a7-95cc-461e-b9d2-9b3abe43983f">


---

_Comment by @dandavison on 2025-04-06 21:48_

In case it helps I think this is another manifestation of the bug

<img width="502" alt="Image" src="https://github.com/user-attachments/assets/8db9f746-2447-49d0-98fc-bada9e6fd0e1" />

<img width="602" alt="Image" src="https://github.com/user-attachments/assets/54bc5402-5bd3-4947-8b0e-076842e79531" />

---
