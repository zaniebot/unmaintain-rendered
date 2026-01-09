---
number: 12991
title: COM812 improvement
type: issue
state: closed
author: danpascu
labels: []
assignees: []
created_at: 2024-08-19T16:05:20Z
updated_at: 2024-08-20T15:58:56Z
url: https://github.com/astral-sh/ruff/issues/12991
synced_at: 2026-01-07T13:12:15-06:00
---

# COM812 improvement

---

_Issue opened by @danpascu on 2024-08-19 16:05_

Hi,

I made a comment a few days ago on a closed PR (#10111), but I'm not sure if anyone received any notification about it with the PR being closed since March and the branch deleted, so I'll paste it here. I've seen the thread related to that PR (#9216) so I'm aware of the discussion in there, this is just a suggestion I have about it. Maybe someone with a in-depth knowledge can chime in if this is possible.

Would it be possible to include this only for the case where arguments are all on one single line (in which case the comma is optional and not flagged by the linter) and consider all other cases where is more than one line to be equivalent with the one-argument-per-line case in which case it requires a comma. This way you do not need to worry about all the alternative formattings, which let's be frank can be many and unpredictable, and lump everything that is not a single line into the one-argument-per-line case. IMO this solves the most common use case where the formatter and the linter are fighting with each other and would cover the needs of most of the users. I believe this is a case where practicality trumps absolute correctness.


---

_Comment by @MichaReiser on 2024-08-20 07:15_

Hi @danpascu 

Would you mind sharing an example in #9216 and explaining how it is different from what has already been discussed in the thread? I would prefer to keep the entire discussion in that thread. 

---

_Closed by @MichaReiser on 2024-08-20 07:15_

---

_Comment by @danpascu on 2024-08-20 15:58_

I don't have any new examples to share. I'm not saying something that wasn't said before, in fact it was mentioned in various forms already by multiple people.

I'm just pointing out that allowing the linter to not require a comma in the single line case would solve a lot of friction between the formatter and linter by addressing the most common use case.

---
