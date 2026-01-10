```yaml
number: 14464
title: "feature request: select ALL until a given version number"
type: issue
state: closed
author: DaniBodor
labels:
  - question
assignees: []
created_at: 2024-11-19T15:35:57Z
updated_at: 2024-11-19T17:26:08Z
url: https://github.com/astral-sh/ruff/issues/14464
synced_at: 2026-01-10T11:09:56Z
```

# feature request: select ALL until a given version number

---

_Issue opened by @DaniBodor on 2024-11-19 15:35_

In the ruff settings, I like to select ALL rules and just ignore the ones that I dont use (which is a much shorter list than those I do). However, as the documentation already warns: "Use ALL with discretion. Enabling ALL will implicitly enable new rules whenever you upgrade." Indeed, I run into this quite frequently and it is a bit frustrating.

I now circumvent this issue by freezing the ruff version I use in the CI, but this is also not ideal, because that means that new improvements other than new rules also don't get implemented (e.g. bug fixes to existing rules, performance increase, etc). Also the script to fetch the version from pyproject.toml is kind of clunky.

I would like to request a setting where you can select ALL rules until a given version number. This would allow me to just specify installing ruff in CI, without fixing a version number. The settings will then take care of not selecting the newest rules, without me having to create a huge list of what to select.

The added benefit of this is that you can then more easily add all new rules at once, without having to go and check what new rules exist and decide one-by-one whether to implement them or not.




---

_Renamed from "feature request" to "feature request: select ALL until a given version number" by @DaniBodor on 2024-11-19 15:36_

---

_Comment by @MichaReiser on 2024-11-19 16:26_

Hi @DaniBodor 

Thanks for reaching out. I can see how using `ALL` with new ruff versions can be frustrating because `ALL` also includes very opinionated rules that many don't agree with. That's also why we recommend people not to use `ALL`. However, I can see why you and many others use it: Ruff doesn't provide a good alternative to `ALL`. There's no easy way to opt-in to a good default rule set that expresses "good community standards" but excludes the very nit-picky rules. We hope to address this shortcoming as part of the #1774 work. Unfortunately, this work is still far out. 

For now, I would suggest you pin the Ruff minor version in CI (or in your project. tool) and bump it every other month or so. You'll still get bug fixes this way, and you won't be surprised by newly added rules because we only ever introduce new rules in minor versions. Bumping the minor version allows you to review the new rules and disable the ones you dislike. 


I don't think that a versioned-`ALL` selector is the right solution for the problem because it would give the wrong impression that it "freezes" the rules that run. This is not the case; removed rules would no longer run even if they were previously selected with `ALL-0.5`. Supporting it would introduce a fair amount of complexity on our end.  

---

_Label `question` added by @MichaReiser on 2024-11-19 16:26_

---

_Comment by @DaniBodor on 2024-11-19 16:45_

Thanks for the response.

I see what you mean, and agree that #1774 would do a lot to solve the reasons for selecting all. Can also see that it would not be easy to implement. 

Just figured I'd throw the idea out there and see what the response is :)

---

_Comment by @MichaReiser on 2024-11-19 17:26_

> Just figured I'd throw the idea out there and see what the response is :)

Yes, thanks for doing this. It's always great to hear from users and what problems they run into. I'll close this issue. Feel free to comment if you want to follow up on something and I can reopen. 

---

_Closed by @MichaReiser on 2024-11-19 17:26_

---
