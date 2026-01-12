```yaml
number: 6484
title: Avoid omitting optional parentheses for argument-less parentheses
type: pull_request
state: merged
author: charliermarsh
labels:
  - formatter
assignees: []
merged: true
base: main
head: charlie/compare
created_at: 2023-08-10T19:37:38Z
updated_at: 2023-08-11T18:24:21Z
url: https://github.com/astral-sh/ruff/pull/6484
synced_at: 2026-01-12T02:52:04Z
```

# Avoid omitting optional parentheses for argument-less parentheses

---

_Pull request opened by @charliermarsh on 2023-08-10 19:37_

## Summary

This PR fixes some misformattings around optional parentheses for expressions.

I first noticed that we were misformatting this:

```python
return (
    unicodedata.normalize("NFKC", s1).casefold()
    == unicodedata.normalize("NFKC", s2).casefold()
)
```

The above is stable Black formatting, but we were doing:
```python
return unicodedata.normalize("NFKC", s1).casefold() == unicodedata.normalize(
    "NFKC", s2
).casefold()
```

Above, the "last" expression is a function call, so our `can_omit_optional_parentheses` was returning `true`...

However, it turns out that Black treats function calls differently depending on whether or not they have arguments -- presumedly because they'll never split empty parentheses, and so they're functionally non-useful. On further investigation, I believe this applies to all parenthesized expressions. If Black can't split on the parentheses, it doesn't leverage them when removing optional parentheses.

## Test Plan

Nice increase in similarity scores.

Before:

- `zulip`: 0.99702
- `django`: 0.99784
- `warehouse`: 0.99585
- `build`: 0.75623
- `transformers`: 0.99470
- `cpython`: 0.75989
- `typeshed`: 0.74853

After:

- `zulip`: 0.99705
- `django`: 0.99795
- `warehouse`: 0.99600
- `build`: 0.75623
- `transformers`: 0.99471
- `cpython`: 0.75989
- `typeshed`: 0.74853



---

_Renamed from "Avoid omitting optional parentheses for argument-less calls" to "Avoid omitting optional parentheses for argument-less parentheses" by @charliermarsh on 2023-08-10 19:49_

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-10 19:50_

---

_Review requested from @konstin by @charliermarsh on 2023-08-10 19:50_

---

_Marked ready for review by @charliermarsh on 2023-08-10 19:50_

---

_@charliermarsh reviewed on 2023-08-10 19:50_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-10 19:50_

@konstin - This feels wrong, but without it, we're dropping parentheses sometimes in call chain assignments. I assume the previous implementation was always returning `false` here, but I wonder if that was just a coincidence based on the definition of a call chain...

---

_Label `formatter` added by @charliermarsh on 2023-08-10 20:13_

---

_@MichaReiser reviewed on 2023-08-10 20:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-10 20:20_

Does black return false from the can omit function for these cases too?

@konstin how does the parentheses preservation for call chains work now?

---

_Comment by @github-actions[bot] on 2023-08-10 20:23_

## PR Check Results
### Benchmark
#### Linux
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.02      9.6±0.15ms     4.2 MB/sec    1.00      9.5±0.23ms     4.3 MB/sec
formatter/numpy/ctypeslib.py               1.00  1871.5±30.58µs     8.9 MB/sec    1.00  1878.3±59.00µs     8.9 MB/sec
formatter/numpy/globals.py                 1.03    216.0±4.37µs    13.7 MB/sec    1.00    209.0±8.02µs    14.1 MB/sec
formatter/pydantic/types.py                1.04      4.1±0.04ms     6.3 MB/sec    1.00      3.9±0.12ms     6.5 MB/sec
linter/all-rules/large/dataset.py          1.00     11.8±0.25ms     3.4 MB/sec    1.01     11.9±0.16ms     3.4 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      3.0±0.06ms     5.5 MB/sec    1.03      3.1±0.06ms     5.3 MB/sec
linter/all-rules/numpy/globals.py          1.01    461.5±5.89µs     6.4 MB/sec    1.00    455.9±6.79µs     6.5 MB/sec
linter/all-rules/pydantic/types.py         1.00      6.3±0.14ms     4.1 MB/sec    1.00      6.3±0.12ms     4.1 MB/sec
linter/default-rules/large/dataset.py      1.00      6.2±0.17ms     6.5 MB/sec    1.01      6.3±0.09ms     6.5 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00  1370.5±29.10µs    12.1 MB/sec    1.01  1386.4±18.80µs    12.0 MB/sec
linter/default-rules/numpy/globals.py      1.06    167.0±2.18µs    17.7 MB/sec    1.00    157.7±4.59µs    18.7 MB/sec
linter/default-rules/pydantic/types.py     1.02      2.8±0.04ms     9.1 MB/sec    1.00      2.8±0.07ms     9.2 MB/sec
```

#### Windows
```
group                                      main                                   pr
-----                                      ----                                   --
formatter/large/dataset.py                 1.01     14.3±0.52ms     2.8 MB/sec    1.00     14.2±0.46ms     2.9 MB/sec
formatter/numpy/ctypeslib.py               1.00      2.6±0.20ms     6.3 MB/sec    1.02      2.7±0.16ms     6.2 MB/sec
formatter/numpy/globals.py                 1.00   302.2±23.03µs     9.8 MB/sec    1.00   302.0±23.91µs     9.8 MB/sec
formatter/pydantic/types.py                1.00      5.8±0.26ms     4.4 MB/sec    1.00      5.8±0.28ms     4.4 MB/sec
linter/all-rules/large/dataset.py          1.00     18.7±0.46ms     2.2 MB/sec    1.01     18.8±0.52ms     2.2 MB/sec
linter/all-rules/numpy/ctypeslib.py        1.00      5.0±0.21ms     3.3 MB/sec    1.03      5.1±0.20ms     3.2 MB/sec
linter/all-rules/numpy/globals.py          1.01   631.5±32.10µs     4.7 MB/sec    1.00   622.5±33.09µs     4.7 MB/sec
linter/all-rules/pydantic/types.py         1.01      9.7±0.42ms     2.6 MB/sec    1.00      9.6±0.40ms     2.7 MB/sec
linter/default-rules/large/dataset.py      1.05     10.5±0.39ms     3.9 MB/sec    1.00     10.0±0.36ms     4.1 MB/sec
linter/default-rules/numpy/ctypeslib.py    1.00      2.1±0.10ms     7.8 MB/sec    1.00      2.1±0.10ms     7.8 MB/sec
linter/default-rules/numpy/globals.py      1.00   263.9±20.53µs    11.2 MB/sec    1.00   264.5±13.18µs    11.2 MB/sec
linter/default-rules/pydantic/types.py     1.00      4.5±0.19ms     5.6 MB/sec    1.00      4.5±0.18ms     5.6 MB/sec
```
<!-- thollander/actions-comment-pull-request "PR Check Results" -->

---

_@konstin reviewed on 2023-08-10 21:17_

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-10 21:17_

iirc `visitor.max_priority == OperatorPriority::Attribute && visitor.max_priority_count > 1` would return false, but i'm not sure if e.g. `(a).b().b()` is correctly handled here, i have to check tomorrow

---

_@charliermarsh reviewed on 2023-08-10 21:22_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-10 21:22_

I think with this change we no longer mark `OperatorPriority::Attribute` in that case, since `has_own_parentheses` now returns false for the the `.b()`?

---

_@charliermarsh reviewed on 2023-08-10 21:28_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-10 21:28_

(I can check what Black returns here.)

---

_@charliermarsh reviewed on 2023-08-10 21:33_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-10 21:33_

Real example that we no longer wrap properly:

```python
query = (
    UserMessage.select_for_update_query()
    .filter(
        user_profile=user_profile,
        message__recipient_id=stream_recipient_id,
    )
    .extra(
        where=[UserMessage.where_unread()],
    )
)
```

---

_@charliermarsh reviewed on 2023-08-10 21:41_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-10 21:41_

Removing the `if` here fixed it:

```rust
// `[a, b].test[300].dot`
Expr::Attribute(ast::ExprAttribute {
    range: _,
    value,
    attr: _,
    ctx: _,
}) => {
    ...
    if has_parentheses(value, self.source) {
        self.update_max_priority(OperatorPriority::Attribute);
    }
    ...
}
```

Micha, do you know why that was necessarily? I went back to the original PR, removed the `if`, and tests passed.

---

_@MichaReiser reviewed on 2023-08-11 05:55_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-11 05:55_

It could be related to the logic here

https://github.com/psf/black/blob/a44dc3d59eb46901f9fe893727280903df41fc20/src/black/brackets.py#L243-L250

The following method determines the max priority operator for black.

https://github.com/psf/black/blob/a44dc3d59eb46901f9fe893727280903df41fc20/src/black/brackets.py#L76-L134

which calls into the above method. 

The way I would debug this. Log the max priority operator and compare it between Ruff and Black. And do the same for the overall change

The logic here is all reverse-engineered. There's a chance that I got it wrong or didn't notice that something is wrong because it created better formatting back then. 

I recommend changing this in isolation and then go through the created black differences manually.

---

_@MichaReiser requested changes on 2023-08-11 05:57_

Please try out black and document if what we return from `can_omit_optional_parentheses` now matches Black's return value. If it already did, then we have to figure out why our formatting doesn't matches Black. 

I'm sorry to be annoying here. I'm cautious about changing the behavior of the whole `maybe_parenthesized` logic because getting something slightly wrong can have far-reaching effects, and we need to avoid fixing symptoms (that then require us to fix other symptoms). If the behavior matches Black's behavior, then it means we have some deviation elsewhere that we need to understand.

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-11 06:01_

Removing this line on `main` doesn't cause any changes in the test fixtures.

I went through the exercise of differing `main` formatting of Zulip to the formatting you get if you remove this line -- here's the full diff:

```diff
diff --git a/zerver/lib/import_realm.py b/zerver/lib/import_realm.py
index c598959819..d93307fd91 100644
--- a/zerver/lib/import_realm.py
+++ b/zerver/lib/import_realm.py
@@ -1146,9 +1146,11 @@ def do_import_realm(import_dir: Path, subdomain: str, processes: int = 1) -> Rea
     # Ensure the invariant that there's always a realm-creation audit
     # log event, even if the export was generated by an export tool
     # that does not create RealmAuditLog events.
-    if not RealmAuditLog.objects.filter(
-        realm=realm, event_type=RealmAuditLog.REALM_CREATED
-    ).exists():
+    if (
+        not RealmAuditLog.objects.filter(
+            realm=realm, event_type=RealmAuditLog.REALM_CREATED
+        ).exists()
+    ):
         RealmAuditLog.objects.create(
             realm=realm,
             event_type=RealmAuditLog.REALM_CREATED,
diff --git a/zerver/lib/push_notifications.py b/zerver/lib/push_notifications.py
index 5e6e2e44ae..efba34096e 100644
--- a/zerver/lib/push_notifications.py
+++ b/zerver/lib/push_notifications.py
@@ -433,9 +433,11 @@ def send_android_push_notification(
             if reg_id == new_reg_id:
                 # I'm not sure if this should happen. In any case, not really actionable.
                 logger.warning("GCM: Got canonical ref but it already matches our ID %s!", reg_id)
-            elif not DeviceTokenClass.objects.filter(
-                token=new_reg_id, kind=DeviceTokenClass.GCM
-            ).count():
+            elif (
+                not DeviceTokenClass.objects.filter(
+                    token=new_reg_id, kind=DeviceTokenClass.GCM
+                ).count()
+            ):
                 # This case shouldn't happen; any time we get a canonical ref it should have been
                 # previously registered in our system.
                 #
diff --git a/zerver/management/commands/sync_ldap_user_data.py b/zerver/management/commands/sync_ldap_user_data.py
index a8a3bd259a..59683c9db3 100644
--- a/zerver/management/commands/sync_ldap_user_data.py
+++ b/zerver/management/commands/sync_ldap_user_data.py
@@ -42,12 +42,14 @@ def sync_ldap_user_data(
                     "Use the --force option if the mass deactivation is intended."
                 )
             for string_id in realms:
-                if not UserProfile.objects.filter(
-                    is_bot=False,
-                    is_active=True,
-                    realm__string_id=string_id,
-                    role=UserProfile.ROLE_REALM_OWNER,
-                ).exists():
+                if (
+                    not UserProfile.objects.filter(
+                        is_bot=False,
+                        is_active=True,
+                        realm__string_id=string_id,
+                        role=UserProfile.ROLE_REALM_OWNER,
+                    ).exists()
+                ):
                     raise Exception(
                         f"LDAP sync would have deactivated all owners of realm {string_id}. "
                         "This is most likely due "
diff --git a/zerver/tests/test_tornado.py b/zerver/tests/test_tornado.py
index 1475bd1046..1e4ef8870a 100644
--- a/zerver/tests/test_tornado.py
+++ b/zerver/tests/test_tornado.py
@@ -69,8 +69,10 @@ class TornadoWebTestCase(ZulipTestCase):
         if "HTTP_HOST" in kwargs:
             kwargs["headers"]["Host"] = kwargs["HTTP_HOST"]
             del kwargs["HTTP_HOST"]
-        return await self.http_client.fetch(
-            f"http://127.0.0.1:{self.port}{path}", method=method, **kwargs
+        return (
+            await self.http_client.fetch(
+                f"http://127.0.0.1:{self.port}{path}", method=method, **kwargs
+            )
         )
 
     def login_user(self, *args: Any, **kwargs: Any) -> None:
diff --git a/zerver/views/reactions.py b/zerver/views/reactions.py
index 316d82eb0d..b1f8317bcf 100644
--- a/zerver/views/reactions.py
+++ b/zerver/views/reactions.py
@@ -59,12 +59,14 @@ def remove_reaction(
         # corresponding code using the current data.
         emoji_code = get_emoji_data(message.sender.realm_id, emoji_name).emoji_code
 
-    if not Reaction.objects.filter(
-        user_profile=user_profile,
-        message=message,
-        emoji_code=emoji_code,
-        reaction_type=reaction_type,
-    ).exists():
+    if (
+        not Reaction.objects.filter(
+            user_profile=user_profile,
+            message=message,
+            emoji_code=emoji_code,
+            reaction_type=reaction_type,
+        ).exists()
+    ):
         raise ReactionDoesNotExistError
 
     # Unlike adding reactions, while deleting a reaction, we don't
diff --git a/zerver/views/realm_emoji.py b/zerver/views/realm_emoji.py
index 35bf25932c..2c5f22fe4d 100644
--- a/zerver/views/realm_emoji.py
+++ b/zerver/views/realm_emoji.py
@@ -55,9 +55,11 @@ def upload_emoji(
 
 
 def delete_emoji(request: HttpRequest, user_profile: UserProfile, emoji_name: str) -> HttpResponse:
-    if not RealmEmoji.objects.filter(
-        realm=user_profile.realm, name=emoji_name, deactivated=False
-    ).exists():
+    if (
+        not RealmEmoji.objects.filter(
+            realm=user_profile.realm, name=emoji_name, deactivated=False
+        ).exists()
+    ):
         raise ResourceNotFoundError(
             _("Emoji '{emoji_name}' does not exist").format(emoji_name=emoji_name)
         )
```

---

_@charliermarsh reviewed on 2023-08-11 06:01_

---

_@MichaReiser reviewed on 2023-08-11 06:04_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-11 06:04_

Does the formatting now match Black's formatting? It otherwise mainly looks like a regression. I leave it to you to investigate this more.

---

_@charliermarsh reviewed on 2023-08-11 06:09_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-11 06:09_

No, I thought it was clear that those do not match Black's formatting. I was reporting back for completeness.

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:584 on 2023-08-11 09:43_

i'd add that we count `()`, `[]` and `{}` as parentheses

---

_@konstin reviewed on 2023-08-11 10:19_

---

_@charliermarsh reviewed on 2023-08-11 13:49_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-11 13:49_

Ok, I think this is the relevant line in Black: https://github.com/psf/black/blob/a44dc3d59eb46901f9fe893727280903df41fc20/src/black/lines.py#L816

---

_@MichaReiser reviewed on 2023-08-11 15:09_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-11 15:09_

Seems reasonable. Does that mean that we now return the same from `can_omit_parentheses` as black?

---

_@charliermarsh reviewed on 2023-08-11 15:30_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:278 on 2023-08-11 15:30_

Yeah

---

_Comment by @charliermarsh on 2023-08-11 15:31_

Okay, I believe this now matches Black's behavior based on debugging against Black. The `CanOmitOptionalParenthesesVisitor` is now unchanged. What's different is the check in `can_omit_optional_parentheses`. When we hit the "Only use the layout if the first or last expression has parentheses..." case, we check to ensure that the parentheses are non-empty.

(I will update the similarity score when complete.)

---

_Review requested from @MichaReiser by @charliermarsh on 2023-08-11 15:31_

---

_Review requested from @konstin by @charliermarsh on 2023-08-11 15:32_

---

_Converted to draft by @charliermarsh on 2023-08-11 15:34_

---

_Comment by @charliermarsh on 2023-08-11 15:34_

Actually, hold on, the similarity scores are improved, but I still see some odd changes in Zulip.

---

_Marked ready for review by @charliermarsh on 2023-08-11 15:57_

---

_Comment by @charliermarsh on 2023-08-11 15:57_

Okay, I think this is finally ready.

Here's the diff against `main` formatting of Zulip:

```diff
diff --git a/zerver/views/home.py b/zerver/views/home.py
index 2873e6f651..725c54e2f1 100644
--- a/zerver/views/home.py
+++ b/zerver/views/home.py
@@ -213,9 +213,10 @@ def home_real(request: HttpRequest) -> HttpResponse:
         first_in_realm = realm_user_count(user_profile.realm) == 1
         # If you are the only person in the realm and you didn't invite
         # anyone, we'll continue to encourage you to do so on the frontend.
-        prompt_for_invites = first_in_realm and not PreregistrationUser.objects.filter(
-            referred_by=user_profile
-        ).count()
+        prompt_for_invites = (
+            first_in_realm
+            and not PreregistrationUser.objects.filter(referred_by=user_profile).count()
+        )
         needs_tutorial = user_profile.tutorial_status == UserProfile.TUTORIAL_WAITING
 
     else:
diff --git a/zerver/views/message_send.py b/zerver/views/message_send.py
index 9d39721c82..fea12afaa9 100644
--- a/zerver/views/message_send.py
+++ b/zerver/views/message_send.py
@@ -89,9 +89,10 @@ def same_realm_zephyr_user(user_profile: UserProfile, email: str) -> bool:
 
     # Assumes allow_subdomains=False for all RealmDomain's corresponding to
     # these realms.
-    return user_profile.realm.is_zephyr_mirror_realm and RealmDomain.objects.filter(
-        realm=user_profile.realm, domain=domain
-    ).exists()
+    return (
+        user_profile.realm.is_zephyr_mirror_realm
+        and RealmDomain.objects.filter(realm=user_profile.realm, domain=domain).exists()
+    )
 
 
 def same_realm_irc_user(user_profile: UserProfile, email: str) -> bool:
```

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:587 on 2023-08-11 16:12_

this comment should include whether we consider comments as non-empty

---

_Review comment by @konstin on `crates/ruff_python_formatter/src/expression/mod.rs`:603 on 2023-08-11 16:16_

i'd switch the checks because i expect `has_own_parentheses` to be faster and be `Some(_)` more often

---

_@konstin approved on 2023-08-11 16:17_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:604 on 2023-08-11 17:20_

The comment needs updating. 

Nit: We could also do `OwnParentheses::from_expr`

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:609 on 2023-08-11 17:21_

I think we should use `comments.has_dangling_comments(item)` because this is our source of truth when it comes to formatting comments (`placement.rs` could decide to move some comments out, this check would then return the wrong result)

---

_@MichaReiser approved on 2023-08-11 17:22_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:609 on 2023-08-11 17:33_

Ahh, smart

---

_@charliermarsh reviewed on 2023-08-11 17:33_

---

_@charliermarsh reviewed on 2023-08-11 17:37_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:604 on 2023-08-11 17:37_

Does it? I think this comment is up-to-date.

---

_@MichaReiser reviewed on 2023-08-11 17:40_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:604 on 2023-08-11 17:40_

The Returns `true` part needs updating because it now returns an Option

---

_@charliermarsh reviewed on 2023-08-11 17:54_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:603 on 2023-08-11 17:54_

Yeah it's just a little tricky because if `has_own_parentheses` tells us the node has parentheses, but they're empty, then we still need to check `is_expression_parenthesized` to handle cases like `([])` (needs to return non-empty, even though the node's _own_ parentheses are empty). I reversed the cases, it's just more branches.

---

_Merged by @charliermarsh on 2023-08-11 17:58_

---

_Closed by @charliermarsh on 2023-08-11 17:58_

---

_Branch deleted on 2023-08-11 17:58_

---
