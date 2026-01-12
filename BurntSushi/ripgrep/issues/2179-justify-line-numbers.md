```yaml
number: 2179
title: Justify line numbers
type: issue
state: closed
author: deivid-rodriguez
labels:
  - duplicate
assignees: []
created_at: 2022-04-12T14:34:06Z
updated_at: 2022-04-13T11:25:39Z
url: https://github.com/BurntSushi/ripgrep/issues/2179
synced_at: 2026-01-12T16:13:24Z
```

# Justify line numbers

---

_@deivid-rodriguez_

First of all, thanks so much for ripgrep, I use it all the time! 😍

Now, my little suggestion

#### Describe your feature request

Currently when searching in a terminals, matching lines are preceded by line numbers followed by ":" or "-" depending on whether they are actual matches or context lines. This is cool.

However, when the output includes successive lines with different number of digits, that can slightly decrease the readability of the output, specially for files where indentation is important. For example:

```
1-uk:
2-  active_admin:
3-    dashboard: "Панель керування"
4-    dashboard_welcome:
5-      welcome: "Ласкаво просимо до Active Admin. Це стандартна сторінка керування сайтом."
6-      call_to_action: "Щоб додати сюди що-небудь, зазирніть у 'app/admin/dashboard.rb'"
7-    view: "Переглянути"
8-    edit: "Змінити"
9-    delete: "Видалити"
10-    delete_confirmation: "Ви впевнені, що хочете це видалити?"
11:    new_model: "Створити %{model}"
12-    edit_model: "Змінити %{model}"
13-    delete_model: "Видалити %{model}"
14-    details: "%{model} детальніше"
15-    cancel: "Скасувати"
16-    empty: "Пусто"
17-    previous: "Поперед."
18-    next: "Наст."
19-    download: "Завантаження:"
20-    has_many_new: "Додати %{model}"
21-    has_many_delete: "Прибрати"
```

The `delete` and `delete_confirmation` keys are siblings, but the indentation makes it not that clear.

Would it be desirable to justify line numbers to the highest number of digits of each chunk of output?

Thanks!


---

_Comment by @BurntSushi on 2022-04-12 15:38_

Something like this was requested in #544. It was added to ripgrep in #723. An issue with the idea was raised in #795 and I ultimately decided to remove the feature in ae6f8714912d522e8042701c30da43c6c667dfd3. The options at this point are:

* Use the `--json` flag and write your own tool that prints results in whatever way you want.
* Somehow find a way to come up with a thorough specification for this feature that works well in all output modes and doesn't require buffering all output.
* Even if you manage to achieve the above, I might still say no to it if I perceive it as increasing the maintenance burden too much.

I'm going to close this for now as a duplicate as I think it's unlikely something like this will get added.

I will note that I suspect you could write a fairly simple wrapper script that will do what you need without too much fuss.

---

_Closed by @BurntSushi on 2022-04-12 15:38_

---

_Label `duplicate` added by @BurntSushi on 2022-04-12 15:38_

---

_Comment by @deivid-rodriguez on 2022-04-13 11:25_

Thank you for your suggestions and context, and sorry I failed to find the previous request before opening this, I must've used the wrong search keys :)

---
