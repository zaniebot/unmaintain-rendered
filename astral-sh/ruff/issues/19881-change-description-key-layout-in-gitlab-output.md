```yaml
number: 19881
title: "Change \"description\" key layout in gitlab output format"
type: issue
state: closed
author: ppiasek
labels:
  - diagnostics
assignees: []
created_at: 2025-08-12T16:34:24Z
updated_at: 2025-08-13T15:19:27Z
url: https://github.com/astral-sh/ruff/issues/19881
synced_at: 2026-01-12T15:54:57Z
```

# Change "description" key layout in gitlab output format

---

_@ppiasek_

Hey! We are using Ruff in our project and have integrated it with Gitlab's codequality reports. Apart from that, we are using a few other tools using the same reports, and we have noticed that we struggle to find what kind of check is reported by Ruff in codequality report, because only the description is displayed there. 

Example: 
`Major - Missing docstring in public package in <file>`

This might be obvious for someone, but we are in the process of setting up a new repo and polishing rule configuration - removing rules that we do not like (we started from enabling all the possible rules). I noticed that pylint, on the other hand, displays it much better.

Example:
`Critical - E1120: No value for argument 'url' in function call in <file>`

The description from pylint contains the actual check name.

How does it look from the code? 

Right now, Ruff has it implemented like this: 
```
let value = json!({
                "check_name": check_name,
                "description": description,
                "severity": "major",
                "fingerprint": format!("{:x}", message_fingerprint),
                "location": {
                    "path": path,
                    "positions": {
                        "begin": start_location,
                        "end": end_location,
                    },
                },
            });
```

How I would imagine it:

```
let value = json!({
                "check_name": check_name,
                "description": check_name + " " + description, # sorry for the syntax, don't know Rust at all
                "severity": "major",
                "fingerprint": format!("{:x}", message_fingerprint),
                "location": {
                    "path": path,
                    "positions": {
                        "begin": start_location,
                        "end": end_location,
                    },
                },
            });
```

Optionally there could be a setting whether to display check_name + description or just description.

---

_Comment by @ntBre on 2025-08-12 17:38_

Thanks for the report! I've been working on our diagnostics recently, so this is good to know.

In our snapshot tests, the rule code is included in the `check_name` field:

https://github.com/astral-sh/ruff/blob/6f42c0d143460adf833caff139c11e9eaff7752d/crates/ruff/tests/snapshots/lint__output_format_gitlab.snap#L19-L24

Is that rendered anywhere by GitLab?

Edit: Sorry, I guess you already noticed that that's what the `check_name` is. I'm just surprised that it's not rendered with the description on GitLab.

---

_Label `diagnostics` added by @ntBre on 2025-08-12 17:38_

---

_Comment by @ntBre on 2025-08-12 17:53_

I was curious and set up a GitLab repo to test this out, and it looks just like you said:

<img width="429" height="128" alt="Image" src="https://github.com/user-attachments/assets/31c3a666-cd79-47bd-938d-e5d9dd8c3c36" />

I think it definitely makes sense to use a description like what you suggested!

---

_Comment by @MichaReiser on 2025-08-13 06:55_

Interesting (and somewhat surprising) design decision from GitLab. I agree, this change makes sense and should be a piece of cake for @ntBre 

---

_Closed by @ntBre on 2025-08-13 15:19_

---
