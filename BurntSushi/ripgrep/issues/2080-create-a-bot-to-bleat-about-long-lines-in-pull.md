```yaml
number: 2080
title: create a bot to bleat about long lines in pull requests
type: issue
state: open
author: BurntSushi
labels:
  - enhancement
assignees: []
created_at: 2021-11-22T14:47:34Z
updated_at: 2022-04-01T02:27:44Z
url: https://github.com/BurntSushi/ripgrep/issues/2080
synced_at: 2026-01-12T16:13:24Z
```

# create a bot to bleat about long lines in pull requests

---

_@BurntSushi_

For context that inspired this: https://github.com/BurntSushi/ripgrep/pull/2079#issuecomment-975584963

In general, when I write code/docs, I still to 79 columns inclusive. No, it's not because I code on punch cards. There are generally two reasons:

1. I find that a line length limit _tends_ to produce clearer code by exerting a pressure against stuffing too much into one line.
2. I like to be able to fit multiple code windows side-by-side. The extent to how well this works is a function of the quality of my eyesight, font size, distance from the monitor and the size of the monitor. Since all auto-line-wrapping I've seen by text editors is not that readable IMO, the smaller the line length limit the more code windows I can fit side-by-side.
3. 79 columns (or 80 columns, not counting the 80th column) is reasonably small and pretty common given its legacy status. (For example, it used to be the recommended style for Python via PEP8.)

Many people do not code with line length limits, or if they do, their limits are likely greater than mine. Because of this, it's not uncommon for PRs to come in that violate the 79 column rule. Not only is detecting this without automation difficult, but it's also kind of deflating to ask folks to reformat stuff based on line length. Often times, I'll just do it for them.

In theory, it's simple to build a tool that scans files and fails the build with the offending lines if any are over the limit. But in practice, the 79 column limit is _not_ enforced strictly: a foolish consistency is the hobgoblin of small minds. Sometimes, code lines are more readable if they're slightly longer. There may be other reasons to write a long line, such as a URL. Therefore, any such tool that errors on long lines also needs a whitelist, and now you have the problem of specifying how such a whitelist is stored and how it's maintained. This is decidedly not simple and quite annoying.

@matkoniecz suggested in #2079 to build a bot that only looks at the _diff_ produced by a PR. If there are lines longer than the limit, then the bot can send a message automatically asking to look into it. This neither fails the build nor does it require me to detect and bleat about these things myself.

So this issue is for building such a bot. I've never built a GitHub bot before myself. If someone else wants to do this, that would be cool, but I would ultimately need to retain exclusive control over its operation on the ripgrep repo.

Also, we should create a `CONTRIBUTING.md` file that includes, at minimum, the line length limit/guideline.

---

_Label `enhancement` added by @BurntSushi on 2021-11-22 14:47_

---

_Comment by @matkoniecz on 2021-11-22 14:54_

For bot itself - I have seen some code coverage monitor bots posting statistics how given PR changed code coverage, Visual Studio Code has some bot actions on issue tracker (automatically and triggered). 

See https://github.com/microsoft/vscode/issues/137641#issuecomment-975481667 https://github.com/microsoft/vscode-github-triage-actions

vscode-github-triage-actions is likely overengineered for needs here but some useful inspiration or copyable code may be there. And basic idea of running it as github actions seems like a good one?

https://github.com/facebook/docusaurus/issues/3521 https://github.com/noqcks/pull-request-size https://www.fullstory.com/blog/build-your-own-automated-sync-bot-via-github-actions/ may or may not be useful

---

_Comment by @deecewan on 2021-11-23 01:25_

i wonder if something like [Danger](https://danger.systems/) could be used? Although not sure you'd want the configuration for that to live inside of this codebase.

---

_Comment by @matkoniecz on 2021-12-01 09:39_

> Also, we should create a CONTRIBUTING.md file that includes, at minimum, the line length limit/guideline.

Can I copy also explanation from https://github.com/BurntSushi/ripgrep/issues/2080#issue-1060244354 ?

It would be verbose, but would not trigger "No, it's not because I code on punch cards" that is triggered in me where I see some projects where it was not changed since times when monitors were 640 x 480.

---

_Comment by @BurntSushi on 2021-12-01 13:07_

How about something shorter with a link to that comment for more
explanation?

On Wed, Dec 1, 2021, 04:40 Mateusz Konieczny ***@***.***>
wrote:

> Also, we should create a CONTRIBUTING.md file that includes, at minimum,
> the line length limit/guideline.
>
> Can I copy also explanation from #2080 (comment)
> <https://github.com/BurntSushi/ripgrep/issues/2080#issue-1060244354> ?
>
> It would be verbose, but would not trigger "No, it's not because I code on
> punch cards" that is triggered in me where I see some projects where it was
> not changed since times when monitors were 640 x 480.
>
> —
> You are receiving this because you authored the thread.
> Reply to this email directly, view it on GitHub
> <https://github.com/BurntSushi/ripgrep/issues/2080#issuecomment-983461301>,
> or unsubscribe
> <https://github.com/notifications/unsubscribe-auth/AADPPYTRCUNLC3WRNCXL2D3UOXUPDANCNFSM5IRF5MFA>
> .
> Triage notifications on the go with GitHub Mobile for iOS
> <https://apps.apple.com/app/apple-store/id1477376905?ct=notification-email&mt=8&pt=524675>
> or Android
> <https://play.google.com/store/apps/details?id=com.github.android&referrer=utm_campaign%3Dnotification-email%26utm_medium%3Demail%26utm_source%3Dgithub>.
>
>


---

_Comment by @eyal0 on 2022-03-31 15:17_

How about using a warning in the GitHub Action?  The warning will attach itself to a line number.

Have the CI take a diff, then filter the diff just for added or modified lines.  Then look for lines that are too long.  When found, report them.

https://docs.github.com/en/actions/using-workflows/workflow-commands-for-github-actions#setting-a-warning-message

Example: https://github.com/carpentries/sandpaper/issues/50

https://github.com/zkamvar/testme/commit/1ab80e278a57a2f038f35b74e8f2777cc3efbc62#annotation_744820416

https://github.com/zkamvar/testme/commit/c4589e565dd87f0181126a3a4e17811f239a5b63#

This will be much easier than writing a bot.

---

_Comment by @BurntSushi on 2022-03-31 15:26_

@eyal0 You're right to question using a bot. A bot is really an implementation detail. What I want is some kind of automated process that looks at the diff in a PR, and if there is a long line, it somehow informs the PR author that they should correct it.

A warning in CI logs may or may not actually get that across. I would guess probably not. A bot leaving comment saying, "hey please fix this" seems more effective to me.

---

_Comment by @eyal0 on 2022-03-31 18:50_

Depends how the warning is displayed.  Here, it seems quite prominent: https://github.com/zkamvar/testme/commit/1ab80e278a57a2f038f35b74e8f2777cc3efbc62#annotation_744820416

But if that isn't prominent enough then have a CI that will check for lines and give an error when it fail.  So it would look just like this one:

![image](https://user-images.githubusercontent.com/109809/161127952-3c3e6306-bd20-4e53-8944-e0f423a1fc01.png)

Except have another row called "ci-check formatting" or something like that.  It'll give a red error "x" if it fails.  When you click on it it would take you to the CI output which could include the filename and line number.

One difficulty that I can think of is that you want this CI to not fail the entire CI.  I'm not sure how that is done but it's likely that there is a way.

---

_Comment by @BurntSushi on 2022-03-31 19:00_

Ah I see. I don't want to fail the build though, because _sometimes_ breaking the line length is okay. See:

> In theory, it's simple to build a tool that scans files and fails the build with the offending lines if any are over the limit. But in practice, the 79 column limit is not enforced strictly: a foolish consistency is the hobgoblin of small minds. Sometimes, code lines are more readable if they're slightly longer. There may be other reasons to write a long line, such as a URL. Therefore, any such tool that errors on long lines also needs a whitelist, and now you have the problem of specifying how such a whitelist is stored and how it's maintained. This is decidedly not simple and quite annoying.

---

_Comment by @BurntSushi on 2022-03-31 19:00_

Notice also that failing the build will cancel the other builds. Maybe that's a configurable behavior, but that's another thing I would not want to happen. (I _do_ want it to happen for legit failures.)

---

_Comment by @eyal0 on 2022-03-31 23:06_

> Notice also that failing the build will cancel the other builds. Maybe that's a configurable behavior, but that's another thing I would not want to happen. (I _do_ want it to happen for legit failures.)

A failing a build will cancel other builds only if you have not set [continue-on-error](https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions#jobsjob_idcontinue-on-error).  So what you could do is have another job that will run in GitHub Actions, in parallel to the existing CI.  It will be marked `continue-on-error`.  It will take a diff, examine for long lines, and then fail the job if a long line is found.

When this new job fails, it will not affect the rest of the CI.  It will, however, put that red "x" in the list of CI statuses.

This seems like a pretty good solution to me!  Is there a usability issue with it, do you think?

---

As an aside, probably the right thing to do is use a code beautifier such as `rustfmt` and see if `rustfmt` is trying to modify any of the lines that this PR has touched.  And if so, raise an error.  And hopefully the beautifier can be configured to only care about line length.

The advantage to using a beautifier is that if, later on, you decide that you care about more than just line length, then you can modify the beautifier's configuration to do that.

I don't know if `rustfmt` does line length.  I didn't find it in the [docs](https://rust-lang.github.io/rustfmt/?version=v1.4.38&search=).

---

_Comment by @eyal0 on 2022-03-31 23:07_

A red "x" on line length doesn't mean that you can't merge.  It will just draw your attention to the issue.  Then you can go examine it and see if you want to ask the author to make a correction or if you want to merge it anyway.  That's a judgement call.

---

_Comment by @BurntSushi on 2022-04-01 02:27_

There is already CI in place to fail if rustfmt fails. The line length limit is also commonly broken on non-code files, such as in markdown files. So rustfmt is itself not sufficient.

What you propose is something I would be willing to try, yes. Thank you for digging into that!

---
