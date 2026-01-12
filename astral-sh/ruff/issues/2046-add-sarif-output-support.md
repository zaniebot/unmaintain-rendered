```yaml
number: 2046
title: Add SARIF output support
type: issue
state: closed
author: jawnsy
labels:
  - good first issue
  - core
assignees: []
created_at: 2023-01-21T01:07:44Z
updated_at: 2023-12-13T05:33:41Z
url: https://github.com/astral-sh/ruff/issues/2046
synced_at: 2026-01-12T15:54:42Z
```

# Add SARIF output support

---

_@jawnsy_

I did a cursory search of the code and didn't find anything related, so I believe this is a feature request. The only reference to "SARIF" that I found was a comment from a few weeks ago: https://github.com/charliermarsh/ruff/issues/1546#issuecomment-1371799394

**Summary**

Add a flag to output SARIF data during a run, instead of, or in addition to, the existing output.

**Description**

GitHub's CodeQL can ingest SARIF files during builds, so that we can track lint issues over time and surface issues inline on diffs. These can be uploaded during pull request builds as well as push event builds.

[The `trivy` code/container image scan tool supports SARIF format](https://github.com/aquasecurity/trivy-action#usage), and this is very helpful for open source projects (since CodeQL is free there) as well as for companies using CodeQL.

---

_Label `core` added by @charliermarsh on 2023-01-21 01:11_

---

_Comment by @charliermarsh on 2023-01-21 01:13_

Makes sense! I'll admit that I know nothing about the format so will have to look into it when I have some time.

Of course, would also make for a great PR if anyone is interested!

---

_Label `good first issue` added by @charliermarsh on 2023-01-21 01:13_

---

_Comment by @padmano on 2023-01-21 06:55_

I can work on this. 

---

_Comment by @edgarrmondragon on 2023-02-09 01:10_

@padmano are you working on this?

---

_Comment by @padmano on 2023-02-10 12:25_

Yes @edgarrmondragon. 

---

_Comment by @zanieb on 2023-04-23 16:52_

I'm looking into this a bit and figure I'll share some resources

There's a bandit formatter plugin at [microsoft/bandit-sarif-formatter](https://github.com/microsoft/bandit-sarif-formatter) — you can see the implementation details in [formatter.py](https://github.com/microsoft/bandit-sarif-formatter/blob/main/bandit_sarif_formatter/formatter.py). They lean on some [generated Python models](https://github.com/microsoft/sarif-python-om) and Microsoft also publishes a [CLI tool](https://github.com/microsoft/sarif-tools) which may be useful for testing.

The latest SARIF specification is available as a [JSON schema](https://github.com/oasis-tcs/sarif-spec/blob/main/Schemata/sarif-schema-2.1.0.json).

@padmano are you still actively working on this? Could you link to a branch here so people don't have to ask?

---

_Comment by @C-Hipple on 2023-10-10 15:54_

I've also been playing with this a bit lately.  My branch is here: https://github.com/astral-sh/ruff/compare/main...C-Hipple:dev-add-sarif-output-support?expand=1 

Approach so far was copy/paste the json emitter and then I'm going to just change up the formatting.  Once it's working i'll clean up the copy/paste and see what can easily be reused without getting too complicated.  Probably not too much will end up shared since sarif files group results per rule found, so `Serialize for ExpandedMessages` is going to look completely different. 

What I still need to figure out in Ruff is the best way to get all of the rules which are enabled, since `sarif` files expect the full list even if there aren't findings for each run.  I started in my last commit with a `with_applied_rules` builder method but it doesn't look like the `EmitterContext` has what I need, and I don't really want to re-parse the `pyproject.toml` file.  I haven't spent too much time looking around here yet, but any direction would be appreciated (maybe there's a global settings singleton i'm missing?)? 

---

_Comment by @C-Hipple on 2023-11-14 22:01_

^^ Found some time and resolved the issues above, now it's just about finding the time to format the json as the sarif format. 

---

_Comment by @C-Hipple on 2023-12-10 02:05_

https://github.com/astral-sh/ruff/pull/9078 PR is here

---

_Comment by @charliermarsh on 2023-12-10 02:08_

Thank you @C-Hipple! Look forward to reviewing. Should be able to turn this around pretty quickly.

---

_Comment by @C-Hipple on 2023-12-10 02:11_

Thank you @charliermarsh , no big rush though, I have it as a draft since I need to work out a few CI issues and possibly tweak a few other things

---

_Comment by @charliermarsh on 2023-12-13 05:33_

This merged as https://github.com/astral-sh/ruff/pull/9078. Thanks @C-Hipple!

---

_Closed by @charliermarsh on 2023-12-13 05:33_

---
