```yaml
number: 20935
title: "docs: revise Ruff setup instructions for Zed editor"
type: pull_request
state: merged
author: LoicRiegel
labels:
  - documentation
assignees: []
merged: true
base: main
head: docs/update-zed-editor-support-instructions
created_at: 2025-10-17T08:44:27Z
updated_at: 2025-11-06T18:30:05Z
url: https://github.com/astral-sh/ruff/pull/20935
synced_at: 2026-01-10T16:53:55Z
```

# docs: revise Ruff setup instructions for Zed editor

---

_Pull request opened by @LoicRiegel on 2025-10-17 08:44_

## Summary

Updated Ruff installation instructions for Zed editor.

## Details

Ruff support is now built in and does not require installing an extension anymore. https://github.com/zed-industries/zed/pull/37804

Also FYI this open issue is related: https://github.com/zed-industries/zed/issues/40266.

But I've just tried the last code snippet from the docs on a fresh Zed install on Windows and it worked well without installing any extension.


---

_Comment by @MichaReiser on 2025-10-20 08:13_

Thank you. There's probably more that we can remove, now that ruff is used as the default formatter and linter (it's enabled by default) as mentioned here https://zed.dev/docs/languages/python#code-formatting--linting

---

_Comment by @LoicRiegel on 2025-10-20 08:56_

I can have a look

---

_Comment by @injust on 2025-10-20 13:36_

Zed comes with formatting and linting out-of-the-box now.

If you want Ruff to organize imports:

```json
{
    "languages": {
        "Python": {
            "code_actions_on_format": { "source.organizeImports.ruff": true }
        }
    }
}
```

If you want Ruff to apply fixes (which includes organizing imports):

```json
{
    "languages": {
        "Python": {
            "code_actions_on_format": { "source.fixAll.ruff": true }
        }
    }
}
```

---

_Comment by @LoicRiegel on 2025-10-23 07:18_

I'll do a commit tonight.

Strange that Zed's documentation does not mention the `code_actions_on_format` anywhere in their python docs. But they have a nice tool to automatically update `settings.json` settings.
They also mention that the ruff configuration can be read from `ruff.toml`, but `.ruff.toml` and `pyproject.toml` also work (just tested, I wanted to be sure).
Maybe I'll make a small PR on their side too.

BTW do we want do add this in ty's documentation, or is this section https://docs.astral.sh/ty/editors/#other-editors enough?:
```jsonc
{
  "languages": {
    "Python": {
      "language_servers": [
        // Disable basedpyright and enable Ty, and otherwise
        // use the default configuration.
        "ty",
        "!basedpyright",
        "..."
      ]
    }
  }
}
```

---

_Comment by @injust on 2025-10-23 07:33_

> Strange that Zed's documentation does not mention the `code_actions_on_format` anywhere in their python docs. But they have a nice tool to automatically update `settings.json` settings.

`code_actions_on_format` was slated to be removed from Zed (in favour of `code_action`), but it broke some uses cases, so it was added back. That might be why.

---

_Comment by @MichaReiser on 2025-10-23 07:53_

> BTW do we want do add this in ty's documentation, or is this section [docs.astral.sh/ty/editors#other-editors](https://docs.astral.sh/ty/editors/#other-editors) enough?:

Adding Zed to other editors makes sense to me, considering that we have a dedicated tab for Zed on the editor settings page.

> I'll do a commit tonight.

Thank you!

---

_Comment by @LoicRiegel on 2025-10-24 09:42_

Okay tell me what you think. I put back "code_actions_on_format" since this is what has to be used at the moment apparently

---

_Label `documentation` added by @MichaReiser on 2025-10-27 08:20_

---

_Review requested from @dhruvmanila by @MichaReiser on 2025-11-03 14:42_

---

_Comment by @MichaReiser on 2025-11-04 20:39_

Sorry for the late review

> Okay tell me what you think. I put back "code_actions_on_format" since this is what has to be used at the moment apparently

Do you have a reference for this. Our recommendation is to run code actions (fixes and organize imports) **before** format but `code_actions_on_format` (at least according to the inline documentation), runs **after** formatting. 

That's why I'd prefer to keep our existing recommended configuration, unless there are issues with it that we aren't aware of.

---

_Review request for @dhruvmanila removed by @MichaReiser on 2025-11-04 20:39_

---

_Review requested from @MichaReiser by @MichaReiser on 2025-11-04 20:39_

---

_Comment by @injust on 2025-11-04 20:43_

> Do you have a reference for this. Our recommendation is to run code actions (fixes and organize imports) **before** format but `code_actions_on_format` (at least according to the inline documentation), runs **after** formatting.

See https://github.com/zed-industries/zed/pull/40409. Zed removed `code_actions_on_format`, then added it back and included a migrator that will convert this:

```json
{
    "languages": {
        "Python": {
            "formatter": [{ "code_action": "source.fixAll.ruff" }]
        }
    }
}
```

to this:

```json
{
    "languages": {
        "Python": {
            "code_actions_on_format": { "source.fixAll.ruff": true }
        }
    }
}
```

---

_Comment by @injust on 2025-11-04 20:45_

cc @probably-neb if you have more insight on the `code_action` -> `code_actions_on_format` migrator causing side effects for code action ordering before/after formatting.

---

_Comment by @probably-neb on 2025-11-04 21:46_

Thanks for the ping @injust 

> Do you have a reference for this. Our recommendation is to run code actions (fixes and organize imports) before format but code_actions_on_format (at least according to the inline documentation), runs after formatting.

Where are you seeing this documentation @MichaReiser? I'd love to fix it. I wrote the code that runs `code_actions_on_format` before the format steps specified in the `"formatter"` setting.

---

_Comment by @MichaReiser on 2025-11-04 21:58_

> Where are you seeing this documentation @MichaReiser? I'd love to fix it. I wrote the code that runs code_actions_on_format before the format steps specified in the "formatter" setting.

Oh great! That's exactly what we need. The description is from Zed's on hover tooltip

<img width="1297" height="466" alt="Screenshot 2025-11-04 at 16 57 32" src="https://github.com/user-attachments/assets/f54754c0-0316-4b70-ad22-b5d0b79504dd" />


---

_@MichaReiser approved on 2025-11-06 01:11_

Thank you

---

_Merged by @MichaReiser on 2025-11-06 01:11_

---

_Closed by @MichaReiser on 2025-11-06 01:11_

---

_Branch deleted on 2025-11-06 07:24_

---

_Comment by @probably-neb on 2025-11-06 18:26_

Docs are fixed in https://github.com/zed-industries/zed/pull/42128. Thanks all!

---
