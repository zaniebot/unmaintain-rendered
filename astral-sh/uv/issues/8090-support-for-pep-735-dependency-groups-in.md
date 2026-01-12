```yaml
number: 8090
title: "Support for PEP 735: Dependency Groups in `pyproject.toml`"
type: issue
state: closed
author: zanieb
labels:
  - enhancement
  - compatibility
assignees: []
created_at: 2024-10-10T13:43:09Z
updated_at: 2024-10-25T20:46:50Z
url: https://github.com/astral-sh/uv/issues/8090
synced_at: 2026-01-12T15:59:19Z
```

# Support for PEP 735: Dependency Groups in `pyproject.toml`

---

_@zanieb_

This PEP was [just accepted](https://discuss.python.org/t/pep-735-dependency-groups-in-pyproject-toml/39233/312) — we plan to support it. We'll track it here.

The implementation can be tracked in https://github.com/astral-sh/uv/pull/8272

---

_Label `enhancement` added by @zanieb on 2024-10-10 13:43_

---

_Label `compatibility` added by @zanieb on 2024-10-10 13:43_

---

_Renamed from "Support for PEP 735" to "Support for PEP 735: Dependency Groups in `pyproject.toml`" by @zanieb on 2024-10-10 13:43_

---

_Comment by @NeilGirdhar on 2024-10-12 02:51_

I'm just curious, but will there be a "dev" dependency group that supplants
```toml
[tool.uv]
dev-dependencies = …
```
or is it up to the user to specify the `group=` when running `uv sync`?

---

_Comment by @henryiii on 2024-10-12 03:34_

Dependency-groups don't allow self-dependencies, so `dev-dependencies = ["<self>[test]"]` wouldn't be replaceable with dependency groups. Also, I think/hope that dependency groups are going to feed into a new environment and task system. :)

---

_Comment by @dschneiderch on 2024-10-12 14:54_

I noticed there is no mention of constraints in the pep. Dev-constraints is
super useful to me . Do you think this concept will continue to be
supported as 735 is implemented?

On Fri, Oct 11, 2024, 20:34 Henry Schreiner ***@***.***>
wrote:

> Dependency-groups don't allow self-dependencies, so dev-dependencies =
> ["<self>[test]"] wouldn't be replaceable with dependency groups. Also, I
> think/hope that dependency groups are going to feed into a new environment
> and task system. :)
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/8090#issuecomment-2408333294>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/ABY5SZNHP7RTAJMIFAHFSW3Z3CKEHAVCNFSM6AAAAABPW2Q3ESVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDIMBYGMZTGMRZGQ>
> .
> You are receiving this because you are subscribed to this thread.Message
> ID: ***@***.***>
>


---

_Comment by @zanieb on 2024-10-12 14:59_

> Dependency-groups don't allow self-dependencies, so dev-dependencies = ["<self>[test]"] wouldn't be replaceable with dependency groups.

Is this explicitly banned? I think `group = ["my-project[test]"]` might "just work" in uv because of how we treat package names that are shadowed by the project?

---

_Comment by @zanieb on 2024-10-12 15:00_

> will there be a "dev" dependency group that supplants ...

I'm not sure yet. This is a significant open question I must address before adding support.

---

_Comment by @zanieb on 2024-10-12 15:00_

> I noticed there is no mention of constraints in the pep...

I don't see any reason that we wouldn't apply constraints to the dependencies in the groups. Do you need a _separate_ set of constraints?

---

_Comment by @charliermarsh on 2024-10-12 15:03_

I only see [this section](https://peps.python.org/pep-0735/#why-not-support-a-dependency-group-which-includes-the-current-project), but I don't see any other discussion in the finalized PEP of self-references of project dependencies in dependency groups.

---

_Comment by @dschneiderch on 2024-10-12 19:55_

> > I noticed there is no mention of constraints in the pep...
> 
> I don't see any reason that we wouldn't apply constraints to the dependencies in the groups. Do you need a _separate_ set of constraints?

I am asking about the equivalent of `constraint-dependencies`, which I don't see addressed. I have a similar use case as discussed https://github.com/astral-sh/uv/issues/6518 . We use a managed airflow service. For dag development we only need a subset of the installed py dependencies and installing every py dependency is challenging and overkill because it requires several system dependencies. I'm slowly converting people to use uv, one of the motivations is having an easy, declarative approach to setting up a virtual environment for development. 

here is an example from pyproject.toml from a repo that contains dags:

```
[tool.uv]
dev-dependencies = [apache-airflow[gcp,beam]==2.9.1]
constraint-dependencies = [ long list of transient dependencies from cloud composer docs]
```
[cloud composer docs](https://cloud.google.com/composer/docs/concepts/versioning/composer-versions#images-composer-2)
I want to install what i need but I want to make sure I don't install a transient dependency with a different version from our prod environment.

1 difference from the other issue is we are not using the official airflow constraints file from the url (this is what i used to use before we moved to the managed airflow). Right now I am copying the list of installed packages into my pyproject.toml file. 

---

_Comment by @charliermarsh on 2024-10-12 21:02_

@dschneiderch -- Apologies if I'm misunderstanding but I think `constraint-dependencies` will just continue to be the right thing to use there.

---

_Comment by @dschneiderch on 2024-10-12 23:55_

Perfect! Wasn't sure if that metadata field was going to be redesigned
alongside dev-dependencies .

On Sat, Oct 12, 2024, 14:02 Charlie Marsh ***@***.***> wrote:

> @dschneiderch <https://github.com/dschneiderch> -- Apologies if I'm
> misunderstanding but I think constraint-dependencies will just continue
> to be the right thing to use there.
>
> —
> Reply to this email directly, view it on GitHub
> <https://github.com/astral-sh/uv/issues/8090#issuecomment-2408699012>, or
> unsubscribe
> <https://github.com/notifications/unsubscribe-auth/ABY5SZLLZYROQTHBXX5QSVLZ3GE7HAVCNFSM6AAAAABPW2Q3ESVHI2DSMVQWIX3LMV43OSLTON2WKQ3PNVWWK3TUHMZDIMBYGY4TSMBRGI>
> .
> You are receiving this because you were mentioned.Message ID:
> ***@***.***>
>


---

_Comment by @jankatins on 2024-10-15 21:54_

Duplicates https://github.com/astral-sh/uv/issues/5632 -> either this or the other one could probably be closed.

---

_Comment by @potiuk on 2024-10-16 21:12_

Would love to get it in  ... PR is coming I see :)

---

_Comment by @kaxil on 2024-10-16 21:14_

Same, looking forward to using it in the Airflow repo

---

_Comment by @zanieb on 2024-10-17 01:35_

@jankatins thanks! Embarrassing I keep creating duplicates of my own issues. I'll close them both when we add PEP 735 support.

---

_Comment by @potiuk on 2024-10-17 08:03_

> @jankatins thanks! Embarrassing I keep creating duplicates of my own issues. I'll close them both when we add PEP 735 support.

With the speed you are all working, occasional duplicate is not embarassing, it's a given :D. You are all doing great job.

---

_Closed by @zanieb on 2024-10-25 18:27_

---

_Comment by @zanieb on 2024-10-25 20:46_

As of [0.4.27](https://github.com/astral-sh/uv/releases/tag/0.4.27) we support this. Details in #8272.

---
