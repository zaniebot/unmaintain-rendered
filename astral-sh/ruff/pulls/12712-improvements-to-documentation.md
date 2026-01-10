```yaml
number: 12712
title: Improvements to documentation
type: pull_request
state: merged
author: eth3lbert
labels:
  - documentation
assignees: []
merged: true
base: main
head: doc-copy-console
created_at: 2024-08-06T09:49:06Z
updated_at: 2024-08-12T07:21:07Z
url: https://github.com/astral-sh/ruff/pull/12712
synced_at: 2026-01-10T21:38:31Z
```

# Improvements to documentation

---

_Pull request opened by @eth3lbert on 2024-08-06 09:49_

## Summary

As we did in https://github.com/astral-sh/uv/pull/5397, we improve the copy behavior for documentation in this PR.



---

_Review requested from @MichaReiser by @eth3lbert on 2024-08-06 09:49_

---

_@eth3lbert reviewed on 2024-08-06 09:50_

---

_Review comment by @eth3lbert on `docs/requirements.txt`:1 on 2024-08-06 09:50_

I had an issue before updating the dependencies. Now these dependencies should be the same as in uv.

---

_Comment by @eth3lbert on 2024-08-06 10:07_

\cc @zanieb 

---

_Comment by @eth3lbert on 2024-08-06 10:10_

(I don't have access to mkdocs insider, so I don't change insider-related dependencies.)

---

_Comment by @github-actions[bot] on 2024-08-06 10:21_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_box.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>

### Formatter (preview)
ℹ️ ecosystem check **encountered format errors**. (no format changes; 1 project error)

<details><summary><a href="https://github.com/openai/openai-cookbook">openai/openai-cookbook</a> (error)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

```
warning: Detected debug build without --no-cache.
error: Failed to parse examples/Chat_finetuning_data_prep.ipynb:6:18:25: Unparenthesized generator expression cannot be used here
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_box.ipynb:13:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_confluence.ipynb:15:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_gmail.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_jira.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_notion.ipynb:15:1:1: Expected an expression
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_doc.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sharepoint_text.ipynb:28:1:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_action_sql_database.ipynb:2:2:5: Simple statements must be separated by newlines or semicolons
error: Failed to parse examples/chatgpt/gpt_actions_library/gpt_middleware_azure_function.ipynb:37:1:13: Simple statements must be separated by newlines or semicolons
```

</p>
</details>




---

_Review comment by @MichaReiser on `README.md`:121 on 2024-08-06 19:43_

Does this change how github or pypi renders the readme?

---

_Review comment by @MichaReiser on `docs/js/extra.js`:33 on 2024-08-06 19:45_

What's the motivation for using an observer? It seems pretty heavy to me to clean up some links. Considering that this code has to run on every scroll (with debounce). Could we instead run this once on page load?

---

_@MichaReiser reviewed on 2024-08-06 19:48_

Nice!

I think the updated dependencies are creating trouble in our documentation generation script (see CI).

Is it intentional that this PR updates markdown files that aren't hosted on the website? 

---

_@dhruvmanila reviewed on 2024-08-07 02:11_

---

_Review comment by @dhruvmanila on `docs/requirements.txt`:1 on 2024-08-07 02:11_

Are there any changes which requires the dependency upgrade? If not, I'd prefer to keep the upgrades separate as they could require some changes in the config / docs.

---

_Label `documentation` added by @MichaReiser on 2024-08-07 05:49_

---

_@eth3lbert reviewed on 2024-08-07 06:57_

---

_Review comment by @eth3lbert on `docs/requirements.txt`:1 on 2024-08-07 06:57_

This PR primarily introduce extra js for the copy button.

Without upgrading dependencies, if you try to start the server, you will encounter a `jinja2.exceptions.UndefinedError: 'mkdocs.config.config_options.ExtraScriptValue object' has no attribute 'endswith'` error.

This issue has been fixed in a newer version, as seen in the release notes: https://www.mkdocs.org/about/release-notes/#version-152-2023-08-02. Therefore, I believe it's best to also upgrade the dependencies to match the version used by uv.

---

_Comment by @MichaReiser on 2024-08-07 06:59_

I can look into the mkdocs failure once we answered all other questions.

---

_@eth3lbert reviewed on 2024-08-07 07:02_

---

_Review comment by @eth3lbert on `README.md`:121 on 2024-08-07 07:02_

Good point!  This does change how it appears on github (and maybe pypi too), but readme on those platforms wouldn't benefit from the patch we introduced in this PR. So, the dollar sign will still be displayed, and we can't just ignore it when copying.

Therefore, we probably shouldn't include this change in the `README.md`, right?

---

_Review comment by @eth3lbert on `docs/js/extra.js`:33 on 2024-08-07 07:04_

Per https://github.com/astral-sh/uv/pull/5397#issuecomment-2260684897.

---

_@eth3lbert reviewed on 2024-08-07 07:04_

---

_@MichaReiser reviewed on 2024-08-07 07:11_

---

_Review comment by @MichaReiser on `README.md`:121 on 2024-08-07 07:11_

Yeah. I think we should only change the markdown files in the docs folder. 

---

_@eth3lbert reviewed on 2024-08-07 07:12_

---

_Review comment by @eth3lbert on `README.md`:121 on 2024-08-07 07:12_

~Hmm, actually it seems we only want to apply the changes to files in the `crates/` or `docs/` directories?~

Yeah, we should just change files that are hosted on site.

---

_Review requested from @MichaReiser by @eth3lbert on 2024-08-07 07:21_

---

_@MichaReiser reviewed on 2024-08-07 07:41_

---

_Review comment by @MichaReiser on `docs/requirements.txt`:1 on 2024-08-07 07:41_

Interesting. It seems that `uv`'s insider version is also fixed to `mkdocs==1.5.0`. 

---

_@dhruvmanila reviewed on 2024-08-07 07:44_

---

_Review comment by @dhruvmanila on `docs/requirements.txt`:1 on 2024-08-07 07:44_

I think it's `mkdocs>=1.5.0` which would install `1.6.0` if possible: https://github.com/astral-sh/uv/blob/d0b16f90185ca7783d114306ade1626156ea6e29/docs/requirements.in#L2

---

_Comment by @MichaReiser on 2024-08-07 07:44_

I upgraded the insider version but it breaks some styling. For example, the tab headers no longer use the correct colors

![image](https://github.com/user-attachments/assets/ea5a199c-155f-411f-8cf0-1361d61f8837)


---

_Comment by @MichaReiser on 2024-08-07 07:49_

For some reason, I'm unable to scroll to the end of the navigation on localhost but it works fine in production. I yet have to verify if this is due to the upgrade

![image](https://github.com/user-attachments/assets/d40ffc9c-4467-4586-bfd0-2544d8674944)


---

_Comment by @MichaReiser on 2024-08-07 07:50_

I clicked through the page and most things look okay after the upgrade but there are some regression that needs fixing before we can ship this. I would also like @charliermarsh or @zanieb  to have a look at this. They'll know better why the insider version on uv is out of sync

This is the `requirements-insiders.txt` that i used

```
PyYAML==6.0.1
black==23.10.0
mkdocs==1.6.0
mkdocs-material @ git+ssh://git@github.com/astral-sh/mkdocs-material-insiders.git@2fcb87fe9f685cae13e7161daf459177d2da08c0
mkdocs-redirects==1.2.1
mdformat==0.7.17
mdformat-mkdocs==3.0.0
mdformat-admon==2.0.6

```

---

_Comment by @eth3lbert on 2024-08-07 08:07_

> I upgraded the insider version but it breaks some styling. For example, the tab headers no longer use the correct colors
> 
> ![image](https://private-user-images.githubusercontent.com/1203881/355722201-ea5a199c-155f-411f-8cf0-1361d61f8837.png?jwt=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3MjMwMTY5NjksIm5iZiI6MTcyMzAxNjY2OSwicGF0aCI6Ii8xMjAzODgxLzM1NTcyMjIwMS1lYTVhMTk5Yy0xNTVmLTQxMWYtOGNmMC0xMzYxZDYxZjg4MzcucG5nP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI0MDgwNyUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNDA4MDdUMDc0NDI5WiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9Yzg1ZTM5MDE3NjAyMDBmMTliZDgwMTg1MjY4NGEyZTg1OGZkZGQwZTZjMzIxNzY1ZDUwZDY2YWRhNWYyN2M2MSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmYWN0b3JfaWQ9MCZrZXlfaWQ9MCZyZXBvX2lkPTAifQ._DNInmOfQMzB9HD_Dbihcpc4KalKonOVwFtyUU3qVkc)

Yeah, I could also see this happening locally without using the insider version.
It seems like it used to use `color: var(--md-accent-fg-color);` but now uses `color: var(--md-default-fg-color);` afterwards.

And this likely didn't happen with `mkdocs-material==9.1.18`.

---

_@MichaReiser reviewed on 2024-08-09 15:49_

---

_Review comment by @MichaReiser on `docs/requirements.txt`:1 on 2024-08-09 15:49_

Yes, but not for the insiders version? It uses the same commit as ruff. https://github.com/astral-sh/uv/blob/d0b16f90185ca7783d114306ade1626156ea6e29/docs/requirements-insiders.txt#L71-L76

---

_@MichaReiser reviewed on 2024-08-09 16:19_

---

_Review comment by @MichaReiser on `docs/requirements.txt`:1 on 2024-08-09 16:19_

What I understand is that this issue was fixed in 1.5.2. I would prefer if we upgraded to the latest `1.5.x` version instead of bumping the minor as part of this PR.

---

_@eth3lbert reviewed on 2024-08-09 16:24_

---

_Review comment by @eth3lbert on `docs/requirements.txt`:1 on 2024-08-09 16:24_

Fair enough!

---

_@MichaReiser reviewed on 2024-08-09 16:25_

---

_Review comment by @MichaReiser on `docs/requirements.txt`:1 on 2024-08-09 16:25_

And I hope that it solves all regressions so that we can merge this PR :) 

---

_@eth3lbert reviewed on 2024-08-09 16:28_

---

_Review comment by @eth3lbert on `docs/requirements.txt`:1 on 2024-08-09 16:28_

Since I don't have access to the insider version, I can't help much if regressions occur. 

---

_Review comment by @eth3lbert on `docs/js/extra.js`:28 on 2024-08-09 16:31_

Should this be removed before merging, or is it intentional?

---

_@eth3lbert reviewed on 2024-08-09 16:31_

---

_Review comment by @eth3lbert on `docs/js/extra.js`:43 on 2024-08-09 16:31_

ditto.

---

_@eth3lbert reviewed on 2024-08-09 16:31_

---

_@MichaReiser reviewed on 2024-08-09 16:37_

---

_Review comment by @MichaReiser on `docs/js/extra.js`:28 on 2024-08-09 16:37_

Oops no. I'm sorry. That's absolutely not intentional 

---

_Comment by @MichaReiser on 2024-08-12 07:13_

The trick is to not upgrade mkdocs-material

---

_Merged by @MichaReiser on 2024-08-12 07:17_

---

_Closed by @MichaReiser on 2024-08-12 07:17_

---

_Comment by @eth3lbert on 2024-08-12 07:20_

Indeed! I also found it https://github.com/astral-sh/ruff/pull/12712#issuecomment-2272874589.

I love the improvement you've introduced in the latest commit. 👍

---

_Branch deleted on 2024-08-12 07:21_

---
