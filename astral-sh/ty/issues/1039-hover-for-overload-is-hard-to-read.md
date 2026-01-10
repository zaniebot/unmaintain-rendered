```yaml
number: 1039
title: hover for Overload is hard to read
type: issue
state: closed
author: asukaminato0721
labels: []
assignees: []
created_at: 2025-08-18T09:39:44Z
updated_at: 2025-08-18T10:20:01Z
url: https://github.com/astral-sh/ty/issues/1039
synced_at: 2026-01-10T02:06:24Z
```

# hover for Overload is hard to read

---

_Issue opened by @asukaminato0721 on 2025-08-18 09:39_

### Summary

https://github.com/langgenius/dify/blob/670d479e32b8c5f881072d8e869c446b60ddd43e/api/controllers/console/admin.py#L127

```py
            app = session.execute(select(App).where(App.id == recommended_app.app_id)).scalar_one_or_none()
```

hover

```py
Overload[(__ent0: @Todo) -> Select[tuple[_T0]], (__ent0: @Todo, __ent1: @Todo) -> Select[tuple[_T0, _T1]], (__ent0: @Todo, __ent1: @Todo, __ent2: @Todo) -> Select[tuple[_T0, _T1, _T2]], (__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3]], (__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4]], (__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5]], (__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo, __ent6: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5, _T6]], (__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo, __ent6: @Todo, __ent7: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5, _T6, _T7]], (__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo, __ent6: @Todo, __ent7: @Todo, __ent8: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5, _T6, _T7, _T8]], (__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo, __ent6: @Todo, __ent7: @Todo, __ent8: @Todo, __ent9: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5, _T6, _T7, _T8, _T9]], (...) -> Select[Any]]
```

Hope can be displayed in lines, or only shows the matched one.

```py
Overload[
(__ent0: @Todo) -> Select[tuple[_T0]],
(__ent0: @Todo, __ent1: @Todo) -> Select[tuple[_T0, _T1]],
(__ent0: @Todo, __ent1: @Todo, __ent2: @Todo) -> Select[tuple[_T0, _T1, _T2]],
(__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3]],
(__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4]],
(__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5]],
(__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo, __ent6: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5, _T6]],
(__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo, __ent6: @Todo, __ent7: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5, _T6, _T7]],
(__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo, __ent6: @Todo, __ent7: @Todo, __ent8: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5, _T6, _T7, _T8]],
(__ent0: @Todo, __ent1: @Todo, __ent2: @Todo, __ent3: @Todo, __ent4: @Todo, __ent5: @Todo, __ent6: @Todo, __ent7: @Todo, __ent8: @Todo, __ent9: @Todo) -> Select[tuple[_T0, _T1, _T2, _T3, _T4, _T5, _T6, _T7, _T8, _T9]],
(...) -> Select[Any]
]
```

### Version

0.0.1a18

---

_Comment by @AlexWaygood on 2025-08-18 10:20_

Thanks for the report! Please see #1000.

This should also be improved once we've got rid of all those `Todo` types 😄 I'm guessing those are caused by #544

---

_Closed by @AlexWaygood on 2025-08-18 10:20_

---
