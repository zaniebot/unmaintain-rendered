```yaml
number: 15757
title: "feat: implement --force flag for lock"
type: pull_request
state: open
author: 11happy
labels: []
assignees: []
base: main
head: force_flag
created_at: 2025-09-09T15:44:16Z
updated_at: 2025-10-12T10:19:10Z
url: https://github.com/astral-sh/uv/pull/15757
synced_at: 2026-01-10T06:36:15Z
```

# feat: implement --force flag for lock

---

_Pull request opened by @11happy on 2025-09-09 15:44_

<!--
Thank you for contributing to uv! To help us out with reviewing, please consider the following:

- Does this pull request include a summary of the change? (See below.)
- Does this pull request include a descriptive title?
- Does this pull request include references to any relevant issues?
-->

## Summary

<!-- What's the purpose of the change? What does it do, and why? -->
This PR fixes #15220 

## Test Plan

<!-- How was it tested? -->
I have tested the updated force flag working.

## CC
@zanieb 


---

_@zanieb reviewed on 2025-09-11 14:27_

---

_Review comment by @zanieb on `crates/uv/src/commands/project/lock.rs`:763 on 2025-09-11 14:27_

I think we still probably want to use the existing versions instead of ignoring the existing lockfile entirely? This might be kind of hard to teach though 🤔

I think this doesn't accomplish the goal from the issue.

---

_Comment by @11happy on 2025-09-13 11:51_

Hii @zanieb 
I see your point about my current implementation not addressing the core issue. To solve the issue , I am thinking to approach it like this -
For the `--force` flag approach:
I can set the ValidatedLock to Preferable/Versions to use existing versions but force a rewrite when `--force` flag is used. 

OR 


When validation succeeds and we reach this section:
```
Some(ValidatedLock::Satisfies(lock)) => {
    // Print the success message after completing resolution.
    logger.on_complete(lock.len(), start, printer)?;
    Ok(LockResult::Unchanged(lock))
}
```
Should I add format checking here? If there's a format mismatch (missing revision = 3, missing upload-time fields, etc.), 

How should I apprach format detection ?
I saw the implementation of `Lock` & it has this revision field,
does checking revision field would work ?
Also is there a suggested way to detect missing upload-time fields or other format indicators?

I'd appreciate any pointers to help me move forward. Also, could you point me to resources about uv lockfile format internals? 

Thanks for your patience as I work through this : )


---

_Comment by @zanieb on 2025-09-22 13:18_

I think you're on the right track with changing validation. I think we want to change

https://github.com/astral-sh/uv/blob/b1fbb524d29476484ab5ba2ddf19c24157b3d277/crates/uv/src/commands/project/lock.rs#L942

to just immediately return `Ok(Self::Preferable(lock))` if `force` is set. I think that should be sufficient to force us to do a new resolution and write to the lockfile?

---

_Comment by @11happy on 2025-09-22 16:12_

> I think you're on the right track with changing validation. I think we want to change
> 
> https://github.com/astral-sh/uv/blob/b1fbb524d29476484ab5ba2ddf19c24157b3d277/crates/uv/src/commands/project/lock.rs#L942
> 
> to just immediately return `Ok(Self::Preferable(lock))` if `force` is set. I think that should be sufficient to force us to do a new resolution and write to the lockfile?

Thank you, I will do it likewise : )

---

_Review requested from @zanieb by @11happy on 2025-09-23 14:49_

---

_Comment by @zanieb on 2025-09-23 17:43_

I think you'll need to merge / rebase with the overlapping work in #15994 

---

_Comment by @11happy on 2025-09-29 17:32_

have rebased it, done @zanieb : )

wait let me fix CI errors & get back !

---

_Comment by @11happy on 2025-09-30 16:52_

Hii @zanieb, I’ve fixed all the CI errors. Could you take a look when you get a chance?  
Thank you : )


---

_Comment by @11happy on 2025-10-12 10:19_

gentle ping !

---
