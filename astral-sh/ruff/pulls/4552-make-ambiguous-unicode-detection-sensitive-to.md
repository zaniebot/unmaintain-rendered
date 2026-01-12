```yaml
number: 4552
title: "Make ambiguous-unicode detection sensitive to 'word' context"
type: pull_request
state: merged
author: charliermarsh
labels: []
assignees: []
merged: true
base: main
head: charlie/boundaries
created_at: 2023-05-20T20:51:47Z
updated_at: 2023-05-22T14:58:29Z
url: https://github.com/astral-sh/ruff/pull/4552
synced_at: 2026-01-12T15:55:15Z
```

# Make ambiguous-unicode detection sensitive to 'word' context

---

_@charliermarsh_

## Summary

This PR upgrades our ambiguous-unicode detection to avoid some false positives by borrowing techniques from VS Code.  

If you look at VS Code (https://github.com/microsoft/vscode/issues/143720#issuecomment-1048757234, https://github.com/microsoft/vscode/issues/143796#issuecomment-1049773616), you'll see that their logic for flagging ambiguous unicode is more nuanced than ours, in that they avoid flagging all-unicode words that contain _non_-ambiguous unicode characters. This is helpful for avoiding false positives like `"Русский"`, which was brought up in a discussion that led me to look into this further.

Closes #4534.

Closes #3947.

---

_Review requested from @MichaReiser by @charliermarsh on 2023-05-20 20:52_

---

_Review requested from @konstin by @charliermarsh on 2023-05-20 20:52_

---

_Comment by @github-actions[bot] on 2023-05-20 21:09_

## PR Check Results
### Ecosystem
ℹ️ ecosystem check **detected changes**. (+3, -69, 0 error(s))

<details><summary>airflow (+0, -4)</summary>
<p>

```diff
- tests/executors/test_kubernetes_executor.py:76:15: RUF001 [*] String contains ambiguous unicode character `К` (did you mean `K`?)
- tests/executors/test_kubernetes_executor.py:76:16: RUF001 [*] String contains ambiguous unicode character `о` (did you mean `o`?)
- tests/executors/test_kubernetes_executor.py:76:22: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- tests/executors/test_kubernetes_executor.py:76:23: RUF001 [*] String contains ambiguous unicode character `р` (did you mean `p`?)
```

</p>
</details>
<details><summary>disnake (+3, -0)</summary>
<p>

```diff
+ disnake/enums.py:749:30: RUF100 [*] Unused `noqa` directive (unused: `RUF001`)
+ disnake/enums.py:757:25: RUF100 [*] Unused `noqa` directive (unused: `RUF001`)
+ disnake/enums.py:803:31: RUF100 [*] Unused `noqa` directive (unused: `RUF001`)
```

</p>
</details>
<details><summary>zulip (+0, -65)</summary>
<p>

```diff
- tools/linter_lib/custom_check.py:546:81: RUF001 [*] String contains ambiguous unicode character `Α` (did you mean `A`?)
- zerver/tests/test_email_mirror.py:137:24: RUF001 [*] String contains ambiguous unicode character `Т` (did you mean `T`?)
- zerver/tests/test_email_mirror.py:137:25: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_email_mirror.py:137:26: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
- zerver/tests/test_email_mirror.py:137:28: RUF001 [*] String contains ambiguous unicode character `о` (did you mean `o`?)
- zerver/tests/test_i18n.py:103:21: RUF001 [*] String contains ambiguous unicode character `Р` (did you mean `P`?)
- zerver/tests/test_i18n.py:103:22: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_i18n.py:103:23: RUF001 [*] String contains ambiguous unicode character `г` (did you mean `r`?)
- zerver/tests/test_i18n.py:103:25: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
- zerver/tests/test_i18n.py:103:27: RUF001 [*] String contains ambiguous unicode character `р` (did you mean `p`?)
- zerver/tests/test_i18n.py:103:28: RUF001 [*] String contains ambiguous unicode character `у` (did you mean `y`?)
- zerver/tests/test_i18n.py:103:29: RUF001 [*] String contains ambiguous unicode character `ј` (did you mean `j`?)
- zerver/tests/test_i18n.py:103:31: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_i18n.py:115:21: RUF001 [*] String contains ambiguous unicode character `Р` (did you mean `P`?)
- zerver/tests/test_i18n.py:115:22: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_i18n.py:115:23: RUF001 [*] String contains ambiguous unicode character `г` (did you mean `r`?)
- zerver/tests/test_i18n.py:115:25: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
- zerver/tests/test_i18n.py:115:27: RUF001 [*] String contains ambiguous unicode character `р` (did you mean `p`?)
- zerver/tests/test_i18n.py:115:28: RUF001 [*] String contains ambiguous unicode character `у` (did you mean `y`?)
- zerver/tests/test_i18n.py:115:29: RUF001 [*] String contains ambiguous unicode character `ј` (did you mean `j`?)
- zerver/tests/test_i18n.py:115:31: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_i18n.py:131:21: RUF001 [*] String contains ambiguous unicode character `Р` (did you mean `P`?)
- zerver/tests/test_i18n.py:131:22: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_i18n.py:131:23: RUF001 [*] String contains ambiguous unicode character `г` (did you mean `r`?)
- zerver/tests/test_i18n.py:131:25: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
- zerver/tests/test_i18n.py:131:27: RUF001 [*] String contains ambiguous unicode character `р` (did you mean `p`?)
- zerver/tests/test_i18n.py:131:28: RUF001 [*] String contains ambiguous unicode character `у` (did you mean `y`?)
- zerver/tests/test_i18n.py:131:29: RUF001 [*] String contains ambiguous unicode character `ј` (did you mean `j`?)
- zerver/tests/test_i18n.py:131:31: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_link_embed.py:660:54: RUF001 [*] String contains ambiguous unicode character `ν` (did you mean `v`?)
- zerver/tests/test_link_embed.py:660:56: RUF001 [*] String contains ambiguous unicode character `ι` (did you mean `i`?)
- zerver/tests/test_markdown.py:2828:46: RUF001 [*] String contains ambiguous unicode character `р` (did you mean `p`?)
- zerver/tests/test_markdown.py:2828:49: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_markdown.py:2831:24: RUF001 [*] String contains ambiguous unicode character `р` (did you mean `p`?)
- zerver/tests/test_markdown.py:2831:27: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_message_send.py:2496:24: RUF001 [*] String contains ambiguous unicode character `Р` (did you mean `P`?)
- zerver/tests/test_message_send.py:2496:25: RUF001 [*] String contains ambiguous unicode character `о` (did you mean `o`?)
- zerver/tests/test_message_send.py:2496:26: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
- zerver/tests/test_message_send.py:2496:27: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
- zerver/tests/test_upload.py:646:27: RUF001 [*] String contains ambiguous unicode character `З` (did you mean `3`?)
- zerver/tests/test_upload.py:646:29: RUF001 [*] String contains ambiguous unicode character `р` (did you mean `p`?)
- zerver/tests/test_upload.py:646:30: RUF001 [*] String contains ambiguous unicode character `а` (did you mean `a`?)
- zerver/tests/test_upload.py:646:32: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/tests/test_upload.py:646:36: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zerver/webhooks/opencollective/tests.py:12:35: RUF001 [*] String contains ambiguous unicode character `υ` (did you mean `u`?)
- zerver/webhooks/opencollective/tests.py:12:38: RUF001 [*] String contains ambiguous unicode character `ρ` (did you mean `p`?)
- zerver/webhooks/opencollective/tests.py:12:42: RUF001 [*] String contains ambiguous unicode character `Κ` (did you mean `K`?)
- zerver/webhooks/opencollective/tests.py:12:43: RUF001 [*] String contains ambiguous unicode character `υ` (did you mean `u`?)
- zerver/webhooks/opencollective/tests.py:12:44: RUF001 [*] String contains ambiguous unicode character `ρ` (did you mean `p`?)
- zerver/webhooks/opencollective/tests.py:12:45: RUF001 [*] String contains ambiguous unicode character `ι` (did you mean `i`?)
- zerver/webhooks/opencollective/tests.py:12:46: RUF001 [*] String contains ambiguous unicode character `α` (did you mean `a`?)
- zerver/webhooks/opencollective/tests.py:12:49: RUF001 [*] String contains ambiguous unicode character `ν` (did you mean `v`?)
- zerver/webhooks/opencollective/tests.py:12:50: RUF001 [*] String contains ambiguous unicode character `ο` (did you mean `o`?)
- zilencer/management/commands/populate_db.py:471:22: RUF001 [*] String contains ambiguous unicode character `А` (did you mean `A`?)
- zilencer/management/commands/populate_db.py:471:24: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zilencer/management/commands/populate_db.py:471:26: RUF001 [*] String contains ambiguous unicode character `с` (did you mean `c`?)
- zilencer/management/commands/populate_db.py:471:27: RUF001 [*] String contains ambiguous unicode character `а` (did you mean `a`?)
- zilencer/management/commands/populate_db.py:471:30: RUF001 [*] String contains ambiguous unicode character `р` (did you mean `p`?)
- zilencer/management/commands/populate_db.py:890:48: RUF003 [*] Comment contains ambiguous unicode character `е` (did you mean `e`?)
- zilencer/management/commands/populate_db.py:890:52: RUF003 [*] Comment contains ambiguous unicode character `е` (did you mean `e`?)
- zilencer/management/commands/populate_db.py:892:90: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zilencer/management/commands/populate_db.py:892:94: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zilencer/management/commands/populate_db.py:906:23: RUF001 [*] String contains ambiguous unicode character `о` (did you mean `o`?)
- zilencer/management/commands/populate_db.py:906:29: RUF001 [*] String contains ambiguous unicode character `е` (did you mean `e`?)
- zilencer/management/commands/populate_db.py:906:30: RUF001 [*] String contains ambiguous unicode character `р` (did you mean `p`?)
```

</p>
</details>
Rules changed: 3

| Rule | Changes | Additions | Removals |
| ---- | ------- | --------- | -------- |
| RUF001 | 67 | 0 | 67 |
| RUF100 | 3 | 3 | 0 |
| RUF003 | 2 | 0 | 2 |

### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.04     14.5±0.28ms     2.8 MB/sec    1.00     13.9±0.05ms     2.9 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.4±0.01ms     4.9 MB/sec    1.00      3.4±0.01ms     4.9 MB/sec
linter/all-rules/numpy/globals.py          1.00    417.6±2.09µs     7.1 MB/sec    1.00    418.2±0.49µs     7.1 MB/sec
linter/all-rules/pydantic/types.py         1.00      5.8±0.01ms     4.4 MB/sec    1.00      5.8±0.02ms     4.4 MB/sec
linter/default-rules/large/dataset.py      1.01      6.7±0.02ms     6.1 MB/sec    1.00      6.6±0.01ms     6.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00   1439.9±5.48µs    11.6 MB/sec    1.01  1449.0±19.49µs    11.5 MB/sec
linter/default-rules/numpy/globals.py      1.00    159.1±0.22µs    18.5 MB/sec    1.01    160.8±0.55µs    18.3 MB/sec
linter/default-rules/pydantic/types.py     1.00      3.0±0.02ms     8.5 MB/sec    1.00      3.0±0.01ms     8.5 MB/sec
parser/large/dataset.py                    1.00      5.1±0.01ms     7.9 MB/sec    1.02      5.2±0.00ms     7.8 MB/sec
parser/numpy/ctypeslib.py                  1.00   1014.6±0.71µs    16.4 MB/sec    1.02   1038.9±0.50µs    16.0 MB/sec
parser/numpy/globals.py                    1.00    104.1±0.15µs    28.3 MB/sec    1.01    105.1±0.72µs    28.1 MB/sec
parser/pydantic/types.py                   1.00      2.2±0.00ms    11.5 MB/sec    1.02      2.3±0.00ms    11.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
linter/all-rules/large/dataset.py          1.00     21.4±0.53ms  1945.7 KB/sec    1.02     21.8±0.72ms  1907.3 KB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.3±0.22ms     3.1 MB/sec    1.00      5.3±0.21ms     3.1 MB/sec
linter/all-rules/numpy/globals.py          1.00   621.0±31.51µs     4.8 MB/sec    1.00   619.3±36.71µs     4.8 MB/sec
linter/all-rules/pydantic/types.py         1.00      8.9±0.31ms     2.9 MB/sec    1.01      9.1±0.32ms     2.8 MB/sec
linter/default-rules/large/dataset.py      1.00     10.5±0.38ms     3.9 MB/sec    1.01     10.5±0.29ms     3.9 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.2±0.12ms     7.6 MB/sec    1.00      2.2±0.09ms     7.6 MB/sec
linter/default-rules/numpy/globals.py      1.00   245.9±15.68µs    12.0 MB/sec    1.02   251.8±21.84µs    11.7 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.6±0.16ms     5.5 MB/sec    1.01      4.7±0.17ms     5.5 MB/sec
parser/large/dataset.py                    1.10      8.8±0.19ms     4.6 MB/sec    1.00      8.0±0.17ms     5.1 MB/sec
parser/numpy/ctypeslib.py                  1.08  1661.9±57.68µs    10.0 MB/sec    1.00  1536.5±89.79µs    10.8 MB/sec
parser/numpy/globals.py                    1.04    164.2±7.51µs    18.0 MB/sec    1.00    157.8±6.21µs    18.7 MB/sec
parser/pydantic/types.py                   1.06      3.7±0.10ms     6.8 MB/sec    1.00      3.5±0.14ms     7.3 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:27 on 2023-05-22 07:20_

Do we have a similar rule for flagging unicode characters in identifiers?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:186 on 2023-05-22 07:23_

Can we add a link pointing to that logic?

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:221 on 2023-05-22 07:24_

Unrelated to this PR: I just noticed this now. Our current approach of creating the diagnostic first and then filtering it out is kind of expensive because it means that we format (and allocate) all messages to then immediately drop them again. 

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:111 on 2023-05-22 07:26_

Can we use a more specific word for `buffer`? I struggle understanding the logic because the name doesn't tell me anything about what data gets stored in the buffer or its intended lifetime.

E.g. word_diagnostics

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:124 on 2023-05-22 07:31_

Nit: This could be slightly faster if we only store the `TextRange`, confusable, and represent in the `buffer` and delay the generation of the diagnostics until here. 

---

_@MichaReiser approved on 2023-05-22 07:33_

---

_Review comment by @sladyn98 on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:124 on 2023-05-22 07:55_

Would that mean we would have to pass the confusable and represent to this function, or would there be any other way to add it to the buffer. (just trying to understand the codebase a bit more)

---

_@sladyn98 reviewed on 2023-05-22 07:55_

---

_Review comment by @MichaReiser on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:124 on 2023-05-22 08:44_

The idea would be to change `buffer` to `Vec<Candidate>` where `Candidate` is defined as 

```rust
struct Candidate {
	range: TextRange,
	confusable: char,
	representant: char
}
```

And change this loop here to:

```rust
if !word_candidates.is_empty() {
	if flags.is_candidate_word() {
		for candidate in word_candidates.drain() {
			let kind = match context { ... };
			if settings.enabled(kind) {
				diagnostics.push(Diagnostic {  ... })
			}
		}
	}
	
	word_candidates.clear();
}

```

---

_@MichaReiser reviewed on 2023-05-22 08:44_

---

_@konstin reviewed on 2023-05-22 09:53_

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:124 on 2023-05-22 09:53_

Wouldn't a `Vec<usize>` for `relative_offset` alone be enough? Once we know we know we want to flag the current word and actually need to emit diagnostics we can read confusable and representant from the word

---

_Review comment by @konstin on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__confusables.snap`:114 on 2023-05-22 09:58_

here we really need #4448

---

_@konstin approved on 2023-05-22 09:58_

can't add much to micha's review

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:27 on 2023-05-22 14:16_

We don't right now.

---

_@charliermarsh reviewed on 2023-05-22 14:16_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/rules/ambiguous_unicode_character.rs`:124 on 2023-05-22 14:29_

Storing the `relative_offset` requires that we then do the lookup again, I think? I opted for `Candidate` for now.

---

_@charliermarsh reviewed on 2023-05-22 14:29_

---

_@charliermarsh reviewed on 2023-05-22 14:29_

---

_Review comment by @charliermarsh on `crates/ruff/src/rules/ruff/snapshots/ruff__rules__ruff__tests__confusables.snap`:114 on 2023-05-22 14:29_

Agree!

---

_Merged by @charliermarsh on 2023-05-22 14:42_

---

_Closed by @charliermarsh on 2023-05-22 14:42_

---

_Branch deleted on 2023-05-22 14:42_

---
