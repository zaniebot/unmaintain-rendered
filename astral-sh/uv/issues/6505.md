```yaml
number: 6505
title: Provide an APE installer instead of curling scripts?
type: issue
state: closed
author: ksamuel
labels:
  - releases
assignees: []
created_at: 2024-08-23T10:01:22Z
updated_at: 2024-08-24T12:57:28Z
url: https://github.com/astral-sh/uv/issues/6505
synced_at: 2026-01-10T04:45:09Z
```

# Provide an APE installer instead of curling scripts?

---

_Issue opened by @ksamuel on 2024-08-23 10:01_

Being easy to install is key for uv usage to spread. 

The installation story is simple, yet still have a few gotchas:

- An upstream issue in PowerShell permissions (https://github.com/astral-sh/uv/issues/2286)
- The fact curl is not installed out of the box on many distros (including ubuntu) and that installing it requires a different command for each distro.
- You have to choose between several installation methods.

While this will not trip a developer, it will likely at the very least slow down a student or a biologist.

Having an .exe for windows, a dev/rpm for linux and a dmg for Mac would be ideal, but a lot of work. Maybe there is an alternative.

In the last few years, Justine Tunney made APE (https://justine.lol/ape.html), a format that creates an executable that works universally on Windows, Mac and Linux. 

It could be worth exploring to provide a single executable as an installer for all major platforms.

It's not guaranteed that it would be the perfect streamlined solution, as it could come with other challenges such as signing and security policies. Indeed, scripts have less restrictions than executable on most platforms. 

What's more, because the rust story for API is incomplete (https://github.com/ahgamut/rust-ape-example), that would mean writing it in C, which I understand can be a big blocker.

This is not a feature request as much as it is way to open the discussion around that topic.

---

_Label `release` added by @zanieb on 2024-08-23 14:13_

---

_Comment by @ksamuel on 2024-08-24 12:57_

Ok, I made some experiments, but there is a blocker: on some linux distros, including Ubuntu, you might get errors relating to run-detectors due to binfmt_misc registrations. 

Now each user can fix that by adding an additional registration for the APE file format but it requires an incantation of 4 arcanes commands, which, while easy to just copy-paste, are not an improvement compared to curl installation. Especially since it would be more trivial to just tell people: 

- download the script
- chmox +x
- sh it

If they don't have curl.

So I don't think that's a good solution.

I'm closing this, you have enough tickets as it is.

---

_Closed by @ksamuel on 2024-08-24 12:57_

---
