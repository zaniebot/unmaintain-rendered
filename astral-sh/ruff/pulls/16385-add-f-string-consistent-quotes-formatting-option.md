```yaml
number: 16385
title: Add f-string consistent quotes formatting option
type: pull_request
state: closed
author: matthewlloyd
labels:
  - configuration
  - formatter
  - needs-decision
assignees: []
base: main
head: main
created_at: 2025-02-25T21:24:01Z
updated_at: 2025-12-31T19:58:26Z
url: https://github.com/astral-sh/ruff/pull/16385
synced_at: 2026-01-10T16:36:18Z
```

# Add f-string consistent quotes formatting option

---

_Pull request opened by @matthewlloyd on 2025-02-25 21:24_

Introduces a new formatter option `f-string-consistent-quotes` that leverages Python 3.12's PEP 701 to use consistent quotes inside f-string expressions rather than alternating quote styles for compatibility.

When enabled and targeting Python 3.12+, the formatter will use the same quote style (following the `quote-style` setting) inside f-string expressions as in the outer f-string. This produces more consistent and readable code.

When disabled (default) or targeting Python versions below 3.12, the formatter will continue to alternate quotes for compatibility.

Implements: https://github.com/astral-sh/ruff/issues/14118

---

_Review requested from @MichaReiser by @matthewlloyd on 2025-02-25 21:24_

---

_Comment by @github-actions[bot] on 2025-02-25 21:32_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Linter (stable)
✅ ecosystem check detected no linter changes.

### Linter (preview)
✅ ecosystem check detected no linter changes.

### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
✅ ecosystem check detected no format changes.




---

_Comment by @MichaReiser on 2025-02-26 07:30_

Thanks for working on this. I don't feel like I've enough information/conviction yet that we should introduce this option. That's why I prefer to wait with this change a little longer.

---

_Label `configuration` added by @MichaReiser on 2025-02-26 07:30_

---

_Label `formatter` added by @MichaReiser on 2025-02-26 07:30_

---

_Label `needs-decision` added by @MichaReiser on 2025-02-26 07:30_

---

_Comment by @matthewlloyd on 2025-02-26 15:52_

Thanks for considering this PR. I'd like to clarify a few points that might help with the decision:

1. This change introduces an *opt-in* configuration option that doesn't alter existing behavior by default. Users who prefer the current approach can continue using it unchanged.

2. The feature aligns with Ruff's existing philosophy of giving _users_ control over code formatting. Just as `quote-style` lets users choose their preferred quotes globally, this option extends that same control to quotes inside f-strings. As it is, Ruff allows users to choose their quotes, yet arbitrarily dictates them in nested f-string expressions.

3. Since Python 3.12 specifically introduced this capability with PEP 701, it seems logical to provide a way for users to take advantage of it. The implementation only applies when targeting Python 3.12+, maintaining backward compatibility. Older versions of Python will one day be obsolete and alternating quotes will become vestigial and unnecessary, so this will likely need to be done eventually anyway.

4. Six users have already expressed interest in this feature in the initial issue (#14118), suggesting there's demand for this level of formatting control.

What additional information would be helpful to move this forward? I'm happy to address any specific concerns about the implementation or rationale.

---

_Comment by @MichaReiser on 2025-02-26 16:03_

It's mainly that I'm not sure that there's enough demand to justify an extra option. We don't have as high a bar as black on options but we still maintain a fairly high bar on new options because each option introduces the risk that people now start discussing options instead of formatting decisions. That's why I want to wait a little longer to see if there's more demand for the option introduced in this PR

---

_Comment by @matthewlloyd on 2025-02-26 16:19_

Thanks for the feedback. I understand your concern about demand, but I'd like to respectfully point out that you accepted and implemented the original feature request to make quote styles alternating (#11056) even though it had been requested by and had support from only a *single* user. This feature has been requested by *six* users already.

The original change to enforce alternating quotes made in 2024 was made to cater to a narrow use case - backporting to Python versions released *before 2022* - while this configuration option will benefit all current users of Python 3.12 (released two years ago already, in 2023) and all future versions. Given that Python's trajectory is clearly moving toward consistent quote handling in f-strings with PEP 701, it seems reasonable to provide users the option to follow this modern approach.

Additionally, this is a non-default configuration option that requires explicit opt-in, so it doesn't impact existing users or add complexity to the default experience. It simply provides flexibility for those who want to leverage Python 3.12's improved f-string capabilities.

---

_Comment by @tlauli on 2025-02-26 17:39_

I would very much like to see this merged. I am currently postponing upgrading ruff to ^0.9 in my repo, because I do not like the alternating style. I was surprised that the option for consistent quotes was not already present when I saw the default was to alternate quotes.

It seems weird to not be able to opt in to something, that in my opinion should be the default behavior after python 3.11 end of life. I do not believe we should be restricted by limitations of old versions forever, and allowing the transition to happen sooner on an opt in basis will only make it smoother.

---

_Comment by @vivodi on 2025-02-28 15:55_

I also really hope this PR gets merged. I think having a unified quote style would look much better. Right now, I can only use `quote-style = "preserve"`. If this PR gets merged, I will switch to `quote-style = "single"`. @MichaReiser

```diff
- f"Employee: {employee['name']}"
+ f'Employee: {employee['name']}'
```

---

_Comment by @vivodi on 2025-03-01 00:50_

> It's mainly that I'm not sure that there's enough demand to justify an extra option. We don't have as high a bar as black on options but we still maintain a fairly high bar on new options because each option introduces the risk that people now start discussing options instead of formatting decisions. That's why I want to wait a little longer to see if there's more demand for the option introduced in this PR

I think the Python official decision to introduce 'Quote reuse in f-strings' in [Python 3.12](https://docs.python.org/3/whatsnew/3.12.html#pep-701-syntactic-formalization-of-f-strings) justifies this PR.

If 'Quote reuse in f-strings' really had no value, why would the Python official team introduce this syntax through [PEP 701 – Syntactic formalization of f-strings](https://peps.python.org/pep-0701/)?

At least for me, I prefer a more unified style for f-strings, and I hope this PR gets merged soon. @charliermarsh 

---

_Comment by @vivodi on 2025-03-01 11:15_

> It's mainly that I'm not sure that there's enough demand to justify an extra option. We don't have as high a bar as black on options but we still maintain a fairly high bar on new options because each option introduces the risk that people now start discussing options instead of formatting decisions. That's why I want to wait a little longer to see if there's more demand for the option introduced in this PR

If you're worried about providing too many options, why not make this PR (f-string consistent quotes) the only choice for users of Python 3.12 and above?

 For code that requires Python 3.12 or later, format it using a consistent quote style. For code before Python 3.12, keep the existing behavior.

I prefer a more unified style for f-strings, and there's **no need** to offer an option for the existing behavior anymore. @MichaReiser @charliermarsh @zanieb 

**Conclusion**: Change the existing behavior to f-string consistent quotes instead of adding a new option. I think this is a reasonable solution, avoiding the issue raised by @MichaReiser about providing too many options.

---

_Comment by @tlauli on 2025-03-01 11:37_

> there's no need to offer an option for the existing behavior anymore

The reason it like like this is backport friendliness, see the original issue #11056. Whether it is a good enough reason to force inconsistent quotes even for people who do not care about supporting python <3.12 is another question.

---

_Comment by @matthewlloyd on 2025-03-01 15:30_

> The reason it like like this is backport friendliness, see the original issue #11056. Whether it is a good enough reason to force inconsistent quotes even for people who do not care about supporting python <3.12 is another question.

I’d argue most developers targeting 3.12+ don’t actually need their code to be backportable to pre-3.12 versions. Most are writing application code or end-user applications and have complete control over their environment, and if they are writing libraries, they are likely writing them for internal use within an organization where they can assume a minimum Python version. Even among public library authors, only some need to support older versions.

Given that, it might make more sense for `f-string-consistent-quotes` to be _enabled_ by default, and perhaps even removed entirely as a config option. Then, those who require backport compatibility can instead rely on setting their target Python version to something below 3.12, or - if needed - the config flag could remain available for them to disable. This way, Ruff would better align with the needs of the majority while still accommodating those who need stricter backwards compatibility.

---

_Comment by @MichaReiser on 2025-03-03 12:30_

Thanks for all your feedback and putting up this PR. I'll close this PR for now because it has the undesired effect that the discussion is now happening in two places. I'll reopen this PR once we've made a decision. 

I'll reply to some of the inputs on the issue.

Please refrain from tagging people unnecessarily. We already deal with a lot of notifications and unnecessary tags only make it more likely that we'll miss pings where they're important.

---

_Closed by @MichaReiser on 2025-03-03 12:30_

---

_Comment by @vivodi on 2025-12-31 19:58_

@matthewlloyd

It looks like @MichaReiser has agreed to reopen this PR https://github.com/astral-sh/ruff/issues/14118#issuecomment-3702479367.

---
