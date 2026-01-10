```yaml
number: 1284
title: "Lots of RUF100 false positive for noqa: E501"
type: issue
state: closed
author: LefterisJP
labels:
  - bug
assignees: []
created_at: 2022-12-19T00:34:22Z
updated_at: 2022-12-19T09:46:08Z
url: https://github.com/astral-sh/ruff/issues/1284
synced_at: 2026-01-10T12:05:25Z
```

# Lots of RUF100 false positive for noqa: E501

---

_Issue opened by @LefterisJP on 2022-12-19 00:34_

I upgraded to `ruff==0.0.186` and started getting lots of `RUF100 Unused `noqa` directive for: E501` errors.

But they are all wrong. There is different kinds of categories but what I see is (assume 99 line length)

#### Directive at end of comment

```python
# this is a veryyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyyy long comment  # noqa: E501
```

#### Directive at end of multiline docstring
```python
    """This is an adjustment of web3's event data decoding to work with our code
    source: https://github.com/ethereum/web3.py/blob/ffe59daf10edc19ee5f05227b25bac8d090e8aa4/web3/_utils/events.py#L201

    Returns a tuple containing the decoded topic data and decoded log data.

    May raise:
    - DeserializationError if the abi string is invalid or abi or log topics/data do not match
    """  # noqa: E501
```

#### Directive at end of multiline string

Same as above, just for `"""` strings that are not function docstrings

---

_Comment by @charliermarsh on 2022-12-19 00:38_

Thanks, will take a look at this tonight.

---

_Label `bug` added by @charliermarsh on 2022-12-19 00:38_

---

_Assigned to @charliermarsh by @charliermarsh on 2022-12-19 00:38_

---

_Comment by @charliermarsh on 2022-12-19 00:40_

(The lines aren’t getting flagged as E501 though, right?)

---

_Closed by @charliermarsh on 2022-12-19 01:08_

---

_Comment by @charliermarsh on 2022-12-19 01:08_

Sorry, bad/lazy application of De Morgan's Law.

---

_Comment by @LefterisJP on 2022-12-19 09:46_

Heh that was fast. Thank you

---
