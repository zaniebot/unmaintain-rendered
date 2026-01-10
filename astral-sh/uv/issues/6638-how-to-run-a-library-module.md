---
number: 6638
title: How to run a library module?
type: issue
state: closed
author: ollimandoliini
labels:
  - good first issue
  - cli
assignees: []
created_at: 2024-08-26T07:11:48Z
updated_at: 2025-05-07T01:46:28Z
url: https://github.com/astral-sh/uv/issues/6638
synced_at: 2026-01-10T01:24:03Z
---

# How to run a library module?

---

_Issue opened by @ollimandoliini on 2024-08-26 07:11_

What is the recommended way for running a library module like I would do in vanilla python with the `-m` flag.

E.g.
```sh
python -m mylibrary.module
```

---

_Comment by @gusutabopb on 2024-08-26 09:47_

I believe that'd be simply

```bash
uv run python -m mylibrary.module
```

---

_Comment by @ollimandoliini on 2024-08-26 10:25_

Thanks! That's what I have been using.

It's a bit confusing because there is no parallel for `uv run file.py` for running a module.

---

_Label `question` added by @charliermarsh on 2024-08-26 12:20_

---

_Label `cli` added by @charliermarsh on 2024-08-26 12:20_

---

_Comment by @charliermarsh on 2024-08-26 12:21_

We could consider adding `-m module` but in the meantime yes, explicitly adding `python` there is expected (\cc @zanieb).

---

_Assigned to @zanieb by @zanieb on 2024-08-26 17:32_

---

_Comment by @zanieb on 2024-08-26 17:33_

I think we should document this. I'm not sure we should provide a shorthand yet... but it does seem kind of nice?

---

_Referenced in [astral-sh/uv#6914](../../astral-sh/uv/issues/6914.md) on 2024-09-03 14:14_

---

_Comment by @johnhoran on 2024-09-05 18:19_

One downside to the `uv run python -m mylibrary.module` approach is that this doesn't allow for the inline script metadata.

---

_Comment by @zanieb on 2024-09-05 18:21_

In that case, your module is defined in a script?

---

_Comment by @johnhoran on 2024-09-06 09:09_

Some are in scripts, others are in folders with a `__main__.py` file.

---

_Comment by @rogerbinns on 2024-09-10 15:23_

My [package](https://github.com/rogerbinns/apsw) has several command line tools that are invoked via `-m`.  They are not installed as separate commands.  For example you can do the following which all take extra parameters.

    # gets a database cli shell
    python -m apsw
    # runs test suite
    python -m apsw.tests
    # runs benchmark
    python -m apsw.speedtest
    # runs automatic tracer of SQL
    python -m apsw.trace

I have 3 more under development.  I was hoping that I could do `uvx run apsw`, `uvx run apsw.speedtest` etc perhaps with a `-m` flag.  Sadly that is not the case.  Doing `uv run python -m apsw.speedtest` doesn't automagically install the package like `uvx run` does.  

Please consider adding `-m` to `uvx run` or perhaps make that a default fallback behaviour if `uvx run` doesn't find an installed script.

---

_Comment by @gotmax23 on 2024-09-11 09:10_

`uv run -m` makes a lot of sense to me. You're trying to *run* a *module*.

---

_Referenced in [astral-sh/uv#7281](../../astral-sh/uv/pulls/7281.md) on 2024-09-11 17:55_

---

_Referenced in [astral-sh/uv#7297](../../astral-sh/uv/issues/7297.md) on 2024-09-11 18:21_

---

_Unassigned @zanieb by @zanieb on 2024-09-11 19:35_

---

_Label `good first issue` added by @zanieb on 2024-09-11 19:35_

---

_Label `question` removed by @zanieb on 2024-09-11 19:35_

---

_Comment by @zanieb on 2024-09-11 19:35_

Unless someone can think of another use for the `-m` flag I think it's fine to use for this and should be an easy first contribution.

cc @konstin 

---

_Comment by @rogerbinns on 2024-09-11 22:12_

Is the `-m` flag even necessary?  If a relevant binary isn't found by uv run, couldn't it automatically then behave as though -m was provided?  I'm most interested in `uvx` working, so my [earlier example](https://github.com/astral-sh/uv/issues/6638#issuecomment-2341245714) would just become `uvx apsw ...` or `uvx apsw.speedtest ...`.

---

_Comment by @zanieb on 2024-09-12 00:34_

I don't think implicit behavior is great when executing things, for safety and security reasons.

Why not expose script entry points for your modules? https://docs.astral.sh/uv/concepts/projects/#defining-entry-points

---

_Comment by @rogerbinns on 2024-09-12 01:39_

I agree with the safety aspect in principle, but in this specific case the user already has explicitly said they are trying to run the provided parameters.  Adding more hoops to jump through will just be annoying.

The problem with `project.scripts` is that it creates new executables that end up in `$PATH`, in completions, and in a global namespace.  My project is python specific and doesn't need to contribute to that pollution.

---

_Referenced in [astral-sh/uv#7322](../../astral-sh/uv/pulls/7322.md) on 2024-09-12 04:10_

---

_Referenced in [astral-sh/uv#7738](../../astral-sh/uv/issues/7738.md) on 2024-09-27 13:20_

---

_Comment by @zanieb on 2024-09-27 13:22_

> end up in $PATH, in completions, and in a global namespace

Fair, but note this isn't the case unless you install the project globally or activate the virtual environment — both of which are not necessary with `uv run`.

---

_Comment by @rogerbinns on 2024-09-27 13:50_

> ... unless you install the project globally

My project **is** installed globally on [a large number of Linux/BSD distros](https://repology.org/project/python:apsw/versions).   I'm left with two choices:

* Add the entry points, and then have every distro maintainer add extra logic in their packaging rules to exclude them when doing system packaging 
* Miss out on easy `uv run` for people using `uv`.

---

_Comment by @zanieb on 2024-09-27 15:32_

I see. Maybe your use-case would be fulfilled by #5903 then. There's also an exploration of module support in https://github.com/astral-sh/uv/pull/7322 — but there was some complexity in the command line handling. Maybe the solution is a `--module` flag like we are adding for `--script` #7739?

---

_Comment by @rogerbinns on 2024-09-27 15:59_

I don't have a preference on the solution implementation, as long as the interface is as friendly as `python -m module args...`, and I'm happy to put whatever uv specific config is needed in pyproject.toml to make that happen.

---

_Comment by @zanieb on 2024-09-27 16:04_

Okay thanks for your feedback. @j178 do you see a problem with just doing `module: bool` instead of `module: Option<String>`?

---

_Referenced in [astral-sh/uv#7754](../../astral-sh/uv/pulls/7754.md) on 2024-09-28 07:19_

---

_Comment by @j178 on 2024-09-28 07:28_

Seems working 😆  #7754

---

_Closed by @zanieb on 2024-09-28 15:07_

---

_Comment by @neopostmodern on 2025-02-27 09:29_

I'm still lost on how to use this. I want to automatically install and execute a PyPi module.
Straightup `uvx` fails and if I understand the discussion above correctly, that's intentional:
```
❯ uvx pixels2svg "/home/neopostmodern/example.png"
   Built pixels2svg==0.2.2
Installed 6 packages in 199ms
The executable `pixels2svg` was not found.
warning: Package `pixels2svg` does not provide any executables.
```
Now I tried the suggested `-m` flag, but then I get:
```
❯ uv run -m pixels2svg "/home/neopostmodern/example.png"
/usr/bin/python3: No module named pixels2svg
```
(why does this refer to `/usr/bin`?)

Meanwhile `uvx -m` unfortunately isn't a thing:
```
❯ uvx -m pixels2svg "/home/neopostmodern/example.png"
error: unexpected argument '-m' found
```

---

_Comment by @zhuoqun-chen on 2025-05-07 01:46_

> I'm still lost on how to use this. I want to automatically install and execute a PyPi module.我还是不太清楚如何使用。我想自动安装和执行一个 PyPi 模块。 Straightup `uvx` fails and if I understand the discussion above correctly, that's intentional:直接 `uvx` 失败，如果我正确理解上面的讨论，这是有意为之的：
> 
> ```
> ❯ uvx pixels2svg "/home/neopostmodern/example.png"
>    Built pixels2svg==0.2.2
> Installed 6 packages in 199ms
> The executable `pixels2svg` was not found.
> warning: Package `pixels2svg` does not provide any executables.
> ```
> 
> Now I tried the suggested `-m` flag, but then I get:现在我尝试了建议的 `-m` 标志，但随后出现：
> 
> ```
> ❯ uv run -m pixels2svg "/home/neopostmodern/example.png"
> /usr/bin/python3: No module named pixels2svg
> ```
> 
> (why does this refer to `/usr/bin`?)（为什么这里引用了 `/usr/bin` ？）
> 
> Meanwhile `uvx -m` unfortunately isn't a thing:同时 `uvx -m` 不幸不存在：
> 
> ```
> ❯ uvx -m pixels2svg "/home/neopostmodern/example.png"
> error: unexpected argument '-m' found
> ```

Hi @neopostmodern, does this works for you?

```
uvx --with pixels2svg python -m pixel2svg.pixel2svg "/home/neopostmodern/example.png"
```


---
