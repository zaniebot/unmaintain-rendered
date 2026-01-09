---
number: 7782
title: "FR: `uv init` option to specify alternative filename to `hello.py`  "
type: issue
state: closed
author: nbbaier
labels:
  - projects
  - cli
assignees: []
created_at: 2024-09-29T16:56:07Z
updated_at: 2025-02-13T22:06:49Z
url: https://github.com/astral-sh/uv/issues/7782
synced_at: 2026-01-07T13:12:17-06:00
---

# FR: `uv init` option to specify alternative filename to `hello.py`  

---

_Issue opened by @nbbaier on 2024-09-29 16:56_

When using `uv init` (or `uv init --app`), I find it kind of strange that entrypoint file generated is named `hello.py` and not `main.py`. Right, as far as I can tell, now the _only_ solution is to manually change the filename.

It would be great to have an option to specify the entrypoint's filename, something like `uv init --entrypoint main.py`, or change the default to `main.py` instead of `hello.py`

---

_Comment by @charliermarsh on 2024-09-29 17:03_

My initial reaction is that adding an argument for this feels unnecessary. It’s almost as much work to provide the argument as it is to rename the file. I’m gonna defer to @zanieb on this one though.

---

_Comment by @nbbaier on 2024-09-29 17:15_

Fair point. I think my preference here would actually be making `uv init` generate `main.py` over `hello.py`. Should have made that my request! For now, I've implemented this manually in my `.zshrc` with:

```sh
uvi() {
    uv init "$@"
    mv hello.py main.py
}
```



---

_Comment by @bluss on 2024-09-29 20:43_

#7670 is similar but not the same, about picking more commonly preferred defaults

---

_Comment by @zanieb on 2024-09-29 21:10_

I think we could call it `main.py` instead — though having a function `main` in a file `main` and also having a `__main__` file in some cases could be confusing. I'm not attached to the default name of `hello.py` though. It was mostly intended to be an example file that matched the other init kinds which have a `hello` command and function.

---

_Label `projects` added by @zanieb on 2024-09-29 21:10_

---

_Label `cli` added by @zanieb on 2024-09-29 21:10_

---

_Referenced in [astral-sh/uv#10304](../../astral-sh/uv/issues/10304.md) on 2025-01-05 20:00_

---

_Comment by @edmorley on 2025-01-06 19:58_

If it helps as a data-point: From the research I did for the Heroku Python buildpacks [1] (using metrics from many thousands of builds/apps), the most common filenames we see in "single `.py` file in the root directory" type projects are `app.py` and `main.py`. The latter I'm guessing due to guides like the FastAPI tutorial using `main.py`. The former I'm less certain about, but I know Flask does auto-detection if it finds an `app.py` - so perhaps that's the reason?


[1]: One of the features of Heroku is that users can `git push` arbitrary code to their app, and the Heroku build system via the buildpack detection feature will determine which language their app uses, and run the appropriate buildpack to build that app. Whilst the primary signal our Python buildpack checks for is the presence of a package manager file (such as `requirements.txt`, `poetry.lock`, and in the future `uv.lock` etc), it's not uncommon for users to have manually `pip install`ed their dependencies locally and either not created a requirements file at all, or forgotten to Git commit it. So we use additional signals to determine the app's primary language is likely Python (bearing in mind apps can be multi-language, and users can have one-off Python scripts in the repo root that are for development use only), so we can then display a "the package manager file is missing" type error to those apps.

---

_Referenced in [astral-sh/uv#10369](../../astral-sh/uv/pulls/10369.md) on 2025-01-07 15:31_

---

_Referenced in [astral-sh/uv#10392](../../astral-sh/uv/issues/10392.md) on 2025-01-08 14:32_

---

_Comment by @artjxm on 2025-02-08 05:35_

Hello to all! I'm new to issues but want to join the pool of feature requesters! It would be great to have alternative names for the initial `.py` file, or at least an option to disable creating it at all (`--no-hello`).

p.s.: Thank you a million for creating such amazing tools like uv and ruff, I just started using them and am already in deep love!! Gonna spread the word across my fellow python devs and enjoyers <3

---

_Comment by @zanieb on 2025-02-08 15:16_

@artjxm you can disable it with `--bare` (added in #11192)

Glad you like the tools <3

How do people feel about `<project-name>.py` vs `app.py` or `main.py`? Just wondering as we get close to changing this in v0.6.0 (#11329)



---

_Comment by @nbbaier on 2025-02-08 17:14_

@zanieb I'd much prefer `app.py` or `main.py` to `<project-name>.py`.

---

_Comment by @zanieb on 2025-02-08 17:42_

@nbbaier Do you have a justification? When you do `uv init --package` we create a `<package>` entrypoint so it's `uv run <package>`. Why not have `uv run <package>.py` for parity? Why is `uv run main.py` better?

---

_Comment by @zanieb on 2025-02-08 17:45_

Similarly, `<package>:main` may make more sense as a module / function relationship than `main:main`.

An example counter-point would be `cargo new` does `main.rs`

```
❯ cat example/src/main.rs
fn main() {
    println!("Hello, world!");
}
```

---

_Comment by @Ravencentric on 2025-02-08 21:05_

Wouldn't the equivalent of cargo's `src/main.rs` be `src/foo/__main__.py`? That would also then allow `python -m foo` (I guess similar to `cargo run`) to work.

---

_Comment by @zanieb on 2025-02-08 23:34_

@Ravencentric that layout requires a build backend, but is ~what we do with `uv init --package`. If we created `<package>.py`, that would allow `python -m <package>`. For example, this works today

```
❯ uv init example
Initialized project `example` at `/Users/zb/workspace/uv/example`
❯ cd example
❯ uv run -m hello
Using CPython 3.12.6
Creating virtual environment at: .venv
Hello from example!
``` 

---

_Comment by @nbbaier on 2025-02-11 23:40_

> [<img alt="" width="16" height="16" src="https://avatars.githubusercontent.com/u/12950157?u=bd276937ff436875a00278cc05c25107a592ecf2&amp;v=4&amp;size=80">@nbbaier](https://github.com/nbbaier?rgh-link-date=2025-02-08T17%3A42%3A33.000Z) Do you have a justification? When you do `uv init --package` we create a `<package>` entrypoint so it's `uv run <package>`. Why not have `uv run <package>.py` for parity? Why is `uv run main.py` better?

I honestly don't have much of a justification, to be honest. I'm mostly work with JS/TS, so I'm not used to my main entry points being named `<package>.ts`, but a consistent `<index>.ts`. 

---

_Referenced in [astral-sh/uv#11485](../../astral-sh/uv/pulls/11485.md) on 2025-02-13 16:51_

---

_Comment by @Gankra on 2025-02-13 22:06_

Fixed in #10369 

---

_Closed by @Gankra on 2025-02-13 22:06_

---
