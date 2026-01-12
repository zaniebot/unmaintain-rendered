```yaml
number: 2344
title: "Github Issue template: Plugins"
type: pull_request
state: closed
author: sbrugman
labels: []
assignees: []
base: main
head: gh-issue-template
created_at: 2023-01-30T09:00:39Z
updated_at: 2023-05-04T19:26:32Z
url: https://github.com/astral-sh/ruff/pull/2344
synced_at: 2026-01-12T15:55:08Z
```

# Github Issue template: Plugins

---

_@sbrugman_

Proposal to leverage Github [Issue Templates](https://docs.github.com/en/communities/using-templates-to-encourage-useful-issues-and-pull-requests/about-issue-and-pull-request-templates). Here I made an example for _plugin support_, but there are already a couple of other scenarios that we can cover in a structured way (rule requests, bugs/false positive/false negatives). The template is rendered into an easy-to-use [form](https://github.com/sbrugman/ruff/blob/gh-issue-template/.github/ISSUE_TEMPLATE/PLUGIN.yml).

The primary benefit would be that it takes a bit of load of triage (e.g. the `plugin` label is assigned automatically, and the field descriptions guide the user through the issue creation). An additional advantage is that issues are more complete and structured where possible, while users maintain the option to submit a free-format ticket.

---

_Comment by @charliermarsh on 2023-01-31 21:26_

I appreciate this but I think I want to let the free-form tickets ride for a little bit longer... We should do this at some point, but I see value in making it very easy for folks to file issues without too much overhead. I'm definitely willing to be convinced that I'm wrong, though -- perhaps the current experience is worse for users _and_ maintainers? Interested in feedback!

---

_Comment by @sbrugman on 2023-01-31 22:08_

Yeah, I think ruff is blessed with high-quality issues (superb community). This solution idea came from my experience with maintaining `pandas-profiling`, where issues were more often than not up to the minimum reproduction standards. 

Still, I think we could use the templates. Perhaps leave it at a single textarea, but with some additional pointers depending on the type? It automatically adds GitHub labels then. If you're still not convinced, just close this issue.

(Keen to experiment with ways that empower users/contributors to do admin such as add appropriate labels)

---

_Closed by @charliermarsh on 2023-05-04 19:26_

---
