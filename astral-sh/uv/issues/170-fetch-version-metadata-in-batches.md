---
number: 170
title: Fetch version metadata in batches
type: issue
state: closed
author: charliermarsh
labels:
  - performance
assignees: []
created_at: 2023-10-23T15:36:36Z
updated_at: 2024-04-08T14:28:58Z
url: https://github.com/astral-sh/uv/issues/170
synced_at: 2026-01-10T01:23:03Z
---

# Fetch version metadata in batches

---

_Issue opened by @charliermarsh on 2023-10-23 15:36_

When resolving:

```
urllib3<1.25.4
boto3
```

We end up testing hundreds of boto versions. After a point, we should start fetching the version metadata in parallel for these cases.


---

_Label `performance` added by @charliermarsh on 2023-10-24 01:18_

---

_Added to milestone `Future` by @charliermarsh on 2023-10-25 04:32_

---

_Comment by @konstin on 2023-11-24 12:44_

This is effectively blocked on being smarter with ranges for me, either by being a lot faster with intersections, or ideally having ranges with fewer segments.

Checking the first ~500 version of tf-models-nightly takes 12s for me, which are nearly exclusively spent on range intersection of ranges with a lot of segments, even though we should be able to merge the segments.

![image](https://github.com/astral-sh/puffin/assets/6826232/9665f3c7-efe1-4c0e-8332-4dea0a674981)


---

_Comment by @charliermarsh on 2023-11-24 12:45_

Is that with all the metadata cached? The fragmented ranges thing is similar to what @zanieb is looking at for error reporting and is related to an issue I filed on PubGrub about this.

---

_Comment by @konstin on 2023-11-24 12:57_

Yes, it basically does not make sense to work on this until https://github.com/pubgrub-rs/pubgrub/issues/135 is fixed or we have some workaround. We could try making intersections really fast but i think this would be effort spent in the wrong place when range shouldn't be that large in the first place.

---

_Referenced in [astral-sh/uv#1398](../../astral-sh/uv/issues/1398.md) on 2024-02-16 00:31_

---

_Comment by @konstin on 2024-02-20 10:33_

As an update, pubgrub is much faster now, and it will be even faster for this case with https://github.com/pubgrub-rs/pubgrub/pull/177 (the benchmark i used hits the boto case), so we can retry doing this.

---

_Referenced in [astral-sh/uv#2421](../../astral-sh/uv/pulls/2421.md) on 2024-03-13 18:57_

---

_Assigned to @konstin by @konstin on 2024-03-14 10:53_

---

_Referenced in [astral-sh/uv#2452](../../astral-sh/uv/pulls/2452.md) on 2024-03-14 11:01_

---

_Closed by @konstin on 2024-04-08 14:28_

---
