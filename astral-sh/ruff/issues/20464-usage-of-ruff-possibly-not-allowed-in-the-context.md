```yaml
number: 20464
title: Usage of ruff possibly not allowed in the context of regulated open source projects (e.g. Eclipse Foundation Development Process)
type: issue
state: closed
author: benjamin-arfa
labels:
  - wish
assignees: []
created_at: 2025-09-18T09:52:45Z
updated_at: 2025-09-23T12:42:56Z
url: https://github.com/astral-sh/ruff/issues/20464
synced_at: 2026-01-12T15:54:57Z
```

# Usage of ruff possibly not allowed in the context of regulated open source projects (e.g. Eclipse Foundation Development Process)

---

_@benjamin-arfa_

### Question

#### Summary

We're developing an open source project under the Eclipse Foundation Development Process (https://www.eclipse.org/projects/dev_process/) and would love to adopt ruff as our Python linter. Ruff's performance and comprehensive feature set make it ideal for our needs. However, we're currently blocked by ClearlyDefined showing 0% licensing discovery for ruff (https://clearlydefined.io/definitions/pypi/pypi/-/ruff/0.9.7), which prevents our mandatory IP compliance review process from proceeding.

#### What Happened

The Eclipse Foundation requires projects to use ClearlyDefined as the authoritative source for license verification. While ruff is clearly MIT-licensed in your repository, ClearlyDefined's automated harvesting tools (ScanCode, FOSSology, etc.) have failed to detect any licensing information, resulting in a 0% licensing discovery score even if the licensing is declared in a LICENSE file. This creates an absolute blocker for Eclipse-governed (or even Apache for that matter) projects: our IP Team cannot even begin their review process without baseline licensing metadata from ClearlyDefined, regardless of how clear the licensing is in the source repository.

#### Suggestions

Would the ruff team be willing to help resolve this issue? I have the feeling, I should not be the only one asking this ...

### Version

any

---

_Label `question` added by @benjamin-arfa on 2025-09-18 09:52_

---

_Comment by @MichaReiser on 2025-09-18 12:13_

HI @benjamin-arfa 

I'm not familiar with clearlydefined.io. So I'm not quite sure what we need to do on our end for it to pick up the right license. It does seem to me that the ruff 0.9.7 entry has the correct license metadata populated. I tried to schedule a manual harvest for ruff 0.13 but I'm not sure if that was successful. 

Either way. I'm happy to accept a PR that helps clearlydefined.io to pickup the metadata. If this is a manual process, then I'm not sure if there's anything we can do on our end. It might then just boil down to someone from the community having to trigger the harvest every now and then.

---

_Comment by @MichaReiser on 2025-09-18 12:15_

Reading https://docs.clearlydefined.io/docs/get-involved/adding-sources I also get the impression that harvesting should *just work* for Ruff and if it isn't, than that seems to be more of an issue with clearlydefined.

---

_Comment by @MichaReiser on 2025-09-18 12:18_

I did find https://github.com/clearlydefined/crawler/issues/561 suggesting that it only harvests source distributions. I double checked and Ruff's source distribution contains the LICENSE file.

---

_Comment by @my1e5 on 2025-09-18 13:27_

> ClearlyDefined's automated harvesting tools (ScanCode, FOSSology, etc.) have failed to detect any licensing information, resulting in a 0% licensing discovery score

What do you mean by this? I just clicked on the link you supplied (https://clearlydefined.io/definitions/pypi/pypi/-/ruff/0.9.7) and it shows a score of 45? And it shows multiple declared licenses:

> Declared:
> 0BSD AND Apache-2.0 AND BSD-3-Clause AND MIT

<img width="1369" height="756" alt="Image" src="https://github.com/user-attachments/assets/fc7a23b2-fb18-4ce0-8c35-bb69d5c37685" />

---

_Renamed from "Usage of ruff not allowed under Eclipse Foundation Development Process" to "Usage of ruff possibly not allowed in the context of regulated open source projects (e.g. Eclipse Foundation Development Process)" by @benjamin-arfa on 2025-09-21 12:26_

---

_Comment by @benjamin-arfa on 2025-09-21 12:26_

TLDR; Adding a SDPX Identifier per file of the ruff package would be greatly appreciated by people working in regulated open source projects

As shown in the screenshot shared by @my1e5, the number of licenses detected or attributed per file during harvesting is very low. This indicates that most files are missing an SPDX header (see: [SPDX License IDs](https://spdx.dev/learn/handling-license-info/#:~:text=SPDX%20License%20IDs&text=In%20each%20file%20in%20your,GPL%2D2.0%2Dor%2Dlater)).
You can see this in the bottom-right corner of the screenshot, under "Files":
Licensed: 19
Attributed: 6
This low coverage makes using Ruff in an regulated open source project challenging, as it adds extra complexity when ensuring license compliance.

I hope this short explanation helps clear any misunderstanding.

---

_Comment by @MichaReiser on 2025-09-22 08:22_

> TLDR; Adding a SDPX Identifier per file of the ruff package would be greatly appreciated by people working in regulated open source projects

This seems fairly painful, and it's not clear to me why it should be necessary when there's a file-level license present. Is there any guidance from the clearlydefined project on how to get a 100 score? I tried to find one but couldn't.

---

_Comment by @benjamin-arfa on 2025-09-23 09:17_

The alternatives would be to, either : 
- commit data to clearly defined, to make the licensing verifiable automatically (PR to https://github.com/clearlydefined/curated-data)
- create a ticket at the Eclipse Foundation team for reviewing ruff (if you would like any version to be used by more OSS Projects under the Eclipse Foundation Development Process) - https://gitlab.eclipse.org/eclipsefdn/emo-team/iplab/-/issues
- or wait for an Eclipse Project to use any of the ruff versions, getting it through the Compliance Team so that it is reusable by other projects. Here is an example (with link to the gitlab) of how such a ticket looks like : https://gitlab.eclipse.org/eclipsefdn/emo-team/iplab/-/issues/23278

Given we were under time pressure for a release, we switched back to black and pylint, which were previously accepted software by the Eclipse Foundation. I hope you find my comment above helpful, and that it would help for a wider adoption of ruff as a MOA Tooling 😃 



---

_Label `question` removed by @MichaReiser on 2025-09-23 09:40_

---

_Label `wish` added by @MichaReiser on 2025-09-23 09:40_

---

_Comment by @MichaReiser on 2025-09-23 09:40_

Thanks for listing all the options. I'm open to improving the situation I'm just a bit confused why e.g. black is accepted when it also doesn't have SPDX in every file. 

Either way. It seems worthwhile to look into this and getting help from someone who knows what needs to be done would be great because I don't have the necessary knowledge :)

---

_Comment by @benjamin-arfa on 2025-09-23 09:49_

@MichaReiser Sorry for tagging, but I just wanted to get your attention to answer to your first paragraph.

Certain versions of `black` and `pylint` have been requested by another Eclipse Project (I think it was [bazel](https://github.com/eclipse-score/bazel-tools-python)) and have been approved, hence those tools being easier to integrate in our projects.

Here are the tickets for your documentation : 
- black : https://gitlab.eclipse.org/eclipsefdn/emo-team/iplab/-/issues/22870
- pylint : https://gitlab.eclipse.org/eclipsefdn/emo-team/iplab/-/issues/22869

---

_Comment by @MichaReiser on 2025-09-23 09:51_

@benjamin-arfa could you request Ruff to be added or is this something that must be done by a maintainer (I'm asking because you seem very excited and knowledgeable in this area and seem a good fit to drive this conversation)

---

_Comment by @benjamin-arfa on 2025-09-23 09:55_

@MichaReiser No problem — happy to help!
Could you let me know which version you’d like me to request? Should I hold off until a 1.0 release, or do you think the current version is stable enough for other projects to start using officially?

---

_Comment by @MichaReiser on 2025-09-23 10:01_

Thank you! 

I suggest requesting it for the current version. It's stable and widely used by the ecosystem.

---

_Comment by @benjamin-arfa on 2025-09-23 10:06_

Hey there !
Here is the IP gitlab issue : https://gitlab.eclipse.org/eclipsefdn/emo-team/iplab/-/issues/23278#note_5175196 for you to follow.

---

_Comment by @MichaReiser on 2025-09-23 11:20_

Awesome, thank you

---

_Comment by @rahulmohang on 2025-09-23 11:48_

Hi, 

I am from the Eclipse Foundation. Thank you for the perspectives. We will discuss this further in the issue that Benjamin raised. If all agree, we may close this issue. 

---

_Comment by @benjamin-arfa on 2025-09-23 12:35_

Hey @MichaReiser,
actually ruff just got approved https://gitlab.eclipse.org/eclipsefdn/emo-team/iplab/-/issues/22867 by the Eclipse Foundation for internal use (given there was a GPL-1.0-or-later license in there). I think it is good enough for now 👍 

---

_Closed by @benjamin-arfa on 2025-09-23 12:35_

---

_Comment by @MichaReiser on 2025-09-23 12:42_

Thank you for getting Ruff approved. I hope you can now use it in your project.

---
