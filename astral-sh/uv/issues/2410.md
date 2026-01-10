```yaml
number: 2410
title: Too long cache filenames
type: issue
state: closed
author: potiuk
labels:
  - bug
  - help wanted
  - cache
assignees: []
created_at: 2024-03-13T13:06:03Z
updated_at: 2025-02-26T22:41:59Z
url: https://github.com/astral-sh/uv/issues/2410
synced_at: 2026-01-10T03:50:29Z
```

# Too long cache filenames

---

_Issue opened by @potiuk on 2024-03-13 13:06_

Sorry - pressed enter too fast.

Some combinations of platform / packages might generate too long cache file names. Tested with `0.1.18` on my mint Linux
(reproducible with `main` airflow`).




```bash
[jarek:~/code/airflow]└2 [apache-airflow] main(+4/-4)+ 8s ± uv pip install --upgrade --editable '.[devel-ci]'
   Built file:///home/jarek/code/airflow                                                                                                                                                                                                                                                                                                                                                      Built 1 editable in 5.20s
error: Failed to download: sqlalchemy==1.4.52
  Caused by: Failed to write to the client cache
  Caused by: Failed to persist temporary file to /home/jarek/.cache/uv/wheels-v0/pypi/sqlalchemy/sqlalchemy-1.4.52-cp38-cp38-manylinux1_x86_64.manylinux2010_x86_64.manylinux_2_12_x86_64.manylinux_2_5_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.msgpack:  File name too long (os error 36)

```

<!--
Thank you for taking the time to report an issue! We're glad to have you involved with uv.

If you're filing a bug report, please consider including the following information:

* A minimal code snippet that reproduces the bug.
* The command you invoked (e.g., `uv pip sync requirements.txt`), ideally including the `--verbose` flag.
* The current uv platform.
* The current uv version (`uv --version`).
-->


---

_Renamed from "Too long cache " to "Too long cache  fk." by @potiuk on 2024-03-13 13:06_

---

_Renamed from "Too long cache  fk." to "Too long cache filenames" by @potiuk on 2024-03-13 13:06_

---

_Label `bug` added by @charliermarsh on 2024-03-13 14:13_

---

_Comment by @charliermarsh on 2024-03-13 14:13_

Oh wow. But that actually _is_ the filename as on PyPI, right?

---

_Label `cache` added by @zanieb on 2024-03-13 14:31_

---

_Comment by @potiuk on 2024-03-14 01:36_

> Oh wow. But that actually _is_ the filename as on PyPI, right?

Certainly: https://pypi.org/project/SQLAlchemy/1.4.52/#files

I think best if you  trim+ add hash if the file name exceeds a certain length. That would be probably best solution (it would be a pity to loose the name from cache and replace it with a meaningless hash - though many similar solutions do it exactly this way.

---

_Comment by @charliermarsh on 2024-03-14 01:37_

Yeah, we just need some kind of deterministic encoding since it's content-addressed. (I.e., when we _have_ that wheel name from PyPI, we need to know where to look in the cache.) Definitely doable!

---

_Label `help wanted` added by @charliermarsh on 2024-03-14 14:10_

---

_Comment by @charliermarsh on 2024-03-14 20:45_

It seems like 255 is the common limit on Linux, but that it can be as low as 144 if you have encryption enabled: https://stackoverflow.com/questions/6114301/git-checkout-index-unable-to-create-file-file-name-too-long/6114588#6114588

---

_Comment by @charliermarsh on 2024-03-14 21:09_

I'm curious what pip does if you try to download that wheel :)

---

_Comment by @potiuk on 2024-03-14 21:19_

Yep. My homedir where I hit it (my linux workstation with Debian Mint) is indeed encrypted. And the reason it is 143 (seems) is explained here https://wiki.archlinux.org/title/ECryptfs#Deficiencies

As of newer version of pip (v23.3+) `pip` stores hashed names in the cache - avoiding the problem altogether:

<img width="779" alt="Screenshot 2024-03-14 at 22 17 27" src="https://github.com/astral-sh/uv/assets/595491/1e030295-263d-4697-9989-2053f0bd55ab">


---

_Comment by @charliermarsh on 2024-03-14 22:00_

Yeah, there's no inherent reason that we need to use full-fidelity filenames here. It's just helpful for debugging.

---

_Comment by @potiuk on 2024-03-14 22:48_

Suggestion: Only use hash if you've hit too long name issue.

---

_Comment by @zanieb on 2024-04-12 19:50_

@charliermarsh have we addressed this with recent changes to the cache?

---

_Comment by @charliermarsh on 2024-04-12 20:57_

No, hasn’t changed yet

---

_Comment by @charliermarsh on 2024-04-12 21:12_

Not trivial right now because we read the tags from the wheel name in the built wheel cache

---

_Comment by @inoa-dmpassy on 2024-04-17 19:09_

This can be a real pain, on my machine the uv default cache dir is:
`C:\Users\DMpassy\AppData\Local\uv\cache\`
already 40 chars
If we take, for instance, DjangoRestFramework, a super popular django lib
`C:\Users\DMpassy\AppData\Local\uv\cache\wheels-v0\index\d92ad663158e5f0f\djangorestframework\djangorestframework-3.10.3-py3-none-any\rest_framework\templates\rest_framework\vertical\checkbox_multiple.html`
206 chars, not that far off from windows 260 upper limit.

I'm currently struggling to use UV on CI with jenkins due to that:
```
C:\Users\jenkins\AppData\Local\uv\cache\.tmp60o0NX\.venv\lib\site-packages\setuptools\command\build_py.py:207: _Warning: Package 'django_saml2_auth.templates.django_saml2_auth' is absent from 
the `packages` configuration.
!!
# bunch of other stuff

error: could not create 'build\bdist.win-amd64\wheel\django_saml2_auth\templates\django_saml2_auth': The filename or extension is too long
```

I'm not sure how it got that long, I imagine it has to do with uv caching + setuptools building.

---

_Comment by @egerlach on 2024-06-05 16:25_

For windows, there might be another solution, I just came across this https://github.com/BurntSushi/ripgrep/blob/master/pkg/windows/README.md, which talks about using a manifest to declare that the application works with paths longer than 260 characters (I suppose the OS libraries change their behaviour then).

---

_Comment by @BurntSushi on 2024-06-05 16:56_

> For windows, there might be another solution, I just came across this https://github.com/BurntSushi/ripgrep/blob/master/pkg/windows/README.md, which talks about using a manifest to declare that the application works with paths longer than 260 characters (I suppose the OS libraries change their behaviour then).

Yeah. Note though that, AIUI, the manifest is only 1 of 2 things that are required. The user also needs to apply a registry edit: https://learn.microsoft.com/en-us/windows/win32/fileio/maximum-file-path-limitation?tabs=registry#enable-long-paths-in-windows-10-version-1607-and-later

---

_Comment by @adaamz on 2024-08-20 20:41_

@potiuk Did you manage to use sqlalchemy with uv? I just tried `uv` with our codebase and it failed on the same package :-/

edit: caption obvious: `uv pip compile ... --no-cache` works around this issue

---

_Comment by @potiuk on 2024-08-21 08:59_

Hello @charliermarsh @zanieb  - would it be possible to prioritise that one a bit now? 

To add a bit of context - we are now discussing Airlfow 3 approach of managing our complex dependencies and providers, and one of the options considered is to use `uv workspace` feature for that (as discussed before - I looked at it recently and I **think** it has all we need now, and I will make a POC to show to other PMC members and development community how uv workspaces might be useful to simplify and standardise our setup for providers.

Now - this one makes my life really hard ... Either I have to remove cache or move the cache somewhere aside of my home directory to another filesystem (but then hard-links don't work) etc. etc.  Also for me - this is quite a blocker to propose it - with our number of contributors (we **just** passed 3000!) - surely there will be some like me who have they home dir encrypted - especially that it's an option in multiple distros that you can select (For me - that came out-of-the box with my Linux mint installation and as a security freak I obviously enabled it).

Maybe a good solution will be to check if the filesystem is encrypted or simply react to "too long filename" and use a hash instead - there is no need to change it in bulk - simply handling the exception when it occurs should be good-enough and have very little effect on debuggability.

Looking forward to having this one solved (I'd do it myself - but I have far too many things on my plate  now to learn rust and all the internals of `uv`.




---

_Comment by @charliermarsh on 2024-08-21 16:55_

Yeah we def need to fix this.

---

_Comment by @inoa-jboliveira on 2024-09-30 20:10_

Hi everyone, the issue with long cache names also displays some cryptic error messages such as `Caused by: stream did not contain valid UTF-8`, but it has no relation to UTF-8 or string encoding.

```uv sync --all-extras
Resolved 203 packages in 2ms
error: Failed to prepare distributions
  Caused by: Failed to fetch wheel: django-async-messages==0.3.1
  Caused by: Failed to run C:\Users\JoaoBernardo\Documents\Projects\myproject-main\.uv_cache\builds-v0\.tmp8EzDKf\Scripts\python.exe
  Caused by: stream did not contain valid UTF-8
```

**If you shorten the the cache dir to e.g. `c:\cache\` it will work fine.**

There is sometimes a second very long error message such as `Build backend failed to build wheel through build_wheel() with exit code: 1` and a large stderr after.

These messages are all irrelevant IMO as they just show where in time UV crashed due to a long file name, but the solution will to shorten the path somehow.

---

_Comment by @potiuk on 2024-10-26 21:19_

Just a small ping on that one.. We already started to move to uv for workspaces, and I got my Linux PC with encryption back from cooling system replacement. Would be nice to start using it with `uv` workspaces, since my homedir is encrypted :)

---

_Comment by @potiuk on 2024-11-18 00:20_

FYI. We've switched Airflow (and I also did it) to use `uv` for recommended workflows and while I have been using uv heavily for syncing and installing things on the regular, encrypted volume I had, I stopped seeing the issue in recent uv version and airflow development environments. 

I have also not heard anyone else having similar issue.

Not sure what caused it though. W have not changed our requirements for sqlalchemy, but maybe either the algorithm that decides which packages to install or other "generic" error handling in `uv` made the error disappear. 

From my perspective - this issue can be closed, I think I'd have seen it again with the heavy usage of `uv` for a number of cases and switching to older versions of airflow for backporting if it was still a problem. It used to occur **every time** when I reported it.

---

_Comment by @inoa-jboliveira on 2024-11-18 03:29_

There were some recent but minor improvements on some file names (according to the changelog on 0.5.x line), but for us, on Windows we still get inexplicable issues that goes away by changing the cache path via pyproject.toml that cause other issues still.

And the issue with Windows docker containers that get an even smaller limit for file names. We were able to partially overcome that on our specific user case, but the whole thing is too buggy for my taste.

Please keep the issue open. You may unsubscribe from it if you like

---

_Comment by @charliermarsh on 2024-11-18 03:49_

Thanks @potiuk. I'll leave it open but I don't plan to prioritize any work here until we have clear reproductions and evidence of significant need.

---

_Comment by @potiuk on 2024-11-18 23:23_

> Please keep the issue open. You may unsubscribe from it if you like

I can be still subscribed :) no worries. I have no plans from my side to close it - not my decision, I think it's far more important to see if there is - like @charliermarsh mentioned - more users affected and clear reproduction/evidence that it disrupts more users. 

From Airflow at least (and our 700+deps + the `uv workflow` that we implemented) - we see the problem is generally gone. I just know - being an OSS maintainer myself - how cool it is to see a "fresh" status on some old "low-priority" issues and feedback from those who raised them.

BTW. I'd personally agree with @charliermarsh  - and even more - qualify that issue as a "fix it if you need" kind of thing. Since this seems like a very low/rare impact issue - I'd say to anyone who wants to get it fixed - "Yeah we know it, and we know how loe impact it has. If you really want to have it solved, feel free to provide a complete PR that we can review and merge, that will not disrupt the `9x%` of cases. We are ok with loosing the 100-9x%". 

But that's just me - with side comment and assessment. 

It's just the question of what else (more important) will not be done if that one is prioritized (i.e. "missed opportunities").

---

_Comment by @tm-adaamz on 2025-02-12 14:17_

@charliermarsh What else we need for reproduction?
I have
- Linux Mint 22.1 (based on Ubuntu 24.04 (based on debian trixie (based on linux 6.8.0-52-generic)))
- encrypted home
- `uv cache dir` points to `$HOME/.cache/uv`
- uv version `0.5.30`
- sqlalchemy >= `1.4.49` (1.4.48 is fine)

```
error: Failed to write to the client cache
  Caused by: failed to rename file from /home/adam/.cache/uv/wheels-v3/index/15ce51a1bc994a93/sqlalchemy/.tmpMXXBGn to /home/adam/.cache/uv/wheels-v3/index/15ce51a1bc994a93/sqlalchemy/sqlalchemy-1.4.49-cp39-cp39-manylinux_2_5_x86_64.manylinux1_x86_64.manylinux_2_12_x86_64.manylinux2010_x86_64.manylinux_2_17_x86_64.manylinux2014_x86_64.msgpack: File name too long (os error 36)
```

Do you have any idea what else I can investigate to help there?



---

_Comment by @charliermarsh on 2025-02-12 15:34_

I don't think we need anything else for a reproduction. We just need to find time to prioritize this.

---

_Comment by @nkitsaini on 2025-02-20 14:48_

I don't have much idea of changes required right now, but I am planning to start working on the implementation. 

I am planning to use msgpack file for storing tags, maybe the full filename itself. 

I am currently going with `{trimmed_filename}-{hash}` for filename, this should be an easy change if we decide to go ahead with pure hash or something else.

~I have not yet decided on the migration strategy yet. I will see how big is the implementation difference between incremental vs once for all.~ Found out that uv already has a cache versioning system, so I will use that itself. No migration (yay!).


---

_Closed by @charliermarsh on 2025-02-26 22:41_

---
