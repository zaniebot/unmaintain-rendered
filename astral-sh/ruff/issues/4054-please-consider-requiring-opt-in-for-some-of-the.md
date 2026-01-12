```yaml
number: 4054
title: Please consider requiring opt-in for some of the new subprocess rules
type: issue
state: open
author: ofek
labels:
  - rule
  - needs-decision
assignees: []
created_at: 2023-04-21T02:56:14Z
updated_at: 2025-01-08T21:34:56Z
url: https://github.com/astral-sh/ruff/issues/4054
synced_at: 2026-01-12T15:54:44Z
```

# Please consider requiring opt-in for some of the new subprocess rules

---

_@ofek_

The ones I think should be disabled by default:

- https://bandit.readthedocs.io/en/latest/plugins/b603_subprocess_without_shell_equals_true.html
- https://bandit.readthedocs.io/en/latest/plugins/b604_any_other_function_with_shell_equals_true.html
- https://bandit.readthedocs.io/en/latest/plugins/b606_start_process_with_no_shell.html
- https://bandit.readthedocs.io/en/latest/plugins/b607_start_process_with_partial_path.html

For example, the documented textbook case for S603 is `subprocess.check_output(['/bin/ls', '-l'])`. That is literally perfect usage of subprocess, it cannot be made better other than simply outright banning the use of subprocess.

Pretty much all our repositories fail now with many examples but I'll just give one:

```
src/ddqa/utils/git.py:61:17: S603 `subprocess` call: check for execution of untrusted input
src/ddqa/utils/git.py:61:17: S607 Starting a process with a partial executable path
```

```python
    def capture(self, *args) -> str:
        import subprocess

        try:
            process = subprocess.run(
                ['git', *args],
                cwd=str(self.path),
                stdout=subprocess.PIPE,
                stderr=subprocess.STDOUT,
                encoding='utf-8',
                check=True,
            )
        except subprocess.CalledProcessError as e:
            message = f'{str(e)[:-1]}:\n{e.output}'
            raise OSError(message) from None

        return process.stdout
```

---

_Comment by @evanrittenhouse on 2023-04-22 14:00_

A similar `subprocess` problem appears in #4045 - opt-out's not a bad idea, IMO, but I'll wait for someone else to confirm that it's appropriate.

---

_Label `question` added by @charliermarsh on 2023-04-25 00:13_

---

_Comment by @ofek on 2023-05-08 16:17_

Has there been any update on this? We're still capping the version to avoid these rules.

---

_Comment by @charliermarsh on 2023-05-08 18:16_

Sorry @ofek, I'll make a decision on this today.

---

_Assigned to @charliermarsh by @charliermarsh on 2023-05-08 18:16_

---

_Comment by @charliermarsh on 2023-05-09 03:35_

A couple observations, to put some of my internal dialogue down on "paper":

1. While these rules do arguably "ban" `subprocess` calls, I think the _intention_ is that they force you to explicitly allow any uses of `subprocess` via a deliberate `noqa` or `nosec` comment.
3. There are likely projects in which that level of care is useful (or even required), and projects in which it's not.
4. Bandit has a parallel "severity" system, and these are generally low-severity rules. We don't support severities. (I don't know that adding severity support necessarily solves this problem.)
5. We don't have a good mechanism for making these rules "opt-in", and I think it would be confusing if they weren't enabled via `--select S`, since there's no precedent for that. \cc @MichaReiser 
6. Flake8 does have a system for this, in that it doesn't enable any `9XX` rules by default. So, e.g., flake8-bugbear's B905 rule isn't enabled with `--select B` -- you have to explicitly `--select B905`. So one solution would be to respect `9XX` rules, and re-code these to the 900-level.
7. Removing these rules entirely is another option. It does mean that some projects that would _like_ to have them enforced won't be able to enforce them, and it's also not zero-cost, since some users have now upgraded and (e.g.) added `noqa` pragmas to ignore specific usages. Those pargmas will be automatically removed via `RUF100`, but just acknowledging that removing these rules (or disabling them even when `S` is selected) would cause some churn.

Given these observations, I feel a bit torn. I'd probably rather remove these rules than make them opt-in via a special mechanism. Can I ask why you don't `ignore` these rules, if they're too strict for your project(s)?

P.S. Separately, is `B604` in the same league as the others? That one seems the most reasonable to me (and is MEDIUM severity, AFAICT).


---

_Comment by @ofek on 2023-05-09 12:49_

> Can I ask why you don't `ignore` these rules, if they're too strict for your project(s)?

I can do that but I opened this issue because I don't think it was known that such calls are effectively banned by default now. If you're now aware and okay with that then feel free to close this!

---

_Comment by @charliermarsh on 2023-05-19 02:07_

One solution I am considering here (which goes beyond this specific case) is adding a "pedantic" or "opinionated" rule concept, for rules that _require_ explicit opt-in via `select`. So, you'd be required to select `S603`, and the rule wouldn't turn on via `select = ["S"]`.

---

_Comment by @evanrittenhouse on 2023-05-19 13:03_

@charliermarsh  would that be a new RuleGroup variant?

---

_Comment by @charliermarsh on 2023-05-19 14:03_

@evanrittenhouse - Yes. The main downside though is it will be confusing and surprising to users that (e.g.) `S603` isn't enabled when `select = ["S"]`. This is fine for nursery, since it's intentionally experimental. But I'm not sure how to resolve that in general.

---

_Comment by @MichaReiser on 2023-05-19 14:23_

What's the main motivation for keeping the same name? Is it so that users can copy-paste their configuration without any changes? Or is it mainly for discoverability (which could be solved by maintaining a mapping table between rules), or are there other reasons?

---

_Comment by @charliermarsh on 2023-05-19 14:27_

Do you mean, why keep these rules under Bandit?

---

_Comment by @MichaReiser on 2023-05-19 14:28_

> Do you mean, why keep these rules under Bandit?

Yes, why keep the S603 name

---

_Comment by @charliermarsh on 2023-05-19 14:39_

It may not be the correct decision, but the arguments in favor would be:

1. Discoverability: it's listed with the other Bandit rules, and shares a prefix that people associate with "security".
2. Portability: people can use their existing Flake8 configuration without having to understand that the code has changed. `# noqa: S603` also continues to work without issue.


---

_Label `rule` added by @charliermarsh on 2023-07-10 01:15_

---

_Label `needs-decision` added by @charliermarsh on 2023-07-10 01:15_

---

_Label `question` removed by @charliermarsh on 2023-07-10 01:15_

---

_Comment by @sbrudenell on 2025-01-08 21:34_

> 1. While these rules do arguably "ban" `subprocess` calls, I think the _intention_ is that they force you to explicitly allow any uses of `subprocess` via a deliberate `noqa` or `nosec` comment.

Can you explain the intended workflow of this?

I thought the usefulness of `noqa` was for rules that are *usually* actionable but can't recognize edge cases. (for example, `FBT001` helps me avoid *new* problematic type signatures, but I `noqa` it when I need to match existing signatures)

I'm not seeing how `S603` is actionable. I don't see meaning to writing `noqa` every time I write `subprocess`. That's automatic, not "deliberate". It's also not ideal to further clutter to my `--ignore` config. (I already don't know why it's all there or when I should review it).

Can you explain the use case? Are there teams who have a process to (regularly?) review all `noqa`s in their code or something? I've never been part of a review process that could make use of an alert like this, but maybe I've never been on a big enough team.

---
