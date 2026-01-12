```yaml
number: 10369
title: "Create `main.py` instead of `hello.py` in `uv init`"
type: pull_request
state: merged
author: Gankra
labels:
  - enhancement
  - breaking
assignees: []
merged: true
base: tracking/060
head: gankra/apppy_juice
created_at: 2025-01-07T15:29:55Z
updated_at: 2025-05-16T06:31:30Z
url: https://github.com/astral-sh/uv/pull/10369
synced_at: 2026-01-12T16:09:15Z
```

# Create `main.py` instead of `hello.py` in `uv init`

---

_@Gankra_

Initially it seemed like `app.py` might be slightly more desirable but people seem to overwhelmingly favour `main.py` as a good "generic" name.

Fixes #7782


---

_Comment by @Gankra on 2025-01-07 15:31_

The only concern I have with this is it changes intro docs, so there will be an awkward period where people have older versions of uv that make `hello.py` but the guide says they make `app.py`. Dunno if we want to make the docs hedge for a bit or what.

---

_Comment by @zanieb on 2025-01-07 15:33_

> The only concern I have with this is it changes intro docs, so there will be an awkward period where people have older versions of uv that make `hello.py` but the guide says they make `app.py`. Dunno if we want to make the docs hedge for a bit or what.

I'd just go for it — you could add an admonition about older versions if you want though.



---

_Label `enhancement` added by @zanieb on 2025-01-07 15:33_

---

_Comment by @zanieb on 2025-01-07 15:35_

Arguably this is sort of breaking, though I'm not sure we consider `uv init` to be a typical programmatic interface. @charliermarsh thoughts?

If we decide it's breaking, we'll just hold it until 0.6.0 (for which there is a [milestone](https://github.com/astral-sh/uv/issues?q=is%3Aissue%20state%3Aopen%20milestone%3Av0.6.0) to track other breaking changes).

---

_Renamed from "rename hello.py to app.py" to "Create `app.py` instead of `hello.py` in `uv init`" by @Gankra on 2025-01-07 15:39_

---

_Comment by @charliermarsh on 2025-01-07 16:37_

It probably makes sense to defer this to v0.6. We could do a v0.6 in the near future though.

---

_Added to milestone `v0.6.0` by @zanieb on 2025-01-07 17:22_

---

_Label `breaking` added by @zanieb on 2025-01-07 17:22_

---

_Comment by @zanieb on 2025-01-07 17:22_

Sorry for giving you an unmergable task!

---

_Comment by @Bengt on 2025-01-24 12:12_

1. For reference,
  - [there are currently 50.700 `hello.py` files on GitHub](https://github.com/search?q=path%3Ahello.py&type=code),
  - [there are currently 132.000 `cli.py` files on GitHub](https://github.com/search?q=path%3Acli.py&type=code), and
  - [there are currently 175.000 `example.py` files on GitHub](https://github.com/search?q=path%3Aexample.py&type=code).

2. In Python, default file names vary a bit with the domain, and hence one's expectations might be shaped in various ways. `app.py` is indeed common, with [currently about 721.000 `app.py` files on GitHub](https://github.com/search?q=path%3Aapp.py&type=code).

3. Still, `main.py` is an even more common name. At the time of writing, [there are 2.2 million `main.py` files on GitHub](https://github.com/search?q=path%3Amain.py&type=code). Interestingly, this is exactly because 'main' is a commonly used name in the Python world, like `main()`, `__main__`, or `__main__.py`. To adhere to this pythonic naming scheme and avoid surprises, `main.py` should in my opinion be default in `uv init`.

4. I do not feel like there is one, universally accepted best practice for naming your first file. I think we would therefore need a command line argument for naming the default file. The command line option should also include a way to disable creating a default file altogether. Currently, one needs to advise `uv init && rm hello.py`, which seems unnecessarily convoluted and is not portable to PowerShell, so portable instructions require duplication. m(

5. The default file is handy if one starts from scratch, but cumbersome if there is an existing codebase. With an existing code base, the use case is to introduce uv to it, not build something new. In projects with preexisting code, uv would ideally detect the presence of a codebase and not create a default file. This should be equivalent to using the deactivation functionality mentioned in 4.

---

_Comment by @Gankra on 2025-01-24 14:14_

Wow, thanks for all the great insight! That all seems pretty agreeable.

---

_Comment by @zanieb on 2025-01-24 15:10_

We are already tracking (5) in https://github.com/astral-sh/uv/issues/6750

---

_Comment by @charliermarsh on 2025-02-08 17:53_

It seems like FastAPI generally recommends `main.py`, at least [here](https://fastapi.tiangolo.com/tutorial/first-steps/), though in my testing `fastpi dev` auto-detects both `main.py` and `app.py` (but not `hello.py`). Here's the full list: https://github.com/fastapi/fastapi-cli/blob/9a4741816dc288bbd931e693166117d98ee14dea/src/fastapi_cli/discover.py#L19-L26.

On the other hand, it looks like Flask auto-detects `app.py`, but not `main.py` (https://flask.palletsprojects.com/en/stable/cli/).


---

_Renamed from "Create `app.py` instead of `hello.py` in `uv init`" to "Create `main.py` instead of `hello.py` in `uv init`" by @Gankra on 2025-02-11 17:45_

---

_@zanieb reviewed on 2025-02-12 15:09_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:808 on 2025-02-12 15:09_

Presumably we should do this too? It's sort of breaking

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:808 on 2025-02-12 15:09_

(Separate PR presumably. #6750)

---

_@zanieb reviewed on 2025-02-12 15:09_

---

_@Gankra reviewed on 2025-02-12 16:39_

---

_Review comment by @Gankra on `crates/uv/src/commands/project/init.rs`:808 on 2025-02-12 16:39_

Wait so does that block this PR or just noticing it because it got poked?

---

_@zanieb reviewed on 2025-02-12 16:49_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/init.rs`:808 on 2025-02-12 16:49_

It doesn't block this no, but we should probably do it for v0.6.0?

---

_Merged by @zanieb on 2025-02-13 21:58_

---

_Closed by @zanieb on 2025-02-13 21:58_

---

_Branch deleted on 2025-02-13 21:58_

---

_Comment by @Kludex on 2025-02-18 11:45_

`main` is far better choice than any alternative. Good job.

---

_Comment by @zanieb on 2025-02-18 15:40_

:D thanks Marcelo

---

_Comment by @eisenhowerj on 2025-05-16 06:29_

Is it possible to have no _example_ file as the default?

It seems the purpose of `hello.py` or `main.py` is to make the "first time" experience easier to document, here: https://github.com/astral-sh/uv/blob/4aa72089a1b73c61feb956d2cf1c95344c9d7d4a/docs/guides/projects.md?plain=1#L39

What I propose (because I'm getting tired of delete/rename hello.py on every project I create): `uv init --getting-started`. Easy to document, easy to remove once you know what you're doing.



---
