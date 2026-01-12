```yaml
number: 1550
title: "Add task-tags & ignore-overlong-task-comments settings"
type: pull_request
state: merged
author: not-my-profile
labels: []
assignees: []
merged: true
base: main
head: comment-tags
created_at: 2023-01-02T09:44:23Z
updated_at: 2023-01-05T05:01:57Z
url: https://github.com/astral-sh/ruff/pull/1550
synced_at: 2026-01-12T05:36:31Z
```

# Add task-tags & ignore-overlong-task-comments settings

---

_Pull request opened by @not-my-profile on 2023-01-02 09:44_

Programmers often leave comments to themselves and others such as:

    # TODO: Use a faster algorithm?

The keywords used to prefix such comments are just a convention and vary from project to project. Other common keywords include FIXME and HACK.

The keywords in use for the codebase are of interest to ruff because ruff does also lint comments. For example the ERA lint detects commented-out code but ignores comments starting with such a keyword. Previously the ERA lint simply hardcoded the regular expression TODO|FIXME|XXX to achieve that. This commit introduces a new `task-tags` setting to make this configurable (and to allow other comment lints to recognize the same set of keywords).

The term "task tags" has probably been popularized by the [Eclipse IDE](https://www.eclipse.org/pdt/help/html/task_tags.htm). For Python there has been the proposal [PEP 350](https://peps.python.org/pep-0350/), which referred to such keywords as "codetags". That proposal however has been rejected. We are choosing the term "task tags" over "code tags" because the former is more descriptive: a task tag describes a task.

While according to the PEP 350 such keywords are also sometimes used for non-tasks e.g. NOBUG to describe a well-known problem that will never be addressed due to design problems or domain limitations, such keywords are so rare that we are neglecting them here in favor of more descriptive terminology. The vast majority of such keywords does describe tasks, so naming the setting "task-tags" is apt.

---

_Comment by @charliermarsh on 2023-01-02 17:14_

I don't think we should avoid flagging long-line violations for TODOs by default. If we want to support this for E501, it should be opt-in, in my opinion.


---

_Comment by @not-my-profile on 2023-01-02 17:24_

Fair enough ... do you have an idea what the flag should be called? How about:

```toml
[tool.ruff.pycodestyle]
allow-overlong-lines-for-comment-tags = true
```

---

_Comment by @charliermarsh on 2023-01-02 18:06_

That looks reasonable. Or `ignore-long-comment-tags` as something more succinct? But what you have is more specific.

---

_Comment by @not-my-profile on 2023-01-02 18:13_

I think long-comment-tags would be confusing since it's not the comment tags that are long but the text that follows them.

---

_@charliermarsh requested changes on 2023-01-02 21:34_

(Just requesting changes to help with my own task management.)

---

_Renamed from "Introduce comment-tags setting (and make E501 skip them)" to "Add comment-tags & allow-overlong-lines-for-comment-tags settings" by @not-my-profile on 2023-01-03 02:38_

---

_Review requested from @charliermarsh by @not-my-profile on 2023-01-03 02:51_

---

_Comment by @charliermarsh on 2023-01-03 04:24_

Will try to get this merged tomorrow.

---

_Review comment by @charliermarsh on `README.md`:1760 on 2023-01-03 18:50_

`comment-tags` feels a bit too generic.

I guess Python almost called these `codetags` ([PEP 350](https://peps.python.org/pep-0350/)).

`todo-tags`?

---

_@charliermarsh reviewed on 2023-01-03 18:50_

---

_@charliermarsh reviewed on 2023-01-03 18:51_

---

_Review comment by @charliermarsh on `README.md`:1760 on 2023-01-03 18:51_

Is "codetags" perhaps the standard terminology for this?

https://marketplace.visualstudio.com/items?itemName=cg-cnu.vscode-codetags
https://trac-hacks.org/wiki/CodeTagsPlugin


---

_Review comment by @charliermarsh on `README.md`:1760 on 2023-01-03 18:51_

I guess in Eclipse these are "task tags" (https://stackoverflow.com/questions/9586478/ide-comment-keywords). That feels most appropriate.

---

_@charliermarsh reviewed on 2023-01-03 18:51_

---

_@charliermarsh reviewed on 2023-01-03 18:52_

---

_Review comment by @charliermarsh on `README.md`:3000 on 2023-01-03 18:52_

I am a little concerned that this option feels quite niche. Can you lay out the reasoning for including it?

---

_@not-my-profile reviewed on 2023-01-03 21:59_

---

_Review comment by @not-my-profile on `README.md`:1760 on 2023-01-03 21:59_

Thanks for bringing this up! I agree that "comment tags" doesn't sound that great. And nice find with PEP 350 ... I didn't know about that PEP.

I am afraid there is no standard terminology for this. The PEP has been rejected and apparently only called these "codetags" because of the Trac plugin, you linked. That plugin is now also unmaintained. Sidenote ad the codetags VS Code extension: [Better Comments](https://marketplace.visualstudio.com/items?itemName=aaron-bond.better-comments) is the much more commonly used extension for this, which calls these just `better-comments.tags` in the configuration and "(annotation) tags" in the documentation.

I don't like "code tag" since it's very uninformative (you cannot put these tags anywhere in the code; only in comments and the tags also don't describe the code but usually some tasks). "todo tags" is too specific since TODO is just one variant of such tags.

"task tags" is interesting. While I cannot recall hearing that term before, it does make sense and does appear to be somewhat established in academia (`task annotation` [2008](https://dl.acm.org/doi/10.1145/1368088.1368123), `task tag` [2013](https://www.cqse.eu/fileadmin/content/news/publications/2013-quality-analysis-of-source-code-comments.pdf), `task comment` [2022](https://research.ou.nl/ws/portalfiles/portal/46333009/Lung_C_IM9906_SE_AF_scriptie_Pure.pdf)).

The only reason I see against "task tags" is that sometimes such tags are also used for non-tasks e.g NOBUG (from the PEP 350) to mark well-known problems that will never be addressed due to design problems or domain limitations. However such tags are really so rare that I think it is fine to go with "task tags".

With all of this in mind "task tags" certainly seems like the best option :)

---

_@not-my-profile reviewed on 2023-01-03 22:08_

---

_Review comment by @not-my-profile on `README.md`:3000 on 2023-01-03 22:08_

I extensively use such task tags for task management and use e.g. `git grep TODO` to list all TODO tasks within the repository. To make that convenient I put the task descriptions in a single line so that they are not cut off in the output by `git grep`.

(Sidenote: `git grep` does not support multi-line grepping ... while it's certainly possible to use `ripgrep` to find TODO comments that span multiple lines that does make collaborating with other people more difficult because then they need to install yet another tool to share the workflow).

---

_Converted to draft by @not-my-profile on 2023-01-03 22:32_

---

_Renamed from "Add comment-tags & allow-overlong-lines-for-comment-tags settings" to "Add task-tags & ignore-overlong-task-comments settings" by @not-my-profile on 2023-01-03 23:42_

---

_Marked ready for review by @not-my-profile on 2023-01-03 23:48_

---

_Comment by @not-my-profile on 2023-01-03 23:51_

I renamed `comment-tags` to `task-tags` and `allow-overlong-lines-for-comment-tags` to `ignore-overlong-task-comments` as per my reasoning above.

I also split up the PR in three commits with extensive commit messages explaining the reasoning behind the changes. If you merge this, please preserve the individual commits ;)

---

_Review requested from @charliermarsh by @not-my-profile on 2023-01-04 08:27_

---

_Merged by @charliermarsh on 2023-01-04 22:10_

---

_Closed by @charliermarsh on 2023-01-04 22:10_

---

_Comment by @not-my-profile on 2023-01-05 02:47_

Thanks for merging ... but I am upset that squashed my carefully crafted commits despite me explicitly requesting you to preserve them. :(

---

_Comment by @not-my-profile on 2023-01-05 02:56_

Maybe you could revert ca48492137d619c2cd25a9df8625464bf4e0a274 with another commit and then apply my commits individually? I wrote these commit messages to be preserved in the main branch ...

---

_Comment by @charliermarsh on 2023-01-05 03:14_

I do apologize. I tried to preserve them in the PR summary (by replacing it with the first commit message), since the project uses a one-commit-per-PR convention and I'm hesitant to deviate. I eventually want to get to a point such that we have clear PR summaries that get committed as part of the log, but we haven't been rigorous about it and so I've just been omitting messages for consistency. The alternative would've been to break each of those commits into its own PR and create a series of stacked diffs, which would've better lended itself to preserving the commit messages.

If you feel strongly in this case, I can go ahead and re-add to the body. Although I'll note that they do continue to exist in the history on GitHub, and any future archaeologists that are interested in these decisions will be able to find them here.

---

_Comment by @not-my-profile on 2023-01-05 03:29_

I feel strongly about descriptive commits in general not just in this case. I absolutely do not care about GitHub PR descriptions ... they do not show up when doing a `git blame` or git logging individual files.

I am afraid that this is quite a dealbreaker for me ... I am not interested in contributing elaborate changes if I have to split them up into separate PRs just to have the commits preserved.

Note that this is not just about preserving the commit messages in the git log but also about having smaller commits with more readable diffs with commit messages that explain the reasoning behind the changes contained in the commit.

---

_Comment by @charliermarsh on 2023-01-05 03:59_

Look, I don't want to be dogmatic. It's very, very reasonable to have a squash-and-merge policy on a codebase -- I can point to a bunch of projects that do this! It has a ton of benefits! -- but I can rebase-and-merge your PRs in the future given that you feel strongly. If, for whatever reason, you choose not to continue contributing to the project, then I earnestly and sincerely thank you for your contributions.

If you're interested in better understanding my perspective, you could Google around for "stacked diffs" (apologies if you're already familiar with this paradigm). It achieves exactly what you've described (smaller, more readable diffs, with commit messages that explain the reasoning), but at the PR level, rather than the commit level.

Separately: regardless of whether you "care" about GitHub PR descriptions, the truth is there's a ton of context in GitHub that isn't captured in the Git log. Look at all the comments on this PR :) From my experience working consistently in large codebases over many years, there's always context that lives in the tool and not in the log. The log is the starting point, not the end point. But, I don't think I'm going to convince you.


---

_Comment by @not-my-profile on 2023-01-05 04:33_

Thanks for offering to rebase my PRs. Yes of course there is much context in issues and PRs but commit messages give us the opportunity to summarize that context so that the log is a more helpful starting point ... and you perhaps do not even need to look up the PRs to understand the reasoning behind the changes.

I am not familiar with "stacked diffs" ... I just looked into it and I think you rather mean "stacked PRs"? Does [this blog post](https://benjamincongdon.me/blog/2022/07/17/In-Praise-of-Stacked-PRs/) describe what you mean? That blog post does mention some command-line tools for managing stacked PRs ... I am not familiar with them, but I can look into these tools ... can you recommend tools for stacked PRs or do you just manage them manually?

---

_Comment by @charliermarsh on 2023-01-05 04:36_

Heads up: I just reopened the PR as a revert + a cherry-pick of the original commits. I'm happy to avoid squashing your PRs in the future as long as the individual commits remain clear and high-quality as they were in this case. I'm sorry to have gone against your wishes in this case, and I hope we're able to continue working together on Ruff!

---

_Comment by @charliermarsh on 2023-01-05 04:40_

Yes, sorry, "stacked diffs" is a term that comes from a tool called Phabricator, which was a code review tool that spun out of Facebook... But it's analogous to "stacked PRs".

GitHub admittedly doesn't have great tooling to support them and it's been a big area of complaint for people that are used to a stacked PR / stacked diff workflow. (They actually released some improvements like [automated retargeting](https://github.blog/changelog/2020-05-19-pull-request-retargeting/).)

I typically manage them "manually" by setting the upstream of each PR to the PR that comes before, then iteratively rebasing and pushing to update the chain.

Here are some more resources on the concept that I'm describing:

- https://matt-rickard.com/stacked-pull-requests
- https://jg.gg/2018/09/29/stacked-diffs-versus-pull-requests/

It's possible of course that I'm totally wrong and this isn't a good model for an open source project (vs. a corporate codebase, which I'm more used to). But [Dagster](https://github.com/dagster-io/dagster/commits/master), for example, does this -- notice that in the Git history, every commit is a PR.

---

_Comment by @andersk on 2023-01-05 04:50_

Unfortunately, since GitHub requires the upstream of each PR to be a branch in the target repository, contributors without write access can’t create stacked PRs at all.

---

_Comment by @charliermarsh on 2023-01-05 04:55_

Wow, that's a bummer! I hadn't even realized that. I'm too used to working within the context of an org.

---

_Comment by @andersk on 2023-01-05 04:58_

Yeah—it seems GitHub [hasn’t realized it either](https://github.com/cli/cli/issues/5626#issuecomment-1125037574), probably for the same reason. 🤦

---

_Comment by @not-my-profile on 2023-01-05 05:00_

I have not contributed to projects using Phabricator or Gerrit for code review yet, so I am not familiar with this stacked PR workflow. Thanks Anders for that insight ... in that case I think I'll just continue to create PRs with carefully crafted commits. I don't have anything against squashing by default as a convention ... I'm just glad that you're willing to make an exception for contributors like me who like to craft self-contained and well-described individual commits :)

---
