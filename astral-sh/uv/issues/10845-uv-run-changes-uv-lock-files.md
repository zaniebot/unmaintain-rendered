---
number: 10845
title: "`uv run` changes uv.lock files"
type: issue
state: closed
author: jaapz
labels:
  - question
assignees: []
created_at: 2025-01-22T08:28:30Z
updated_at: 2025-07-23T09:43:51Z
url: https://github.com/astral-sh/uv/issues/10845
synced_at: 2026-01-10T01:24:58Z
---

# `uv run` changes uv.lock files

---

_Issue opened by @jaapz on 2025-01-22 08:28_

When running a command through `uv run`, sometimes the `uv.lock` file gets changed. This is a bit annoying when the `uv.lock` file is in a git repository, as it introduces unrelated changes to the lock file to merge requests or commits (or you have to undo the change or explicitly not commit it...).

I know about the `--frozen`  argument, which helps (although it might do with a shortcut argument).

However doesn't it make more sense for `uv run` not to modify any files unless explicitly allowed, instead of the other way around? For example to instruct `uv` to change dependencies in the lockfile, you need to explicitly tell `uv` to do so. But `uv run` just changes files as a side-effect of whatever command you actually want to run.

The changes done by `uv run` also always seem quite insignificant, and do not seem to change how the lockfile seems to be interpreted.

As an example, when I run `uv run` in my ansible playbooks repository it now changes the `uv.lock` file with the following diff:

```
diff --git a/playbooks/uv.lock b/playbooks/uv.lock
index c138ac2dd4..74438e6462 100644
--- a/playbooks/uv.lock
+++ b/playbooks/uv.lock
@@ -92,7 +92,6 @@ wheels = [

 [[package]]
 name = "my-playbooks"
-version = "0.0.0"
 source = { virtual = "." }
 dependencies = [
     { name = "ansible" },
```

So now every time I run a playbook, the `uv.lock` changes and my repository is dirty.

---

_Comment by @konstin on 2025-01-22 12:28_

Can you share a minimal reproducible example, following https://github.com/astral-sh/uv/issues/9452, as well as python version, your platform and your uv version?

---

_Label `needs-mre` added by @konstin on 2025-01-22 12:28_

---

_Comment by @charliermarsh on 2025-01-22 13:30_

Your problem in this case is that you’re using an inconsistent uv version.

---

_Comment by @jaapz on 2025-01-23 09:41_

Yes, we use different `uv` versions across developers. For something as quickly developed as `uv` that is something you can't really avoid I guess?

I am currently on 0.5.20, which I updated to only a week ago and already there are 3 new versions. Don't get me wrong, that's great and I love the momentum you guys have, `uv` is an amazing tool.

But I'm just trying to raise the point that `uv run` by default having as side effect that it can change files. For me it makes more sense that file changes are only expected when explicitly asked for (for example when running `uv lock`, you expect the lockfile to change).

With `uv run`, I expect a command to be run in the right environment, but changing files was a surprise to me.

---

_Label `needs-mre` removed by @konstin on 2025-01-23 10:12_

---

_Label `question` added by @konstin on 2025-01-23 10:12_

---

_Comment by @konstin on 2025-01-23 10:12_

You can make `uv run` not touch the lockfile by using `uv run --frozen` or setting `UV_FROZEN=1`.

The idea behind `uv run` is that it runs your code with the declared dependencies (`pyproject.toml` or inline script dependencies). If you change pyproject.toml, we'll update the lockfile and venv for you in background. We try to preserve the versions in `uv.lock` to avoid churn and increase consistency, but in cases like this where we changed the lockfile format to better accommodate dynamically versioned projects it can give an odd diff. With the `--frozen` and `--locked` options you can toggle the behavior to avoid accidental lockfile modifications.

---

_Comment by @jaapz on 2025-01-23 15:24_

I am aware of the `--frozen` argument (as I mentioned in the original report). In my mind however, it makes more sense for the behaviour of `--frozen` to be the default behavior of `uv run` - I expected `uv run` just to "run" my command, and not have any side effects changing files.

I understand this is a deliberate design decision, but it was something that slightly annoyed me, if it is something you are not likely to change then this issue can be closed.

---

_Closed by @konstin on 2025-01-23 15:51_

---

_Comment by @zanieb on 2025-01-23 15:56_

It's a big value proposition of `uv run` to ensure things are up to date. Once we add support for configuring options per-command, you could toggle the default behavior. Similarly, if you have a strong preference for this I'd recommend just setting up an alias that includes `--frozen`.

---

_Referenced in [GridTools/gt4py#1841](../../GridTools/gt4py/pulls/1841.md) on 2025-01-31 08:26_

---

_Comment by @silverwind on 2025-07-16 11:08_

> Once we add support for configuring options per-command

Maybe a option in `pyproject.toml` to always have `run` run with `--frozen`:

```toml
[tool.uv]
commandOptions = {run = ["--frozen"]}
```

But imho, `run` should never touch the lockfile, its not an action that's supposed to change the dependencies.

---

_Referenced in [go-gitea/gitea#35097](../../go-gitea/gitea/pulls/35097.md) on 2025-07-16 12:20_

---

_Comment by @UmaisZahid on 2025-07-23 09:43_

I agree with the comments above, this is a counter-intuitive aspect of the API. I think most people would assume `uv run` to be a stateless op for the underlying venv.

Something like the behaviour of `--locked` should probably be the default, so that an error is raised if your `uv.lock` is out of sync with your `pyproject.toml`, with the error message informing the user that they should update explicitly with `uv sync` or `uv run` with `--frozen` if they want to proceed anyway.  

---

_Referenced in [perrygeo/python-rasterstats#309](../../perrygeo/python-rasterstats/pulls/309.md) on 2025-09-02 18:31_

---

_Referenced in [ecolabdata/ecospheres-universe#18](../../ecolabdata/ecospheres-universe/pulls/18.md) on 2025-10-20 12:56_

---
