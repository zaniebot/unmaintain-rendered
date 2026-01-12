```yaml
number: 1675
title: Simplify release workflow
type: pull_request
state: closed
author: murlakatamenka
labels: []
assignees: []
base: master
head: update-actions-release
created_at: 2020-09-02T10:01:17Z
updated_at: 2021-06-01T07:32:24Z
url: https://github.com/BurntSushi/ripgrep/pull/1675
synced_at: 2026-01-12T18:23:14Z
```

# Simplify release workflow

---

_@murlakatamenka_

Remove unnecessary steps in the current release workflow to make it simpler and less hacky.

Good reference:
- https://eugene-babichenko.github.io/blog/2020/05/09/github-actions-cross-platform-auto-releases/

[Runs successfully](https://github.com/murlakatamenka/ripgrep/actions/runs/235810016) across all the platforms.

---

Also relevant thing that I learned alongside:

> `::set-env name={name}::{value}`
>
> The action that creates or updates the environment variable does not have access to the new value, but all subsequent actions in a job will have access.

[source](https://docs.github.com/en/actions/reference/workflow-commands-for-github-actions#setting-an-environment-variable)

Isn't intuitive to me.

---

Workflow file can be additionally formatted for better indentation, I didn't do it to make reviewing easier.

---

_Review comment by @BurntSushi on `.github/workflows/release.yml`:38 on 2020-09-13 14:36_

Can you say why this works?

Oh, I see, you moved the logic down below.

---

_@BurntSushi approved on 2020-09-13 14:38_

I think this LGTM. I'll merge and tweak this is a bit once I'm ready to do the next release. Thanks!

---

_@murlakatamenka reviewed on 2020-09-14 14:07_

---

_Review comment by @murlakatamenka on `.github/workflows/release.yml`:38 on 2020-09-14 14:07_

It's right from the examples of [`@actions/create-release`](https://www.github.com/actions/create-release) and [`@actions/upload-release-asset`](https://www.github.com/actions/upload-release-asset). They're smart enough to strip the unnecessary part and leave only tag name. Cannot find the resource where it was mentioned, but it works as we can see :man_shrugging: 

---

_Closed by @BurntSushi on 2021-06-01 01:51_

---

_Comment by @murlakatamenka on 2021-06-01 07:32_

Good to have CI / CD improved since it often serves as a reference. I've seen

> Apparently, this is the right way to get a tag name. Really?

in many other Rust projects 😄 

Thanks for the good work, as always 👍 

---
