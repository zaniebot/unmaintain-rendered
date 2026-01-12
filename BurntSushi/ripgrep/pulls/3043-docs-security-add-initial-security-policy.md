```yaml
number: 3043
title: "docs(security): add initial security policy"
type: pull_request
state: closed
author: janderssonse
labels: []
assignees: []
base: master
head: fix/add-security-policy
created_at: 2025-05-11T06:38:28Z
updated_at: 2025-05-11T13:09:34Z
url: https://github.com/BurntSushi/ripgrep/pull/3043
synced_at: 2026-01-12T18:23:15Z
```

# docs(security): add initial security policy

---

_@janderssonse_

This PR adds a SECURITY.md file, battle tested in other projects and orgs, (the construct is CC0 ie public domain, for example from here https://raw.githubusercontent.com/itiquette/git-provider-sync/refs/heads/main/SECURITY.md so just reuse)

A SECURITY.md would help anyone assessing the project for use, give a hint of how it handles critical no public security issues, and give anyone a clear instruction on how to report them non public.

**IE, for someone thinking about using ripgrep in an organization or privately it would give an extra trust factor.**

This policy basically says "send your findings, and we will see if we handle them, we will notify you".

Besides, being a good FOSS practice, makes the project look more professional and it is heavily supported by GitHub https://docs.github.com/en/communities/setting-up-your-project-for-healthy-contributions/creating-a-default-community-health-file etc as one of the community health files, so it will pop up automatically in the UI for the end user. 

Examples:
Security Tab in project front will be added automatically
![Skärmbild från 2025-05-11 05-57-27](https://github.com/user-attachments/assets/db47a7d5-1c70-400c-bbca-46f8949995b8)


Security Policy in the top right corner of UI will be added automatically

![Skärmbild från 2025-05-11 05-58-05](https://github.com/user-attachments/assets/85afc0d8-df10-4091-bfec-612560f72bf3)

Security Policy under Security Overview for the project will have the Security Policy green and enabled.
![Skärmbild från 2025-05-11 05-58-23](https://github.com/user-attachments/assets/3162ee7d-7990-4029-9186-0e2d0538d64b)

NOTE: there is a <...> in the text, where the preferred channel for reporting should be added I left that for you, (or tell me what to add there, and I'll rebase with that).

NOTE: I had this in multiple orgs and projects over the years. Only once I had a report, so I don't think one should be worry about getting to much reports from this, this is at least my experience.

---

_Comment by @BurntSushi on 2025-05-11 11:54_

I appreciate the effort and the idea here. But I don't think this is worth doing personally for ripgrep. And specifically, I do not want to deal with the overhead of confidentiality. I'm not being paid for this.

If there are people wanting to use ripgrep in an organization privately and this is the only thing that would make them trust ripgrep enough to do it, then they can reach out to me privately and discuss their concerns.

---

_Closed by @BurntSushi on 2025-05-11 11:54_

---

_Comment by @janderssonse on 2025-05-11 12:48_

Thanks for having a look!

A bit puzzled by the overhead answer, the purpose of the SECURITY.md is give a project less maintenance, as it is used is to let any users know how any security issues should be reported to the project (in public, in private, etc?). 

If that is preferred, one  could simplify the suggested SECURITY.md to something like  "Add an public issue about it",  and it would still fill it's purpose - easing maintenance by telling the end user what the preferred way of reporting security related issues are, before they ask.


---

_Comment by @BurntSushi on 2025-05-11 13:09_

I don't think we need an entire document just to tell people to file an issue.

---
