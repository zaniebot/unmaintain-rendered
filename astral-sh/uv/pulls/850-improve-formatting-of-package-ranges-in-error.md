```yaml
number: 850
title: Improve formatting of package ranges in error messages
type: pull_request
state: closed
author: zanieb
labels:
  - error messages
assignees: []
draft: true
base: main
head: zb/range-fmt
created_at: 2024-01-09T16:55:16Z
updated_at: 2024-01-10T00:20:35Z
url: https://github.com/astral-sh/uv/pull/850
synced_at: 2026-01-12T16:04:14Z
```

# Improve formatting of package ranges in error messages

---

_@zanieb_

Requires https://github.com/zanieb/pubgrub/pull/18

- Wrap `Range` types in reporter for customizable display impl
- Remove spaces from display of ranges


---

_Comment by @zanieb on 2024-01-09 17:39_

I wonder if I should go forward with this as-is and track further improvements separately?

---

_@zanieb reviewed on 2024-01-09 17:40_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:67 on 2024-01-09 17:40_

I really don't like this pattern but don't know a better one.

---

_@zanieb reviewed on 2024-01-09 17:40_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:67 on 2024-01-09 17:40_

Could be nice to rename to `DisplayRange` or something instead

---

_Review comment by @BurntSushi on `crates/puffin-cli/tests/pip_compile.rs`:2229 on 2024-01-09 17:49_

Hmmm I think I actually like the previous one a lot better.

What about using more vertical space? So for example:

```
        ----- stderr -----
          × No solution found when resolving dependencies:
          ╰─▶ Because there are no versions satisfying all of the following:
                  attrs>20.3.0
                  attrs<21.2.0
              and root depends on:
                  attrs>20.3.0
                  attrs<21.2.0
```

---

_Review comment by @BurntSushi on `crates/puffin-cli/tests/pip_compile.rs`:2229 on 2024-01-09 17:49_

And basically the same idea for elsewhere. I assume these lists are variable in length, right? If so, I feel like using one per line "scales" better? Otherwise everything just get smushed together and it feels like my eyes are glazing over.

---

_Review comment by @BurntSushi on `crates/puffin-resolver/src/pubgrub/report.rs`:67 on 2024-01-09 17:51_

> I really don't like this pattern but don't know a better one.

Are you referring to the mix of `{package}`, `{}` and `{reason}`? I usually like to introduce an intermediate name via `let` before `format!` so that everything uses names.

---

_@BurntSushi reviewed on 2024-01-09 17:52_

---

_Review comment by @zanieb on `crates/puffin-resolver/src/pubgrub/report.rs`:67 on 2024-01-09 17:54_

I'm fine with introducing an intermediate name. I'm more bothered by the fact that you have to remember to make this cast in so many places.

---

_@zanieb reviewed on 2024-01-09 17:54_

---

_@zanieb reviewed on 2024-01-09 17:56_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:2229 on 2024-01-09 17:56_

Thanks for the feedback!

I agree multiple lines seems nice. It still leaves us with problems re and/or operators and precedence though.

---

_Label `error messages` added by @zanieb on 2024-01-09 18:15_

---

_@zanieb reviewed on 2024-01-09 18:19_

---

_Review comment by @zanieb on `crates/puffin-cli/tests/pip_compile.rs`:2229 on 2024-01-09 18:19_

Maybe we can use "any of the following" and "all of the following" for "or" and "and"

---

_Comment by @zanieb on 2024-01-10 00:20_

Closing in favor of #864 

---

_Closed by @zanieb on 2024-01-10 00:20_

---
