```yaml
number: 11211
title: "Add new configuration setting `lint.pydocstyle.allow-missing-docstring-nested-class-names`"
type: issue
state: open
author: CarrotManMatt
labels:
  - configuration
  - docstring
assignees: []
created_at: 2024-04-30T11:50:55Z
updated_at: 2024-04-30T15:08:09Z
url: https://github.com/astral-sh/ruff/issues/11211
synced_at: 2026-01-10T11:09:53Z
```

# Add new configuration setting `lint.pydocstyle.allow-missing-docstring-nested-class-names`

---

_Issue opened by @CarrotManMatt on 2024-04-30 11:50_

This setting would be used by linting rule [D106](https://docs.astral.sh/ruff/rules/undocumented-public-nested-class/).
It would be of type `list[str]` representing the names of public nested classes that are allowed to not contain a docstring.
The format would be similar to [lint.pylint.allow-dunder-method-names](https://docs.astral.sh/ruff/settings/#lint_pylint_allow-dunder-method-names).

The major example for the usefulness of this rule would be for creating certain classes within the Django web framework, that require a nested `Meta` class. Models, ModelForms, some generic class-based-Views and ModelAdmin classes all require a `Meta` class to define certain functionality. It is superfluous to include a docstring for every definition of the `Meta` class as they all serve a similar purpose, and are used only by the Django runtime in the background. They should not be accessed by any project-code.

When a developer doesn't want to include docstrings purposefully for a specific nested-class name (see the example above) it is annoying to have to use the `# noqa: D106` assertion on every line that the class name is used. This rule would prevent the repeated use of this ignore statement.


---

_Comment by @MichaReiser on 2024-04-30 13:50_

Thanks for opening this. I wonder if we could do better once we have a multifile analysis and maintain such a non-configurable allow list. But maybe that's too opinionated?

---

_Label `configuration` added by @MichaReiser on 2024-04-30 13:50_

---

_Label `docstring` added by @AlexWaygood on 2024-04-30 14:05_

---

_Comment by @CarrotManMatt on 2024-04-30 15:08_

> maintain such a non-configurable allow list

As in a hard coded set of values that are ignored? What is the benefit of not allowing users to customise the list? At the minimum the hard coded ones could just be the defaults, like some of the other configuration lists that have defaults that can be extended by users.

---
