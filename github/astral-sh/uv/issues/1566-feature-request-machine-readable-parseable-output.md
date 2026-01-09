---
number: 1566
title: "Feature request: machine-readable/parseable output"
type: issue
state: open
author: joeyballentine
labels:
  - enhancement
assignees: []
created_at: 2024-02-17T03:08:58Z
updated_at: 2024-02-17T19:14:13Z
url: https://github.com/astral-sh/uv/issues/1566
synced_at: 2026-01-07T13:12:16-06:00
---

# Feature request: machine-readable/parseable output

---

_Issue opened by @joeyballentine on 2024-02-17 03:08_

Hi! This seems like a really cool project that could end up being a really nice replacement for pip :)

One thing I'd personally really like to see in this is either to have the ability to have full parseable output when running in a subprocess. Let me explain:

I have a project called chaiNNer which offers a bit of python environment management built-in, namely installing dependencies automatically. Part of this functionality is reporting to the user the progress of the installation in a nice UI form. However, since pip hides all progress bars when inside a subprocess (thanks to rich, its progress bar library), I first had to implement a jank way of parsing the found wheel (with regex), canceling the install, downloading the wheel manually from pypi, and then installing it from the wheel file -- then I switched part of this implementation into just outputting json to stdout in a fork of pip I now bundle with the program. 

Neither of these were ideal solutions, and while the pip team was somewhat open to merging my PR of json download output, due to the difficult nature of writing tests for that and realizing it wasn't worth polluting pip with my one-off feature that didn't match anything else, I decided to scrap that idea and just use my fork myself.

I think it would be really great if you could design uv with machine-readable information in mind from the early stages.

Specifically, I would essentially like to have a --json mode, where all outputs to the console are written as json strings so that they can be easily parsed to data objects in whichever language is running uv in a subprocess. Important information such as what package is currently being downloaded, the total size, the current download percentage (or size), would be the most useful bits of information to have.

Is this something the uv team would be willing to implement at some point? I did notice that at its current stage, it doesn't appear to even report download progress to the console, so maybe this is something that would have to come way down the line.

Thank you for your time.


---

_Renamed from "Machine-readable output" to "Feature request: machine-readable/parseable output" by @joeyballentine on 2024-02-17 03:20_

---

_Comment by @zanieb on 2024-02-17 04:05_

Thanks for your feedback and excitement about the project!

Always showing download progress is slow :) but we do want to do it sometimes e.g. #1209 

We support an `--output-format json` in Ruff, I imagine we'll consider supporting something similar here but it may take some significant work.

---

_Label `enhancement` added by @zanieb on 2024-02-17 04:05_

---

_Comment by @joeyballentine on 2024-02-17 04:14_

Haha funnily enough pytorch is the sole reason I want to display download progress at all, so I definitely agree with that linked issue 😄 

Glad to hear you're open to it. I look forward to seeing the progress on this project :)

---

_Comment by @pradyunsg on 2024-02-17 10:53_

(this is very much intended to be a drive-by comment from me)

FWIW, pip's got a `--report` flag that provides installation-related reports: https://pip.pypa.io/en/stable/reference/installation-report/ -- you might want to consider building off of that. :)

---

_Comment by @joeyballentine on 2024-02-17 19:14_

@pradyunsg that does look interesting, but after a little investigation it looks like the report gets output _after_ it's done doing what it usually does. So while it does output the wheel url in a nicely parseable format, it does so after it would even matter, which is unfortunate. and it doesn't change the way download progress is logged.

---

_Referenced in [moreati/ansible-uv#2](../../moreati/ansible-uv/issues/2.md) on 2024-11-29 22:09_

---
