---
number: 6100
title: "Odd behavior of `ERA001` on code-like comments"
type: issue
state: open
author: tylerlaprade
labels:
  - bug
  - accepted
assignees: []
created_at: 2023-07-26T17:54:50Z
updated_at: 2023-12-15T18:23:27Z
url: https://github.com/astral-sh/ruff/issues/6100
synced_at: 2026-01-10T01:22:45Z
---

# Odd behavior of `ERA001` on code-like comments

---

_Issue opened by @tylerlaprade on 2023-07-26 17:54_

<img width="276" alt="image" src="https://github.com/astral-sh/ruff/assets/5475199/513b1292-880b-40d6-b2b9-e24529fdf7f5">

I understand how this could _look_ like code, although it's a bit noisy to have this flagged. Even if `django` were a variable, this would do nothing as a standalone expression.

That being said, why does it get flagged when the `=` is included, but not without it? I tested all 4 permutations of `<`/`>` and `=` or not.

---

_Comment by @charliermarsh on 2023-07-26 17:58_

It's just a quirk of the implementation, which comes from Eradicate. We detect snippets that contain `=`, but not `>` or `<`.

---

_Label `needs-decision` added by @charliermarsh on 2023-07-28 04:29_

---

_Label `needs-decision` removed by @charliermarsh on 2023-07-28 04:29_

---

_Label `accepted` added by @charliermarsh on 2023-07-28 04:29_

---

_Comment by @charliermarsh on 2023-07-28 04:29_

We can probably special-case these version marker comments (though low priority).

---

_Label `bug` added by @charliermarsh on 2023-07-28 04:29_

---

_Comment by @dbohdan on 2023-10-21 15:53_

I have encountered this false positive with ERA001:

```none
> head -n 5 search.py
#! /usr/bin/env python3
# External program requirements:
# - gum(1) (https://github.com/charmbracelet/gum)
# - less(1) (common)
# - rg(1) with PCRE2 support (`sudo apt install ripgrep`)
> ruff search.py
search.py:4:1: ERA001 Found commented-out code
Found 1 error.
```

---

_Comment by @ThiefMaster on 2023-11-18 23:42_

More false-positives on what doesn't even look like valid code:

```py
indico/modules/events/abstracts/blueprint.py:98:1: ERA001 Found commented-out code
    |
 96 |                  methods=('GET', 'POST'))
 97 |
 98 | # Emailing (management)
    | ^^^^^^^^^^^^^^^^^^^^^^^ ERA001
 99 | _bp.add_url_rule('/manage/abstracts/api/email-roles/metadata', 'api_email_abstract_roles_metadata',
100 |                  abstract_list.RHAbstractsAPIEmailAbstractRolesMetadata, methods=('POST',))
```

```py
indico/modules/attachments/models/folders.py:110:5: ERA001 Found commented-out code
    |
108 |     # relationship backrefs:
109 |     # - all_attachments (Attachment.folder)
110 |     # - legacy_mapping (LegacyAttachmentFolderMapping.folder)
    |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ ERA001
111 |
112 |     @property
    |
    = help: Remove commented-out code
```

---

_Comment by @ThiefMaster on 2023-11-18 23:44_

And an interesting case that's not an FP, but maybe commented-out code that has a TODO, FIXME, etc. comment in the same line should be allowed?

```py
indico/modules/events/registration/util.py:61:5: ERA001 Found commented-out code
   |
59 |     text: str
60 |     icon_name: str
61 |     # _: dataclasses.KW_ONLY  # TODO uncomment once we require python3.10+; until then consider everything below kw-only
   |     ^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^^ ERA001
62 |     weight: int = 0
63 |     dialog_title: str = None  # defaults to `text`
   |
   = help: Remove commented-out code
```

Clearly removing something like this would not be in the spirit of the rule which asks you to use source control in case you need to bring removed code back, instead of keeping a commented-out version. But when you *know* that you'll have to bring it back (hence the TODO comment), then it makes no sens to rely on your VCS for this.

---

_Comment by @dhruvmanila on 2023-11-20 19:19_

Those does look like code where line 98 could be a function / class call and so on. Although, I do think we should probably allow if it has a TODO comment on the commented line.

---

_Comment by @ThiefMaster on 2023-11-20 19:26_

I think the logic to determine what's code and what isn't should be pretty strict - you really don't want to have to put noqa on your comments just so they are considered comments. Even more so when it's a single-line comment. With multi-line it's probably less likely to get a block of valid Python code that's not actually code.

For things that look like valid function/class call statements, one possibility could be checking if the function name exists in the scope. If not, don't consider it a statement.

IMHO this kind of rule is the one where you should rather get false negative than false positives: There's no harm/annoyance in missing some commented-out code. But it's very annoying if your valid comments trigger this rule

---

_Referenced in [streamlit/streamlit#11210](../../streamlit/streamlit/pulls/11210.md) on 2025-04-28 12:01_

---

_Referenced in [astral-sh/ruff#17713](../../astral-sh/ruff/issues/17713.md) on 2025-04-29 15:10_

---
