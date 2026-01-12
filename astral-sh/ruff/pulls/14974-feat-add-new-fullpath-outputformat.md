```yaml
number: 14974
title: "feat: add new FullPath OutputFormat"
type: pull_request
state: closed
author: brupelo
labels: []
assignees: []
base: main
head: users/bpl/output-format-fullpaths
created_at: 2024-12-14T18:33:46Z
updated_at: 2024-12-15T16:01:10Z
url: https://github.com/astral-sh/ruff/pull/14974
synced_at: 2026-01-12T15:55:49Z
```

# feat: add new FullPath OutputFormat

---

_@brupelo_

## Summary

Hi, I'm new to Rust and have been working on a feature for that allows retrieving full paths (ie: --output-format=full-path). I've put some time into implementing it, but I'd appreciate it if someone familiar with the Rust or Ruff codebase could give me some feedback to ensure I'm on the right track. Also, is this the appropriate channel to ask about Ruff PR reviews? Thanks in advance!

Here's the issue https://github.com/astral-sh/ruff/issues/9417

## Test Plan

Hoping someone familiar with the codebase could guide me through... still trying to finish it up

---

_Comment by @github-actions[bot] on 2024-12-14 20:29_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.




---

_Comment by @MichaReiser on 2024-12-15 15:32_

Hi

Thanks for trying to implement this functionality yourself and for linking to the relevant issue. Unfortunately, I don't think a new output format is the right solution for the feature you need, and we deliberately only support a few output formats. It's also not entirely clear why you need absolute paths (other that your IDE doesn't understand relative paths but I'd like to learn more about this). 

I suggest to open a new issue where we can discuss your specific feature request and align on how this could be supported in Ruff. 

 



---

_Closed by @MichaReiser on 2024-12-15 15:32_

---

_Comment by @brupelo on 2024-12-15 16:01_

@MichaReiser Hey Micha, thanks for the feedback.

I agree with what you're saying. I was just reviewing and pushing my branch hoping for feedback on the PR, but you closed it without any review. I was trying to catch up and learn best practices in the codebase. Instead of adding a new output format, I decided to modify some TextEmitter subclasses to support printing relative or full paths, following the existing conventions. In some places, flags are used, and in others, flat attributes. Anyway, I've pushed again against latest origin/main.

At this point, I’ve managed to propagate the path-setting attribute to the printer through the flags, but I’ve hardcoded it for now and was waiting for advice on the best way to proceed with this.

Out of curiosity, I’ll open a new issue to discuss this further, but I was wondering: what is the typical procedure for getting a PR released effectively? Should an issue be opened, discussed, and accepted first? I’m still new to Rust and not very familiar with the most effective way to contribute, especially to ensure that everything makes it all the way to production.

That said, I don’t have unlimited time for this wonderful project, but I’m using this opportunity to learn Rust and make sure I can use Ruff in my IDE. Regarding relative paths, I have a variety of reasons for not being able to use them, and I really want to move away from the black + isort + autoflake + flake8 combo. My goal is to start using Ruff and adopting it within my organization.

I’ll go ahead and open the issue as you suggested.

---
