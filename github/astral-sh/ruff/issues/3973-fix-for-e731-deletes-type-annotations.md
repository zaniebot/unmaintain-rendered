---
number: 3973
title: (🐞) fix for E731 deletes type annotations
type: issue
state: closed
author: KotlinIsland
labels:
  - bug
  - good first issue
assignees: []
created_at: 2023-04-14T07:07:45Z
updated_at: 2023-04-16T23:15:40Z
url: https://github.com/astral-sh/ruff/issues/3973
synced_at: 2026-01-07T13:12:14-06:00
---

# (🐞) fix for E731 deletes type annotations

---

_Issue opened by @KotlinIsland on 2023-04-14 07:07_

input
```py
sort_work_items: Callable[[int],int] = lambda it: it
```

expected
```py
def sort_work_items(it: int) -> int:
    return it
```

actual
```py
def sort_work_items(it):
    return it
```

[playground](https://play.ruff.rs/#N4KABGBECGA2sHsDuBTAJgWgMYIHYDMBXAZ2gCNYVjIAuMAbQF0AacKMwgS1gBdPdqdJqwiQ0hALYSAnhgBu0AE6dylDIoDmAD1pQAegAoA+gGoAPsZP1oGAF4BBDAC0ADBgCcRxgCprdx64ejCYA-ACUYQAkkCJQKFo8KLiYnBq4CIoousJskPGJyRjEKJRYPNksufkoirhwFbGQqemZDbmw-CgYlLgaPAAWugAcQ43FpeVCbKIAojHTUABikGyVojxKGig88jXEnHi6kAAO0gDMAIwu86L4sNAA1ihDGNC46Rt8eIJgoBCiMlOGH4nB2mR4hFqunwcGKsVExEIx2OmWIxAw4iksk2PxhsDhC0giORqPR6VwXXBkNw-A00NhKHhUDgiCQRQ2ilemleuGk9PxjMJzQyXSI8FkhFwPGkx3Q-OKbAAvo07o9nhgyG80KDdH9-pB+ko0Dg0OgMDwJMcMZxMmUMnypv99QB6C3HG5OqDOhSKV2Wj1OyDO01yZ3EfoSFZOtb6rD9FBYB7mmVm+JYFDHL64eUoJUq+5PF4cDRkFBKXWE6qFThSQgbChdLAsn5MPO5VWFjVcXj8H560Qcbh8ATAtIijrESYMRht24F9U4S2ZeMCA4CCuBlnIa1lbDNjBIUH9DBPaRIDKYTaSJI8XEM2dQDvqmqKCTEOl0ftQCTQLTs5S9N0SR9IMdAuA+kBPi8ACOhAIIkfZCrgHQUhgsHwVQRxoAghANgG36ED2KFdOhCFYTheFMmICBYJOAEaGhcFkXQ1G4ZQ+EwHICCcJgVBNrKug8IohC5hAyrtvOLzjPgG76sKmQYHUEiYY6nqQEYP5PPhohGNAxDamU2lQEYmTHPc6ZGep+CcCUaDUEyOnWbZRimjChG3lG-wzmJ+Zqi8fBoLINbHBkHmfoSmq4OoJTQHwchdMFoU-CcSg3vZEVvBSl7HJwuqKhBUHJrK2DxomtKyQiQmcIZdB4gSgbxCglo7BICDiJQLYLOsMrlQsMaiMJUo1l0KAKLAhCxWamrFNg9xoip05UYNfDKRgo1wBNiSYKaOCKLFGQtt5YDiXOfkYFsPCJAkFWPpKZRrop0DKZ1nrGZZF1XeUDlQL02yfZ5ED9XECRJJgRC4PdeCPc9FQFZJwKWh0WCgv+2B4E2U5fjA8DbhI7ncJ0gnCaJx2+Z2iWKDsOC4PFQ3fDd2MqMUiGvSlyR6UcxxoJZpxKIoyCc9APOIDwsDSAAdPEKJUMlxw6N9RJlmQGTZixxDrgrBTEBkdwCyxPAyQruCSKcRy4O6Cs-jwZnwR0ZBHIjllWzboucGQ4unC7nO8DzCD3IosuwDzbwlJz2YK3AGw2kckeWf0fsIHINlIMl-RyADx1w2dpwITsk7SOx4WBtZCSQl0xypVK8bM0TIlURXe3KVVthdEpVBFVk+tIux9dKE92zKC38gbe30oCSxE5fYSDf983XRjSJ6L82yY+d1AEJmVkS3QJwzPRbBNpdFbcYYPgGTZF1UAAEJ6SgMxaOmmZrpZ9+P1mlkAGojzMij84olkAHkADKP8-6WQAJIANAefBWMwaY2jwMpKU0D-4K21ombYktf7nz6tvXe7cqyYEyAfBSx9jxn3-kIIGkAfyKCTA3G81cFpCRElnTskoSBmivEgsKvwhRjgUj6FQBloYLTqiTE6UBd6hQZhQ9MB49pWjgEzMRDIqJyK6PsXoahiI5iolojQOjOhrQfuNfY9MqFUUXGQYxekEYhUpneAU+izIoyhkJHeKEGKLh-LXQUgYLw1A1LIVefj1EZHkdrSmB4jz8CKAmLMTj6r6g0eaBAaT3SWMJA8dISAorWQDjsBu0o2iBhycgKKAwbSYGKQ6Ra2TclRUQE2WAp8-amkofUhqCQ9rsi1Eabobs9qKDqTkQMmR7hxQSpaJKGBAmdMgpCAYVAdg8HSVgRAzMp7jJQCQs0FNeFjNjHNGuWTAzU0nG8Q51ChGqAWkc0Q6RBkUnRKWCha8HlQGIuiaA+BEicgOT8DAFwqLfI1NsVASQO4-BcOExQ6ZMDFBnokUp+piiQyivMi+rMiAQlaGgjY7NFCYDtsMvkmt+jVIwLUyyBTJzUqUCUhWzS4BtNgB0jO1D0WJLyhBCQWAmylgZj+P8i5N5aFBHUq4EFZTHBeEpcqRc5ICNbv3F6alig8AAKoW0vpARISgAAiFTLKap1QAYROZZA1ihjV5MtXpdKrMzXHAALJtUIlvPVNq7W4Hde1L1rM9LSAhkA7YOrLLBohgAFTLLak1aDw3HFjZOQ1sUhYKxhNwMur8Mzv2ZXgDQrqZbQC2E7X8hrOD4BkrgwkGzHVN3jttBMGR9oB1RZVWK1VG1tQxC2vaaz21UJldIE0KyC5ryxvJLoicaiIEAhsYgSYfFpRzCO7CtFpSFz4YGadfbdptsOvXfmspKayB2q2wdh0R0oUxoSLcbIfwaGqsPcaXRV7qsDHRSyZBpBkVrYGEVGo9oQ2rroC4AAmKiQGqS1B+AANmg7+LkGgfgAFYkN-kuYkHh6HwI+VyKcJEGg9qmgZk8DM6hJQrXfT1Xoa6xIgEVJEFjUSeBGHPHQowoImrEDoOalkdz6D0H4DwFgonGBgAALxgHuBIMgaBoBgFBHQUEIAgA)

---

_Renamed from "(🐞) fix for E701 deletes type annotations" to "(🐞) fix for E731 deletes type annotations" by @KotlinIsland on 2023-04-14 07:08_

---

_Label `bug` added by @charliermarsh on 2023-04-14 12:44_

---

_Label `good first issue` added by @charliermarsh on 2023-04-14 12:44_

---

_Comment by @dhruvmanila on 2023-04-15 10:12_

I'll take this up today :)

---

_Assigned to @dhruvmanila by @charliermarsh on 2023-04-15 20:54_

---

_Comment by @charliermarsh on 2023-04-15 20:55_

Thanks @dhruvmanila!

---

_Referenced in [astral-sh/ruff#3983](../../astral-sh/ruff/pulls/3983.md) on 2023-04-16 06:34_

---

_Closed by @charliermarsh on 2023-04-16 23:15_

---
