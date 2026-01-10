```yaml
number: 4845
title: "ERA001: autofix removes valid comments that do not contain code or removes not everything"
type: issue
state: closed
author: saippuakauppias
labels: []
assignees: []
created_at: 2023-06-03T23:41:00Z
updated_at: 2025-04-19T19:55:57Z
url: https://github.com/astral-sh/ruff/issues/4845
synced_at: 2026-01-10T11:09:47Z
```

# ERA001: autofix removes valid comments that do not contain code or removes not everything

---

_Issue opened by @saippuakauppias on 2023-06-03 23:41_

Example:
```python
# Example of headers:
# X-token: <token_id>
# X-scope: no-scope
# End of comment about headers


# {
#    "some-key": {
#         '1': 1,
#         '2': 2',
#     }
# }


# some = 1
# awesome = 2
#
# foobar = 3
# barfoo = 4


# example code from IPython:
# In [20]: parse.parse_qsl('&l=55.123;123.1&count=3', keep_blank_values=1)
# Out[20]: [('l', '55.123'), ('123.1', ''), ('count', '3')]
# End of comment


# (пример, пример2) = (example1, example2)


# if not success:  # TODO: comment
#    errors.append('Ошибка: error')


# comment
# 1. [{пример первый}]
# 2. [[пример]]
# 3. [] - пусто


# Пример: {"description": "weight: "}


# comment about calculation
# 100 / 2 = 50
# end of comment


# comment:
# - tuple: ()
# - list: []
# - set: set()
# - dict: {}


# example of output:
# 'OK: message sent. Message-ID: 123456'


# MySQL: enum('positive', 'negative')


# query: lock + user.id
```

Ruff output:
```sh
$ ruff --fix --select ERA001 ruff_errors/ERA001.py
Found 24 errors (24 fixed, 0 remaining).
```

Contents of the file after fix:
```python
# Example of headers:
# X-token: <token_id>
# End of comment about headers


#    "some-key": {


#


# example code from IPython:
# End of comment




# if not success:  # TODO: comment


# comment
# 1. [{пример первый}]
# 3. [] - пусто




# comment about calculation
# end of comment


# comment:


# example of output:





```

---

_Comment by @charliermarsh on 2023-06-05 02:45_

I don't think we're in a position to improve this much, the logic itself is derived from eradicate, and I'd bet eradicate has the same or very similar behavior. Perhaps we should remove the autofix from this rule though. It seems correct to flag that there's some commented-out code here, but rely on users to remove it.

Alternatively, we could only autofix if _all_ lines in a comment block are identified as code.

---

_Comment by @dhruvmanila on 2023-06-05 04:06_

We could make this as a Suggested (`Applicability::Suggested`) fix.

```rust
    /// The fix may be what the user intended, but it is uncertain.
    /// The fix should result in valid code if it is applied.
    /// The fix can be applied with user opt-in.
```

---

_Comment by @zanieb on 2023-06-05 04:39_

I like

> Alternatively, we could only autofix if all lines in a comment block are identified as code.

Perhaps we could use `Suggested` in that case and `Manual` otherwise?

---

_Comment by @karolinepauls on 2023-06-15 08:45_

Another example: it thinks the comment is code even though `params` isn't a valid keyword
```
# params: abc
query = """
SELECT :abc;
"""
```

---

_Comment by @dhruvmanila on 2023-06-15 14:04_

> Another example: it thinks the comment is code even though `params` isn't a valid keyword
> 
> ```
> # params: abc
> query = """
> SELECT :abc;
> """
> ```


I believe this is considered as a valid code (`variable: annotation`):
```python
params: int
```


---

_Comment by @bodograumann on 2023-08-24 13:21_

I am absolutely in favor of not automatically removing these comments in any situation. Yes, commented-out code is usually a mistake, but you really don't want to loose actual comments accidentally.

Our example:
```
# foo and bar (JIRA-9999)
```

---

_Comment by @zanieb on 2023-08-24 14:01_

@bodograumann what is the benefit of enabling the ERA001 rule for you then?

---

_Comment by @bodograumann on 2023-08-24 14:35_

It will still warn me and usually correctly so, but I still need to decide myself whether a line needs to be removed.

---

_Comment by @zanieb on 2023-08-24 15:09_

We've updated the applicability of the fix here to "Manual" which will _never_ automatically apply once we merge #5119 — I don't think there's more to do here this issue will be resolved once applicability is respected by the CLI per https://github.com/astral-sh/ruff/issues/4185

---

_Comment by @jaap3 on 2023-08-30 10:49_

Was about to open a new issue, but I think this fits here:

While testing out ruff, I ran into some false positives for `ERA001` that confused me a bit. 

Some examples:

```python
# Keep this list sorted alphabetically
# (it is used in example)
```

```python
# Check if the ip address in either:
# local (loopback) or internal (private)
```

```python
##
# Client (consumers)
##

...

##
# Server (protocol)
##
```

All trigger `ERA001` even though they are not really code.

I had also hoped that code fragments that are part of a larger comment wouldn't trigger `ERA001`, yet all of these do:

```python
# XXX: Cannot use:
# self.example(foo, 'bar')
# because of issue #1337
```

```python
# XXX: Example category has a duplicate 'code' and is omitted
# EXAMPLE_CATEGORY = '1.0', 'Example Category'
```

```python
# Note: Admin site instance should be passed when calling
# ExampleAdminView.as_view(site=site)
```

```python
# main categories themselves are not part of the return value of
# get_subcategories()
```

This is all using `ruff 0.0.286`.

I've already marked `ERA001` as `unfixable` in my configuration file. I'm hoping it is possible to tweak the heuristics of `ERA001` a bit so it takes the surrounding context into account or something?

For now I'll just be working around this by rewording/reformatting these comments until they don't trigger anymore.

---

_Comment by @zanieb on 2023-08-30 14:52_

Yeah the ERA001 heuristics are very prone to false positives, unfortunately. I think there's room for improvement, but it's not entirely clear how.

```python
def foo():
    x = 1
    # add to x
    # x += 1
   return x
```

I think this should raise as an ERA001 violation but if we ignore comments that are part of a larger non-code context then we won't raise here. Maybe that's worth the false negative to reduce the false positives.
    

---

_Comment by @jaap3 on 2023-08-31 11:53_

This seems like a bug:

A single parenthesis triggers `ERA001`:

```sh
echo "# )" | ruff --select ERA001,RUF100 --stdin-filename test.py
test.py:1:1: ERA001 [*] Found commented-out code
```

Adding a `noqa` to that line triggers `RUF100`:

```sh
echo "# )  # noqa: ERA001" | ruff --select ERA001,RUF100 --stdin-filename test.py
test.py:1:6: RUF100 [*] Unused `noqa` directive (unused: `ERA001`)
```


---

_Comment by @charliermarsh on 2023-08-31 13:31_

@jaap3 - Funnily enough that specific case was reported recently and is fixed on `main`.

---

_Closed by @zanieb on 2023-10-06 03:41_

---

_Comment by @tusharsadhwani on 2024-06-27 12:25_

Is there scope for a PR for better algorithms, and a better set of false positive rules here?

It will deviate from the eradicate plugin, would that be okay or should it be a different rule entirely then?

---

_Comment by @charliermarsh on 2024-06-27 12:31_

Yeah definitely fine to fold in improvements even if it deviates from eradicate.

---

_Comment by @tusharsadhwani on 2024-06-27 12:40_

Self-documenting here for later.

Examples of false negatives:

```py
# if foo:

# elif bar:

# if (
#  x
# ):

# else:
```

Examples of false positives:

```py
# continue
# pragma: cover
# (something)
# O(1)
# this is O(1)
```

---

_Comment by @abhijith-404 on 2024-12-26 07:17_

```
#  Checking if the first default_path is there in api_paths if true then, return the action i.e create, read, update, delete

```

this is the comment I wrote, it contains `return` in this sentence only because of that, it is raising a warning

---

_Comment by @tusharsadhwani on 2024-12-26 07:25_

That should be fixable, let me do just that much for now.

---

_Comment by @webknjaz on 2025-04-19 19:55_

> I don't think we're in a position to improve this much, the logic itself is derived from eradicate, and I'd bet eradicate has the same or very similar behavior. Perhaps we should remove the autofix from this rule though. It seems correct to flag that there's some commented-out code here, but rely on users to remove it.
> 
> Alternatively, we could only autofix if _all_ lines in a comment block are identified as code.

@charliermarsh FYI flake8-eradicate has a `eradicate-whitelist-extend` setting (and a few more) that allow tweaking the regex used to get rid or false-positives: https://github.com/cherrypy/cheroot/blob/733cbd1/.flake8#L158-L160.

---
