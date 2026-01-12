```yaml
number: 14422
title: "Revert warnings for tilde version specifiers without a patch component (e.g., `~=X.y`)"
type: issue
state: open
author: zanieb
labels:
  - needs-decision
assignees: []
created_at: 2025-07-02T14:34:55Z
updated_at: 2025-07-07T16:34:22Z
url: https://github.com/astral-sh/uv/issues/14422
synced_at: 2026-01-12T16:01:48Z
```

# Revert warnings for tilde version specifiers without a patch component (e.g., `~=X.y`)

---

_@zanieb_

See comment at https://github.com/astral-sh/uv/pull/14008#issuecomment-3028097262

---

_Label `needs-decision` added by @zanieb on 2025-07-02 14:35_

---

_Comment by @zanieb on 2025-07-02 14:37_

As I mentioned at https://github.com/astral-sh/uv/pull/14335#issuecomment-3025583184, we are willing to revert the warning but we'll need feedback from more than one project that this is not helpful.



---

_Comment by @zanieb on 2025-07-02 15:31_

Some usage data...

- [`>=3...`: 83k hits](https://github.com/search?q=%2Frequires-python+%3D+%22%3E%3D3%28%5C.%5Cd%29*%22%24%2F&type=code)
- [`~=3.X`: 4k hits](https://github.com/search?q=%2Frequires-python+%3D+%22~%3D3%5C.%5Cd%2B%22%24%2F&type=code)
- [`~=3.X.y`: 1k hits](https://github.com/search?q=%2Frequires-python+%3D+%22~%3D3%5C.%5Cd%2B%5C.%5Cd%2B%22%24%2F&type=code)
- [`>=3..., <4`: 1k hits](https://github.com/search?q=%2Frequires-python+%3D+%22%3E%3D3%28%5C.%5Cd%29*%2C+%3F%3C4%22%24%2F&type=code)
- [`>=3..., <3...`: 100 hits](https://github.com/search?q=%2Frequires-python+%3D+%22%3E%3D3%28%5C.%5Cd%29*%2C+%3F%3C3%28%5C.%5Cd%29*%22%24%2F&type=code)

---

_Comment by @ashb on 2025-07-02 15:41_

[`~=3.X.0`: 780 hits](https://github.com/search?q=%2Frequires-python+%3D+%22%7E%3D3%5C.%5Cd%2B%5C.0%22%24%2F&type=code)

---

_Comment by @notatallshaw on 2025-07-02 15:51_

FYI I've made a more detailed comment on the PR that his is NOT a spec issue: https://github.com/astral-sh/uv/pull/14008#issuecomment-3028376538

---

_Comment by @zanieb on 2025-07-02 16:01_

Or rather, the underlying motivation for the warning "is" a spec issue because the semantics are underspecified and therefore the expected behavior is ambiguous.

---

_Comment by @ashb on 2025-07-02 16:16_

@notatallshaw It doesn't explicitly say it in the packaging metadata spec, but by linking to the Version specifier spec, I as a user, would be surprised if it doesn't also follow the meaning.

To give an extreme example of this, if I specified `>=3.6` and tool understood that but didn't also follow the sematics described in the Version Specifier section, it could chose to understand that as "anything less than 3.6" and they could still claim to follow the spec. Now, this is an extremely ridiculous strawman, but it is why I think the only sensible way of reading the metadata spec around `requires-python` linking to Version specifier is also the meaning of the comparison operators. 

---

_Comment by @notatallshaw on 2025-07-02 16:25_

I agree the specification is unfortunately worded, and the example is misleading, but it is what it is. The example is just that, an example, and it also says a tool MAY use this metadata field to restrict installation not even SHOULD, let alone MUST.

There are multiple practical problems with interpreting requires-python or Requires-Python to exactly follow the semantics of packaging Version specifiers, which you can read on the discussions I linked on the detailed post.

The one that brought me to this issue is pre-releaes, if you specify "`>=3.6`" that would not match 3.6 beta 1 as a packaging Version specifier. But everyone expects "`>=3.6`" to include Python alpha and beta releases. 

---

_Comment by @potiuk on 2025-07-02 21:52_

> The one that brought me to this issue is pre-releaes, if you specify ">=3.6" that would not match 3.6 beta 1 as a packaging Version specifier. But everyone expects ">=3.6" to include Python alpha and beta releases.

Which is very very very unfortunate and led Airflow to implement multiple workarounds - including pre-processing of pyproject.toml files before building pre-release distributions to add pre-release extensions when there are references to other distributions that could be built at the same time.

> I agree the specification is unfortunately worded, and the example is misleading, but it is what it is. The example is just that, an example, and it also says a tool MAY use this metadata field to restrict installation not even SHOULD, let alone MUST.

Yes. But here I am siding with Ash, I think while specification is indeed somewhat ambiguous I am more into a pragamatic approach that will not be too disruptive to the ecosystem.

Adding the warning will be **somewhat disruptive** but if it can be silenced easily, there is at least a chance to keep it as it is - and I think currently all the tools will interpret it same way as Ash did - following the semantics. And there is virtually no problem for ecosystem, because the warning is really targetted for "maintainers" of the project - not for users - and they can make conscious decision to keep it and silence the warning **for their project**.  So I'd say (also learning about those ambiguities) that keeping the warning for educational purpose and new projects is OK, while also allowing the maintainer to stay with their choices and silencing the warning makes it far less disruptive.

---

_Comment by @notatallshaw on 2025-07-02 22:49_

As I said in the PR I have no objections in uv doing what it thinks is right for it's users, whatever that choice.

The spec unambiguously does not define the semantics of Requires Python, so any argument about what semantics uv should implement based on the spec is flawed. And different tools HAVE made different choices on the semantics, so appealing a to the authority of the ecosystem is also flawed, just because you didn't hit an edge case until today doesn't mean other people weren't hitting different edge cases.

Personally I am making no judgement nor providing any advise on what uv should do here. That said, this discussion has given me some inspiration for writing a PEP on standardizing this, so I'll give writing a draft another try. 

---

_Comment by @potiuk on 2025-07-06 07:27_

> Personally I am making no judgement nor providing any advise on what uv should do here. That said, this discussion has given me some inspiration for writing a PEP on standardizing this, so I'll give writing a draft another try.

This is cool. However it will take time. In the meantime our logs in CI are pretty "interesting"

<img width="1615" height="927" alt="Image" src="https://github.com/user-attachments/assets/820d00f3-433a-4341-8d29-bc7efc5cb39f" />

Is it possible to at least implement a way to silence it for now ? 


---

_Comment by @potiuk on 2025-07-06 07:37_

It would really require from us to rebuild our automation that produces/verifies and modifies pyproject.toml  file - especially that we are adding back automated exclusions for Python 3.13 -> https://github.com/apache/airflow/pull/46891 where we have to exclude some providers from being available in Python 3.13 explicitly, and that means that we would have to change our automation. And I really like simplicity of `~=3.10`

@zanieb -> any chance of having that added soon? 

Actually that was one of my potential worries that relying on `uv` exclusively might lead to the situation that when we are using perfectly fine "standard" (even if ambiguous but widely accepted) ways of describing our project, we will be somehow forced to change things. 

This is a small thing of course, and we could adapt, but IMHO that's one of the first cases where `uv` is somewhat trying to fix ambiguous behaviour in well established standards where `pip` is perfectly fine with it. This was usually the other way round - i.e. PyPa and `pip` maintainers were deciding on which direction the ecosystem is going even if some behaviours are ambiguous or popular and eveyrone else followed.

Philosophically I see it as the first case (at least first time affecting me) where we are more-or-less pushed (without an easy opt-out) to change our - perfectly fine  - definition of `pyproject.toml`. Recognised and accepted by other tools in non-uv specific setting (actually one of the most comon settings out there).

I would very much want to avoid it in the future, and I think this sets a very bad precedent if it stays like that.

---

_Comment by @potiuk on 2025-07-06 07:51_

And just to be very clear - the "recommend" in the message there  is quite an understatement. The way how `uv` workflows work (and following all that @charliermarsh was talking about that this is how Astral and `uv` is trying to change the workflows of people) - having a non-silencable warning on every single `uv sync` and `uv run` is not a recommendation. It's a borderline forcing people to change.

Many of the warnings in `pip` are silencable, even though `pip` workflow is quite different - with `pip` even if you see the warning when you install things, you can easily ignore it. But with `uv` commands that you run maybe 50 times a day while working on your repo (when you have a lot of dependencies, and use `uv run`) such warning carries a much, much heavier weight if it is not easily silenceable by `pyproject.toml` configuration.   

---

_Comment by @potiuk on 2025-07-06 08:00_

This is how terminal output looks like now for every single airflow contributor that runs `uv sync`:

<img width="2056" height="987" alt="Image" src="https://github.com/user-attachments/assets/1c522b22-082d-4e9b-9b9d-10f08ec51626" />

---

_Comment by @zanieb on 2025-07-06 18:57_

At the very least, we can consolidate all of those into a single warning.

> that's one of the first cases where uv is somewhat trying to fix ambiguous behaviour in well established standards where pip is perfectly fine with it.

To be clear, this is far from the first case where we've deviated from pip or addressed ambiguous behavior and, as Damian has said a couple times now, there is not a well established standard here.

Regardless, we can add a way to opt-out of this warning.


---

_Comment by @potiuk on 2025-07-06 19:21_

> Regardless, we can add a way to opt-out of this warning.

That's all I am asking for. Consolidating makes no sense (mostly from coding point of view). I can imagine there will be projects that also have workspace with several distributions with similar approach - but being able to opt-out of the warning - either on per-workspace or per-distribution level will be more than enough to handle the fact of not having the warning in the first place.

---

_Comment by @potiuk on 2025-07-06 19:23_

Thanks for considering it!

---

_Comment by @potiuk on 2025-07-07 09:20_

Also FYI. I started the discussion on Airflow's public devlist https://lists.apache.org/thread/ofq38t0pzxs6n0jk38r6twl99glpdvbv -> explaining (I hope pretty objectively) the situation we are in and asked the community for their preference of `python-requires` form. While it **MAY** end with us adopting the `>=,<` pattern (depending on what the discussion will be and possibly consensus we will reach - my strong opinion is that it should not be the Astral team to "de-facto" force people to change the way how they are defining their requirements, because it's not Astral's job to influence PyPA standards in that regard.

If I misstated anything in the devlist - please feel free to join Airflow devlist and correct my statements, I could have been slightly non-objective there. When I was very young, I was bullied at school and I might be not too objective when it comes to someone forcing me (and my community) to do stuff, where deep inside I feel they have no right nor power to do so. This might explain my a bit strong reaction to this seemingly "small" thing - and maybe I am misinterpreting facts, so if you think any of the facts require correction - feel free to do so.

---

_Comment by @potiuk on 2025-07-07 13:20_

And update after starting the discussion in airflow devlist. @uranusjr who is one of the `pip` maintainers have mentioned in the discussion that this also - maybe not explicit but implicit view of `pip` maintainers (and maybe not all but some of them for sure) that using `~3.10` for `requires-python` makes very little sense. And that it even should not be replaced with `>=3.10,<4` but simply `>=3.10` is a way better choice - which I quite agree with.

So maybe we can turn it into a "community" approach - where also `pip` maintainers might say - "yeah ~3.10 in requires-python makes very little sense and raising a warning is good. 

I would be more than happy to follow it - if this is a "community view" and then I'd replace it  with `>=3.10` and if we all agree that this is a good idea, maybe the warning can even say that `>=3.X` is a better choice for requires-python?

WDYT?

The exact comments start here https://lists.apache.org/thread/fsm708rfh23no2jjkxz9vzcpphxgwtdy


---

_Comment by @zanieb on 2025-07-07 13:23_

We also are also generally very opposed to including upper bounds on the `requires-python` range.

See https://github.com/astral-sh/uv/issues/4022 and https://discuss.python.org/t/requires-python-upper-limits/12663

---

_Comment by @potiuk on 2025-07-07 14:07_

Yeah. The more I read it, the more "community" advice it seems to be to avoid upper-binding on requires-python (which ~= implies) - so I think I would be personally very happy to see the warning generated (but suggesting `>=` instead - not because of ambiguity but because upper-binding is not a recommended practice

---

_Comment by @potiuk on 2025-07-07 14:19_

And of course it should not only be triggered by ~=, but also by < used in `requires-python`

---

_Comment by @konstin on 2025-07-07 15:41_

Adding a few points from my end on `requires-python`:

* While `requires-python` technically matches against the exact (CPython) version, it really matches an API level. API level is in sense that the code needs least a Python 3.x level of syntax, semantics and std symbols. Alternative interpreters that are not CPython and don't follow CPython patch releases can also use `requires-python` since they know what language grammar and std they use. `requires-python` is used to also exclude faulty Python patch releases, but imho this is better solved by upgrading users to the latest patch release; patch releases are highly compatible and fix (only) high-impact bugs.
* Libraries should not put an upper bound on requires-python, neither a `<3.x` nor a `<4` (https://github.com/astral-sh/uv/issues/4022). For the `<3.x`, it's cause we don't know future compatibility, for extension modules such as numpy we can use the published wheels as compatibility marker and we ignore the upper bound anyway, coming from the experience poetry had with enforcing it. Given the Python 2 -> 3 transition, it's safe to assume there won't be any similarly breaking Python 4, so we don't need a `<4` bound. Informally, there seems to be a recommendation to use `>=3.x`, but there's no formal consensus or documentation for that.
* For applications, we (and others) allow an upper bound to determine on which Python versions the application should run. For example, you can set `requires-python == "3.11.*"` for an application that's only tested and deployed for Python 3.11. This is somewhat overlapping with pinning 3.11 in `.python_version`, but it's a stronger check.

(There obviously is an important point about how this is not widely known - I didn't know this before working on uv - and as usually, we don't want to break existing workflows)

---

_Comment by @potiuk on 2025-07-07 15:45_

Very interesting discussion resulting from that .. I've learned a lot - thanks !

We already pretty much decided to use `>=3.X` in Airflow as the result of this discussion - https://github.com/apache/airflow/pull/52980 with 5 maintainers already approving it .. So that alone was a very positive result of that warning :) 

Update: 6 approvals and merged.


---
