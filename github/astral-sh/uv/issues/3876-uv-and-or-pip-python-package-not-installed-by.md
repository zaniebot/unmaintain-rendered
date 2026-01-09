---
number: 3876
title: uv and/or pip python package not installed by default in new venv created by uv
type: issue
state: closed
author: mcrumiller
labels:
  - question
assignees: []
created_at: 2024-05-28T12:20:13Z
updated_at: 2025-12-07T07:16:15Z
url: https://github.com/astral-sh/uv/issues/3876
synced_at: 2026-01-07T13:12:17-06:00
---

# uv and/or pip python package not installed by default in new venv created by uv

---

_Issue opened by @mcrumiller on 2024-05-28 12:20_

When I create a new virtual environment using `uv`, I cannot use uv or pip in that environment, making it difficult to install other packages.

```shell
C:\test_uv>python -m uv venv 
Using Python 3.11.7 interpreter at C:\Users\<user>\AppData\Local\Programs\Python\Python311\python.exe
Creating virtualenv at: .venv
Activate with: .venv\Scripts\activate

C:\test_uv>.venv\Scripts\activate

(test_uv) C:\test_uv>python -m uv
C:\test_uv\.venv\Scripts\python.exe: No module named uv

(test_uv) C:\test_uv>python -m pip pip
C:\test_uv\.venv\Scripts\python.exe: No module named pip
```

I cannot access `uv.exe` directly due to company policy.

---

_Renamed from "`uv` and/or `pip` not installed by default in new venv created by `uv`" to "`uv` and/or `pip` python package not installed by default in new venv created by `uv`" by @mcrumiller on 2024-05-28 12:20_

---

_Renamed from "`uv` and/or `pip` python package not installed by default in new venv created by `uv`" to "uv and/or pip python package not installed by default in new venv created by uv" by @mcrumiller on 2024-05-28 12:21_

---

_Comment by @charliermarsh on 2024-05-28 13:10_

We never add uv to the virtual environment because there’s no need. uv doesn’t need to be installed in a given environment — your global uv can install into any environment.

If you want to include pip, you can run “uv venv —seed”.

---

_Comment by @mcrumiller on 2024-05-28 13:18_

> your global uv can install into any environment

Sorry, I must not be familiar enough with virtual environments. What exactly do you mean here? How do I use my "global uv" after I've activated a uv-created environment?

---

_Comment by @charliermarsh on 2024-05-28 13:44_

Like, typically, if you install uv globally, then you'd run `uv venv` and `uv pip install`, and uv will install into the activated environment even though it's not installed into the environment, because the `uv` in `uv venv` will resolve to the uv installed outside of the environment, and uv supports that just fine.

If you can't run uv directly due to company policy (any idea why this is?), then I guess you do need to install it into the environment? Because `python -m uv` won't work once you've activated the environment. So in that case, I think you'd need:

```
uv venv --seed
.venv\Scripts\activate
pip install uv
uv pip install flask
```


---

_Comment by @charliermarsh on 2024-05-28 13:44_

It's not really a workflow that we'd intended but it should work.

---

_Comment by @charliermarsh on 2024-05-28 13:45_

(`--seed` is intended for compatibility with non-uv workflows -- it installs `pip` into the virtualenv, as you would get if you ran `python -m venv`. We omit `pip` by default since you typically don't need it if you're using uv already.)

---

_Comment by @mcrumiller on 2024-05-28 14:39_

We use Airlock which blocks anonymous `.exe` files from executing on the system, which includes `uv.exe` in a virtual env. I could submit a ticket for an exception but it's easier to just use pip to create the env at this point, especially if I'm trying to get others set up.

---

_Comment by @charliermarsh on 2024-05-28 15:42_

Ok -- in that case, `uv venv --seed` will enable you to install `uv` into the `venv` with `pip install`.

---

_Closed by @charliermarsh on 2024-05-28 15:42_

---

_Label `question` added by @charliermarsh on 2024-05-28 15:42_

---

_Comment by @mcrumiller on 2024-05-28 16:03_

Thanks, will do that.

---

_Comment by @campbellmc on 2024-06-19 10:28_

This happens when installing uv as a non privileged Windows user. In that case, uv is installed to user profile AppData site packages and it won't be available from within virtual envs.
Install uv from an elevated terminal and it will be available from within virtual environments.

---

_Comment by @mcrumiller on 2024-06-19 11:13_

@campbellmc that requires the ability to open an elevated terminal...

---

_Comment by @campbellmc on 2024-06-19 11:16_

yes it does.
I was just commenting here for the sake of anyone who comes across the problem and can fix it that way

---

_Referenced in [astral-sh/uv#9157](../../astral-sh/uv/issues/9157.md) on 2024-11-16 22:25_

---

_Referenced in [astral-sh/uv#9689](../../astral-sh/uv/pulls/9689.md) on 2024-12-06 16:32_

---

_Comment by @antazoey on 2025-02-19 15:37_

OK I am commented my less-than-good experience here in hopes of helping others and hopefully leading to some improvements!

I am following the docs. I need to create a globally accessible virtual environment. So I did:

```
uv venv uvvenvs/gen13
source uvvenvs/gen13/bin/activate
```

note: gen13 is the name because it is "general" python 3.13, a scratch-like env not tied to a specific project. I need one for each Python version for my use-cases.

Both `uv` and `pip` are not available in this environment, so I had to back out. I don't know how to install anything. So, I ran:

```
deactivate
rm -rf ~/uvvenvs/gen13
```

I try to create the gen13 venv again, only this time using `--seed` _and_ the path to the venv:

```
uv venv uvvenvs/gen13 --seed
```

OK so it looks like it gave me `pip` now? Cool, I will use that.

```
source uvvenvs/gen13/bin/activate
which pip
> /Users/antazoey/uvvenvs/gen13/bin/pip
pip install -e /Users/antazoey/path/to/my./project
```

**Things I don't understand:**

1. What is the point of _not_ using `--seed`? Why would you need a Python environment which you can't install anything in? It seems like I am not understanding how to use `uv` yet.
2. Why am I stuck using `pip` in my venv? I thought `uv` was the new, faster `pip` but I still have to use `pip` anyway?



---

_Comment by @zanieb on 2025-02-19 15:54_

> What is the point of not using --seed? Why would you need a Python environment which you can't install anything in? It seems like I am not understanding how to use uv yet.

You don't need `pip` because you have uv. You can do `uv pip ...` instead of `pip ...` — it's a compatible interface.



---

_Comment by @zanieb on 2025-02-19 15:55_

Can you share some more about what you need a global environment for? It's helpful context as we improve our support for that use-case.

---

_Comment by @antazoey on 2025-02-22 21:02_

> Can you share some more about what you need a global environment for? It's helpful context as we improve our support for that use-case.

I need scratchy envs for each python for experimenting and frequently changing packages, it is for adhoc development of a vast series of annoying python libraries

---

_Referenced in [astral-sh/uv#12738](../../astral-sh/uv/issues/12738.md) on 2025-04-08 12:40_

---

_Referenced in [astral-sh/uv#14873](../../astral-sh/uv/issues/14873.md) on 2025-07-24 15:58_

---

_Comment by @XanthanGum on 2025-08-04 03:27_

Is there a one liner to include uv as a seed package?

Similar issue where I can not install uv as a privileged user, so calls to uv pip install don't seem to find my .venv. Including uv as a seed would avoid an extra execution when rebuilding a venv.

py -m uv venv --seed
.venv/Scripts/activate
pip install uv
uv pip install -r requirements.txt

---

_Comment by @JtMotoX on 2025-10-01 04:58_

> You don't need `pip` because you have uv. You can do `uv pip ...` instead of `pip ...` — it's a compatible interface.

@zanieb Many install scripts use pip. You expect me to modify vendor install scripts to replace 'pip' with 'uv pip'? Your answer is not a solution.

---

_Comment by @charliermarsh on 2025-10-01 13:47_

Any package that depends on pip as a build dependency should declare it as a build-time dependency -- it's an error not to.

---

_Comment by @Albert-Gao on 2025-10-13 23:12_

I wonder if we can have the `--seed` as the default so I do not need to type it every time, the test explorer in VSCode requires it, otherwise, it does not work.

---

_Comment by @eranhirs on 2025-12-07 07:16_

In case you have an existing venv environment and you don't want to create a new one, you can use `python -m ensurepip --default-pip`

(See https://docs.python.org/3/library/ensurepip.html)

---
