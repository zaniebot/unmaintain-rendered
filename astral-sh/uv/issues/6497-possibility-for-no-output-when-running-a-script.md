```yaml
number: 6497
title: "Possibility for no output when running a script with `uv run main.py` (that has filled requirements)"
type: issue
state: closed
author: justcallmelarry
labels:
  - documentation
  - question
assignees: []
created_at: 2024-08-23T07:48:55Z
updated_at: 2024-11-06T08:58:05Z
url: https://github.com/astral-sh/uv/issues/6497
synced_at: 2026-01-12T15:59:04Z
```

# Possibility for no output when running a script with `uv run main.py` (that has filled requirements)

---

_@justcallmelarry_

Perhaps this is possible already, but I tried both looking in the documentation and in the code but was not able to figure out a  way, if it is already possible, sorry for the extra work…

In the section of the docs regarding [script dependencies](https://docs.astral.sh/uv/guides/scripts/#declaring-script-dependencies) (which is a GREAT feature!), it shows example output which does not have the "Reading inline script metadata from: example.py" line at the top.

I can't seem to replicate the behaviour though, and it looked to my inexperienced Rust eyes that it is in fact always printed to stderr if it is a script being run.

Would love to have a config setting possibility to not print this line, and only the output (if any) from the script.

---

_Comment by @zanieb on 2024-08-23 13:23_

Did you try `uv run -q main.py`? 

---

_Label `question` added by @zanieb on 2024-08-23 13:23_

---

_Label `documentation` added by @zanieb on 2024-08-23 13:23_

---

_Assigned to @zanieb by @zanieb on 2024-08-23 13:23_

---

_Comment by @justcallmelarry on 2024-08-23 14:24_

@zanieb nope! thanks! i should have found that one, i was mainly looking in the scripts part (as the run command doesn't give output generally while running in projects), but I should have checked the `--help` as well (or the corresponding page).

Thanks!

I did put it in my alias now, so for my workflow it works out nicely :) Sorry for the extra ticket and thanks for all your great work!

---

_Closed by @justcallmelarry on 2024-08-23 14:24_

---

_Comment by @mschoettle on 2024-09-16 21:13_

Is this output necessary to have by default? I've just tried out `uv run` for the first time with a script that has the inline metadata specified. My expectation was that it does not print that, only if I call it with `--verbose`.

---

_Comment by @zanieb on 2024-09-16 21:17_

It seems generally helpful to understand that your invocation is going to run in an entirely different environment than other `uv run` requests. What's the problem with the output?

---

_Comment by @mschoettle on 2024-09-17 16:10_

Apologies, that was my bad. I realized after using it more that there is more output that can be shown besides the one line that I saw "Reading inline script metadata from: ...".

---

_Comment by @woutervh on 2024-11-06 08:58_

@zanieb 
> What's the problem with the output?

why should the output of "uv run hello.py"  be different than python hello.py?  


```
uv run -q hello.py

Hello world!
```

This still prints a newline, which can be annoying/breaking if you need to pipe the output for further processing.





---
