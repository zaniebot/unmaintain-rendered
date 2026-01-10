```yaml
number: 11296
title: Hitting ulimit with huge workspace
type: issue
state: open
author: potiuk
labels:
  - bug
assignees: []
created_at: 2025-02-06T20:44:39Z
updated_at: 2025-05-20T12:53:22Z
url: https://github.com/astral-sh/uv/issues/11296
synced_at: 2026-01-10T03:41:47Z
```

# Hitting ulimit with huge workspace

---

_Issue opened by @potiuk on 2025-02-06 20:44_

### Summary

When running uv sync with huge workspace (90+ subprojects) uv fails with "too many open files" when the default ulimit for number of opened file descriptors per process is used (256 is the default on MacOS).

<img width="892" alt="Image" src="https://github.com/user-attachments/assets/16927ece-0665-47b5-b008-bfa339c01123" />

Setting `ulimit -n 2048` fixes the problem, however it would be great if uv respects the limit that is set per process and does not open more files than it can.

How to reproduce:

1) Checkout https://github.com/apache/airflow
2) Run `uv sync`



### Platform

MacOS

### Version

uv 0.5.25 (9c07c3fc5 2025-01-28)

### Python version

Python 3.11.10

---

_Label `bug` added by @potiuk on 2025-02-06 20:44_

---

_Comment by @zanieb on 2025-02-06 20:47_

macOS has a really small default ulimit, I'm hesitant to significantly change our behavior to account for that — but if it's feasible I agree it makes sense as a better experience.

---

_Comment by @potiuk on 2025-02-06 20:47_

BTW. `workspace` feature works great for us. We are about to complete the migration of all 90+ providers to separate sub-projects in our workspace. 

---

_Comment by @potiuk on 2025-02-06 20:50_

I am updating our docs to include it, byt yeah it would be great to account for that. Another option could be to catch this error and ask the user to run `ulimit -n 2048` or smth.

---

_Comment by @zanieb on 2025-02-06 20:54_

I think we'd need to use something like https://docs.rs/sysctl/latest/sysctl/ to retrieve this programatically.

---

_Comment by @potiuk on 2025-02-06 21:01_

Yep. Sounds about right :)

---

_Comment by @zanieb on 2025-02-06 21:03_

cc @konstin perhaps this is covered by some existing concurrency limit?

---

_Comment by @konstin on 2025-02-06 21:10_

We currently don't have a limit for this, especially if things like launching a subprocess count as an open file. If it was only `File` files, we could let's say patch `fs_err`, but like this it requires more auditing. It would be interesting to see the list of open files at the point of the error. Maybe there is a specific function that is currently holding to many files open that we could trim down without threading through real limit everywhere?

---

_Comment by @zanieb on 2025-02-06 21:11_

I am wondering if most of the open files are `python` build subprocesses? I agree, it would be very nice to see the open files to make an informed decision here.

---

_Comment by @charliermarsh on 2025-02-06 21:12_

Isn't workspace discovery also sort of exponential? Like for each member, we have to discover all members? :)

(I might be wrong, I can't remember, there are some TODOs around caching discovery.)

---

_Comment by @potiuk on 2025-02-06 21:15_

> Isn't workspace discovery also sort of exponential? Like for each member, we have to discover all members? :)

Hopefully not.. We are not fully done yet :P -> but with Airflow you can likely test the limits, it's likely one of the largest workspaces you can get :)


---

_Comment by @konstin on 2025-02-06 21:38_

Airflow is already my favorite big benchmarking case, lovely to see such large scale adoption of workspaces, too.

I just tried `uv sync` on airflow ea07814a72a46f7aa34553b65ab5cb929d3b27a5 on linux with uv 0.5.29, I got an error though:

```
$ uv sync
error: Failed to parse `uv.lock`
  Caused by: TOML parse error at line 9, column 1
  |
9 | [[distribution]]
  | ^^^^^^^^^^^^^^^^
missing field `source`
```

---

_Comment by @potiuk on 2025-02-06 21:41_

I think it's something in your lock :) . We currently do not commit `uv.lock` :)

---

_Comment by @konstin on 2025-02-06 21:47_

oops, thanks!

---

_Comment by @potiuk on 2025-03-06 17:57_

We would really love to have it fixed  soon :). From our slack:


![Image](https://github.com/user-attachments/assets/3b72c4ec-d7cf-4616-b8e5-1ac6c9e6c1d2)

---

_Comment by @potiuk on 2025-03-06 17:58_

That really breakes the nice "first time experience" for `uv` for Airflow :( 

---

_Comment by @zanieb on 2025-03-06 18:07_

At the very least, can we catch this error and hint how to fix it?

---

_Comment by @potiuk on 2025-03-06 18:40_

> At the very least, can we catch this error and hint how to fix it?

That would work too

---

_Comment by @zanieb on 2025-03-06 18:43_

Seems vaguely hard because this can presumably happen on most file system operations?

---

_Comment by @potiuk on 2025-03-06 18:53_

I think (at least in our case)  really the problem is when workspace is used for multiple (local) packages - this is where we really experience it. I am not sure internally why it is triggered only for this case - maybe other operations are using multiple processes for building - but in this case it seems that single process opens a lot of files at the same time


---

_Renamed from "Hiiting ulimit with huge workspace" to "Hiting ulimit with huge workspace" by @potiuk on 2025-03-06 19:38_

---

_Renamed from "Hiting ulimit with huge workspace" to "Hitting ulimit with huge workspace" by @potiuk on 2025-03-06 19:38_

---

_Comment by @omkar-foss on 2025-03-06 20:03_

> I think we'd need to use something like https://docs.rs/sysctl/latest/sysctl/ to retrieve this programatically.

uv seems to be using nix already (to get [process group id here](https://github.com/astral-sh/uv/blob/main/crates/uv/src/commands/run.rs#L42-L43)), perhaps using nix's [getrlimit](https://docs.rs/nix/latest/nix/sys/resource/fn.getrlimit.html)/[setrlimit](https://docs.rs/nix/latest/nix/sys/resource/fn.setrlimit.html) could also be explored?

> At the very least, can we catch this error and hint how to fix it?

Yes a hint would be nice, and I guess could be shown to user even without uv erroring out - if on start uv can determine a "large" workspace by counting number of members (or something similar) and compare it to the soft limit from getrlimit, then show a hint or warning accordingly.

Just thinking out loud above, hope this helps in some way :)

---

_Comment by @konstin on 2025-03-07 13:06_

FWIW I just tried `rm -rf .venv && ulimit -n 256 && uv sync` on linux and it didn't trigger the error, it seems this is mac-specific problem and needs to be debugged on a mac.

---

_Comment by @potiuk on 2025-03-07 16:42_

I think `ulimit` with `&&`  might not work - your `uv sync` shell interpreter is different than the one that had ulimit changed.

I can esily repro it on my linux if I do it as separate step

```
[jarek:~/code/airflow] patch-1+ 6s ± rm -rf .venv 
[jarek:~/code/airflow] patch-1+ 4s ± git checkout main 
Switched to branch 'main'
Your branch is up to date with 'apache/main'.
θ61° [jarek:~/code/airflow] main+ ± ulimit -n 256
θ61° [jarek:~/code/airflow] main+ ± uv sync
Using CPython 3.12.7
Creating virtual environment at: .venv
Resolved 791 packages in 16.73s
  × Failed to build `apache-airflow-providers-github @ file:///home/jarek/code/airflow/providers/github`
  ├─▶ Failed to run `/home/jarek/.cache/uv/builds-v0/.tmpOb7Tky/bin/python`
  ╰─▶ Too many open files (os error 24)
  help: `apache-airflow-providers-github` was included because `apache-airflow:dev` depends on `apache-airflow-providers-github`
```

---

_Comment by @konstin on 2025-03-07 18:18_

Odd, tried those exact instructions and couldn't reproduce, some `taskset` attempts for less cores also didn't help (though maybe someone else on the team can reproduce).

---

_Comment by @potiuk on 2025-03-08 17:10_

> Odd, tried those exact instructions and couldn't reproduce, some `taskset` attempts for less cores also didn't help (though maybe someone else on the team can reproduce).

It could be because architecture/OS - i use Mint 23.1, and it's likely that more packages need building on my machine than on yours. Maybe decrease the limits to 50 or so ..

---

_Comment by @sananand007 on 2025-05-20 12:46_

We are Also migrating a large `python` internal package mono repo to `uv` and we hit this issue. It would be great if we can `catch` and show what to do for this error to the user, for better `user experience` 

Everything is on `macOS` 

---

_Comment by @sananand007 on 2025-05-20 12:50_

> BTW. `workspace` feature works great for us. We are about to complete the migration of all 90+ providers to separate sub-projects in our workspace.

Hi @potiuk We are also doing the same  for our `mono` repo, which houses a large number of Internal Python packages (we do not release but change and test). Would you mind sharing your `steps` and the structure of the `main` `pyproject.toml` file on how you structure for your repository. 

- Also I see you say you do not `commit` the `uv.lock` , how does that work ? It is just like `poetry.lock` or `npm_lock.json` and they are all committed to the repository ?

thanks

---
