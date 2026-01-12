```yaml
number: 15196
title: document uv version for environment variables
type: pull_request
state: merged
author: samypr100
labels:
  - documentation
assignees: []
merged: true
base: main
head: envvar-versioning
created_at: 2025-08-10T01:53:55Z
updated_at: 2025-10-08T17:42:22Z
url: https://github.com/astral-sh/uv/pull/15196
synced_at: 2026-01-12T16:11:37Z
```

# document uv version for environment variables

---

_@samypr100_

## Summary

As new environment variables get introduced (e.g. `UV_EDITABLE`) I thought it would useful to start tracking which release they were introduced. I think its a common workflow to navigate to the [env var documentation](https://docs.astral.sh/uv/reference/environment) to know what the env var for something is but then end up in a situation where one is using an environment variable with the wrong version of uv and not notice immediately that its not compatible and therefore ignored.

## Test Plan

Existing tests.

The versions in `since` have all been manually reviewed to the best of my ability for correctness. 


---

_Marked ready for review by @samypr100 on 2025-08-10 02:05_

---

_Label `documentation` added by @samypr100 on 2025-08-10 02:09_

---

_Comment by @zanieb on 2025-08-11 16:37_

How will we maintain this once we have 1.x.y versioning and don't know what the next version will be until release-time?

---

_Comment by @samypr100 on 2025-08-11 16:58_

> How will we maintain this once we have 1.x.y versioning and don't know what the next version will be until release-time?

Could we add some a standard stub e.g.  `Upcoming Release` and have rooster (or other tooling) look for that on a release to replace with?

---

_Comment by @zanieb on 2025-08-11 17:02_

Yeah Rooster could do a replacement for that, seems reasonable. We probably need that before merging. Even if we don't have 1.x.y versioning, there's no way to know which version your PR will land in when you create it.

---

_Comment by @samypr100 on 2025-08-11 17:10_

> Yeah Rooster could do a replacement for that, seems reasonable. We probably need that before merging. Even if we don't have 1.x.y versioning, there's no way to know which version your PR will land in when you create it.

There isn't any new "unreleased" environment variables as part of this PR, so I don't think we have that risk in this PR. All the environment variables here already been released and are documented for previous releases. I can make a follow up PR with rooster integration for when we do add new env vars? Would that be reasonable? Or did I misunderstand your concern?

---

_Comment by @samypr100 on 2025-09-22 22:21_

@zanieb this should be ready for another look (sorry forgot to mark it as draft during the back and forth)

I went with a macro approach on the second commit as I thought it'd be better Dev Ex.

Note, the OIDC errors are unrelated to this PR but likely due to the OIDC setup related to my fork and depot.

---

_@zanieb reviewed on 2025-09-23 01:26_

---

_Review comment by @zanieb on `pyproject.toml`:70 on 2025-09-23 01:26_

Since it's scoped to this file I wonder if we should just do `"NEW"` or something (including the quotes in the match)?

---

_@samypr100 reviewed on 2025-09-23 13:03_

---

_Review comment by @samypr100 on `pyproject.toml`:70 on 2025-09-23 13:03_

Done in 2117e1485da00a7f642acc773d58d0c6566844b6

This is how it would render
<img width="870" height="873" alt="render" src="https://github.com/user-attachments/assets/4fbbf0a0-e626-4ebb-b812-33fa8b879eee" />


---

_@zanieb reviewed on 2025-09-23 14:10_

---

_Review comment by @zanieb on `pyproject.toml`:70 on 2025-09-23 14:10_

Oh that's not quite what I meant — I forgot we replaced the whole match so matching the quotes doesn't actually work like I was imagining.

---

_@samypr100 reviewed on 2025-09-23 14:39_

---

_Review comment by @samypr100 on `pyproject.toml`:70 on 2025-09-23 14:39_

Another thought. We could also change pretext `Since` for `Added in`, that allows us to use a more natural reading placeholder like `"Next Release"` or `"Upcoming Release"` ... e.g. `Added in "Next Release"`

---

_@zanieb reviewed on 2025-09-23 14:44_

---

_Review comment by @zanieb on `pyproject.toml`:70 on 2025-09-23 14:44_

Yeah I like "Added in" more, I was actually exploring that separately.

I'm trying to futz with the styling

https://github.com/astral-sh/uv/commit/719d2d0343e41e8b0525ade87a59468bc4036a49

I tried including it on the same line (as in that commit) and also tried applying some styling to the h3 / p elements to reduce the vertical margin between the heading and this notation. I'm not sure what's best. I don't love any of the styles yet.

---

_@zanieb reviewed on 2025-09-23 14:44_

---

_Review comment by @zanieb on `pyproject.toml`:70 on 2025-09-23 14:44_

<img width="775" height="388" alt="Screenshot 2025-09-23 at 9 44 54 AM" src="https://github.com/user-attachments/assets/bc66dfd1-57cc-42ae-91df-b63f50f1ad51" />


---

_@samypr100 reviewed on 2025-09-23 15:50_

---

_Review comment by @samypr100 on `pyproject.toml`:70 on 2025-09-23 15:50_

I do like it next to the heading; I think we can solve this with pure css without changing to full-blown html ids (avoiding potentially breaking plugins/themes). I'll give it a shot in a bit

---

_@zanieb reviewed on 2025-09-23 16:01_

---

_Review comment by @zanieb on `pyproject.toml`:70 on 2025-09-23 16:01_

We do this in the CLI reference and it's fine fwiw, but happy to let you explore it — I don't think I'll have time to find something I like myself.

---

_@samypr100 reviewed on 2025-09-23 16:07_

---

_Review comment by @samypr100 on `pyproject.toml`:70 on 2025-09-23 16:07_

New changes now use "added in" pretext and "next release" (unquoted) as the placeholder. Also the "added in" is inlined w/ heading using pure minimal css

<img width="883" height="796" alt="render_2" src="https://github.com/user-attachments/assets/edec47d0-2c85-42fd-ac1d-3037ee0134f9" />

Thoughts?

---

_Comment by @liamreece on 2025-09-24 13:03_

+1 I ran into this today with [UV_S3_ENDPOINT_URL](https://docs.astral.sh/uv/reference/environment/#uv_s3_endpoint_url) not realizing it was available only on 0.8.21+

---

_@zanieb reviewed on 2025-09-24 19:23_

---

_Review comment by @zanieb on `crates/uv-macros/src/lib.rs`:80 on 2025-09-24 19:23_

Can you rename `since` to `added_in` as I did in my branch?

---

_@zanieb approved on 2025-09-24 19:23_

---

_@samypr100 reviewed on 2025-09-24 22:11_

---

_Review comment by @samypr100 on `crates/uv-macros/src/lib.rs`:80 on 2025-09-24 22:11_

👍 done in 94f7a3927688ec3b69b1ca127c6e7c9ee06d5283

---

_Comment by @samypr100 on 2025-10-08 16:29_

@zanieb any further thoughts on this? thanks 😄 

---

_@zanieb approved on 2025-10-08 17:31_

saintlike patience

---

_Merged by @zanieb on 2025-10-08 17:31_

---

_Closed by @zanieb on 2025-10-08 17:31_

---

_Comment by @zanieb on 2025-10-08 17:31_

sorry for the delays, stretched thin on reviews right now

---

_Branch deleted on 2025-10-08 17:42_

---
