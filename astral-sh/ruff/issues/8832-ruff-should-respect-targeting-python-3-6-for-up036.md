```yaml
number: 8832
title: ruff should respect targeting Python 3.6 for UP036
type: issue
state: open
author: ktbarrett
labels:
  - needs-decision
assignees: []
created_at: 2023-11-24T14:52:10Z
updated_at: 2024-08-18T05:00:50Z
url: https://github.com/astral-sh/ruff/issues/8832
synced_at: 2026-01-10T11:09:51Z
```

# ruff should respect targeting Python 3.6 for UP036

---

_Issue opened by @ktbarrett on 2023-11-24 14:52_

My project ([cocotb](https://github.com/cocotb/cocotb)) will continue to support Python 3.6 for the foreseeable future. However ruff doesn't seem to respect this. With the following in pyporject.toml:

```toml
[project]
requires-python = ">=3.6"
```

And this code in my project:

```python
# On python 3.7 onwards, `dict` is guaranteed to preserve insertion order.
# Since `OrderedDict` is a little slower that `dict`, we prefer the latter
# when possible.
if sys.version_info[:2] >= (3, 7): 
    insertion_ordered_dict = dict
else:
    import collections

    insertion_ordered_dict = collections.OrderedDict
```

I get a UP036 error (`Version block is outdated for minimum Python version`) and `--unsafe-fixes` attempts to remove the version ifdef. Clearly ruff is internally bumping the version to python 3.7 and then complaining about this ifdef rather than respecting the *actual* version. I don't much care if Python 3.6 gets no official support, but this isn't a very good behavior =/

---

_Comment by @charliermarsh on 2023-11-24 15:52_

I'm honestly not sure what the right behavior is here! It's good, at least, that these are gated behind `--unsafe-fixes`, since users are at least required to opt-in to explicit, unsafe changes. But I agree that we can do better.

It would be straightforward to either (1) log a warning if you're using an unsupported `requires-python` range, or (2) throw an error. I'm not sure that we'd want to go through and audit all rules to determine whether they require Python 3.7 or above though -- I mean, we definitely _could_, I just don't know if we'd prioritize it.

Any opinion on the behavior you'd _expect_, assuming Ruff doesn't support Python 3.6?


---

_Label `needs-decision` added by @charliermarsh on 2023-11-24 15:53_

---

_Comment by @ktbarrett on 2023-11-24 16:28_

The output of ruff assuming 3.7 works as is in 3.6 for us. I'd be happy if there was at least "unofficial" support for 3.6. As in you can give 3.6 as the version, ruff respects it, but maybe there isn't the full range of checks there should be. This would be accompanied by a "consider all fixes unsafe unless proven otherwise" warning. Existing rules that are gated to version 3.7 could be brought back to 3.6 until proven otherwise by a user.

Personally I don't care about anything before 3.6. The only reason I care about 3.6 is because it's the standard Python version on RHEL8, which a lot of our users are stuck on, and it's at least being unofficially supported by Red Hat. Everything before that can and should be damned.

---

_Comment by @Avasam on 2024-08-18 04:57_

My two cents: I don't think rules need to have consideration for EOL Python past a certain point. But `UP036` doesn't need special consideration, it should be able to just compare the number configured against the number found in code.

Giving a warning for finding a selected version outside the supported range is probably fine. It might be an "annoying message", but only for users stuck in specific situations.

---
