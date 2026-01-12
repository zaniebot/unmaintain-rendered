```yaml
number: 12044
title: "[FALSE NEGATIVE] \"S608 Possible SQL injection\""
type: issue
state: closed
author: mpolyakov-plutoflume
labels:
  - bug
  - good first issue
  - rule
assignees: []
created_at: 2024-06-26T10:59:27Z
updated_at: 2024-10-17T05:42:05Z
url: https://github.com/astral-sh/ruff/issues/12044
synced_at: 2026-01-12T15:54:51Z
```

# [FALSE NEGATIVE] "S608 Possible SQL injection"

---

_@mpolyakov-plutoflume_

Hello.
I'm very exited to migrate our codebase to ruff. However, while doing so I've noticed, that rule `S608` works different from the corresponding `B608`.
It only triggers if `SELECT` is on the same line.
So `SELECT * FROM {foo}.table` is an error and
```
SELECT *
FROM {foo}.table
```
Is not.

Kind regards, Mikhail

* B608
* S608
* sql injection



---

_Label `bug` added by @AlexWaygood on 2024-06-26 11:27_

---

_Label `rule` added by @AlexWaygood on 2024-06-26 11:27_

---

_Comment by @MichaReiser on 2024-06-26 11:30_

I just tried and the rule works as expected for triple-quoted f-strings. Any chance that you're using a line continuation token? 

https://play.ruff.rs/1c1c9715-2f69-48e1-9e31-f2edcef405ac

---

_Comment by @mpolyakov-plutoflume on 2024-06-26 15:39_

Than you for a quick reply.
I've added a couple of examples to https://play.ruff.rs/d3adeb3f-9657-4314-8d99-a45d426d6674

Basically, it does not work, when `SELECT` is on a new line or `SELECT` and `FROM` are on different lines.

---

_Comment by @bricker on 2024-08-12 19:49_

The rule seems to be sensitive to certain indentation. Here are a few examples where the rule is _not_ triggered but it should be:

```python
def sql():
    foo = "foo"

    f"""
SELECT {foo}
    FROM bar
    """
```

```python
def sql():
    foo = "foo"

    f"""
    SELECT
        {foo}
    FROM bar
    """
```

```python
def sql():
    foo = "foo"

    f"""
    SELECT {foo}
    FROM
        bar
    """
```

---

_Comment by @dhruvmanila on 2024-08-13 08:20_

> The rule seems to be sensitive to certain indentation.

Yeah, I think the regex is not considering the case where there could be "one or more whitespace characters":

https://github.com/astral-sh/ruff/blob/7027344dfce3c6ea13d96490e262551026357c1b/crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs#L13-L16

So, the `\s` should be `\s+` instead.

https://regex101.com/r/UPYDXC/2

---

_Comment by @MichaReiser on 2024-08-13 15:21_

@dhruvmanila do you plan to PR or should we mark this as a good first issue?

---

_Label `good first issue` added by @dhruvmanila on 2024-08-13 15:56_

---

_Comment by @DataEnggNerd on 2024-08-22 16:10_

@dhruvmanila Shall I pick this issue? 

---

_Comment by @dhruvmanila on 2024-08-23 05:00_

@DataEnggNerd For sure! Thanks

---

_Comment by @DataEnggNerd on 2024-08-23 20:00_

Need help here. 
I have added `\s+` before `from` but it is failing with examples given at https://github.com/astral-sh/ruff/issues/12044#issuecomment-2284789116 during compilation, but being detected at regex101. 

---

_Comment by @DataEnggNerd on 2024-09-14 13:18_

@MichaReiser I need a clarity here. As per my understanding based on the documentation [here](https://bandit.readthedocs.io/en/latest/plugins/b608_hardcoded_sql_expressions.html), every SQL statement which have string formatting should be reported with `S608`. Did I get it right? 

---

_Comment by @MichaReiser on 2024-09-16 09:22_

@DataEnggNerd Yes, the rule ideally flags all SQL statements with as few false positives as possible. 

The change mentioned in this issue is that we should [re-visit Ruff's regex](https://github.com/astral-sh/ruff/blob/90e5bc2bd95e15086a3af81589cc2a1af298b6b2/crates/ruff_linter/src/rules/flake8_bandit/rules/hardcoded_sql_expression.rs#L13-L16) to allow optional whitespace between the different keywords. It's a non-goal to change how the rule detects SQL expressions significantly. 

As reference, the Regex used by bandit 

https://github.com/PyCQA/bandit/blob/4ac55dfaaacf2083497831294b2dcd7e679f8428/bandit/plugins/injection_sql.py#L74-L80

---

_Comment by @DataEnggNerd on 2024-09-17 17:21_

@MichaReiser Yes, I have started with modifying the regex first and faced some problem with the regex. @dhruvmanila mentioned there is something missing beyond the regex(you can refer closer PR linked to the issue), so i started looking into this. Apologies it is making more time to implement at code. 


---

_Comment by @MichaReiser on 2024-09-18 07:11_

@DataEnggNerd 

Playing with it in Regex101, the following should work: `(?i)\b(select\s+.*\s+from)` https://regex101.com/r/T8ojD0/1

---

_Comment by @DataEnggNerd on 2024-09-22 03:19_

@MichaReiser  With the changes you suggested, along with the regex from bandit, unfortunately both are not helping. 
I have added details in the draft PR - https://github.com/astral-sh/ruff/pull/13445? 
Help me out if I am missing something. 

---

_Closed by @MichaReiser on 2024-10-17 05:42_

---
