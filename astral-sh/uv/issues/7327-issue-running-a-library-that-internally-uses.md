```yaml
number: 7327
title: "Issue running a library that internally uses `python -m pip ...`"
type: issue
state: open
author: EdgyEdgemond
labels:
  - bug
assignees: []
created_at: 2024-09-12T09:22:26Z
updated_at: 2025-10-01T21:17:28Z
url: https://github.com/astral-sh/uv/issues/7327
synced_at: 2026-01-12T15:59:12Z
```

# Issue running a library that internally uses `python -m pip ...`

---

_@EdgyEdgemond_

I understand the issue behind this, uv is replacing pip so there is no pip in the virtualenv, I am wondering if there is a way to ensure there is a pip in the venv using uv correctly.

MRE:

pyproject.toml
```
[project]
name = "uv-test"
version = "0.0.0"
requires-python = ">=3.10,<3.11"

dependencies = [
    "numpy == 1.26.3",
    "great_expectations == 0.18.1",
    "spacy[transformers] == 3.6.1",
]
```

lock/sync and then run:
```
>> uv run spacy download en_core_web_md-3.6.0 --direct
/home/edgy/code/nrwl/scripts/uv-test/.venv/bin/python: No module named pip
```

Under the hood `spacy download` is firing off `python -m pip *args` which understandably blows up. In our docker images and pipelines I can `uv pip compile` to get a requirements.txt and use the system pip to install things and spacy is happy. 

But locally it picks up the `.venv` and triggers the error above.

---

_Comment by @kbernhagen on 2024-09-12 22:31_

Maybe use
`uv add --dev pip`

---

_Comment by @zanieb on 2024-09-13 02:15_

Or `uv run --with pip spacy download  ...` to add it temporarily.

---

_Label `question` added by @zanieb on 2024-09-13 02:15_

---

_Comment by @zanieb on 2024-09-13 02:16_

You could also do something more controversial and alias `pip` to `uv pip` which would work fine if they're not depending on less common `pip` behaviors.

---

_Comment by @EdgyEdgemond on 2024-09-13 05:11_

Was coming to the realisation pip isn't a special python tool, it's just a module I can use as a dependency. Thanks for the confirmation on the solution I should have realised yesterday. I'll confirm it works shortly.

---

_Comment by @EdgyEdgemond on 2024-09-13 08:43_

Adding pip as a specified dependency did work.

Surprisingly `--with pip` didn't work, it did install pip before running.

```
>> uv run --with pip spacy download en_core_web_md-3.6.0 --direct 
Installed 1 package in 8ms
/uv-test/.venv/bin/python: No module named pip
```

---

_Comment by @EdgyEdgemond on 2024-09-13 09:36_

Feel free to close this if `--with pip` is somehow expected behaviour, as I have a solution that works for me.

---

_Comment by @charliermarsh on 2024-09-14 00:22_

What did you end up doing to solve it? `uv tool run --with pip spacy download en_core_web_md-3.6.0 --direct` works for me, for what it's worth.

---

_Comment by @charliermarsh on 2024-09-14 00:26_

I think the `uv run` version fails due to how our environment layering works... They use `[sys.executable, "-m", "pip", "install"]`, and I guess `sys.executable` refers to the base interpreter despite the fact that we layer the `--with` requirements on top as a separate environment.

---

_Comment by @EdgyEdgemond on 2024-09-14 06:59_

I did not try uv tool run, i ended up adding pip to the projects requirements, as spacy does indeed require it :)

Ill keep `uv tool run` in mind next time i encounter an odd situation. 

At this point we have successfully migrated all projects at work to `uv`, so happy with the current state of things. Thanks for all your work on this tool. Looking forward to if/when build tooling is in house.

---

_Label `bug` added by @charliermarsh on 2024-09-14 12:33_

---

_Label `question` removed by @charliermarsh on 2024-09-14 12:33_

---

_Comment by @charliermarsh on 2024-09-14 12:35_

Awesome, thank you @EdgyEdgemond. (We do have `uv build` but maybe you're referring to something different :))

---

_Comment by @charliermarsh on 2024-09-14 12:35_

I think I'd consider this a bug, though it's hard to say, arguably SpaCy should be importing / calling pip some other way than via `subprocess`... We can only fix this by redesigning the overlay system.

---

_Comment by @EdgyEdgemond on 2024-09-14 12:59_

I think this is an edge case of a library performing under assumptions, that can be handled with existing functionality. It should have pip as a dependency in an ideal world etc. It would be nice to work around using `--with` but not a deal breaker.

@charliermarsh I was thinking of publish architecture not build, twine etc :)

---

_Comment by @urvisism on 2024-09-24 08:20_

@EdgyEdgemond @charliermarsh 
I installed a spacy model using the given command and it worked.
```
uv tool run --with pip spacy download de_core_news_lg-3.7.0 --direct
```

But, when I tried to use it in the given code. It gives me an error.
Code:
```
import spacy
nlp = spacy.load("de_core_news_lg")
```
Error:
```
OSError: [E050] Can't find model 'de_core_news_lg'. It doesn't seem to be a Python package or a valid path to a data directory.
```

Need some help.

---

_Comment by @EdgyEdgemond on 2024-09-24 11:01_

I solved this by adding pip as a requirement of my project (as it is a requirement of spacy), and ran spacy in the venv directly.

Im not sure if uv tool puts pip somewhere else and as a result spacy download ends up in a different location. That would be my guess without understanding the internals like @charliermarsh 

---

_Comment by @urvisism on 2024-09-27 07:20_

@EdgyEdgemond 
Could you please guide me how to add pip as a requirement in project ?

---

_Comment by @EdgyEdgemond on 2024-09-27 15:17_

Simple as adding pip to your dependencies. `pip` is just another python package.

```
dependencies = [
    ...,
    "pip"
]
```

---

_Comment by @realyukii on 2025-02-01 06:56_

I tried @kbernhagen's solution:
```
uv add --dev pip
```

but it throw an error:
```
error: No `project` table found in: `C:\<path-to>\stable-diffusion-webui\pyproject.toml`
```

is there a way to trick python that pip module is indeed exists? I can't figure out how to do that

for context: I'm trying to install [stable diffusion webui](https://github.com/AUTOMATIC1111/stable-diffusion-webui) using python provided by uv and it demand pip module to be installed in the python:
```
C:\<path-to>stable-diffusion-webui\venv\Scripts\python.exe: No module named pip
```

---

_Comment by @EdgyEdgemond on 2025-02-01 07:32_

If you are using this outside a dev environment you need pip as a main dependency. Ultimately pip needs to be added as a dependency in the same uv group as the thing requiring it. The same way you would solve any other missing module/package issue.

---

_Comment by @realyukii on 2025-02-01 07:38_

Thanks for the reply, this comment: https://github.com/astral-sh/uv/issues/1551#issuecomment-2016618403 helped me solve the problem ^^

---

_Comment by @thewh1teagle on 2025-02-05 00:22_

This worked for me:

```console
uv run --no-project python main.py
```

---

_Comment by @ivan-kleshnin on 2025-03-06 06:55_

In my case, surprisingly, `uv run --with=pip spacy download` fails with the same message:

```
(project_venv) ~/Projects/project $ uv run --with=pip spacy download en_core_web_md
/Users/username/Projects/project/.venv/bin/python3: No module named pip
```
but
```
uv add pip
spacy download en_core_web_md
✔ Download and installation successful
```

does work normally 🤔 

---

_Comment by @EdgyEdgemond on 2025-03-29 00:03_

@charliermarsh i feel we can agree this is not a `uv` issue but a spacy issue. Feel free to close this. Or move it to a discussion, if you want to leave it open for people to chat about it.

---

_Comment by @bowenliang123 on 2025-04-02 13:31_

I've got similar problem when use `mypy` uv `uv`.
Both 
`uv run pip mypy --install-types --non-interactive .`
and
`uv run --with pip mypy --install-types --non-interactive .`
failed the tests.

Getting these erro logs.
```
Downloading pip (1.8MiB)
 Downloaded pip
Installed 1 package in 10ms
/home/runner/work/dify/dify/api/.venv/bin/python: No module named pip
core/ops/ops_trace_manager.py:11: error: Library stubs not installed for "cachetools"  [import-untyped]
core/ops/ops_trace_manager.py:11: note: Hint: "python3 -m pip install types-cachetools"
core/ops/ops_trace_manager.py:11: note: (or run "mypy --install-types" to install all missing stub packages)
core/ops/ops_trace_manager.py:11: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
Installing missing stub packages:
/home/runner/work/dify/dify/api/.venv/bin/python -m pip install types-Markdown types-PyMySQL types-Pygments types-aiofiles types-cachetools types-colorama types-oauthlib types-protobuf types-pywin32 types-simplejson types-tzlocal types-ujson


Found 1 error in 1 file (checked 1040 source files)
```

---

_Comment by @zanieb on 2025-04-02 13:37_

@bowenliang123 I'd recommend just adding that missing library stub as a development dependency? `uv add --dev types-cachetools`

---

_Comment by @zanieb on 2025-04-02 13:38_

As Charlie noted, we overlay environments so `--with pip` does not make it available via a subprocess that invokes `python -m pip`.

---

_Comment by @bowenliang123 on 2025-04-02 13:51_

> [@bowenliang123](https://github.com/bowenliang123) I'd recommend just adding that missing library stub as a development dependency? `uv add --dev types-cachetools`

But it still requires `pip` to install all the runtime determined required pacakges `types-Markdown types-PyMySQL types-Pygments types-aiofiles types-cachetools types-colorama types-oauthlib types-protobuf types-pywin32 types-simplejson types-tzlocal types-ujson`.

---

_Comment by @zanieb on 2025-04-02 13:58_

@bowenliang123 you should just specify those as part of your dependencies instead of relying on mypy's hacky auto-install behavior.

---

_Comment by @bowenliang123 on 2025-04-02 14:01_

> [@bowenliang123](https://github.com/bowenliang123) you should just specify those as part of your dependencies instead of relying on mypy's hacky auto-install behavior.

-1, disagree with this suggestion. It's a helpful feature for a collaberated open sourced project, which in my case, the Dify project (https://github.com/langgenius/dify/pull/16317).  It does block the Dify switching from Poetry to UV.

---

_Comment by @zanieb on 2025-04-02 14:13_

Sorry you don't think it's a good suggestion. As mentioned above, you can also add `pip` as a development dependency if you want the mypy installer to work. It's better practice to have the type stubs locked alongside the rest of your dependencies, but do what you want of course.

---

_Comment by @bowenliang123 on 2025-04-02 14:21_

My consideration is that the stub type dependencies are never properly maintained. As many dependencies in and out, there's no way to get rid of the delcared outdated stub type dependencies. As for whether they are pinned or not, it's not contribute much to it.

---

_Comment by @bowenliang123 on 2025-04-06 01:50_

> I've got similar problem when use `mypy` uv `uv`. Both `uv run pip mypy --install-types --non-interactive .` and `uv run --with pip mypy --install-types --non-interactive .` failed the tests.
> 
> Getting these erro logs.
> 
> ```
> Downloading pip (1.8MiB)
>  Downloaded pip
> Installed 1 package in 10ms
> /home/runner/work/dify/dify/api/.venv/bin/python: No module named pip
> core/ops/ops_trace_manager.py:11: error: Library stubs not installed for "cachetools"  [import-untyped]
> core/ops/ops_trace_manager.py:11: note: Hint: "python3 -m pip install types-cachetools"
> core/ops/ops_trace_manager.py:11: note: (or run "mypy --install-types" to install all missing stub packages)
> core/ops/ops_trace_manager.py:11: note: See https://mypy.readthedocs.io/en/stable/running_mypy.html#missing-imports
> Installing missing stub packages:
> /home/runner/work/dify/dify/api/.venv/bin/python -m pip install types-Markdown types-PyMySQL types-Pygments types-aiofiles types-cachetools types-colorama types-oauthlib types-protobuf types-pywin32 types-simplejson types-tzlocal types-ujson
> 
> 
> Found 1 error in 1 file (checked 1040 source files)
> ```

My workaround for this is that change direct use of `mypy` to `python -m mypy` with `uv run --with pip` and it works.

Change from 
```
uv run pip mypy --install-types --non-interactive .
```
to
```
uv run --with pip python -m mypy --install-types --non-interactive .
```



---

_Comment by @zk1989 on 2025-09-30 15:02_

I was just struggling with this and found the thread. TBH, I'm not a huge fan of the fact that I have to install pip just to download the spaCy model. As far as I can tell, there's currently no way around it…
Isn't the end goal of uv to eventually eliminate the need for a separate pip installation altogether?
@EdgyEdgemond – you mentioned that spaCy requires pip, which makes sense for now, but I'd still argue that uv is aiming to handle all cases that pip does—and ideally go beyond that 🙂

---

_Comment by @zanieb on 2025-09-30 15:06_

Please take it up with spaCy. There's not much we can do about it.

---

_Comment by @zk1989 on 2025-09-30 15:14_

I've also noticed that spacy model will get removed at the first instantiation of uv remove command, even when the removed package is entirely unrelated, i.e.:

$ uv remove dotenv
Resolved 60 packages in 104ms
Uninstalled 3 packages in 27ms
 - dotenv==0.9.9
 - en-core-web-sm==3.8.0 (from https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.8.0/en_core_web_sm-3.8.0-py3-none-any.whl)
 - python-dotenv==1.1.1

Is this also related to the specifics of how spacy designed this and at the same time WAD from uv perspective?

---

_Comment by @EdgyEdgemond on 2025-09-30 15:58_

@zk1989 the issue is that `pip` is fundamentally just another python module, but spaCy makes the assumption that all python environments will have it already installed, which isn't a safe assumption.

uv replaces your usage of pip, but other libraries can still depend on it to install things if thats how they want to behave. But they should include pip as a dependency in that case. The work around here is to add it as a dependency yourself while awaiting spaCy to fix the root issue.

---

_Comment by @EdgyEdgemond on 2025-09-30 15:59_

In regards to it being removed is it one of your dependencies? i.e. in pyproject.toml, theres no reason pip missing from dependencies would cause that

---

_Comment by @zk1989 on 2025-09-30 18:35_

It seems to have to do with how --with flag works... As per the docs I'm only installing that package to a "a separate, _ephemeral_ environment" (https://docs.astral.sh/uv/reference/cli/#uv-run--with).

My dependencies:

dependencies = [
    "dotenv>=0.9.9",
    "google-genai>=1.39.0",
    "pandas>=2.3.3",
    "pip>=25.2",
    "plotly[express]>=6.3.0",
    "spacy>=3.8.7",
]

I'm successfully installing spacy model, cause pip is there:
                                                                                                               
$ uv run --with spacy spacy download en_core_web_sm
Collecting en-core-web-sm==3.8.0
  Downloading https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.8.0/en_core_web_sm-3.8.0-py3-none-any.whl (12.8 MB)
     ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━ 12.8/12.8 MB 12.3 MB/s  0:00:01                                                                                                       
✔ Download and installation successful
You can now load the package via spacy.load('en_core_web_sm')

But if I want to later execute uv remove for whatever reason (and removing not necessarily pip, just anything), it will also remove that language package:

$ uv remove pandas
Resolved 62 packages in 1.12s
Uninstalled 6 packages in 1.59s
 - en-core-web-sm==3.8.0 (from https://github.com/explosion/spacy-models/releases/download/en_core_web_sm-3.8.0/en_core_web_sm-3.8.0-py3-none-any.whl)
 - pandas==2.3.3
 - python-dateutil==2.9.0.post0
 - pytz==2025.2
 - six==1.17.0
 - tzdata==2025.2

---

_Comment by @EdgyEdgemond on 2025-10-01 06:24_

That makes sense (i think), spacy is using pip to install that module. pip is installing it in your venv. uv is detecting an untracked module when removing (will likely happen on add or sync as well) and cleaning it up. 

Same will happen if you `uv pip install xxx`to add a package temporarily. 

Why are you using --with? spacy is in your venv, it's one of your dependencies.

---

_Comment by @zk1989 on 2025-10-01 18:41_

Yes, both spacy and pip are in my venv now, and both uv run _--with spacy_ spacy download en_core_web_sm and uv run _--with pip_ spacy download en_core_web_sm work. Unfortunately I'm not sure what shorter command (or one without --with) I can use to do the same. These are the ones I found in the threads relating to this issue, and so I'm using those... If you can share the better way to do that I'd be grateful :)

---

_Comment by @EdgyEdgemond on 2025-10-01 21:17_

`uv run spacy ...`,  or just `spacy ...` with the venv active. Any temporal dependency (like things installed by spacy) will get cleaned up by uv when it resyncs. 

Whatever commands you run that require the additional module, wrap them together in a makefile, invoke or poe etc. so it ensures the extra modules exist before running 

---
