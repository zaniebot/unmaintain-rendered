```yaml
number: 2065
title: Confusing error message when a constraint is the cause of not finding a version
type: issue
state: open
author: notatallshaw
labels:
  - error messages
assignees: []
created_at: 2024-02-29T00:31:17Z
updated_at: 2024-03-04T23:51:30Z
url: https://github.com/astral-sh/uv/issues/2065
synced_at: 2026-01-10T05:40:32Z
```

# Confusing error message when a constraint is the cause of not finding a version

---

_Issue opened by @notatallshaw on 2024-02-29 00:31_

Environment: Linux Python 3.10.13 uv 0.1.12

Steps to reproduce:

1. `wget https://raw.githubusercontent.com/apache/airflow/constraints-2.3.4/constraints-3.10.txt`
2. `echo "apache-airflow==2.3.4" | uv pip compile - --constraint constraints-3.10.txt`

Error:

```
  × No solution found when resolving dependencies:
  ╰─▶ Because there is no version of exceptiongroup==1.0.0rc8 and cattrs==22.1.0 depends on exceptiongroup==1.0.0rc8, we can conclude that
      cattrs==22.1.0 cannot be used.
      And because apache-airflow==2.3.4 depends on cattrs==22.1.0 and you require apache-airflow==2.3.4, we can conclude that the requirements
      are unsatisfiable.

      hint: exceptiongroup was requested with a pre-release marker (e.g., exceptiongroup==1.0.0rc8), but pre-releases weren't enabled (try:
      `--prerelease=allow`)
```

This is confusing because `cattrs==22.1.0` doesn't depend only on `exceptiongroup==1.0.0rc8` and further `apache-airflow==2.3.4 ` does not depend only on `cattrs==22.1.0`, as you will see if you compie  `apache-airflow==2.3.4`  or `cattrs==22.1.0` or both of them together.

There is no mention that it is a user constraint which caused this failure, I beleive pip does mention when a top level user requirement or constraint caused the failure.



---

_Label `error messages` added by @charliermarsh on 2024-02-29 01:35_

---

_Comment by @zanieb on 2024-02-29 02:15_

Ah interesting, thanks for the report!

I'm not entirely sure why these constraints are not represented in the resolver (and subsequently the error report).

---

_Comment by @potiuk on 2024-03-02 08:25_

Interesting is that while the message is confusing, the hint is correct in this case - adding `--prerelease allow` fixes the problem and gives the information why `uv` concludes that it's cattrs.

* root cause is that airflow constraints for 2.3.4 contain exceptiongroup==1.0.0rc8 (the constraints were generated when there was no 1.0.0 and >= 1.0.0rc8 is coming from pytest actually, not cattrs) 

When you run the `pip compile` above with `--prerelease allow` - it will produce the right output and part of it is 

```
exceptiongroup==1.0.0rc8
    # via cattrs
```

* apparently `uv` **thinks** that it's cattrs that caused exceptiongroup problem, but in this case it's the constraint file that contributed the `prerelese` version.


---

_Comment by @notatallshaw on 2024-03-02 14:27_

Don't focus on the prerelease part of the error, I've reported that separately and it's already fixed: https://github.com/astral-sh/uv/issues/2063

After the next release of uv I'll see if I can come up with another example.

---

_Comment by @notatallshaw on 2024-03-04 23:51_

Okay, now there's a release with the other fix my steps to reproduce don't work. Here are a new set of steps to reproduce:

Environment: Linux Python 3.10.13 uv 0.1.14

1. `wget https://raw.githubusercontent.com/apache/airflow/constraints-2.3.4/constraints-3.10.txt`
2. Edit `constraints-3.10.txt` and replace `pendulum==2.1.2` with `pendulum==1.0.0` 
3. `echo "apache-airflow==2.3.4" | uv pip compile - --constraint constraints-3.10.txt`

You get the error:

```
$ echo "apache-airflow==2.3.4" | uv pip compile - --constraint constraints-3.10.txt
  × No solution found when resolving dependencies:
  ╰─▶ Because apache-airflow==2.3.4 depends on pendulum>=2.0 and apache-airflow==2.3.4 depends on pendulum==1.0.0, we can conclude that apache-airflow==2.3.4 cannot be used.
      And because you require apache-airflow==2.3.4, we can conclude that the requirements are unsatisfiable.
```


The confusing part is `apache-airflow==2.3.4 depends on pendulum==1.0.0` without mentioning that `pendulum==1.0.0` comes from the constraints. It would be more confusing if pendulum was a more distant transitive dependency.

---
