```yaml
number: 15658
title: "better error messages while loading configuration `extend`s"
type: pull_request
state: merged
author: purajit
labels:
  - cli
assignees: []
merged: true
base: main
head: 20250121-inheritance-failure-err-msg
created_at: 2025-01-21T22:41:43Z
updated_at: 2025-02-17T09:37:25Z
url: https://github.com/astral-sh/ruff/pull/15658
synced_at: 2026-01-12T15:55:52Z
```

# better error messages while loading configuration `extend`s

---

_@purajit_

Improves error messages during errors caused while extending a configuration using `extend`.

In the case of circular dependencies, it actually fixes the error too!

Old error messages:
```
# circular dependency - without even using pyproject.toml!
ruff failed
  Cause: Circular dependency detected in pyproject.toml

# file in "extends" doesn't exist
ruff failed
  Cause: Failed to read <path>/ruff4.toml
  Cause: No such file or directory (os error 2)
```

Now:
```
# circular dependency
ruff failed
  Cause: Circular dependency detected: <path>/ruff.toml -> <path>/ruff2.toml -> <path>/ruff3.toml -> <path>/ruff.toml

# circular dependency with a single file!
ruff failed
  Cause: Circular dependency detected: <path>/ruff.toml -> <path>/ruff.toml

# file in "extends" doesn't exist
ruff failed
  Cause: Failed to load last configuration in chain: <path>/ruff.toml -> <path>/ruff2.toml -> <path>/ruff3.toml -> <path>/ruff4.toml
  Cause: Failed to read <path>/ruff4.toml
  Cause: No such file or directory (os error 2)
```


---

_Comment by @github-actions[bot] on 2025-01-21 22:49_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Marked ready for review by @purajit on 2025-01-21 22:53_

---

_Label `cli` added by @MichaReiser on 2025-01-22 07:35_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:318 on 2025-01-22 07:44_

I find the wording here slightly confusing. It suggests that the path is inherited where it is the configuration. 

I also think we should track the entire `stack` because the message is now inprecise in case there's a inheritance chain like `a -> b -> c` because the message only mentions that `b` extends `c` without mentioning `a`. 

There are also other paths where this method returns an error (e.g. bail) where the inheritance chain will be missing. 

That's why I suggest making the following changes:

1. Store the path in the `stack` so that we get the entire inheritance chain instead of just the last 
2. move the body of the while loop into a function (or lambda) to ensure we attach the context to all errors returned in the while loop
3. Change the error to `Failed to load configuration `{path}`: error ({chain})` where `chain` is only shown if the stack isn't empty (and it shows `a -> b -> c)`

---

_@MichaReiser reviewed on 2025-01-22 07:44_

---

_@MichaReiser reviewed on 2025-01-22 07:45_

Nice, this is a great improvement. I've some suggestions on how we can improve the message further. It would probably also be good to add a CLI test for it, similar to what we have here

---

_@purajit reviewed on 2025-01-22 09:19_

---

_Review comment by @purajit on `crates/ruff_workspace/src/resolver.rs`:318 on 2025-01-22 09:19_

Hmm in the case of `a -> b -> c`, if `c` doesn't exist, why do we care about `a`? Fixing the `extends` clause in `b` should be enough, and providing the full chain might be confusing vs just providing the file that caused the error, and which file tried to extend it. The meaning of the direction of the arrow could be confusing too.

However, down to make the changes and my opinion isn't too strong here. I'll take a stab tomorrow.

---

_@MichaReiser reviewed on 2025-01-22 13:46_

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:318 on 2025-01-22 13:46_

That makes sense. I understood that the main motivation is that it's hard to understand why a file gets imported (how does Ruff end up with that given path and where can I change it). That's why I thought it's helpful if we show the entire inheritance chain over letting users guess how Ruff ended uploading that file, but it might go beyond of what the goal of this PR is. 

I'm open to use anything other than `->` if you consider its meaning ambigious. We could also use `a` extends `b`

---

_@purajit reviewed on 2025-01-23 20:11_

---

_Review comment by @purajit on `crates/ruff_workspace/src/resolver.rs`:318 on 2025-01-23 20:11_

Definitely makes sense! And like you said, would add context in several cases, including the cyclic one. Will try to have it updated soon!

---

_Review comment by @MichaReiser on `crates/ruff_workspace/src/resolver.rs`:318 on 2025-01-23 20:13_

Thank you. I'll be out next week, so it might be a while before someone does a re-review.

---

_@MichaReiser reviewed on 2025-01-23 20:13_

---

_@purajit reviewed on 2025-02-16 10:06_

---

_Review comment by @purajit on `crates/ruff_workspace/src/resolver.rs`:318 on 2025-02-16 10:06_

Sorry it took a while - updated the PR; since there are only two specific places where we return an error and they're slightly different, I just updated both. LMK if this makes sense.

I also added some tests, and the only issue with them right now is the final system error on Windows (vs unix). What do you think I should do for those? Add an insta filter for it? Conditionally compiled values?

---

_Renamed from "better error message when an inherited config path doesn't exist" to "better error messages while loading configuration `extend`s" by @purajit on 2025-02-16 10:31_

---

_Merged by @MichaReiser on 2025-02-17 09:35_

---

_Closed by @MichaReiser on 2025-02-17 09:35_

---
