---
number: 6750
title: "Avoid creating `main.py` in existing projects"
type: issue
state: closed
author: zanieb
labels:
  - projects
assignees: []
created_at: 2024-08-28T14:26:07Z
updated_at: 2025-04-18T09:48:05Z
url: https://github.com/astral-sh/uv/issues/6750
synced_at: 2026-01-10T01:24:05Z
---

# Avoid creating `main.py` in existing projects

---

_Issue opened by @zanieb on 2024-08-28 14:26_

e.g. in `uv-fastapi-example` I don't want a `hello.py` when I regenerate the `pyproject.toml`

---

_Label `projects` added by @zanieb on 2024-08-28 14:26_

---

_Referenced in [astral-sh/uv#6850](../../astral-sh/uv/pulls/6850.md) on 2024-08-30 02:56_

---

_Referenced in [astral-sh/uv#6853](../../astral-sh/uv/issues/6853.md) on 2024-08-30 12:04_

---

_Comment by @TheMightyRaider on 2024-10-30 15:54_

Is this possible now?

---

_Comment by @soapergem on 2024-11-27 06:02_

What pdm does when you run init is it [creates a template](https://pdm-project.org/latest/usage/template/). It seems uv is trying to do the same thing here, but I would suggest that unless/until you have a similar templating system built out, you should **never** create the hello.py file, or any extraneous files. I cannot think of a single use case where anyone would want that.

Left as-is, you're going to see a lot of tutorials that teach people to run these commands in sequence:

```
uv init
rm -f hello.py
```

---

_Comment by @hay on 2024-12-16 22:33_

I'm not sure why i would want a `hello.py` file in a new project as well. There's already a `--no-readme` flag for not creating a `README`, maybe there could also be a `--no-hello` flag to avoid making a `hello.py`?

---

_Comment by @steeladept on 2024-12-30 19:18_

I am sort of okay with this, but would like to be able to edit/override it instead.  Basically in the `uv/crates/uv/src/commands/project/init.rs` file, lines 364-380 should be rewritten to allow users to overwrite it with a global default template providing both a template name (instead of hello.py).

A bonus would be being able to define a default global structure, so all py files are in src directory, for example, and test.py files in a test directory.  But this is then getting to the full structure templating, which would be a much larger change.

---

_Comment by @ebc-wingmen on 2025-01-10 09:19_

I never want a `hello.py` under any circumstance. The name alone is a little bit laughable if I am being honest. If `uv` absolutely must generate a python file, an acceptable alternative could be `main.py`, `__main__.py` or `project_name.py`. Unfortunately every guide I write for colleagues or students have to include an extra line to handle this file unless I am creating a bare `venv` - but then I am missing out on the pyproject.toml file which is even worse.

---

_Comment by @arthuralvim on 2025-01-15 17:10_

Maybe `uv init` default behavior should be equivalent to `poetry init -n` where only the `pyproject.toml` is created.

Right now I have an alias for this command:
`uv init --no-workspace --no-pin-python --no-readme --no-package mynewproject && rm -f mynewproject/hello.py`



---

_Referenced in [astral-sh/uv#10369](../../astral-sh/uv/pulls/10369.md) on 2025-01-24 15:10_

---

_Comment by @Bengt on 2025-01-24 15:21_

(I basically agree with everyone in this thread, but I collected some data in the thread @zanieb pointed me from to here, so I am shamelessly reposting my comment from there here:)

1. For reference,
  - [there are currently 50.700 `hello.py` files on GitHub](https://github.com/search?q=path%3Ahello.py&type=code),
  - [there are currently 132.000 `cli.py` files on GitHub](https://github.com/search?q=path%3Acli.py&type=code), and
  - [there are currently 175.000 `example.py` files on GitHub](https://github.com/search?q=path%3Aexample.py&type=code).

2. In Python, default file names vary a bit with the domain, and hence one's expectations might be shaped in various ways. `app.py` is indeed common, with [currently about 721.000 `app.py` files on GitHub](https://github.com/search?q=path%3Aapp.py&type=code).

3. Still, `main.py` is an even more common name. At the time of writing, [there are 2.2 million `main.py` files on GitHub](https://github.com/search?q=path%3Amain.py&type=code). Interestingly, this is exactly because 'main' is a commonly used name in the Python world, like `main()`, `__main__`, or `__main__.py`. To adhere to this pythonic naming scheme and avoid surprises, `main.py` should in my opinion be default in `uv init`.

4. I do not feel like there is one, universally accepted best practice for naming your first file. I think we would therefore need a command line argument for naming the default file. The command line option should also include a way to disable creating a default file altogether. Currently, one needs to advise `uv init && rm hello.py`, which seems unnecessarily convoluted and is not portable to PowerShell, so portable instructions require duplication. m(

5. The default file is handy if one starts from scratch, but cumbersome if there is an existing codebase. With an existing code base, the use case is to introduce uv to it, not build something new. In projects with preexisting code, uv would ideally detect the presence of a codebase and not create a default file. This should be equivalent to using the deactivation functionality mentioned in 4.

---

_Comment by @hay on 2025-01-24 16:18_

@Bengt thanks for collecting all the data! I would agree with you on points 4) and 5). If there should be a default file made by uv it should probably be called `main.py` and not `hello.py`.

---

_Comment by @zyrikby on 2025-01-27 21:38_

I have one more use-case -- I use uv to create a directory hosting data analysis notebooks and manage the dependencies. Now, I create the project and then I need to delete manually `hello.py`. I think that it would be nice to add an option, e.g. `--clean` (similar to  `--app` or `--lib`), that would by default only create pyproject.toml (without readme, vcs and python version). 

---

_Referenced in [astral-sh/uv#11192](../../astral-sh/uv/pulls/11192.md) on 2025-02-03 19:55_

---

_Comment by @Gankra on 2025-02-12 17:39_

Just updating the status of this since a lot of the comments are outdated and kinda talking about an orthogonal question:

* In latest versions you can do `uv init --bare` which indeed just makes the pyproject.toml
* In v0.6 we're planning to rename `hello.py` to `main.py`: https://github.com/astral-sh/uv/pull/10369

As such this issue is really now about "should we have a heuristic where we detect you already have python files setup and automatically avoid creating main.py" and "what would that heuristic be".

I think the heuristic is a good idea but it's less clear what it should be. Two options come to mind for when to not make `main.py`:

* `*.py`: if there's any `.py` in the same directory we would create the `main.py` (i.e. the current working dir)
* `**/*.py`: if there's any `.py` in the same directory *or any subdirectory*

The first seems nice and simple but maybe too weak, the second could be overzealous. Perhaps something more precise than this could be better though? That said, on balance "making a file you have to delete" seems less bad than "not making a file you expect to be there and being confused".

---

_Referenced in [astral-sh/uv#11329](../../astral-sh/uv/pulls/11329.md) on 2025-02-12 17:47_

---

_Added to milestone `v0.6.0` by @Gankra on 2025-02-12 17:48_

---

_Comment by @phaabe on 2025-02-12 18:38_

Great improvement! Thanks!

Why at all initialize with a `main.py` if there are already `*.py` files in root or subdirs?

I think it would make sense to not do that in this case. 

---

_Renamed from "Avoid creating `hello.py` in existing projects" to "Avoid creating `main.py` in existing projects" by @zanieb on 2025-02-13 22:07_

---

_Comment by @zanieb on 2025-02-13 22:08_

Let's consider this closed by the `--bare` option for now. I think it'd be too confusing to omit this file _sometimes_.

---

_Closed by @zanieb on 2025-02-13 22:08_

---

_Comment by @simeneide on 2025-03-10 12:50_

I got to this after being annoyed by the hello.py files. they were good for the two or three first times i used uv, but now they are just annoying. Instead, why not make the default opposite: `uv init --template` if I want helper files? althogh, `--bare` will work for now, just thought about throwing the idea in there

---

_Comment by @edward-jazzhands on 2025-04-17 20:53_

I'd like to add, I think the idea of a heuristic is interesting. But my concern is that the creation of the main.py file where it is, is just confusing to people that are newer to Python. I've already seen this numerous times helping people on Discord. It doesn't help that it makes it in the project root folder. Which is odd, since it goes against normal package organization conventions. Perhaps if it placed the file at `src/my_project_name/main.py` then I could see how that would make sense. But it doesn't do that, which I fear is just more confusing to new people than it's helping.


---

_Comment by @zanieb on 2025-04-17 21:03_

@edward-jazzhands we want to use a `src/` layout by default, but that requires a build backend and ours is not stable yet (and when we used other ones by default, it was even more confusing).

---

_Comment by @edward-jazzhands on 2025-04-18 09:48_

ah I see. Well in that case I'd like to propose a tiny solution: Simply add a couple lines of comments to the top of the main.py file that gets generated by uv init. I think something along the lines of "This file is created to help with project scaffolding but you can safely move or delete it if you want to" would go a long way to avoiding confusion for newer python coders.

---
