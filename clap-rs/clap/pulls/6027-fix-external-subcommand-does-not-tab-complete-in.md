---
number: 6027
title: "Fix: external_subcommand does not 'tab complete' in ZSH"
type: pull_request
state: merged
author: Alpha1337k
labels: []
assignees: []
merged: true
base: master
head: master
created_at: 2025-06-09T13:55:21Z
updated_at: 2025-06-09T19:05:14Z
url: https://github.com/clap-rs/clap/pull/6027
synced_at: 2026-01-10T01:28:26Z
---

# Fix: external_subcommand does not 'tab complete' in ZSH

---

_Pull request opened by @Alpha1337k on 2025-06-09 13:55_

Proposal for #6016.

Same fix is applied as in the linked issue. Added zsh only test since the issue only persists on zsh.

This fix allows default completions on external commands. Right now the completions are disabled, making for an annoying DX. The behaviour is now in line with bash, fish.



---

_@epage reviewed on 2025-06-09 13:58_

---

_Review comment by @epage on `clap_complete/tests/testsuite/zsh.rs`:85 on 2025-06-09 13:58_

Could you add this test in a commit prior to the fix, with it passing?

---

_Review comment by @epage on `clap_complete/tests/testsuite/zsh.rs`:86 on 2025-06-09 14:00_

There is an expectation that tests are repeated across all shells

---

_@epage reviewed on 2025-06-09 14:00_

---
