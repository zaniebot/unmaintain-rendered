```yaml
number: 3143
title: "Recent `uv` dependency resolution installs older version of package"
type: issue
state: closed
author: pjbull
labels: []
assignees: []
created_at: 2024-04-19T17:47:31Z
updated_at: 2025-05-11T19:43:50Z
url: https://github.com/astral-sh/uv/issues/3143
synced_at: 2026-01-10T03:41:46Z
```

# Recent `uv` dependency resolution installs older version of package

---

_Issue opened by @pjbull on 2024-04-19 17:47_

Our CI process that uses uv fails because dependency resolution now selects a much older version of `boto3` and `botocore`.

Seems related to #3127, but it's still not doing the right thing in 0.1.34.


## Repro:
```
git clone https://github.com/drivendataorg/zamba
cd zamba
uv pip install -e ".[tests]"
```


## Result:

Installs ancient versions of boto:

```
 + boto3==1.7.84
 + botocore==1.10.84
```

## Expected:

Recent boto, e.g., `boto3==1.34.84` and `botocore==1.34.84`. (This is what pip does).


-------------------------------------


For completeness, here are the logs from our CI, which you can [see in full here](https://github.com/drivendataorg/zamba/actions/runs/8756285665/job/24032719807).


```
installing uv...
done! ✨ 🌟 ✨
  installed package uv 0.1.34, installed using Python 3.10.12
  These apps are now globally available
    - uv

...

Run uv pip install -e .[tests]
Built 1 editable in 271ms
Resolved 125 packages in 56.85s
warning: The package `typer==0.12.3` does not have an extra named `all`.
Downloaded 100 packages in 20.03s
Installed 125 packages in 288ms
 + absl-py==2.1.0
 + aiohttp==3.9.5
 + aiosignal==1.3.1
 + appdirs==1.4.4
 + async-timeout==4.0.3
 + attrs==[23](https://github.com/drivendataorg/zamba/actions/runs/8756285665/job/24032719807#step:7:24).2.0
 + av==12.0.0
 + boto3==1.7.84
 + botocore==1.10.84

...

```



And here are the [previous scheduled run](https://github.com/drivendataorg/zamba/actions/runs/8677163381/job/23792460498) we have with `uv==0.1.31` boto3 and botocore resolved to correct (recent) versions.  

```
installing uv...
done! ✨ 🌟 ✨
  installed package uv 0.1.31, installed using Python 3.10.12
  These apps are now globally available
    - uv

...

Run uv pip install -e .[tests]
Built 1 editable in 515ms
Resolved 124 packages in 1m 52s
warning: The package `typer==0.12.3` does not have an extra named `all`.
Downloaded 99 packages in 23.67s
Installed 124 packages in 297ms
 + absl-py==2.1.0
 + aiohttp==3.9.4
 + aiosignal==1.3.1
 + appdirs==1.4.4
 + async-timeout==4.0.3
 + attrs==[23](https://github.com/drivendataorg/zamba/actions/runs/8677163381/job/23792460498#step:7:24).2.0
 + av==12.0.0
 + boto3==1.34.84
 + botocore==1.34.84

 ...

```

---

_Comment by @charliermarsh on 2024-04-19 18:27_

I'm a little bit mixed on this because the updated resolution is "equally" valid -- some packages resolved to newer versions, e.g., the newer resolution was able to upgrade to `urllib3==2.2.1` instead of `urllib3==1.26.18`, but at the cost of having to use older boto versions.

To _ensure_ that you receive newer boto versions, I think you should be specifying lower bounds on them?

---

_Comment by @charliermarsh on 2024-04-19 18:28_

I'll see if I can understand why the resolution changed. It may not actually be a bug though.

---

_Comment by @pjbull on 2024-04-19 19:02_

I think that `botocore` is a transitive dependency from `cloudpathlib` (which we also happen to maintain), and we recently added a floor for `boto` dependencies there that will be released shortly.

That said, this still feels like `uv` used to do the expected thing and `pip` still does, so there may be a bug there.

FWIW, though I don't see a hard requirement listed in the codebase, the version of botocore that gets picked up doesn't even list any non-EOL'd Python versions for support: https://github.com/boto/botocore/blob/1.10.84/setup.py#L72-L81




---

_Comment by @charliermarsh on 2024-04-19 19:15_

Yeah, I think the previous resolution was more "intuitive", I'm mostly just pointing out that they're both arguably "correct". (The new resolution would only be incorrect if it didn't contain _any_ upgraded versions.) May be able to get back to that state, I have to play around with the heuristics.

---

_Comment by @pjbull on 2024-04-19 19:26_

Sounds good. This may be some pathological case for the heuristics since the dependency tree for this application gets pretty gross.

FWIW, switching to `uv` has saved us ~5-10 minutes per CI run, which has been awesome. Thanks for that!

---

_Assigned to @charliermarsh by @charliermarsh on 2024-04-19 20:08_

---

_Comment by @charliermarsh on 2024-04-19 21:57_

So I just ran `cargo run pip install -e ".[tests]"` in Zamba and got `boto3==1.34.88` and `botocore==1.34.88`. I'm wondering if something changed in the dependency tree between the creation of the issue and now? Trying to repro...

---

_Comment by @pjbull on 2024-04-19 22:45_

Yep, sorry. Just added a `botocore` pin as a workaround. You can checkout at commit `a3e4feee6ea5a90f37ed9ede90341fb345d39096` and it should repro.

---

_Comment by @notatallshaw on 2024-04-19 23:04_

I can not reproduce what you're seeing, I tried on Python 3.10, Linux, using that checkout, and trying to exclude anything newer than the last few hours:

```
$ git checkout a3e4feee6ea5a90f37ed9ede90341fb345d39096
HEAD is now at a3e4fee Experimental image support (#314)
$ uv pip install --dry-run --upgrade --reinstall --exclude-newer 2024-04-19T12:00:00Z -e  ".[tests]"
...
 + boto3==1.34.87
 + botocore==1.34.87
...
```

Btw, pip's resolver also gets into these situations where it picks a much older version of a package the user would prefer not to have.

---

_Comment by @charliermarsh on 2024-04-19 23:10_

Ok, now I was able to reproduce by passing `--python-version 3.8`.

---

_Comment by @charliermarsh on 2024-04-19 23:10_

(But on Python 3.11 I don't see the behavior even as of `a3e4feee6ea5a90f37ed9ede90341fb345d39096`.)

---

_Comment by @notatallshaw on 2024-04-19 23:10_

Ah, I can reproduce now, but on Python 3.8 (which going through the CI logs I notice). I'm going to try and see if I can figure out what pip does here.

---

_Comment by @charliermarsh on 2024-04-19 23:12_

I think I know the cause, but not the details of why / how. pip has a better heuristic for "package depth", whereas we're preferring constrained packages over unconstrained packages regardless of the depth.

---

_Comment by @notatallshaw on 2024-04-19 23:19_

> pip has a better heuristic for "package depth",

btw, this sometimes helps pip resolution and sometimes hurts it, I don't have a good intuition or evidence if it is a net benefit. Though, it seems here it probably helps.

---

_Closed by @charliermarsh on 2024-04-19 23:30_

---

_Comment by @michealroberts on 2025-04-09 18:08_

I'm not sure if this is related to my issue, but I am still seeing this issue as of today -> a minor version of 0.8.0 is available, but uv insists on installing 0.7.0 ...

---

_Comment by @michealroberts on 2025-04-09 18:09_

<img width="930" alt="Image" src="https://github.com/user-attachments/assets/25c793b6-28f6-46b2-9a55-814a418364dd" />

<img width="290" alt="Image" src="https://github.com/user-attachments/assets/9831b49e-5fa6-4723-9c6b-d492cb129d2b" />

---

_Comment by @michealroberts on 2025-04-09 18:11_

If anyone also experiences this behaviour, the workaround is to clear out the uv cache:

```
uv cache clean
```

or specifically for a package, e.g.,:

```
uv cache clean numpy
```

<img width="608" alt="Image" src="https://github.com/user-attachments/assets/43d6f38a-9ba9-4c2e-8520-3038fc45ed66" />

---

_Comment by @peterschmidt85 on 2025-05-11 18:53_

I have the same issue with Python 3.9.

---

_Comment by @notatallshaw on 2025-05-11 19:43_

> I have the same issue with Python 3.9.

Please open a new issue with a reproducible example. 

---
