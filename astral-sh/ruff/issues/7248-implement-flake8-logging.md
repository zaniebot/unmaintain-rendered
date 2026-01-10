```yaml
number: 7248
title: "Implement `flake8-logging`"
type: issue
state: open
author: LaBatata101
labels:
  - plugin
assignees: []
created_at: 2023-09-08T19:46:27Z
updated_at: 2025-06-03T19:35:48Z
url: https://github.com/astral-sh/ruff/issues/7248
synced_at: 2026-01-10T11:09:49Z
```

# Implement `flake8-logging`

---

_Issue opened by @LaBatata101 on 2023-09-08 19:46_

Issue to track the implementation of [`flake8-logging`](https://github.com/adamchainz/flake8-logging) rules.

- [x] `LOG001` use logging.getLogger() to instantiate loggers
- [x] `LOG002` use `__name__` with `getLogger()`
- [x] `LOG003` extra `key '<key>'` clashes with `LogRecord` attribute **/** Covered by [`G101`](https://beta.ruff.rs/docs/rules/logging-extra-attr-clash/)
- [x] `LOG004` avoid `exception()` outside of exception handlers
- [x] `LOG005` use `exception()` within an exception handler **/** Covered by [`TRY400`](https://beta.ruff.rs/docs/rules/error-instead-of-exception/)
- [x] `LOG006` redundant `exc_info` argument for `exception()` **/** Covered by [`G202`](https://beta.ruff.rs/docs/rules/logging-redundant-exc-info/)
- [x] `LOG007` use `error()` instead of `exception()` with `exc_info=False`
- [x] `LOG008` `warn()` is deprecated, use `warning()` instead **/** Covered by [`G010`](https://beta.ruff.rs/docs/rules/logging-warn/)
- [x] `LOG009` `WARN` is undocumented, use `WARNING` instead
- [ ] `LOG010` `exception()` does not take an exception 
- [x] `LOG011` avoid pre-formatting log messages **/** Covered by [`G004`](https://beta.ruff.rs/docs/rules/logging-f-string/), [`G003`](https://beta.ruff.rs/docs/rules/logging-string-concat/), [`G002`](https://beta.ruff.rs/docs/rules/logging-percent-format/), [`G001`](https://beta.ruff.rs/docs/rules/logging-string-format/)
- [ ] `LOG012` formatting error: `<n>` `<style>` placeholders but `<m>` arguments
- [ ] `LOG013` formatting error: `<missing/unreferenced>` keys: `<keys>`
- [x] `LOG014` avoid `exc_info=True` outside of exception handlers
- [x] `LOG015` avoid logging calls on the root logger




---

_Label `plugin` added by @zanieb on 2023-09-08 21:51_

---

_Comment by @qdegraaf on 2023-09-08 23:29_

I'm implementing `LOG007` but will wait for review and merge of https://github.com/astral-sh/ruff/pull/7249 and the boilerplate is in so it's easier to review  

---

_Comment by @LaBatata101 on 2023-09-08 23:33_

I'm implementing `LOG001`, just waiting for #7249 to get merged

---

_Comment by @jonprindiville on 2023-09-11 14:38_

Hm, `LOG001` feels a bit like a specific case of [banned-api (`TID251`)](https://beta.ruff.rs/docs/rules/banned-api/).

Not to say that `LOG001` *can't* have an explicit implementation, just reminds me of this other thing.

---

_Comment by @qdegraaf on 2023-09-15 16:21_

It looks like `LOG012` is covered by `PLE1206`|`LoggingTooFewArgs` it only appears to cover `%s` formatting but so does `LOG012` according to [upstream README](https://github.com/adamchainz/flake8-logging#log012-formatting-error-n-style-placeholders-but-m-arguments)

---

_Comment by @tjkuson on 2023-09-15 16:34_

`LOG007` should probably be implemented only once #4136 is resolved; otherwise, it conflicts with `TRY400`.

---

_Comment by @9128305 on 2023-11-28 15:09_

New rules
https://github.com/adamchainz/flake8-logging#log013-formatting-error-missingunreferenced-keys-keys
https://github.com/adamchainz/flake8-logging#log014-avoid-exc_infotrue-outside-of-exception-handlers

---

_Comment by @dhruvmanila on 2023-11-28 15:13_

Thanks! I've updated the issue description.

---

_Comment by @javulticat on 2023-12-19 20:16_

Might I suggest that `ruff` drops/ignores `flake8-logging-format` (and the `G` error codes) entirely in favor of using `flake8-logging` (and exclusively the `LOG` error codes) as its source-of-truth for logging-related linting rules?

Currently, it is pretty confusing that `ruff` has logging-related linting rules spread out in two separate places (`G` and `LOG`).  To an end user of `ruff`, there really shouldn't be two separate places to find logging-related linting rules - it really seems like it's just an implementation detail that they were originally derived from two separate projects.

To illustrate a concrete example of how this is a problem for our organization, I see here that you've marked that some of `flake8-logging`'s `LOG` rules are already covered by some `G` rules from `flake8-logging-format` you already have, so you aren't planning on implementing those `LOG` rules (from what I gather, please correct me if I am wrong).  As an organization that is currently using `flake8` with `flake8-logging`, if we were to switch to `ruff`, how are our engineers supposed to know which of `ruff`'s `G` rules map to the `LOG` rules they are familiar with (short of having our engineers digging through Github and finding this Issue, which isn't a realistic option)?  I suppose you could put it in the official docs, but it would _vastly_ reduce the friction for us to migrate to `ruff` if we could simply enable `LOG` rules and get the same behavior we currently get from `flake8-logging`.  Because, even if the mapping from `G` to `LOG` rules was documented, is there any guarantee that the rules derived from `flake8-logging-format` have identical behavior as the corresponding rule(s) derived from `flake8-logging`?

That brings me to what is an even more important point, in my opinion.  Compared to `flake8-logging-format`, `flake8-logging` seems to pretty objectively be a **far** better project to use as the source-of-truth from which you derive your logging-related linting rules.  `flake8-logging-format` is "maintained" (and I use that word very lightly, because the project hasn't seen any activity in _over a year_) by some unknown corporate entity.  On the other hand, `flake8-logging` was created and is actively maintained (most recent commit is less than a week old) by one of the most respected developers in the world of OSS Python software, [`adamchainz`](https://github.com/adamchainz) (whose Github profile should speak for itself, but highlights: core member of both `Django` and `pytest` teams, primary author/maintainer of many other highly regarded and heavily used OSS Python projects - including some I believe `ruff` is already using, such as [`flake8-comprehensions`](https://github.com/adamchainz/flake8-comprehensions)).

I encourage you to read [Adam's blog post](https://adamj.eu/tech/2023/09/07/introducing-flake8-logging/) on why he created `flake8-logging`.  Essentially, it sounds like he had his own concerns about using `flake8-logging-format` to enforce logging-related linting rules and decided to create a higher-quality, up-to-date `flake8` plugin that contained a superset of those rules that would render `flake8-logging-format` obsolete: `flake8-logging`.  So, if I understand that correctly, I believe you should be able to look at `flake8-logging` exclusively for (likely higher-quality) implementations of all of the rules that `flake8-logging-format` supports, plus all of the additional rules that `flake8-logging` has added (and will continue to add if/when helpful new rules are identified, such as the recent LOG013 and LOG014 rules, which is something `flake8-logging-format` hasn't done in years).

Thanks for considering!  I've had a lot of my engineers ask about switching to `ruff`, and we've been watching the project with great interest, but this is definitely one of the larger blockers that are still preventing us from adopting it in my organization.

---

_Comment by @jakkdl on 2024-03-24 17:31_

New rule `LOG015` added upstream: https://github.com/adamchainz/flake8-logging?tab=readme-ov-file#log015-avoid-logging-calls-on-the-root-logger

---

_Comment by @JCWasmx86 on 2024-11-10 10:14_

I'm implementing LOG004

---

_Comment by @qartik on 2024-11-16 13:37_

The recently implemented [LOG015](https://docs.astral.sh/ruff/rules/root-logger-call/#example) in #14302 is in conflict with [LOG002](https://docs.astral.sh/ruff/rules/invalid-get-logger-argument/#example). One suggests using `logging.getLogger(__file__)`, the other calls that a bad practice. LOG015 should suggest `logging.getLogger(__name__)`.

It also seems to be triggering when `logging.getLogger(__name__)` is already being used, ie, not a root logger.

---

_Comment by @InSyncWithFoo on 2024-11-16 15:23_

> It also seems to be triggering when `logging.getLogger(__name__)` is already being used, ie, not a root logger.

I [can't reproduce](https://play.ruff.rs/3c72ff48-70bc-4488-aa4b-fc7e64486ad7) the problem. Could you show a minimal reproducible example, please?

---

_Comment by @tmct on 2024-11-16 16:58_

The ruff docs do look surprising there - the original LOG015 [docs](https://github.com/adamchainz/flake8-logging?tab=readme-ov-file#log015-avoid-logging-calls-on-the-root-logger) and implementation definitely expect `__name__` for this pattern rather than `__file__`

---

_Comment by @tmct on 2024-11-16 17:03_

I believe the intent of LOG015 is to promote "namespaced" logging e.g. "logger.info" in favour of plain "logging.info" calls - doing so usually adds very helpful context. (Getting hold of the root logger at the start of applications to configure it should not be discouraged - but logging from it directly in "library" code.)

---

_Comment by @MichaReiser on 2024-11-16 21:56_

Thanks. Makes sense. I created a new issue to track the resolution.

---

_Comment by @qartik on 2024-11-17 13:25_

> I [can't reproduce](https://play.ruff.rs/3c72ff48-70bc-4488-aa4b-fc7e64486ad7) the problem. Could you show a minimal reproducible example, please?

You are right. I was confused and my comment about wrongly triggering is invalid.

Thanks for the update to the doc however.

---
