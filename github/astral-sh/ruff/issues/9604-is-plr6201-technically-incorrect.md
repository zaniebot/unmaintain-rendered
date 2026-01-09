---
number: 9604
title: Is PLR6201 technically incorrect?
type: issue
state: closed
author: trag1c
labels: []
assignees: []
created_at: 2024-01-22T00:44:49Z
updated_at: 2024-01-22T01:11:46Z
url: https://github.com/astral-sh/ruff/issues/9604
synced_at: 2026-01-07T13:12:15-06:00
---

# Is PLR6201 technically incorrect?

---

_Issue opened by @trag1c on 2024-01-22 00:44_

From [the docs](https://docs.astral.sh/ruff/rules/literal-membership/):
> When testing for membership in a static sequence, prefer a `set` literal over a `list` or `tuple`, as Python optimizes `set` membership tests.

This optimization isn't significant when using literals—while `set`s do have faster membership checks, doing `1 in (1, 2, 3)` is faster than `1 in {1, 2, 3}` as it takes 4–5x as much time to just construct a `set` compared to a `tuple` (see benchmarks below).

<details>
<summary>Benchmarks</summary>

### Creating an object with elements `1, 2, 3`
<table>
<tr>
 <th>Version</th>
 <th>tuple</th>
 <th>set</th>
 <th>list</th>
 <th>tuple vs set</th>
 <th>list vs set</th>
</tr>
<tr>
 <td>3.12</td>
 <td>10.28ns</td>
 <td>48.71ns</td>
 <td>42.37ns</td>
 <td>+374%</td>
 <td>+15%</td>
</tr>
<tr>
 <td>3.11</td>
 <td>8.24ns</td>
 <td>39.88ns</td>
 <td>25.55ns</td>
 <td>+384%</td>
 <td>+56%</td>
</tr>
<tr>
 <td>3.10</td>
 <td>7.98ns</td>
 <td>40.73ns</td>
 <td>26.86ns</td>
 <td>+410%</td>
 <td>+52%</td>
</tr>
<tr>
 <td>3.9</td>
 <td>8.73ns</td>
 <td>40.00ns</td>
 <td>26.03ns</td>
 <td>+358%</td>
 <td>+54%</td>
</tr>
<tr>
 <td>3.8</td>
 <td>9.43ns</td>
 <td>39.84ns</td>
 <td>27.38ns</td>
 <td>+322%</td>
 <td>+46%</td>
</tr>
</table>

### Checking whether `1` is a member of the object (w/ a literal)
<table>
<tr>
 <th>Version</th>
 <th>tuple</th>
 <th>set</th>
 <th>list</th>
 <th>tuple vs set</th>
 <th>list vs set</th>
</tr>
<tr>
 <td>3.12</td>
 <td>15.19ns</td>
 <td>16.54ns</td>
 <td>15.20ns</td>
 <td>+9%</td>
 <td>+9%</td>
</tr>
<tr>
 <td>3.11</td>
 <td>13.47ns</td>
 <td>14.74ns</td>
 <td>14.14ns</td>
 <td>+9%</td>
 <td>+4%</td>
</tr>
<tr>
 <td>3.10</td>
 <td>13.83ns</td>
 <td>15.20ns</td>
 <td>13.83ns</td>
 <td>+10%</td>
 <td>+10%</td>
</tr>
<tr>
 <td>3.9</td>
 <td>15.66ns</td>
 <td>16.94ns</td>
 <td>15.61ns</td>
 <td>+8%</td>
 <td>+9%</td>
</tr>
<tr>
 <td>3.8</td>
 <td>17.83ns</td>
 <td>19.00ns</td>
 <td>17.81ns</td>
 <td>+7%</td>
 <td>+7%</td>
</tr>
</table>
</details>

Therefore (even though the difference isn't that substantial) I think it would make more sense for the rule to suggest
* using a `tuple` when checking against a literal, e.g. `1 in (1, 2, 3)`, and
* using a `set` when checking against a constant, e.g. `S = {1, 2, 3}; 1 in S` (although I suspect literals may be faster than constants for small objects (because of variable lookup?), in which case tuples would be the way).

Hopefully I haven't made any mistakes with my reasoning 😄

---

_Comment by @charliermarsh on 2024-01-22 00:57_

I think this was discussed a bit in https://github.com/astral-sh/ruff/issues/8758. Do you mind reading that issue first?

---

_Comment by @trag1c on 2024-01-22 01:10_

I somehow missed this specific one, my bad—seems like I missed a lot of details here, so I guess it's fine to keep it the way it is in most cases 🙂

---

_Closed by @trag1c on 2024-01-22 01:10_

---

_Referenced in [astral-sh/ruff#12051](../../astral-sh/ruff/pulls/12051.md) on 2024-06-27 09:53_

---
