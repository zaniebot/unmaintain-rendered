```yaml
number: 9209
title: "Fix: Avoid parenthesizing subscript targets and values"
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: fix-avoid-parenthesizing-attribute-chains
created_at: 2023-12-20T07:51:30Z
updated_at: 2023-12-20T23:42:26Z
url: https://github.com/astral-sh/ruff/pull/9209
synced_at: 2026-01-10T23:07:18Z
```

# Fix: Avoid parenthesizing subscript targets and values

---

_Pull request opened by @MichaReiser on 2023-12-20 07:51_

## Summary

Avoid adding parentheses around assignment targets that contain subscripts and use the best fit layout for attribute chains containing subscripts. 

Fixes https://github.com/astral-sh/ruff/issues/9112

## Test Plan

Verified that the ecosystem changes now match black.


---

_Label `formatter` added by @MichaReiser on 2023-12-20 07:53_

---

_Label `preview` added by @MichaReiser on 2023-12-20 07:53_

---

_Comment by @github-actions[bot] on 2023-12-20 08:01_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+15 -20 lines in 5 files in 3 projects; 38 projects unchanged)

<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/98e1d2f8cec1593bb8bc6a8dbdd164372286ea09/pandas/core/reshape/melt.py#L122'>pandas/core/reshape/melt.py~L122</a>
```diff
     if frame.shape[1] > 0 and not any(
         not isinstance(dt, np.dtype) and dt._supports_2d for dt in frame.dtypes
     ):
-        mdata[value_name] = (
-            concat([frame.iloc[:, i] for i in range(frame.shape[1])]).values
-        )
+        mdata[value_name] = concat([
+            frame.iloc[:, i] for i in range(frame.shape[1])
+        ]).values
     else:
         mdata[value_name] = frame._values.ravel("F")
     for i, col in enumerate(var_name):
```

</p>
</details>
<details><summary><a href="https://github.com/tiangolo/fastapi">tiangolo/fastapi</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/tiangolo/fastapi/blob/36c26677682c4245183912e00fc057c05cc6cf7a/fastapi/dependencies/utils.py#L343'>fastapi/dependencies/utils.py~L343</a>
```diff
             if isinstance(arg, (params.Param, params.Body, params.Depends))
         ]
         if fastapi_specific_annotations:
-            fastapi_annotation: Union[
-                FieldInfo, params.Depends, None
-            ] = fastapi_specific_annotations[-1]
+            fastapi_annotation: Union[FieldInfo, params.Depends, None] = (
+                fastapi_specific_annotations[-1]
+            )
         else:
             fastapi_annotation = None
         if isinstance(fastapi_annotation, FieldInfo):
```

</p>
</details>
<details><summary><a href="https://github.com/zulip/zulip">zulip/zulip</a> (+9 -14 lines across 3 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/lib/message.py#L458'>zerver/lib/message.py~L458</a>
```diff
             # sender's user ID.
             obj["sender_full_name"] = str(UserProfile.INACCESSIBLE_USER_NAME)
             sender_id = obj["sender_id"]
-            obj["sender_email"] = (
-                Address(
-                    username=f"user{sender_id}", domain=get_fake_email_domain(realm_host)
-                ).addr_spec
-            )
+            obj["sender_email"] = Address(
+                username=f"user{sender_id}", domain=get_fake_email_domain(realm_host)
+            ).addr_spec
 
         MessageDict.set_sender_avatar(obj, client_gravatar, can_access_sender)
         if apply_markdown:
```
<a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/lib/push_notifications.py#L961'>zerver/lib/push_notifications.py~L961</a>
```diff
         # inaccessible user as we allow unsubscribed users to send
         # messages to streams. For direct messages, the guest gains
         # access to the user if they where previously inaccessible.
-        data["sender_email"] = (
-            Address(
-                username=f"user{message.sender.id}",
-                domain=get_fake_email_domain(message.realm.host),
-            ).addr_spec
-        )
+        data["sender_email"] = Address(
+            username=f"user{message.sender.id}", domain=get_fake_email_domain(message.realm.host)
+        ).addr_spec
     else:
         data["sender_email"] = message.sender.email
 
```
<a href='https://github.com/zulip/zulip/blob/653901fc30e4ae2b44326ca5f7efea01f9cc80f9/zerver/lib/users.py#L747'>zerver/lib/users.py~L747</a>
```diff
     for user_id in target_user_ids:
         target_user_subbed_recipients = target_user_subscriptions_dict[user_id]
         for recipient_id in target_user_subbed_recipients:
-            users_subbed_to_target_user_subscriptions_dict[
-                user_id
-            ] |= subscribers_dict_by_recipient_ids[recipient_id]
+            users_subbed_to_target_user_subscriptions_dict[user_id] |= (
+                subscribers_dict_by_recipient_ids[recipient_id]
+            )
 
     return users_subbed_to_target_user_subscriptions_dict
 
```

</p>
</details>




---

_Converted to draft by @MichaReiser on 2023-12-20 08:02_

---

_Marked ready for review by @MichaReiser on 2023-12-20 10:15_

---

_@konstin approved on 2023-12-20 11:52_

---

_Merged by @MichaReiser on 2023-12-20 23:42_

---

_Closed by @MichaReiser on 2023-12-20 23:42_

---

_Branch deleted on 2023-12-20 23:42_

---
