```yaml
number: 1786
title: "Rename does not consider all `def/class` statements when rebinding a name"
type: issue
state: open
author: MeGaGiGaGon
labels:
  - server
assignees: []
created_at: 2025-12-05T19:51:36Z
updated_at: 2025-12-05T20:07:26Z
url: https://github.com/astral-sh/ty/issues/1786
synced_at: 2026-01-10T01:56:41Z
```

# Rename does not consider all `def/class` statements when rebinding a name

---

_Issue opened by @MeGaGiGaGon on 2025-12-05 19:51_

### Summary

I saw #1784 so decided to mess around with renaming a bit, and was able to find this edge case where I think the wrong set of things is being renamed:
```py
A = ""
class A: ...
def A(): ...
print(A)
```

This is a bit of an odd case since it's a top level name rebinding, where the current behavior of ty is inconsistent:
- Cursor on the `A = ""` or `print(A)` renames only those two:

<img width="232" height="142" alt="Image" src="https://github.com/user-attachments/assets/28e9d295-8e6c-4181-909e-87174cb9ffca" />

<img width="271" height="136" alt="Image" src="https://github.com/user-attachments/assets/e857b078-c978-45f7-9257-b46929060caf" />

- Cursor on the `class A` renames the `class A`, `A = ""`, and `print(A)`, but not the `def A`:

<img width="276" height="146" alt="Image" src="https://github.com/user-attachments/assets/76cacc41-02b8-4192-969b-307018f978fe" />

- Cursor on the `def A` renames the `def A`, `A = ""`, and `print(A)`, but not the `class A`:

<img width="261" height="143" alt="Image" src="https://github.com/user-attachments/assets/53b61564-2180-49cb-a617-0ba6c3d6fd34" />

Ideally ty would not rename a different set of items depending on where you rename from.

(On the playground I couldn't figure out how to enable renaming, but you can still see the same inconsistent set of symbols are considered equivalent by placing the cursor on the different `A`s and seeing which others are highlighted.)

### Version

ty VSCode 2025.63.13380910 / playground ef45c97da

---

_Comment by @MeGaGiGaGon on 2025-12-05 19:55_

(Also just to be clear this is a very minor edge case and I don't think it should block stabilizing renaming)

---

_Label `server` added by @AlexWaygood on 2025-12-05 20:07_

---
