```yaml
number: 2083
title: Add CHANGELOG.md file
type: pull_request
state: closed
author: not-my-profile
labels: []
assignees: []
base: main
head: changelog
created_at: 2023-01-22T07:08:02Z
updated_at: 2023-07-24T11:57:28Z
url: https://github.com/astral-sh/ruff/pull/2083
synced_at: 2026-01-12T15:55:07Z
```

# Add CHANGELOG.md file

---

_@not-my-profile_

We are currently using the [GitHub releases page](https://github.com/charliermarsh/ruff/releases) as our changelog, which I think is suboptimal because it just lists all merged PRs in the order they were merged ... which isn't relevant to users at all. Internal refactors also aren't really of interest to people who are just end-users.

I think it would make sense to introduce a `CHANGELOG.md` file like other projects have one, e.g:

*  the [Pygments changelog](https://pygments.org/docs/changelog/)
* the [Clippy changelog](https://github.com/rust-lang/rust-clippy/blob/master/CHANGELOG.md)
* the [Pylint changelog](https://pylint.pycqa.org/en/latest/whatsnew/2/index.html)

All of these group changes by their kind, which I think is really helpful for users.

(Sidenote: Note that GitHub unfortunately doesn't linkify pull request mentions in `.md` files ... however I think ruff will eventually end up with its own website anyway where we can easily postprocess the file to linkify not only these pull request numbers but also all mentions of rules to their respective documentation (once #1467 has been implemented).)

Maintaining such a `CHANGELOG.md` file however obviously is additional effort for you @charliermarsh, since you'll often have to remind contributors to update the changelog and bump the Unreleased section for every release (although that could probably be automated) so it comes down to whether you think this is worth it :)

---

_Comment by @JonathanPlasse on 2023-01-22 07:39_

You could also use [towncrier](https://github.com/twisted/towncrier).

---

_Comment by @not-my-profile on 2023-01-22 07:49_

I think towncrier is neat but I think it would also be neat to keep our dev dependencies Rust only and towncrier is implemented in Python.

---

_Comment by @JonathanPlasse on 2023-01-22 07:57_

I would follow [keep a changelog](https://keepachangelog.com/en/1.0.0/) as convention.

---

_Comment by @not-my-profile on 2023-01-22 08:09_

I assume you refer to the "Guiding Principles" mentioned there? In that case I agree they seem reasonable :) ... I just force pushed to add the release date. Whether or not ruff follows semver isn't really relevant yet since we haven't yet reached 1.0.0.

I do think that we should have "Added rules" & "Updated rules" as groups so that e.g. an added command-line flag would be listed separately after rule-specific changes.

---

_Comment by @JonathanPlasse on 2023-01-22 08:54_

Yes, that is what I meant.
I nitpick but I would use header 3 for Added rules, ...

---

_Comment by @charliermarsh on 2023-01-23 17:58_

Gah, I do want a better changelog and I agree with all the arguments put forward here, but I'm hesitant to introduce another maintenance burden right now... It saves me _so_ much time to auto-generate the release notes. Let me think on this for a bit. (Please don't feel the need to keep the current PR's changelog updated for now, don't want you to have to do all that work.)


---

_Comment by @JonathanPlasse on 2023-01-23 18:30_

Why not beginning to do a release to auto generate the changelog then cancelling, adding it to the changelog, then doing the final release?

---

_Comment by @not-my-profile on 2023-01-23 18:37_

I think the point here is that the GitHub changelog autogeneration isn't very good ... I think we should be able to come up with some better changelog generation system.

---

_Comment by @JonathanPlasse on 2023-01-23 18:39_

I was thinking of using it as a base to improve upon following keep a changelog spec.

---

_Comment by @not-my-profile on 2023-01-24 05:11_

"Keep a Changelog" is explicitly about not dumping git logs into changelog files ... dumping PR logs into changelog files wouldn't pose any improvement to the status quo as far as I'm concerned. I think we should either have a nice CHANGELOG.md or none at all.

As I said I think we should be able to come up with a system to get a nice changelog without putting all the burden on the maintainer.

---

_Comment by @hugovk on 2023-01-24 07:47_

You can use add labels to PRs, corresponding to Keep a Changelog's categories, and GitHub will use those to group the auto-generated release notes:

* https://docs.github.com/en/repositories/releasing-projects-on-github/automatically-generated-release-notes#configuration-options

I'm using Release Drafter to do this (because it was available before GitHub introduced it, but they look pretty similar) and it's working smoothly. For each PR, I just need to add one of the Keep a Changelog labels. For example:

* https://github.com/hugovk/norwegianblue/releases/tag/0.8.0
* https://github.com/hugovk/norwegianblue/blob/main/.github/release-drafter.yml
* https://github.com/hugovk/norwegianblue/blob/main/.github/workflows/release-drafter.yml

I also have a workflow to make sure I remember to add one of the "changelog: X" labels (or skip):

* https://github.com/hugovk/norwegianblue/blob/main/.github/workflows/require-pr-label.yml

---

_Comment by @not-my-profile on 2023-01-24 08:43_

Thanks Hugo! Using that GitHub feature would certainly be an improvement over the status quo, since it would let us group the changes and exclude refactors. I see two disadvantages that could probably be addressed:

1. regular contributors cannot apply labels to their own PRs, so the categorization would still have to be done by the maintainer (though I assume that could be remedied by setting up some GitHub bot)
2. the changelog entries would not be as neatly consistently formatted as proposed by this PR, with "New rules" and "Updated rules" entries always starting with `<rule-name> (<rule-code>)` (though I assume that could also be enforced via another GitHub workflow job)

However I think the bigger problem is that PR titles are in general written from a developer perspective to succinctly summarize the changes of a PR, while changelog entries should be written from a user perspective and complex changes can very well require more explanation than you could fit in a PR title. Not to mention that big PRs can very well introduce separate changes that should be documented in separate changelog entries.

So with that in mind I do actually think that a setup in the vain of towncrier as suggested by Jonathan would be preferable.

---

_Comment by @JonathanPlasse on 2023-01-26 08:04_

FYI, Pyo3 uses towncrier.

---

_Comment by @charliermarsh on 2023-01-26 23:50_

As a half measure, while we figure this out, I went ahead and categorized the PRs (and removed anything internal) from the autogenerated release notes. I know that's not perfect.

https://github.com/charliermarsh/ruff/releases/tag/v0.0.236

---

_Comment by @not-my-profile on 2023-01-28 07:04_

While working on ruff I encountered a clippy lint span that could be improved and made my first clippy contribution: https://github.com/rust-lang/rust-clippy/pull/10226

While doing so I have noticed their [PULL_REQUEST_TEMPLATE.md](https://github.com/rust-lang/rust-clippy/blob/master/.github/PULL_REQUEST_TEMPLATE.md), in particular:

> We're collecting our changelog from pull request descriptions. If your PR only includes internal changes, you can just write `changelog: none`. Otherwise, please write a short comment explaining your change.
> 
> It's also helpful for us that the lint name is put within backticks (`` ` ` ``), and then encapsulated by square brackets (`[]`), for example:
> ```
> changelog: [`lint_name`]: your change
> ```

I guess we could set up something similar ... however I think I'd prefer a tool like towncrier since I'd rather have the CI fail because of a malformed file than because of a malformed PR description, since I personally rather (force) push fixes than updating PR descriptions.

---

_Comment by @not-my-profile on 2023-02-10 06:45_

Closing this ... the GitHub changelog has improved somewhat and there does not appear to be much interest in producing a more consistently formatted changelog as suggested by this PR.

---

_Closed by @not-my-profile on 2023-02-10 06:45_

---

_Comment by @hugovk on 2023-02-10 09:05_

For reference, here's how release notes are auto-generated:

* https://github.com/charliermarsh/ruff/blob/main/.github/release.yml
* https://github.com/charliermarsh/ruff/pull/2341

---

_Comment by @tony on 2023-06-23 11:10_

@not-my-profile Are you willing to reopen? I can also create a new issue

I would highly prefer a changelog + git tags - the reason why is it's very difficult to cite a range of package updates downstream, e.g. 0.270 -> 0.275. That can be conveyed in a CHANGELOG via a git tag easily.

---

_Comment by @not-my-profile on 2023-06-23 11:18_

The GitHub changelog has been improved since I opened this PR. I agree that having an additional CHANGELOG file would be nice as well and it could probably even be automatically generated but yeah if you want that it's best for you to open a new issue for that. Note that releases are already tagged in git.

---

_Comment by @tony on 2023-07-24 11:57_

## New thread

I created a new thread at https://github.com/astral-sh/ruff/discussions/6027 

This re-initiates a recommendation for a `CHANGELOG.md` file. With this, downstream users could have much easier access to release notes.

If you have an opinion (affirmative or negative) in lieu of any accommodation already existing in this thread, definitely give it a vote or comment. Thank you!

---
