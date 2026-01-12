```yaml
number: 3859
title: support flake8-fixme
type: issue
state: closed
author: finswimmer
labels:
  - plugin
assignees: []
created_at: 2023-04-03T09:31:42Z
updated_at: 2023-06-02T06:18:48Z
url: https://github.com/astral-sh/ruff/issues/3859
synced_at: 2026-01-12T15:54:44Z
```

# support flake8-fixme

---

_@finswimmer_

Hey,

until know I could find rules for all my flake8 plugins or at least a feature request to support those.

The only one I'm missing is [flake8-fixme](https://pypi.org/project/flake8-fixme/). 

Would be great to see this supported :)

fin swimmer


---

_Label `plugin` added by @charliermarsh on 2023-04-03 15:21_

---

_Comment by @edgarrmondragon on 2023-04-03 15:31_

Maybe https://github.com/orsinium-labs/flake8-todos would be preferred?

It seems more comprehensive

<blockquote>

+ **T001**: use TODO instead of FIXME (or BUG) for consistency.
+ **T002**: add author into TODO ([motivation](https://dave.cheney.net/practical-go/presentations/qcon-china.html#_dont_comment_bad_code_rewrite_it)).
+ **T003**: add link on issue into TODO.
+ **T004**: missed colon in TODO.
+ **T005**: missed text in TODO.
+ **T006**: write TODO instead of ToDo (use upper case).
+ **T007**: missed space after colon in TODO.

</blockquote>


---

_Comment by @finswimmer on 2023-04-03 16:05_

> Maybe https://github.com/orsinium-labs/flake8-todos would be preferred?

The purpose if `flake8-todos` seems to another one, compared to `flake8-fixme`. The later one ensures that there are no `TODO`s, `FIXME` etc. in the code base. And that's the functionality I like to have in `ruff` :smiley: 

---

_Comment by @edgarrmondragon on 2023-04-03 20:54_

You're right, the two plugins address different problems! I'll create a separate issue for my suggestion.

---

_Comment by @evanrittenhouse on 2023-04-20 00:31_

@charliermarsh can you please assign this to me? Once #3921 lands, I'll be able to refactor some of the `TODO` detection out to a common location - when _that's_ done, it'll be pretty trivial to clear this as well.

---

_Assigned to @evanrittenhouse by @MichaReiser on 2023-04-20 04:36_

---

_Comment by @evanrittenhouse on 2023-05-22 02:05_

The fix I have in mind depends on [this PR](https://github.com/charliermarsh/ruff/pull/4558) being merged so I can rebase on top of it

---

_Closed by @MichaReiser on 2023-06-02 06:18_

---
