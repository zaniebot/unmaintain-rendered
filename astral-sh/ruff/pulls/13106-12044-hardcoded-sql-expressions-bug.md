```yaml
number: 13106
title: "#12044 hardcoded_sql_expressions bug"
type: pull_request
state: closed
author: DataEnggNerd
labels: []
assignees: []
draft: true
base: main
head: main
created_at: 2024-08-26T09:03:57Z
updated_at: 2024-08-29T03:19:55Z
url: https://github.com/astral-sh/ruff/pull/13106
synced_at: 2026-01-12T15:55:43Z
```

# #12044 hardcoded_sql_expressions bug

---

_@DataEnggNerd_

## Summary
This PR tried to fix #12044 raised for identifying all patterns which will be classified as prone to sql injections. 

## Test Plan
Created  `tests/test.py` will statements included in bug issue to check the implementation identifies all patterns. 


With the current change, only 2 scenarios are getting caught. Below is the log
```shell
cargo run -p ruff -- check --select S tests/test.py --no-cache
   Compiling ruff v0.6.2 (/Users/santhosh/Projects/personal/ruff/crates/ruff)
    Finished `dev` profile [unoptimized + debuginfo] target(s) in 4.87s
     Running `target/debug/ruff check --select S tests/test.py --no-cache`
tests/test.py:24:1: S608 Possible SQL injection vector through string-based query construction
   |
22 | foo = "foo"
23 |
24 | f"SELECT * FROM {foo}.table"
   | ^^^^^^^^^^^^^^^^^^^^^^^^^^^^ S608
   |

tests/test.py:27:1: S608 Possible SQL injection vector through string-based query construction
   |
27 | / f"""SELECT * FROM
28 | | {foo}.table
29 | | """
   | |___^ S608
30 |
31 |   f"""SELECT *
   |

Found 2 errors.
```

While when testing at online regex platforms, it is matching all the possible pattern in the test file. Below is the screenshot. 
![Screenshot 2024-08-26 at 2 32 56 PM](https://github.com/user-attachments/assets/2848e84f-c0a0-4a22-bd6e-1d4342dd839b)


---

_Review requested from @dhruvmanila by @dhruvmanila on 2024-08-26 09:24_

---

_Comment by @dhruvmanila on 2024-08-26 13:22_

Looking at this

---

_Comment by @dhruvmanila on 2024-08-26 13:33_

I think there's more to it. The initial `\b` suggests that something like `\nSELECT` won't be matched because the "select" should be a complete word i.e., there should be some "gap" before the "select" word (newline doesn't count).

---

_Comment by @dhruvmanila on 2024-08-26 13:34_

It makes sense to use word boundary because we don't want to match `deselect ...` here.

---

_Comment by @DataEnggNerd on 2024-08-26 13:36_

Shall I rewrite regex to match this rule? 

---

_Comment by @dhruvmanila on 2024-08-26 13:42_

I don't really have a good solution here, Rust regex engine doesn't support lookbehind other that could've been used probably.

---

_Comment by @dhruvmanila on 2024-08-26 13:44_

Or, if you're interested you can remove the regex completely and implement a custom parser ;)

---

_Comment by @dhruvmanila on 2024-08-26 13:50_

> Or, if you're interested you can remove the regex completely and implement a custom parser ;)

Here's our `noqa` comment parser: https://github.com/astral-sh/ruff/blob/99df859e20e9527cbfea0b7f88d40c5f195dc57d/crates/ruff_linter/src/noqa.rs#L423-L584

But, it's totally fine otherwise and a regex solution will be good as well.

---

_Comment by @DataEnggNerd on 2024-08-26 16:48_

@dhruvmanila Thanks for the suggestions. I will try implementing with Rust first. ✨ 

---

_Closed by @DataEnggNerd on 2024-08-26 16:48_

---

_Comment by @dhruvmanila on 2024-08-28 06:09_

> @dhruvmanila Thanks for the suggestions. I will try implementing with Rust first. ✨

Feel free to ask any questions either on GitHub or Discord.

---

_Comment by @DataEnggNerd on 2024-08-29 03:19_

> > @dhruvmanila Thanks for the suggestions. I will try implementing with Rust first. ✨
> 
> Feel free to ask any questions either on GitHub or Discord.

For sure! Will be starting by this weekend. 

---
