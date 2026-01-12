```yaml
number: 2519
title: No build error message very verbose
type: issue
state: closed
author: konstin
labels:
  - error messages
assignees: []
created_at: 2024-03-18T19:31:29Z
updated_at: 2024-08-16T20:20:01Z
url: https://github.com/astral-sh/uv/issues/2519
synced_at: 2026-01-12T15:58:38Z
```

# No build error message very verbose

---

_@konstin_

The no build error messages are very verbose since they do no collapse versions. Example:

```
$ echo "pytest-pep8" | cargo run -q pip compile --no-build -
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of pytest-pep8 are available:
          pytest-pep8==0.5
          pytest-pep8==0.6
          pytest-pep8==0.7
          pytest-pep8==0.8
          pytest-pep8==0.9
          pytest-pep8==0.9.1
          pytest-pep8==1.0
          pytest-pep8==1.0.1
          pytest-pep8==1.0.2
          pytest-pep8==1.0.3
          pytest-pep8==1.0.4
          pytest-pep8==1.0.5
          pytest-pep8==1.0.6
      and pytest-pep8==0.5 is unusable because no wheels are usable and building from source is disabled, we can conclude that pytest-pep8<0.6 cannot be used.
      And because pytest-pep8==0.6 is unusable because no wheels are usable and building from source is disabled, we can conclude that pytest-pep8<0.7 cannot be used.
      And because pytest-pep8==0.7 is unusable because no wheels are usable and building from source is disabled and pytest-pep8==0.8 is unusable because no wheels
      are usable and building from source is disabled, we can conclude that pytest-pep8<0.9 cannot be used.
      And because pytest-pep8==0.9 is unusable because no wheels are usable and building from source is disabled and pytest-pep8==0.9.1 is unusable because no wheels
      are usable and building from source is disabled, we can conclude that pytest-pep8<1.0 cannot be used.
      And because pytest-pep8==1.0 is unusable because no wheels are usable and building from source is disabled and pytest-pep8==1.0.1 is unusable because no wheels
      are usable and building from source is disabled, we can conclude that pytest-pep8<1.0.2 cannot be used.
      And because pytest-pep8==1.0.2 is unusable because no wheels are usable and building from source is disabled and pytest-pep8==1.0.3 is unusable because no
      wheels are usable and building from source is disabled, we can conclude that pytest-pep8<1.0.4 cannot be used.
      And because pytest-pep8==1.0.4 is unusable because no wheels are usable and building from source is disabled and pytest-pep8==1.0.5 is unusable because no
      wheels are usable and building from source is disabled, we can conclude that pytest-pep8<1.0.6 cannot be used.
      And because pytest-pep8==1.0.6 is unusable because no wheels are usable and building from source is disabled and you require pytest-pep8, we can conclude that
      the requirements are unsatisfiable.
```

Different example:

```
$ echo "superset" | cargo run -q pip compile --no-build -
  × No solution found when resolving dependencies:
  ╰─▶ Because only the following versions of superset are available:
          superset<=0.26.3
          superset>=0.27.0,<=0.28.1
          superset>=0.30.0
      and superset==0.13.2 is unusable because no wheels are usable and building from source is disabled, we can conclude that any of:
          superset<0.14.0
          superset>0.26.3,<0.27.0
          superset>0.28.1,<0.30.0
       cannot be used.
      And because superset==0.14.0 is unusable because no wheels are usable and building from source is disabled and superset==0.14.1 is unusable because no wheels
      are usable and building from source is disabled, we can conclude that any of:
          superset<0.15.0
          superset>0.26.3,<0.27.0
          superset>0.28.1,<0.30.0
       cannot be used.
      And because superset==0.15.0 is unusable because no wheels are usable and building from source is disabled and superset==0.15.1 is unusable because no wheels
      are usable and building from source is disabled, we can conclude that any of:
          superset<0.15.3
          superset>0.26.3,<0.27.0
          superset>0.28.1,<0.30.0
       cannot be used.
[...many more cases]
      And because superset==0.26.2 is unusable because no wheels are usable and building from source is disabled and superset==0.26.3 is unusable because no wheels
      are usable and building from source is disabled, we can conclude that any of:
          superset<0.27.0
          superset>0.28.1,<0.30.0
       cannot be used.
      And because superset==0.27.0 is unusable because no wheels are usable and building from source is disabled and superset==0.28.0 is unusable because no wheels
      are usable and building from source is disabled, we can conclude that any of:
          superset<0.28.1
          superset>0.28.1,<0.30.0
       cannot be used.
      And because superset==0.28.1 is unusable because no wheels are usable and building from source is disabled and superset==0.30.0 is unusable because no wheels
      are usable and building from source is disabled, we can conclude that superset<0.30.1 cannot be used.
      And because superset==0.30.1 is unusable because no wheels are usable and building from source is disabled and you require superset, we can conclude that the
      requirements are unsatisfiable.

      hint: Pre-releases are available for superset in the requested range (e.g., 0.29.0rc7), but pre-releases weren't enabled (try: `--prerelease=allow`)
```


---

_Label `error messages` added by @konstin on 2024-03-18 19:31_

---

_Comment by @zanieb on 2024-03-18 19:36_

I'm not sure why these don't collapse, but I'd love to collapse them.

---

_Comment by @Eh2406 on 2024-03-20 15:25_

In the long run, I'd like to see https://github.com/pubgrub-rs/pubgrub/pull/163 but for `unavailable` constraints. This should probably happen after upstreaming the ability to customize the error message for each constraint. Which in turn is dependent on https://github.com/pubgrub-rs/pubgrub/pull/190

---

_Comment by @owenlamont on 2024-03-24 21:35_

I've just run into a similar issue with how verbose _uv pip compile_ is which was more than just a slight inconvenience since we run it as a docker build step and docker compose truncates the step output at 2 MiB with no work-around (maybe a work-around with buildx but not docker compose build - see https://github.com/docker/buildx/issues/484 and https://github.com/docker/for-mac/issues/6332 for context). I see the output of _uv pip compile_ for a while then just:  _[output clipped, log limit 2MiB reached]_

A dependency error happened after that but I spent hours looking for a Docker limit work-around to no avail and ended up having to hack the constraints to find out what the problem dependency was. I was checking the _uv pip compile_ arguments to see if there's a way to make it less verbose or only show errors but I saw no options, other than _--quiet_ which I didn't try because it sounded like that would remove all output completely and I wasn't sure if that included errors)

---

_Comment by @zanieb on 2024-03-24 21:53_

@owenlamont this issue is for a specific no solution error message case — it sounds like you're talking about something else. Please open a new issue with more details about the invocation you're using and the output you are seeing.

---

_Comment by @konstin on 2024-06-07 09:38_

Found another case with tensorflow and reported it upstream: https://github.com/pubgrub-rs/pubgrub/issues/232

---

_Closed by @zanieb on 2024-08-16 20:20_

---
