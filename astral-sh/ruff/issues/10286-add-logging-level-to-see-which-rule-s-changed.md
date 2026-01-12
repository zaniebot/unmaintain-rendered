```yaml
number: 10286
title: Add logging level to see which rule(s) changed particular source line(s)
type: issue
state: closed
author: ssteinerx
labels:
  - question
  - wontfix
assignees: []
created_at: 2024-03-08T02:00:20Z
updated_at: 2024-03-14T03:50:36Z
url: https://github.com/astral-sh/ruff/issues/10286
synced_at: 2026-01-12T15:54:50Z
```

# Add logging level to see which rule(s) changed particular source line(s)

---

_@ssteinerx_

**The current Ruff version (`ruff --version`).**
```bash
$ ruff --version
ruff 0.3.1
```
**The command you invoked (e.g., `ruff /path/to/file.py --fix`), ideally including the `--isolated` flag.**
```bash
$ ruff format --isolated --verbose module/__init__.py
```
When integrating code into a project, sometimes there are meaningless differences in formatting that trigger a huge number of file changes by our pre-commit ruff hook.  

Right now, it's hard to know exactly which rules need to be modified to keep this from happening.  We don't want to just wholesale skip the pre-commit rules or try to figure out how to make an exception for certain code; just a temporary tweak to get the code integrated without having to deal with a zillion trivial, semantically meaningless changes.

Unfortunately, there doesn't seem to be a way to get ruff to explicitly log which rule prompted which formatting change.

The proposition is to add a logging option to show which formatting rules triggered which changes.

Here's the specific scenario.

This:
```python
class OMG_SomethingBadHasHappened(Exception):
    ...
```
becomes:
```python
class class OMG_SomethingBadHasHappened(Exception): ...
```
We don't want it to do that particular piece of formatting any more.

So I'll just go configure *The Rule* that made it do that.  

Uh, which one?  I _**Haven't The Slightest Clue.**_

Just for laughs, I asked an AI (which shall remain nameless) about how to track the specific rule causing specific changes and its suggestion was to:
> Start by disabling rules one at a time in the Ruff configuration file...to see if the unwanted formatting change persists. This process can be somewhat tedious but effective for pinpointing the exact rule.  

And if that doesn't demonstrate a human-like "gift for understatement," I guess I don't know what does... 

---

_Label `question` added by @MichaReiser on 2024-03-08 07:45_

---

_Comment by @MichaReiser on 2024-03-08 07:49_

Hi @ssteinerx, Thank you for the detailed write-up. This is very helpful!

The `format` command is separate from the `linter` and enabling or disabling lint rules does not change the formatter's behavior. The formatter has [a few settings](https://docs.astral.sh/ruff/settings/#format) that you can set to change how it formats your code but they're [intentionally limited](https://docs.astral.sh/ruff/formatter/#philosophy).


---

_Comment by @ssteinerx on 2024-03-09 00:33_

The document you linked does indicate that *options* are limited, but as far as I could see, didn't address the issue of logging the reasons for code changes it does make.

Configuration options are great -- it's one of the reasons I won't use Black; I disagree with some of the choices it makes and those choices make my work harder.  It's hard enough, thanks.

That said, configuration isn't really configurable if you can't figure out which configuration option would configure the thing you want configured.  (say that *one* time fast...)


---

_Comment by @zanieb on 2024-03-09 00:58_

The formatter can't feasibly share _why_ lines would change — it's just not how it's designed. I don't know of any code formatters that do this.

I would recommend just reading our configuration options for the formatter: https://docs.astral.sh/ruff/settings/#format

There are not many settings and each one has a concrete example of what it changes. There are likely not settings for the changes you want.

You may want to just use some of the format-related lint rules instead of the formatter itself — it sounds like it does not match your use-case.

---

_Closed by @zanieb on 2024-03-11 16:56_

---

_Label `wontfix` added by @zanieb on 2024-03-11 16:56_

---

_Comment by @ssteinerx on 2024-03-14 03:50_

>  I don't know of any code formatters that do this.

I don't either, which I didn't even realize until you said it.  Sure would be handy for when one formatting choice is creating a giant diff and you'd just like to disable that one rule to get everything started.  

I realize the formatter has way fewer options, I guess I'll just hunt it down.

Thanks!



---
