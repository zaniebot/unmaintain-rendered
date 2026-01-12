```yaml
number: 12382
title: Add a migration guide from pip to uv projects
type: pull_request
state: merged
author: zanieb
labels:
  - documentation
assignees: []
merged: true
base: main
head: zb/pip-wip
created_at: 2025-03-21T21:15:19Z
updated_at: 2025-07-03T01:05:05Z
url: https://github.com/astral-sh/uv/pull/12382
synced_at: 2026-01-12T16:10:15Z
```

# Add a migration guide from pip to uv projects

---

_@zanieb_

[Rendered](https://github.com/astral-sh/uv/blob/zb/pip-wip/docs/guides/migration/pip-to-project.md)

---

_Label `documentation` added by @zanieb on 2025-03-21 21:15_

---

_Review comment by @samypr100 on `docs/guides/migration/pip-to-project.md`:18 on 2025-03-23 23:54_

```suggestion
When using `pip` with `pip-tools`, requirements files specify both the dependencies for your project and lock
dependencies to a specific version. For example, if you require `fastapi` and `pydantic`, you'd
specify these in a `requirements.in` file:
```

---

_Review comment by @samypr100 on `docs/guides/migration/pip-to-project.md`:92 on 2025-03-23 23:56_

```suggestion
The requirements file format can only describe a single set of dependencies at once. This means if you have
```

---

_Review comment by @samypr100 on `docs/guides/migration/pip-to-project.md`:103 on 2025-03-23 23:57_

```suggestion
Notice the base requirements are included with `-r requirements.in` with version constraints specified by `-c requirements.txt`. This is common, as it ensures
```

---

_Review comment by @samypr100 on `docs/guides/migration/pip-to-project.md`:67 on 2025-03-23 23:57_

```suggestion
```text title="requirements.txt"
```

---

_Review comment by @samypr100 on `docs/guides/migration/pip-to-project.md`:59 on 2025-03-23 23:58_

```suggestion
`pip-compile` from `pip-tools`. The `requirements.txt` can also be generated using `pip freeze` on a pristine environment, by
```



---

_@samypr100 reviewed on 2025-03-24 00:00_

---

_@zanieb reviewed on 2025-04-02 21:12_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:103 on 2025-04-02 21:12_

I need to decide if it should just be `-r requirements.txt`

---

_@zanieb reviewed on 2025-04-02 21:13_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:59 on 2025-04-02 21:13_

A little scared to try to explain what a pristine environment is

---

_Marked ready for review by @zanieb on 2025-06-04 15:28_

---

_Comment by @oconnor663 on 2025-06-04 19:02_

My understanding is that the first half of the doc is saying "you're probably already doing something like this, let's flesh out an example to make sure we're all on the same page, before we suggest what you can do differently". But a lot of the individual lines in that section sound more like "here's what you should do", i.e.:

> when your project requires a package, you can install it with pip

> It's best practice to create a virtual environment for each project, to avoid mixing packages between them

Would it be better to use the "you might be doing XYZ" tense throughout the first half?

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:19 on 2025-06-04 19:54_

Oh this is about requirements-as-in-dependencies, and not requirements-for-following-this-guide? (A bit confusing, but maybe fine if you needed this guide)

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:19 on 2025-06-04 19:59_

This whole section is kinda fascinating to me, explaining how to use not-uv to the explain how to use uv...

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:21 on 2025-06-04 20:06_

```suggestion
`pip` supports installing a package your project needs, e.g.:
```
Not sure if this is exactly the right wording but I like that the next section leads with "pip supports..." emphasizing that we're talking about a pip workflow, and not like, that uv expects you to use pip.

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:82 on 2025-06-04 20:07_

Feel like we want to show `pip compile ...` invocation here, given everywhere else shows a relevant invocation?

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:10 on 2025-06-04 20:10_

Kinda a wild thought, but what if we had all the parts that show how to use `pip` instead explicitly invoke `uv pip` to kinda double-up?

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:115 on 2025-06-04 20:12_

This is what I'm talking about like with us using `uv pip ...` everywhere -- what if this was just ambiently demonstrated?

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:10 on 2025-06-04 20:13_

(The counter-argument to this is, it might lead to confusion about "wait i AM doing it with uv...")

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:164 on 2025-06-04 20:14_

This is great!

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:159 on 2025-06-04 20:20_

It would be nice to annotate this or include a line before/after saying that this is specifically expressing "the development dependencies are pytest (plus all the normal project dependencies)". I kinda skimmed over pytest and then was like "wait there's no actual dev-deps??"

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:262 on 2025-06-04 20:23_

Rad. Also I'm increasingly convinced that this is just the "using `uv pip` guide" after all.

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:293 on 2025-06-04 20:43_

(Just calling out this is a lingering TODO)

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:310 on 2025-06-04 20:45_

Given the previous sections it really feels like you want to write `uv sync` or `uv lock` or something at this point... or `uv pip install ...` lol

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:320 on 2025-06-04 20:50_

Hmm I kinda feel like these two sections want to be smooshed together.

"The preferred workflow with uv replaces all your `requirements.in` files with a single\* `pyproject.toml` and all your `requirements.txt` files with a single `uv.lock`." kinda thing

Or maybe before we immediately stumble into pyproject.tomls there should be a new higher level header indicating we've entered "the uv zone" with that summary.

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:325 on 2025-06-04 20:55_

```suggestion
preserve locked versions:
```

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:401 on 2025-06-04 20:57_

Rad.

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:430 on 2025-06-04 21:01_

Interesting, is this smart enough to diff out the dev part from the common dependencies? (Given you previously showed that requirements-dev.in files tend to include requirements.in)

If that diffing works, does this require you to `uv add requirements.in` first, and then `uv add requirements-dev.in --dev`, or will you get the same result regardless?

---

_Review comment by @Gankra on `docs/guides/migration/pip-to-project.md`:412 on 2025-06-04 21:03_

Great

---

_@Gankra reviewed on 2025-06-04 21:04_

---

_@zanieb reviewed on 2025-06-04 21:37_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:10 on 2025-06-04 21:37_

(Very afraid of the counter-argument, I did consider it :D)

---

_@zanieb reviewed on 2025-06-04 21:38_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:262 on 2025-06-04 21:38_

Yeah I might drop this, not sure how much to mix in the `uv pip` stuff. I'm going to have a separate page for that!

---

_@zanieb reviewed on 2025-06-04 21:39_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:430 on 2025-06-04 21:39_

> (Given you previously showed that requirements-dev.in files tend to include requirements.in)

Uhhh that might be a problem. I will test. Good catch!

---

_Review comment by @mkniewallner on `docs/guides/migration/pip-to-project.md`:313 on 2025-06-04 22:15_

```suggestion
The simplest way to import requirements is with `uv add`:
```

---

_Review comment by @mkniewallner on `docs/guides/migration/pip-to-project.md`:347 on 2025-06-04 22:15_

```suggestion
```console
```

---

_Review comment by @mkniewallner on `docs/guides/migration/pip-to-project.md`:327 on 2025-06-04 22:16_

```suggestion
```console
```

---

_Review comment by @mkniewallner on `docs/guides/migration/pip-to-project.md`:52 on 2025-06-04 22:41_

```suggestion
```python title="requirements.txt"
```

This enables proper highlighting of requirements files (like in https://github.com/astral-sh/uv/pull/6549).

---

_Review comment by @mkniewallner on `docs/guides/migration/pip-to-project.md`:333 on 2025-06-04 22:47_

Maybe it's a way less common use case, but would the same issue applies to Python versions too, for projects that need to support multiple versions of Python? And if so, do you think a specific section would be worth having?

---

_Review comment by @mkniewallner on `docs/guides/migration/pip-to-project.md`:380 on 2025-06-04 22:48_

```suggestion
have groups of dependencies for development purposes.
```

---

_Review comment by @mkniewallner on `docs/guides/migration/pip-to-project.md`:398 on 2025-06-04 22:49_

```suggestion
Unlike `pip`, uv is not centered around the concept of an "active" virtual environment. Instead, uv
```

---

_Review comment by @mkniewallner on `docs/guides/migration/index.md`:1 on 2025-06-04 22:53_

Shouldn't this be something like:
```suggestion
# Migration guides
```
?

---

_@mkniewallner reviewed on 2025-06-04 22:56_

---

_@zanieb reviewed on 2025-06-05 13:05_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:333 on 2025-06-05 13:05_

Yeah that problem would apply. I'm not sure 🤔. I don't know if I've seen many requirements files toggled on version. I can try to add a note of some sort.

---

_@zanieb reviewed on 2025-06-05 13:05_

---

_Review comment by @zanieb on `docs/guides/migration/index.md`:1 on 2025-06-05 13:05_

Yes! Thank you for catching my silly mistakes <3

---

_@zanieb reviewed on 2025-06-05 13:06_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:19 on 2025-06-05 13:06_

Yeah sort of wild but also... necessary I think?

I can consider tweaking this title.

---

_@zanieb reviewed on 2025-06-05 13:07_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:69 on 2025-06-05 13:07_

```suggestion
```python title="requirements.in"
```

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:118 on 2025-06-05 13:09_

```suggestion
```python title="requirements.txt"
```

---

_@zanieb reviewed on 2025-06-05 13:09_

---

_@zanieb reviewed on 2025-06-05 13:10_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:169 on 2025-06-05 13:10_

```suggestion
```python title="requirements-dev.txt"
```

---

_Review comment by @konstin on `docs/guides/migration/pip-to-project.md`:84 on 2025-06-11 14:14_

```suggestion
$ pip-compile requirements.in -o requirements.txt
```

---

_Review comment by @konstin on `docs/guides/migration/pip-to-project.md`:318 on 2025-06-11 14:18_

Not specifically here, but we should make a point about how the uv lockfile is always universal, and if only want your platform (let's the backend only runs on linux anyway), you can restrict it with `tool.uv.environments`. Not sure how common of a need this is and if we need to mention `tool.uv.environments`, but we should tie the loop about `requirements.txt` being platform specific.

---

_@konstin approved on 2025-06-11 14:35_

---

_Merged by @zanieb on 2025-07-02 17:25_

---

_Closed by @zanieb on 2025-07-02 17:25_

---

_Branch deleted on 2025-07-02 17:25_

---

_@eli-yip reviewed on 2025-07-03 00:38_

---

_Review comment by @eli-yip on `docs/guides/migration/pip-to-project.md`:327 on 2025-07-03 00:38_

It seems there is something missing here.

---

_@zanieb reviewed on 2025-07-03 01:00_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:327 on 2025-07-03 01:00_

Tragic, thanks!

---

_@zanieb reviewed on 2025-07-03 01:05_

---

_Review comment by @zanieb on `docs/guides/migration/pip-to-project.md`:327 on 2025-07-03 01:05_

Fixed

---
