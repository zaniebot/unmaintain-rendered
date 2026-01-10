---
number: 5459
title: "Python environment \"layering\" Rye vs Uv"
type: issue
state: closed
author: bluss
labels:
  - enhancement
assignees: []
created_at: 2024-07-25T18:33:04Z
updated_at: 2024-07-25T23:32:45Z
url: https://github.com/astral-sh/uv/issues/5459
synced_at: 2026-01-10T01:23:49Z
---

# Python environment "layering" Rye vs Uv

---

_Issue opened by @bluss on 2024-07-25 18:33_

Here we are comparing `rye run` and `uv run`, how they behave w.r.t the python virtual environment they are started from.

Steps to reproduce: https://github.com/bluss/uv-issue-5459

We have a process tree like this

- rye run foo   &nbsp;&nbsp;&nbsp; *This starts some bigger application foo*
  - rye run bar  &nbsp;&nbsp;&nbsp;  *Somewhere inside foo another application bar is started.*

Foo and bar live in different projects or in different packages. And we can do the same thing using `uv run` instead of `rye run`:

- uv run foo
  - uv run bar

.

- What happens in Rye is that it doesn't matter what virtualenv is active and so on, `foo` is executed in its project context with its own virtualenv. `bar` is executed the same way, with its own virtualenv.

- What happens in Uv is that `bar` is executed in a python environment that "layers" on top of `foo`. So what the python program in bar sees is that it can use a mix of packages from both foo and bar.

There's a different case for `uv run --with`, then we have:

- base project environment .venv
  - uv run --with's ephemeral environment

In this case the two environments also layer each other. I thought that was kind of cool, in this case. But not in the original case.


I think this strict project focus and separation is an important selling point of rye. It makes more robust applications possible.

I don't know why this happens, but one difference I can notice when comparing rye run python and uv run python in the same project, is that only uv sets PYTHONPATH.

---

_Comment by @charliermarsh on 2024-07-25 18:35_

> What happens in Uv is that bar is executed in a python environment that "layers" on top of foo. So what the python program in bar sees is that it can use a mix of packages from both foo and bar.

Hmm, I don't think it's intentional that `uv run bar` (is that being run from within `bar`?) is layered on top of `foo`. We should only be layering when `--with` is provided. Can you add an MRE?


---

_Comment by @bluss on 2024-07-25 19:02_

Sure, came up with this repository which I hope shows what happens https://github.com/bluss/uv-issue-5459

As one can guess, my real life use case for this is that foo=jupyterlab and bar=notebook kernel, which live in separate projects. But you could imagine that you would run an editor (spyder?) as the "foo" and any other developer tool as "bar".

---

_Comment by @bluss on 2024-07-25 19:45_

I would consider dropping setting PYTHONPATH entirely in uv run - (if this should be fixed! Don't know that..)

Nesting can possibly be done right using a [path file](https://docs.python.org/3/library/site.html) when using ephemeral environments? I don't have experience with that, but it sounds right - I don't know what's been attempted already(!)

---

_Comment by @charliermarsh on 2024-07-25 20:58_

Thank you! What do you see as the issue with modifying `PYTHONPATH`?

---

_Comment by @charliermarsh on 2024-07-25 21:03_

This is a very good example, I _think_ I consider it a bug.

---

_Comment by @bluss on 2024-07-25 21:06_

PYTHONPATH seems to be the factor that causes this.

PYTHONPATH shouldn't need to be set for using a virtual environment in the normal way. I can see why the environment layering (base project + ephemeral) would reach for using it, however, but maybe that can be avoided too. (I'm sure someone somewhere in history has lost a lot of time to virtual environment chaining and I haven't and don't know what works well..)

One can compare uv vs rye sys.path outcomes and notice that PYTHONPATH usage also moves the venv site-packages to be earlier than stdlib for uv, while it is not for rye. Does this mean that execution of a program - if it depends on shadowing one way or the other - could be subtly different for uv run thescript and .venv/bin/thescript?

---

_Comment by @charliermarsh on 2024-07-25 21:10_

Yeah, but we can fix the proximate issue by not "propagating" `PYTHONPATH` through nested calls:

```diff
diff --git a/crates/uv/src/commands/project/run.rs b/crates/uv/src/commands/project/run.rs
index 34348a8f0..97f8153d1 100644
--- a/crates/uv/src/commands/project/run.rs
+++ b/crates/uv/src/commands/project/run.rs
@@ -452,15 +452,11 @@ pub(crate) async fn run(
                     .flatten(),
             )
             .map(PathBuf::from)
-            .chain(
-                std::env::var_os("PYTHONPATH")
-                    .as_ref()
-                    .iter()
-                    .flat_map(std::env::split_paths),
-            ),
     )?;
     process.env("PYTHONPATH", new_python_path);
```

There may be other issues with it though. Gonna ask some more knowledge people.


---

_Comment by @bluss on 2024-07-25 21:23_

It would be cool to try to drop PYTHONPATH entirely and try the path configuration file route. Do you know why pythonpath is used at all? Is it in case the user has existing settings for PYTHONPATH?

---

_Comment by @charliermarsh on 2024-07-25 21:26_

I think I learned about it from [pip-run](https://github.com/jaraco/pip-run) and Jason knows a lot about this kind of manipulation.

---

_Comment by @charliermarsh on 2024-07-25 21:27_

Maybe a `.pth` file would work, though.

---

_Assigned to @charliermarsh by @charliermarsh on 2024-07-25 21:47_

---

_Comment by @charliermarsh on 2024-07-25 21:47_

I talked to @carljm and he walked me through how we might do it with `sitecustomize.py`, so I'll give that a shot.

---

_Label `enhancement` added by @charliermarsh on 2024-07-25 21:47_

---

_Referenced in [astral-sh/uv#5462](../../astral-sh/uv/pulls/5462.md) on 2024-07-25 23:19_

---

_Comment by @carljm on 2024-07-25 23:30_

To be clear, the reason for `sitecustomize.py` over a pth file is to get processing of pth files in the added directory, so editable installs in the base environment are still importable. 

---

_Closed by @charliermarsh on 2024-07-25 23:32_

---

_Closed by @charliermarsh on 2024-07-25 23:32_

---

_Referenced in [astral-sh/uv#7699](../../astral-sh/uv/pulls/7699.md) on 2024-09-26 16:16_

---
