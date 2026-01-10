```yaml
number: 16213
title: Add issue templates
type: pull_request
state: merged
author: zanieb
labels:
  - internal
assignees: []
merged: true
base: main
head: zb/ruff-templates
created_at: 2025-02-17T14:25:39Z
updated_at: 2025-02-25T10:59:20Z
url: https://github.com/astral-sh/ruff/pull/16213
synced_at: 2026-01-10T19:49:01Z
```

# Add issue templates

---

_Pull request opened by @zanieb on 2025-02-17 14:25_

Follows https://github.com/astral-sh/ruff/pull/15651

Preview: https://github.com/dhruvmanila/ruff-issue-templates/issues

GitHub made the interface for single-template repositories worse. While they might fix it, it encouragement to just do this work. They still haven't fixed the teeny tiny emojis which makes me think this won't be fixed quickly.

Before:

<img width="1267" alt="Screenshot 2025-02-17 at 8 26 08 AM" src="https://github.com/user-attachments/assets/e69ef630-4296-470e-ab4d-a22d55785444" />

After:

<img width="1688" alt="Screenshot 2025-02-24 at 3 05 35 PM" src="https://github.com/user-attachments/assets/61033666-1fe5-421b-a69c-1aa79bcc85b5" />



---

_Label `internal` added by @zanieb on 2025-02-17 14:25_

---

_@MichaReiser reviewed on 2025-02-17 14:29_

---

_Review comment by @MichaReiser on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:29 on 2025-02-17 14:29_

I don't think we need this. It's normally not relevant fo rruff

---

_@MichaReiser reviewed on 2025-02-17 14:29_

---

_Review comment by @MichaReiser on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:44 on 2025-02-17 14:29_

This is also rarely needed. 

---

_@MichaReiser reviewed on 2025-02-17 14:30_

Hmm, not sure. I'd rather just go back to delete the issue template again or we dedicate time to design issue templates for ruff:

* new rule
* bug (should mention the rule, if applicable)


but that may require some more thought and I'm not sure it's worth it.

---

_Comment by @MichaReiser on 2025-02-17 14:32_

I reported the issue on their feedback discussion https://github.com/orgs/community/discussions/148713#discussioncomment-12225604

---

_Comment by @zanieb on 2025-02-17 14:33_

I'm not sure I agree it's really an either or here? Adding the initial templates is still a step forward. Adding a "new rule" template is another step forward.

---

_@zanieb reviewed on 2025-02-17 14:33_

---

_Review comment by @zanieb on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:44 on 2025-02-17 14:33_

(missed the `uv run` there)

---

_@zanieb reviewed on 2025-02-17 14:34_

---

_Review comment by @zanieb on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:44 on 2025-02-17 14:34_

Still seems useful for them to provide? It's not useful in every uv issue but when it is it saves time.

---

_@MichaReiser reviewed on 2025-02-17 14:34_

---

_Review comment by @MichaReiser on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:44 on 2025-02-17 14:34_

It's churn for users where we only really need it in less than 1 percent. I rather have their configuration. 

---

_@zanieb reviewed on 2025-02-17 14:38_

---

_Review comment by @zanieb on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:44 on 2025-02-17 14:38_

Do you want a dedicated field for that?

---

_Comment by @dhruvmanila on 2025-02-17 15:09_

I agree with Micha that this might require some more thought but I'd be ok with including "Bug report", "Feature request", and a "Blank issue".

For Ruff, there are two places where users can ask questions - discussions or issue tracker. We haven't provided any strong recommendation on where to ask questions and we leave it to the user wherever they feel comfortable. For this reason, I'm not sure if we should include a "Question" template.

It might be useful to have a single place where we collect all these questions. I very much prefer discussions for user queries because of separate threads.

So, we could possibly replace the "Question" template with redirecting the users to the discussion or include the following two links in the "Question" template:
* Discussion - https://github.com/astral-sh/ruff/discussions
* Searching resolved questions: https://github.com/astral-sh/ruff/issues?q=is:issue%20state:closed%20label:question

---

_Comment by @zanieb on 2025-02-17 15:10_

> For Ruff, there are two places where users can ask questions - discussions or issue tracker. We haven't provided any strong recommendation on where to ask questions and we leave it to the user wherever they feel comfortable. For this reason, I'm not sure if we should include a "Question" template.

This is a great point. We can link to the Discussions page instead.

<img width="988" alt="Screenshot 2025-02-17 at 9 14 53 AM" src="https://github.com/user-attachments/assets/83378797-3111-4552-b3a4-6183b8d859de" />


---

_Comment by @MichaReiser on 2025-02-17 15:19_

Hmm. I'm sort of leaning towards not having to think about this right now ;)

I'm not sure if I agree with promoting discussions for questions because questions sometimes turn out to be bugs that then need to be transferred. I also don't track discussions as closely as I do issues. 

---

_Comment by @MichaReiser on 2025-02-17 15:20_

Github's now aware of the bug https://github.com/orgs/community/discussions/148713#discussioncomment-12226081

---

_Comment by @zanieb on 2025-02-17 15:39_

I think we should just bias towards action here because this is very low-cost to iterate on and the current UX is clearly worse.

---

_Comment by @dhruvmanila on 2025-02-18 07:00_

> I'm not sure if I agree with promoting discussions for questions because questions sometimes turn out to be bugs that then need to be transferred. I also don't track discussions as closely as I do issues.

I don't think transferring should be much of an issue as GitHub provides a one-click solution to this. My main motivation for keeping questions in a single place is for visibility and provide an easy way to search through existing resolved queries. This is not to say to transfer all existing issues but just to divert users to use a single place to store all queries.

---

_Comment by @zanieb on 2025-02-18 21:19_

I'm going to hand this over to @dhruvmanila — I don't think I can advocate for any particular solution over here.

---

_@dhruvmanila reviewed on 2025-02-19 15:58_

---

_Review comment by @dhruvmanila on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:44 on 2025-02-19 15:58_

I've removed this. I agree that it's not really required and the `target-version` might be more useful which should ideally be present in the config they provide.

---

_Renamed from "Port uv issue templates to ruff" to "Add issue templates" by @dhruvmanila on 2025-02-19 16:55_

---

_Marked ready for review by @dhruvmanila on 2025-02-24 08:39_

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-24 08:39_

---

_Review comment by @MichaReiser on `.github/ISSUE_TEMPLATE/config.yml`:5 on 2025-02-24 09:09_

I'd prefer to keep using issues for questions. Let's decide this separtely from this PR

Why: because they sometimes turn into bug reports which then requires transferring issues (which creates noise because it results in extra notifications). I also sometimes go over existing issues to see if they've been resolved, and I'd prefer if I don't have to do this for discussions and issues.

---

_Review comment by @MichaReiser on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:32 on 2025-02-24 09:13_

Let's not make the version mandatory. I don't want users to avoid reporting bugs just because they struggle to figure out the version (it's also less clear how to get the version if you e.g. use the vs code extension)

---

_@MichaReiser reviewed on 2025-02-24 09:13_

I'm still undecided on this. I think what we have now works well and I see little reason for changing it. But I also don't want to stay in the way but:

* Let's add a *Blank issue* entry. There are many more reasons other than questions, bug reports, and new rules (e.g. formatter style, rule changes, rule removals, etc)
* I'd prefer to keep adding labels manually. There are often bug reports that are none. I also use the labels to know if someone from the team looked at the issue. We'd lose that bit of information by auto labelling issues. 


---

_@dhruvmanila reviewed on 2025-02-24 09:28_

---

_Review comment by @dhruvmanila on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:32 on 2025-02-24 09:28_

Good point. I'll make it optional.

---

_Comment by @dhruvmanila on 2025-02-24 09:28_

> * Let's add a _Blank issue_ entry. There are many more reasons other than questions, bug reports, and new rules (e.g. formatter style, rule changes, rule removals, etc)

Are you suggesting an explicit entry because it is automatically created (refer to the screenshot in the PR description)?

> * I'd prefer to keep adding labels manually. There are often bug reports that are none. I also use the labels to know if someone from the team looked at the issue. We'd lose that bit of information by auto labelling issues.

That seems reasonable, I'll remove it. A potential solution would be to add a "needs-triage" label explicitly but we could leave that for future as we decided (in the past) to not move forward with it.

This does mean that the main benefit of using issue templates would be to provide suggestions based on what the user is intending to open an issue about. For example, if it's a bug report, we're asking them to check existing issues and optionally include the playground link for better reproduction.

---

_@dhruvmanila reviewed on 2025-02-24 09:28_

---

_Review comment by @dhruvmanila on `.github/ISSUE_TEMPLATE/config.yml`:5 on 2025-02-24 09:28_

That's reasonable. I'll revert it.

---

_Review comment by @dhruvmanila on `.github/ISSUE_TEMPLATE/1_bug_report.yaml`:32 on 2025-02-24 09:30_

This is not an argument against your point but I think at least for the VS Code extension issues, typically users should and do open an issue at `astral-sh/ruff-vscode`. And, if not, I usually transfer it unless it's specifically relevant to the language server.

---

_@dhruvmanila reviewed on 2025-02-24 09:30_

---

_Comment by @dhruvmanila on 2025-02-24 09:39_

Preview link to try out: https://github.com/dhruvmanila/ruff-issue-templates/issues

---

_Review requested from @MichaReiser by @dhruvmanila on 2025-02-24 09:39_

---

_Comment by @MichaReiser on 2025-02-24 11:05_

> Are you suggesting an explicit entry because it is automatically created (refer to the screenshot in the PR description)?

Sorry, I didn't realize it was there. There are so many options! 

>  For example, if it's a bug report, we're asking them to check existing issues and optionally include the playground link for better reproduction.

Yeah, that's fair. At least if we use the same fields. We could consider explicitly asking for the rule code for bug reports.

---

_@MichaReiser approved on 2025-02-24 11:06_

---

_Comment by @dhruvmanila on 2025-02-25 10:59_

> Yeah, that's fair. At least if we use the same fields. We could consider explicitly asking for the rule code for bug reports.

Yeah, we could add multiple fields based on what's the most useful information that's required but I think it's best to iterate on it in follow-ups.

---

_Merged by @dhruvmanila on 2025-02-25 10:59_

---

_Closed by @dhruvmanila on 2025-02-25 10:59_

---

_Branch deleted on 2025-02-25 10:59_

---
