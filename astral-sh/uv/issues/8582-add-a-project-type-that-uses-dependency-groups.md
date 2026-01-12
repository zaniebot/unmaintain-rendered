```yaml
number: 8582
title: "Add a project type that uses `[dependency-groups]` only and doesn't define a `[project]` table? Sync `dependency-groups.main` by default?"
type: issue
state: open
author: matterhorn103
labels: []
assignees: []
created_at: 2024-10-25T23:43:26Z
updated_at: 2025-09-19T01:06:10Z
url: https://github.com/astral-sh/uv/issues/8582
synced_at: 2026-01-12T15:59:29Z
```

# Add a project type that uses `[dependency-groups]` only and doesn't define a `[project]` table? Sync `dependency-groups.main` by default?

---

_@matterhorn103_

Amazing to see PEP-735 so fast! And cool of you to adopt it immediately for dev dependencies. You are doing great things 🙂 

One of the motivating use-cases for the PEP was science/data science projects that need to specify dependencies but aren't packages. One reason being that a `[project]` table needs a name and a version which can is a bit weird in that context. Another was the attitude of some of the Python devs that `project.dependencies]` should only be used by things that get packaged and not as an alternative to `requirements.txt`.

The PEP [envisages](https://peps.python.org/pep-0735/#data-science-projects) for this use-case that the `pyproject.toml` file wouldn't contain a `[project]` table at all but instead just a `[dependency-groups]` one containing a `main` group.

I'm curious - have you guys considered supporting this exact scenario? By this I mean doing the following:

1. Syncing a `dependency-groups.main` by default if present, i.e. special casing it in the same way as you have decided to do for `dependency-groups.dev` (which is a nice move by the way)

2. Adding a dependency to `dependency-groups.main` if `uv add` is run on a project whose `pyproject.toml` doesn't contain a `[project]` table

3. Creating a `pyproject.toml` that matches this setup for some new kind of project distinct from `--app`, `--lib`, and `--package`

This new project type would also be very much applicable to e.g. a folder of random scripts where the user wants to maintain a common set of installed packages and nothing more (this is something I'd use it for, on top of for scientific projects). At the moment this is still best managed  using `uv venv` and `uv pip`.

I don't know what this new project type would be called. "Workspace" would have been fitting but now has a different meaning. "Environment" is honestly not wrong as a way of describing the concept but has so much baggage (`uvenv` anybody? Lol). Maybe a "directory". My best suggestion would be "minimal" or "bare", the latter of which I particularly like. But no idea really.

The closest project type currently is `--app` but neither its behaviour nor its name quite feel like a good fit for the science use-case or the other I mentioned.  So this would help with that too perhaps.

If you were to implement something like this, I also think it'd be worth making "bare" the default project type of `uv init` without flags, as I think the people who are most likely to want this kind of minimal behaviour are also probably the ones least knowledgeable about Python project management. But that's a separate point tbh.

---

_Assigned to @zanieb by @zanieb on 2024-10-25 23:47_

---

_Comment by @zanieb on 2024-10-25 23:48_

I'll return to this, but in brief we're pretty interested in supporting this use case and, more generally, we'll be able to support dependency groups without a `[project]` section soon.

---

_Comment by @vlcinsky on 2024-10-26 21:24_

The PEP735 names this type of project "non-package". But this sounds too close to already taken "--no-package" option.

I like the `--bare` proposal.

Btw: I am amazed by the speed it resolves, installs and syncs the packages. It is so fast it is hard to believe it did anything.





---

_Comment by @charliermarsh on 2024-10-28 12:47_

You can already do something like this with:

```toml
[dependency-groups]
test = ["pytest"]

[tool.uv.workspace]  # Must be present, but can be empty.
```

But that structure (no `[project]` table) is considered legacy in uv right now.


---

_Comment by @bluss on 2024-10-28 14:15_

PEP735 talks about non-package, but there's no way to actually designate something as that, is there? (In PEP terms). (Mainly thinking about how no build-system == build me with setuptools, so everything with a pyproject.toml becomes a buildable.)

---

_Comment by @zanieb on 2024-10-28 14:27_

As far as the PEP is concerned, you designate something as non-packaged by omitting a `[project]` table entirely.

---

_Comment by @CarrotManMatt on 2025-09-18 22:51_

The ability to have a `pyproject.toml` file without a `[project]` section is now possible since uv 0.8.18 and the merge of #14113. This does not, however support locking an environment without a project and requires the use of `managed = false`.

---

_Comment by @zanieb on 2025-09-18 23:54_

Oh, `uv lock` should support that imo. I think that might just be an oversight as it's not in the list at https://github.com/astral-sh/uv/pull/14113#issuecomment-2981324244. cc @Gankra ?

---

_Comment by @matterhorn103 on 2025-09-19 01:06_

I've only just seen #14113, it's really nice to see you guys implement better support for "project-less" projects!

As far as I can tell, other than the mentioned `uv lock`, the one other thing that hasn't actually been implemented by #14113 is a way to actually create a project with `uv init` that is missing a `[project]` table? While `uv init --bare` was added a while back by #11192 (and I have indeed used it several times), it still creates a `[project]` table.

I'm curious to know if you guys have had any further ideas or concrete plans for the non-versioned/scientific project type. The "Note" box at the top of [the dependency docs page](https://docs.astral.sh/uv/concepts/projects/dependencies/) reads a little like you're waiting to see how adoption of `dependency-groups` pans out and whether the community instead just decides to ignore the original pyproject PEP and co-opt `[project.dependencies]` for non-packages too?

---
