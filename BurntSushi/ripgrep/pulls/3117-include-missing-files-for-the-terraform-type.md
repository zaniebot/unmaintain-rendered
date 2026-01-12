```yaml
number: 3117
title: Include missing files for the Terraform type
type: pull_request
state: closed
author: Porkepix
labels:
  - rollup
assignees: []
base: master
head: extend_tf_type
created_at: 2025-07-31T12:40:03Z
updated_at: 2025-09-20T01:08:43Z
url: https://github.com/BurntSushi/ripgrep/pull/3117
synced_at: 2026-01-12T18:23:15Z
```

# Include missing files for the Terraform type

---

_@Porkepix_

Existing matches were too restrictives, so we simplify those to every type of tfvars file we can encounter.

---

_Comment by @Porkepix on 2025-07-31 12:41_

Before this is merged (or after, actually, it can be one or two PRs), I'm wondering if OpenTofu should also be included and if it should if this would be within the tf type, another dedicated one or both?

---

_Comment by @BurntSushi on 2025-07-31 12:50_

I don't know what OpenTofu is. I don't have experience with all of ripgrep's file types. I generally rely on users to adjudicate it. Adjudication can be done by someone with significant experience with the software in question, or by a link to an official source indicating a file type or by examples of some kind.

---

_Comment by @Porkepix on 2025-07-31 12:59_

> I don't know what OpenTofu is. I don't have experience with all of ripgrep's file types. I generally rely on users to adjudicate it. Adjudication can be done by someone with significant experience with the software in question, or by a link to an official source indicating a file type or by examples of some kind.

OpenTofu is a community-led fork of Terraform following Hashicorp's license changes perceived as a long-term issue in general (the same kind that happened with ElasticSearch, Redis, …). It was launched by the Linux foundation and supported by quite some other actors, cf. https://www.linuxfoundation.org/press/announcing-opentofu and https://opentofu.org/

It rely on many common files extension as such, as it wants to remain compatible (for the time being), but also have its own: https://opentofu.org/docs/language/files/

---

_Comment by @BurntSushi on 2025-07-31 13:06_

It should probably be a distinct file type then. It sounds like OpenTofu subsumes Terraform globs, but not the other way around.

---

_Comment by @Porkepix on 2025-07-31 13:11_

> It should probably be a distinct file type then. It sounds like OpenTofu subsumes Terraform globs, but not the other way around.

I'd say it's probably the case yes, although it'd need a check to be sure it doesn't skip on some specific config files (or if it doesn't matter if those are included).
I guess it'd also require checking if the type definitions here allows to write an "extend" where we build a type from merging another one with new entries, instead of repeating things, and that it'd anyway need another PR, making this one good for review.

EDIT: Reordered entries for better ordering logic.

---

_@BurntSushi approved on 2025-08-19 20:43_

---

_Label `rollup` added by @BurntSushi on 2025-08-19 20:43_

---

_Comment by @Porkepix on 2025-08-19 20:59_

I'm trying to figure out, how does this rollup tag and related branch, `ag/rollup` works, is there some place I could read about it?

---

_Comment by @BurntSushi on 2025-08-19 21:05_

I tend to let projects languish with minimum involvement, and then when I get the time and motivation, I do as much work as I can on just that project. In the case of ripgrep, I'm working through the opened PRs. Instead of leaving feedback and waiting for folks to respond (which I try to do at all times), most PRs require very modest touchups in order to merge. So I just do it myself. And instead of opening new PRs for every single instance of this, I create one branch and "roll" all the PRs into that one.

I have a [little script](https://github.com/BurntSushi/dotfiles/blob/275005b705fc42b92bfca5627ccd69e705648f7b/bin/hub-rollup) that takes a PR number and applies the PR's commits on to my current branch. Then I touch up the commit history and so on, while preserving the original author's name.

Once I'm ready, I put this branch as its own PR. Once merged, it will close out all of the PRs that rolled up into it. [Here's a past example.](https://github.com/BurntSushi/ripgrep/pull/1884)

---

_Closed by @BurntSushi on 2025-09-20 01:08_

---
