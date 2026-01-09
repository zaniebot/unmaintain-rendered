---
number: 18154
title: "[Rule-Request] Do not perform equality checks with floating point values."
type: issue
state: closed
author: Anselmoo
labels: []
assignees: []
created_at: 2025-05-17T16:34:40Z
updated_at: 2025-05-18T16:13:48Z
url: https://github.com/astral-sh/ruff/issues/18154
synced_at: 2026-01-07T13:12:16-06:00
---

# [Rule-Request] Do not perform equality checks with floating point values.

---

_Issue opened by @Anselmoo on 2025-05-17 16:34_

### Summary

**Summary:**

Introduce a new linting rule in Ruff that warns when floating point numbers are compared using equality (==) or inequality (!=) operators. This aligns with [SonarSource Rule RSPEC-1244](https://rules.sonarsource.com/python/RSPEC-1244/), which highlights the unreliability of such comparisons due to the imprecision inherent in floating point arithmetic.

**Motivation:**

Comparing floating point numbers directly for equality can lead to unexpected behavior because of rounding errors and representation issues. This is particularly problematic in scientific computing, data analysis, and machine learning applications where floating point operations are prevalent. Implementing this rule in Ruff would help developers identify and rectify potential bugs early in the development process.

**References:**
	•	SonarSource RSPEC-1244: [Floating point numbers should not be tested for equality](https://rules.sonarsource.com/python/RSPEC-1244/)



---

_Renamed from "Do not perform equality checks with floating point values." to "[Rule-Request] Do not perform equality checks with floating point values." by @Anselmoo on 2025-05-17 16:35_

---

_Closed by @MichaReiser on 2025-05-18 16:13_

---

_Referenced in [astral-sh/ruff#20579](../../astral-sh/ruff/issues/20579.md) on 2025-09-25 18:46_

---
