---
number: 14085
title: Adding new class of static analysis rules using LLMs
type: issue
state: open
author: bbkgh
labels:
  - wish
  - needs-design
assignees: []
created_at: 2024-11-04T05:56:18Z
updated_at: 2024-11-04T08:30:17Z
url: https://github.com/astral-sh/ruff/issues/14085
synced_at: 2026-01-07T13:12:16-06:00
---

# Adding new class of static analysis rules using LLMs

---

_Issue opened by @bbkgh on 2024-11-04 05:56_

Hi, I would like to suggest adding a new class of rules to Ruff that utilizes Local/Remote LLM APIs for running rules over code files. I believe this can greatly improve code quality and result in fewer bugs. Additionally, writing rules in plain text is more convenient. For example, we could add a llm_rules.yaml file to project containing something like this:

```yaml
type: ollama/qwen2.5
endpoint: http://localhost:11434
rules:
  - "There must not be any usage of datetime.now(); use timezone.now() from django.utils instead"
  - "There must not be any Typos in comments"
  - "Always use MyCustomObject for calling ServiceX"
```

---

_Comment by @MichaReiser on 2024-11-04 08:30_

Using LLMs for analysis is certainly interesting but I'm not sure if asking the LLM to do the analysis is the solution because LLMs are much slower than regular rules. Instead, we should explore if it's possible for the LLM to derive the necessary checks once ahead of time that Ruff can then run as part of its engine. This also removes any need to directly integrate with an LLVM in ruff itself. 

I also think LLMs should be used very sparsely. E.g. the first rule is already covered by Ruff. 

---

_Label `wish` added by @MichaReiser on 2024-11-04 08:30_

---

_Label `needs-design` added by @MichaReiser on 2024-11-04 08:30_

---
