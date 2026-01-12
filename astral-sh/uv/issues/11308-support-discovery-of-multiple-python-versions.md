```yaml
number: 11308
title: Support discovery of multiple python versions that are not on path
type: issue
state: closed
author: mneumei
labels:
  - enhancement
  - needs-decision
assignees: []
created_at: 2025-02-07T07:05:18Z
updated_at: 2025-03-06T19:26:08Z
url: https://github.com/astral-sh/uv/issues/11308
synced_at: 2026-01-12T16:00:33Z
```

# Support discovery of multiple python versions that are not on path

---

_@mneumei_

### Summary

We have multiple python versions that we do not want to put on path. For a single venv, we run uv with --python and there is no issue, but when we want to run tox, we can not tell uv where to find those different python versions anymore (original flag: discover, with as many python versions as you want).

Ideally, you could e.g. give multiple --python options, and uv will select the most suitable (matching the python requirements, newest, first)

Linked Issue:
https://github.com/tox-dev/tox-uv/issues/170#issuecomment-2642026663


### Example

_No response_

---

_Label `enhancement` added by @mneumei on 2025-02-07 07:05_

---

_Comment by @zanieb on 2025-02-07 15:56_

I don't think we'll allow `--python` to be repeated like this, it sounds too complicated. Can't tox invoke uv with the proper `--python` invocation for each requested version? Or why can't the `PATH` be changed just for the uv invocation?


---

_Comment by @gaborbernat on 2025-02-07 18:30_

While `tox` could do this, I don't think is unreasonable for uv to handle it. `uv` already does some kind of Python interpreter discovery, it's not unreasonable to expect to be able to extend the list of Pythons to consider with a list of static paths 🤔 that `uv` can then also inspect part of the discovery. 

---

_Comment by @zanieb on 2025-02-07 18:36_

I don't quite understand the use-case though. Like, why support things in addition to `PATH`? Interpreter discovery is already very complicated.

---

_Comment by @gaborbernat on 2025-02-07 18:49_

Is an escape hatch to force a given Python interpreter to be used, basically pushes that interpreter as the highest priority onto the discovery chain. Has a few advantages:

- does not require changing an environment variable and can be explicit use this first without needing to change scripts. 
- if that PATH contains multiple Python scripts you don't' have to add the ones you don't want to use.

It follows the idea of explicit is better than implicit. It's primary use case is that imagine you have a script that requires Python `3.13`. You have two builds of that Python on your system, one with debug symbols and one without. In general you want to use the non-debug symbol one as is faster. But you ran into a bug and you need more information so you want to temporarily switch to the debug variant.

In your script you specify `uv venv -p 3.13`. Now how can you temporarily switch between the two variants? You could add to the beggining of the PATH the one you want to use (but perhaps there are other Python versions too, and you're now sure which will be picked), however now you're changing an environment variable that could have other side effects to. Or you can change your script, but this now means you modified a file that you don't want to commit. 

A not complicated workaround for these use cases that `virtualenv` has is the https://virtualenv.pypa.io/en/latest/cli_interface.html#try-first-with flag. This allows the user to short circuit the discovery, and from `virtualenv` POV is just first trying those passed in if compatible and use that, if they are not can fallback to the more complicated discovery mechanism. This issue is asking for something similar in `uv`; via a flag and/or environment variable.

---

_Comment by @zanieb on 2025-02-07 18:54_

> Is an escape hatch to force a given Python interpreter to be used, basically pushes that interpreter as the highest priority onto the discovery chain. 

We do support this already, via `-p <path>`.

I'm pushing against accepting _multiple_ Python paths and choosing between them.

`--try-with-first` is... interesting. I've never heard of that. It seems peculiar.

Anyway, happy to consider — but I probably won't prioritize adding this without more feedback about why it's needed / use-cases / user requests.

---

_Comment by @gaborbernat on 2025-02-07 18:57_

> We do support this already, via `-p <path>`.

This doesn't work if you're script specifies the generic `-p 3.13` and you don't want to change your script to hardcode a path.

Another use case was of forcing thread free Python versions before we explicitely added support for them. Because before adding support the virtualenv discovery didn't know the difference between a threaded and non thread versions, so by settting that environment variable the user could force start/use this interpreter even if they are other compatible versions on the machine.

@mneumei can add their own use case, but I present one above 👍 

---

_Label `needs-decision` added by @zanieb on 2025-02-07 19:08_

---

_Comment by @AdrianCert on 2025-02-09 17:35_

Our use case is that we don’t have an internet connection during our CI process, so we cannot rely on Python being managed by UV. Instead, we have a predefined set of Python versions available that we can use.

---

_Comment by @gaborbernat on 2025-02-09 18:10_

> Our use case is that we don’t have an internet connection during our CI process, so we cannot rely on Python being managed by UV. Instead, we have a predefined set of Python versions available that we can use.

Can you explain why you can't just set the PATH environment variable for your use case? Thank you.

---

_Comment by @mneumei on 2025-02-10 06:25_

Usually, the PATH is a very messy thing to deal with. Multiple applications put their "things" on path. We had the case where the proxy app px was shipped with a python.exe, which messed up everything that relied on a proper setup of path for python. In addition, you might have venvs and other processes put their paths on top, making it very hard to know what you actually end up with.

As @gaborbernat pointed out, its about beeing explicit. We want to test with tox 4 different python versions, and we want them to be explicitely those we preconfigured, nothing else. 

My proposal is to have some option where you basically say: don´t download, don´t take from path, but take a look at specifically those versions, then do your selection from the executables as you would do when you e.g. discovered them from path.

The idea of --python of course is to add less complexity, but it might also be: --python-interpreters, --python-discovery-locations or similar

---

_Comment by @zanieb on 2025-02-11 22:42_

It looks like this is vaguely a duplicate of https://github.com/astral-sh/uv/issues/9506


---

_Comment by @zanieb on 2025-03-06 19:26_

Going to close this in favor of the other issue, which covers the same use-case.

---

_Closed by @zanieb on 2025-03-06 19:26_

---
