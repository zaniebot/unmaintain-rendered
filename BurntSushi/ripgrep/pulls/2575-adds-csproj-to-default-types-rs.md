```yaml
number: 2575
title: Adds .csproj to default_types.rs
type: pull_request
state: merged
author: vidarasberg
labels: []
assignees: []
merged: true
base: master
head: patch-1
created_at: 2023-07-31T08:11:35Z
updated_at: 2023-08-01T15:05:50Z
url: https://github.com/BurntSushi/ripgrep/pull/2575
synced_at: 2026-01-12T18:23:14Z
```

# Adds .csproj to default_types.rs

---

_@vidarasberg_

Supports the .NET C# Project file extension

---

_@BurntSushi approved on 2023-07-31 11:08_

LGTM. How many more C# types are we going to have? This makes 4 by my count.

---

_Merged by @BurntSushi on 2023-07-31 11:08_

---

_Closed by @BurntSushi on 2023-07-31 11:08_

---

_Comment by @vidarasberg on 2023-08-01 10:21_

> LGTM. How many more C# types are we going to have? This makes 4 by my count.

Thank you!

Haha yes but csharp and cs are both *.cs.

I now see that I could have used msbuild as *.csproj was included there. I hope it helps someone that now both options exist.

---

_Branch deleted on 2023-08-01 10:22_

---

_Comment by @ltrzesniewski on 2023-08-01 15:05_

> I hope it helps someone that now both options exist.

I'll certainly use `-tcsproj` as it feels better than `-tmsbuild`, thanks! 🙂

`-tcsharp` is a bit redundant IMO as it's longer than `-tcs`.


---
