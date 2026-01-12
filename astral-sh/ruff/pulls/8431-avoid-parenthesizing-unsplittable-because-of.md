```yaml
number: 8431
title: Avoid parenthesizing unsplittable because of comments
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - style
assignees: []
merged: true
base: main
head: unsplittable-statement-comments
created_at: 2023-11-02T02:45:36Z
updated_at: 2023-11-03T05:13:01Z
url: https://github.com/astral-sh/ruff/pull/8431
synced_at: 2026-01-10T23:40:55Z
```

# Avoid parenthesizing unsplittable because of comments

---

_Pull request opened by @MichaReiser on 2023-11-02 02:45_

## Summary

This PR implements a fix for https://github.com/astral-sh/ruff/issues/8041 where Ruff parenthesized unsplittable values in assignments if it has a trailing end-of-line comment that makes the assignment go over the line length. 

```python
____aaa = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbvvvvvvvvvvvv  # c

# Formatted
____aaa = (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbvvvvvvvvvvvv
)  # c
```

The way black *solves* this is by moving the trailing end-of-line comment into the assignment so that it only parenthesizes the value (and comment) if formatting it on a new line makes both fit. 

```python
# 88 characters unparenthesized, fits on a single line
____aaa = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbvvvvv  # c

# 88 characters parenthesized
____aaa = (
    aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbvvvvvvvvvv  # c
)

# 89 characters parenthesized. Collapse to avoid unnecessary parentheses
____aaa = aaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaaabbbbbbbbbbbbbbbbbbbbbbbbbbbvvvvvvvvvvvv  # c
```

Closes https://github.com/astral-sh/ruff/issues/8041

## Notes

There's one downside to this approach. It violates our principle of never collapsing an expression if it has trailing line comments. Let's take this example:

```python
a = (
	short  # comment
)
```

It is now formatted as 

```python
a = short  # comment
```

To ensure the new comment style is reversible. 

## Non-Fluent attribute chains

Non-fluent attribute chains aren't splittable. However, Black formats the comments after the closing parentheses as ruff used to before this change (related https://github.com/astral-sh/ruff/issues/8182). I decided to favour Black compatibility over consistence because it introduces about 30 file regressions in the similarity index check. Which is more than this PR fixes.

## Test Plan

I added extensive tests and reviewed the ecosystem changes. They all seem to capture better the user's intent (and e.g. avoid moving pragma comments.

**Main**

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| home-assistant |           0.99963 |             10596 |               148 |
| poetry         |           0.99925 |               317 |                12 |
| transformers   |           0.99967 |              2657 |               328 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| zulip          |           0.99970 |              1459 |                22 |

**PR**

| project        | similarity index  | total files       | changed files     |
|----------------|------------------:|------------------:|------------------:|
| cpython        |           0.75804 |              1799 |              1648 |
| django         |           0.99984 |              2772 |                34 |
| **home-assistant** |           0.99963 |             10596 |               143 | 
| poetry         |           0.99925 |               317 |                12 |
| **transformers**   |           0.99967 |              2657 |               322 |
| twine          |           1.00000 |                33 |                 0 |
| typeshed       |           0.99980 |              3669 |                18 |
| warehouse      |           0.99977 |               654 |                13 |
| **zulip**          |           0.99970 |              1459 |                21 |

---

_Comment by @MichaReiser on 2023-11-02 02:45_

Current dependencies on/for this PR:
* main
  * **PR #8431** <a href="https://app.graphite.dev/github/pr/astral-sh/ruff/8431" target="_blank"><img src="https://static.graphite.dev/graphite-32x32-black.png" alt="Graphite" width="10px" height="10px"/></a>  👈

This [stack of pull requests](https://stacking.dev/?utm_source=stack-comment) is managed by [Graphite](https://app.graphite.dev/github/pr/astral-sh/ruff/8431?utm_source=stack-comment).

---

_Comment by @github-actions[bot] on 2023-11-02 03:04_

## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+352 -378 lines in 69 files in 40 projects; 1 project error)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+4 -6 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/PostHog/HouseWatch/blob/bf61a8e227311b3bc2fecd53823e4c1b37eb4a6d/housewatch/settings/__init__.py#L260'>housewatch/settings/__init__.py~L260</a>
```diff
 CELERY_IMPORTS = []  # type: ignore
 CELERY_BROKER_URL = RABBITMQ_URL  # celery connects to rabbitmq
 CELERY_BEAT_MAX_LOOP_INTERVAL = (
-    30
-)  # sleep max 30sec before checking for new periodic events
+    30  # sleep max 30sec before checking for new periodic events
+)
 CELERY_RESULT_BACKEND = REDIS_URL  # stores results for lookup when processing
-CELERY_IGNORE_RESULT = (
-    True
-)  # only applies to delay(), must do @shared_task(ignore_result=True) for apply_async
+CELERY_IGNORE_RESULT = True  # only applies to delay(), must do @shared_task(ignore_result=True) for apply_async
 CELERY_RESULT_EXPIRES = timedelta(
     days=4
 )  # expire tasks after 4 days instead of the default 1
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+5 -9 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/server.py#L880'>rasa/server.py~L880</a>
```diff
 
         try:
             async with app.ctx.agent.lock_store.lock(conversation_id):
-                tracker = await (
-                    app.ctx.agent.processor.fetch_tracker_and_update_session(
-                        conversation_id
-                    )
+                tracker = await app.ctx.agent.processor.fetch_tracker_and_update_session(
+                    conversation_id
                 )
 
                 output_channel = _get_output_channel(request, tracker)
```
<a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/server.py#L933'>rasa/server.py~L933</a>
```diff
 
         try:
             async with app.ctx.agent.lock_store.lock(conversation_id):
-                tracker = await (
-                    app.ctx.agent.processor.fetch_tracker_and_update_session(
-                        conversation_id
-                    )
+                tracker = await app.ctx.agent.processor.fetch_tracker_and_update_session(
+                    conversation_id
                 )
                 output_channel = _get_output_channel(request, tracker)
                 if intent_to_trigger not in app.ctx.agent.domain.intents:
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+3 -7 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/tests/synthesis/test_deep_feature_synthesis.py#L229'>featuretools/tests/synthesis/test_deep_feature_synthesis.py~L229</a>
```diff
 
 
 def test_ignore_columns_input_type(es):
-    error_msg = (
-        r"ignore_columns should be dict\[str -> list\]"
-    )  # need to use string literals to avoid regex params
+    error_msg = r"ignore_columns should be dict\[str -> list\]"  # need to use string literals to avoid regex params
     wrong_input_type = {"log": "value"}
     with pytest.raises(TypeError, match=error_msg):
         DeepFeatureSynthesis(
```
<a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/tests/synthesis/test_deep_feature_synthesis.py#L253'>featuretools/tests/synthesis/test_deep_feature_synthesis.py~L253</a>
```diff
 
 
 def test_ignore_columns_with_nonstring_keys(es):
-    error_msg = (
-        r"ignore_columns should be dict\[str -> list\]"
-    )  # need to use string literals to avoid regex params
+    error_msg = r"ignore_columns should be dict\[str -> list\]"  # need to use string literals to avoid regex params
     wrong_input_keys = {1: ["a", "b", "c"]}
     with pytest.raises(TypeError, match=error_msg):
         DeepFeatureSynthesis(
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+32 -36 lines across 6 files)</summary>
<p>
<pre>ruff format --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/AWS_SystemManager/Integrations/AWSSystemManager/AWSSystemManager.py#L31'>Packs/AWS_SystemManager/Integrations/AWSSystemManager/AWSSystemManager.py~L31</a>
```diff
 }
 SERVICE_NAME = "ssm"  # Amazon Simple Systems Manager (SSM).
 DEFAULT_TIMEOUT = 600  # Default timeout for polling commands.
-MAXIMUM_COMMAND_TIMEOUT = (
-    2592000  # Maximum timeout for running commands in ssm (30 days).
-)
+MAXIMUM_COMMAND_TIMEOUT = 2592000  # Maximum timeout for running commands in ssm (30 days).
 MINIMUM_COMMAND_TIMEOUT = 30  # Minimum timeout for running commands in ssm.
 DEFAULT_INTERVAL_IN_SECONDS = 30  # Interval for polling commands.
 TERMINAL_AUTOMATION_STATUSES = {  # the status for run automation command
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/AWS_SystemManager/Integrations/AWSSystemManager/AWSSystemManager.py#L1480'>Packs/AWS_SystemManager/Integrations/AWSSystemManager/AWSSystemManager.py~L1480</a>
```diff
     aws_role_arn = params.get("roleArn")
     aws_role_session_name = params.get("roleSessionName")
     aws_role_session_duration = params.get("sessionDuration")
-    aws_role_policy = (
-        None  # added it for using AWSClient class without changing the code
-    )
+    aws_role_policy = None  # added it for using AWSClient class without changing the code
     timeout = params["timeout"]
     retries = params["retries"]
 
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L150'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L150</a>
```diff
         SMB_CONNECTIONS_QUERY_SUFFIX = "\n| summarize RemoteIPCount=dcount(RemoteIP) by DeviceName, InitiatingProcessFileName, InitiatingProcessId, InitiatingProcessCreationTime\n|{} limit {}"  # noqa: E501
         CREDENTIAL_DUMPING_QUERY_SUFFIX = "\n| project Timestamp, DeviceName, ActionType, FileName, ProcessCommandLine, AccountName, InitiatingProcessIntegrityLevel, InitiatingProcessTokenElevation\n| limit {}"  # noqa: E501
         MANAGEMENT_CONNECTION_QUERY_SUFFIX = (
-            "\n| summarize TotalCount=count() by DeviceName,LocalIP,RemoteIP,RemotePort\n| order by TotalCount\n| limit {}"
-        )  # noqa: E501
+            "\n| summarize TotalCount=count() by DeviceName,LocalIP,RemoteIP,RemotePort\n| order by TotalCount\n| limit {}"  # noqa: E501
+        )
 
         def __init__(
             self,
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L257'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L257</a>
```diff
         """QUERY PREFIX"""
 
         SCHEDULE_JOB_QUERY_PREFIX = (
-            'DeviceEvents | where ActionType == "ScheduledTaskCreated" and InitiatingProcessAccountSid != "S-1-5-18" and'
-        )  # noqa: E501
+            'DeviceEvents | where ActionType == "ScheduledTaskCreated" and InitiatingProcessAccountSid != "S-1-5-18" and'  # noqa: E501
+        )
         REGISTRY_ENTRY_QUERY_PREFIX = 'DeviceRegistryEvents | where ActionType == "RegistryValueSet" and'
         STARTUP_FOLDER_CHANGES_QUERY_PREFIX = 'DeviceFileEvents | where FolderPath contains @"\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup" and ActionType == "FileCreated" and'  # noqa: E501
         NEW_SERVICE_CREATED_QUERY_PREFIX = 'DeviceRegistryEvents | where RegistryKey contains @"HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services" and ActionType == "RegistryKeyCreated" and'  # noqa: E501
         SERVICE_UPDATED_QUERY_PREFIX = 'DeviceRegistryEvents | where RegistryKey contains @"HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services" and ActionType has_any ("RegistryValueSet","RegistryKeyCreated") and'  # noqa: E501
         FILE_REPLACED_QUERY_PREFIX = (
-            'DeviceFileEvents | where FolderPath contains @"C:\Program Files" and ActionType == "FileModified" and'
-        )  # noqa: E501
+            'DeviceFileEvents | where FolderPath contains @"C:\Program Files" and ActionType == "FileModified" and'  # noqa: E501
+        )
         NEW_USER_QUERY_PREFIX = 'DeviceEvents | where ActionType == "UserAccountCreated" and'
         NEW_GROUP_QUERY_PREFIX = 'DeviceEvents | where ActionType == "SecurityGroupCreated" and'
         GROUP_USER_CHANGE_QUERY_PREFIX = 'DeviceEvents | where ActionType == "UserAccountAddedToLocalGroup" and'
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L576'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L576</a>
```diff
         GENERIC_PROCESS_DETAILS_QUERY_PREFIX = "DeviceProcessEvents | where"
         BECAONING_QUERY_PREFIX = "DeviceNetworkEvents | where"
         POWERSHELL_EXECUTION_PROCESS_QUERY_PREFIX = (
-            'DeviceProcessEvents | where FileName in~ ("powershell.exe", "powershell_ise.exe",".ps") and'
-        )  # noqa: E501
+            'DeviceProcessEvents | where FileName in~ ("powershell.exe", "powershell_ise.exe",".ps") and'  # noqa: E501
+        )
         POWERSHELL_EXECUTION_PROCESS_UNSIGNED_QUERY_PREFIX = 'DeviceProcessEvents | where FileName in~ ("powershell.exe", "powershell_ise.exe",".ps") and ( InitiatingProcessFileName != "SenerIR.exe" and InitiatingProcessParentFileName != "MsSense.exe" and InitiatingProcessSignatureStatus != "Valid" ) and (InitiatingProcessFileName != "CompatTelRunner.exe" and InitiatingProcessParentFileName != "CompatTelRunner.exe")'  # noqa: E501
 
         """QUERY SUFFIX"""
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L741'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L741</a>
```diff
         """QUERY SUFFIX"""
         EXTERNAL_ADDRESSES_QUERY_SUFFIX = "\n| summarize TotalConnections = count() by DeviceName, RemoteIP, RemotePort, InitiatingProcessFileName,InitiatingProcessFolderPath | order by TotalConnections\n| limit {}"  # noqa: E501
         DNS_QUERY_SUFFIX = (
-            "| project Timestamp,DeviceName,ActionType,RemoteIP,Packetinfo = url_decode(AdditionalFields)\n| limit {}"
-        )  # noqa: E501
+            "| project Timestamp,DeviceName,ActionType,RemoteIP,Packetinfo = url_decode(AdditionalFields)\n| limit {}"  # noqa: E501
+        )
         ENCODED_COMMANDS_QUERY_SUFFIX = "\n| limit {}"
 
         def __init__(
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L894'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L894</a>
```diff
         COMPROMISED_INFORMATION_QUERY_SUFFIX = "\n| project Timestamp, DeviceId, DeviceName, ActionType, FileName, FolderPath, SHA1, SHA256, MD5, InitiatingProcessFileName\n| limit {}"  # noqa: E501
         CONNECTED_DEVICES_QUERY_SUFFIX = "\n| summarize by DeviceName\n| limit {}"
         ACTION_TYPES_QUERY_SUFFIX = (
-            "\n| summarize Number_of_actions=count(ActionType) by ActionType,DeviceName | order by Number_of_actions\n| limit {}"
-        )  # noqa: E501
+            "\n| summarize Number_of_actions=count(ActionType) by ActionType,DeviceName | order by Number_of_actions\n| limit {}"  # noqa: E501
+        )
         COMMON_FILES_QUERY_SUFFIX = "\n| summarize Number_of_accoiated_events=count(FileName) by FileName, MD5, SHA1, SHA256 | order by Number_of_accoiated_events\n| limit {}"  # noqa: E501
 
         def __init__(
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/SentinelOne/Integrations/SentinelOneEventCollector/SentinelOneEventCollector_test.py#L7'>Packs/SentinelOne/Integrations/SentinelOneEventCollector/SentinelOneEventCollector_test.py~L7</a>
```diff
 """ CONSTANTS """
 
 ACTIVITIES_MOCK_URL = (
-    "https://test.com/web/api/v2.1/activities?createdAt__gt=2022-01-04+00%3A00%3A00&limit=1000&sortBy=createdAt&sortOrder=asc"
-)  # noqa: E501
+    "https://test.com/web/api/v2.1/activities?createdAt__gt=2022-01-04+00%3A00%3A00&limit=1000&sortBy=createdAt&sortOrder=asc"  # noqa: E501
+)
 ACTIVITIES_SECOND_MOCK_URL = "https://test.com/web/api/v2.1/activities?createdAt__gt=2022-09-06T20%3A37%3A55.912951Z&limit=1000&sortBy=createdAt&sortOrder=asc"  # noqa: E501
 THREATS_MOCK_URL = (
-    "https://test.com/web/api/v2.1/threats?createdAt__gt=2022-01-04+00%3A00%3A00&limit=1000&sortBy=createdAt&sortOrder=asc"
-)  # noqa: E501
+    "https://test.com/web/api/v2.1/threats?createdAt__gt=2022-01-04+00%3A00%3A00&limit=1000&sortBy=createdAt&sortOrder=asc"  # noqa: E501
+)
 THREATS_SECOND_MOCK_URL = "https://test.com/web/api/v2.1/threats?createdAt__gt=2022-12-20T15%3A51%3A17.514437Z&limit=1000&sortBy=createdAt&sortOrder=asc"  # noqa: E501
 ALERTS_MOCK_URL = "https://test.com/web/api/v2.1/cloud-detection/alerts?limit=1000&createdAt__gt=2022-01-04+00%3A00%3A00&sortBy=alertInfoCreatedAt&sortOrder=asc"  # noqa: E501
 ALERTS_SECOND_MOCK_URL = "https://test.com/web/api/v2.1/cloud-detection/alerts?limit=1000&createdAt__gt=2022-12-20T13%3A54%3A43.027000Z&sortBy=alertInfoCreatedAt&sortOrder=asc"  # noqa: E501
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/XsoarWebserver/Integrations/XSOARWebServer/XSOARWebServer.py#L87'>Packs/XsoarWebserver/Integrations/XSOARWebServer/XSOARWebServer.py~L87</a>
```diff
         link_uuid = str(uuid.uuid4())
         partial_link = f"{xsoar_external_url}/instance/execute/{integration_instance_name}/process/{entry_uuid}/{link_uuid}/"
         partial_link_port = (
-            f"{xsoar_external_url}:{port}/instance/execute/{integration_instance_name}/process/{entry_uuid}/{link_uuid}/"
-        )  # noqa: E501
+            f"{xsoar_external_url}:{port}/instance/execute/{integration_instance_name}/process/{entry_uuid}/{link_uuid}/"  # noqa: E501
+        )
         temp_link_tracker = {"response": "", "response_received": False, "emailaddress": email}
 
         for ind, action in enumerate(input_list):
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/XsoarWebserver/Integrations/XSOARWebServer/XSOARWebServer.py#L131'>Packs/XsoarWebserver/Integrations/XSOARWebServer/XSOARWebServer.py~L131</a>
```diff
         link_uuid = str(uuid.uuid4())
         partial_link = f"{xsoar_external_url}/instance/execute/{integration_instance_name}/processform/{entry_uuid}/{link_uuid}/"
         partial_link_port = (
-            f"{xsoar_external_url}:{port}/instance/execute/{integration_instance_name}/processform/{entry_uuid}/{link_uuid}/"
-        )  # noqa: E501
+            f"{xsoar_external_url}:{port}/instance/execute/{integration_instance_name}/processform/{entry_uuid}/{link_uuid}/"  # noqa: E501
+        )
 
         if xsoar_proxy == "true":
             action_url = partial_link
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/rasterize/Integrations/rasterize/rasterize.py#L42'>Packs/rasterize/Integrations/rasterize/rasterize.py~L42</a>
```diff
 DEFAULT_W, DEFAULT_H = "600", "800"
 DEFAULT_W_WIDE = "1024"
 CHROME_USER_AGENT = (
-    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.64 Safari/537.36"
-)  # noqa
+    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.64 Safari/537.36"  # noqa
+)
 MAX_FULLSCREEN_W = 8000
 MAX_FULLSCREEN_H = 8000
 DRIVER_LOG = f"{tempfile.gettempdir()}/chromedriver.log"
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Tests/Marketplace/search_and_install_packs.py#L37'>Tests/Marketplace/search_and_install_packs.py~L37</a>
```diff
 PACKS_DIR = "Packs"
 PACK_METADATA_FILE = Pack.USER_METADATA
 GITLAB_PACK_METADATA_URL = (
-    f"{{gitlab_url}}/api/v4/projects/{CONTENT_PROJECT_ID}/repository/files/{PACKS_DIR}%2F{{pack_id}}%2F{PACK_METADATA_FILE}"
-)  # noqa: E501
+    f"{{gitlab_url}}/api/v4/projects/{CONTENT_PROJECT_ID}/repository/files/{PACKS_DIR}%2F{{pack_id}}%2F{PACK_METADATA_FILE}"  # noqa: E501
+)
 
 
 @lru_cache
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/tests/unit/fake_api.py#L6'>tests/unit/fake_api.py~L6</a>
```diff
 
 FAKE_CONTAINER_ID = "81cf499cc928ce3fedc250a080d2b9b978df20e4517304c45211e8a68b33e254"
 FAKE_IMAGE_ID = (
-    "sha256:fe7a8fc91d3f17835cbb3b86a1c60287500ab01a53bc79c4497d09f07a3f0688"
-)  # noqa: E501
+    "sha256:fe7a8fc91d3f17835cbb3b86a1c60287500ab01a53bc79c4497d09f07a3f0688"  # noqa: E501
+)
 FAKE_EXEC_ID = "b098ec855f10434b5c7c973c78484208223a83f663ddaefb0f02a242840cb1c7"
 FAKE_NETWORK_ID = "1999cfb42e414483841a125ade3c276c3cb80cb3269b14e339354ac63a31b02c"
 FAKE_IMAGE_NAME = "test_image"
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+6 -6 lines across 2 files)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/milvus-io/pymilvus/blob/9ad0ceada72396be890b746fe3a78ed4d74b3589/examples/example_bulkinsert_json.py#L50'>examples/example_bulkinsert_json.py~L50</a>
```diff
 _JSON_FIELD_NAME = "json_field"
 _VARCHAR_FIELD_NAME = "varchar_field"
 _DYNAMIC_FIELD_NAME = (
-    "$meta"
-)  # dynamic field, the internal name is "$meta", enable_dynamic_field=True
+    "$meta"  # dynamic field, the internal name is "$meta", enable_dynamic_field=True
+)
 
 # minio
 DEFAULT_BUCKET_NAME = "a-bucket"
```
<a href='https://github.com/milvus-io/pymilvus/blob/9ad0ceada72396be890b746fe3a78ed4d74b3589/examples/example_bulkinsert_numpy.py#L51'>examples/example_bulkinsert_numpy.py~L51</a>
```diff
 _JSON_FIELD_NAME = "json_field"
 _VARCHAR_FIELD_NAME = "varchar_field"
 _DYNAMIC_FIELD_NAME = (
-    "$meta"
-)  # dynamic field, the internal name is "$meta", enable_dynamic_field=True
+    "$meta"  # dynamic field, the internal name is "$meta", enable_dynamic_field=True
+)
 
 # minio
 DEFAULT_BUCKET_NAME = "a-bucket"
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/6e71393ed5f29234071da2b43192fc12d9bdbbd7/reflex/vars.py#L506'>reflex/vars.py~L506</a>
```diff
             else:
                 # apply operator to operands (left operand <operator> right_operand)
                 operation_name = (
-                    f"{left_operand._var_full_name} {op} {right_operand._var_full_name}"
-                )  # type: ignore
+                    f"{left_operand._var_full_name} {op} {right_operand._var_full_name}"  # type: ignore
+                )
                 operation_name = format.wrap(operation_name, "(")
         else:
             # apply operator to left operand (<operator> left_operand)
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+294 -304 lines across 55 files)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/accounting/export/csv.py#L158'>rotkehlchen/accounting/export/csv.py~L158</a>
```diff
 
         index = event.index + CSV_INDEX_OFFSET
         value_formula = f"{amount_column}{index}*H{index}"
-        total_value_formula = (
-            f"(F{index}*H{index}+G{index}*H{index})"
-        )  # formula of both free and taxable  # noqa: E501
+        total_value_formula = f"(F{index}*H{index}+G{index}*H{index})"  # formula of both free and taxable  # noqa: E501
         cost_basis_column = "K" if name == "taxable" else "L"
         cost_basis = f"{cost_basis_column}{index}"
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/accounting/structures/eth2.py#L53'>rotkehlchen/accounting/structures/eth2.py~L53</a>
```diff
 ]
 
 STAKING_DB_INSERT_QUERY_STR = (
-    "eth_staking_events_info(identifier, validator_index, is_exit_or_blocknumber) VALUES (?, ?, ?)"
-)  # noqa: E501
+    "eth_staking_events_info(identifier, validator_index, is_exit_or_blocknumber) VALUES (?, ?, ?)"  # noqa: E501
+)
 STAKING_DB_UPDATE_QUERY_STR = (
-    "UPDATE eth_staking_events_info SET validator_index=?, is_exit_or_blocknumber=?"
-)  # noqa: E501
+    "UPDATE eth_staking_events_info SET validator_index=?, is_exit_or_blocknumber=?"  # noqa: E501
+)
 
 
 class EthStakingEvent(HistoryBaseEntry, metaclass=ABCMeta):  # noqa: PLW1641  # hash in superclass
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py#L38'>rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py~L38</a>
```diff
 L2_ERC20_GATEWAY = string_to_evm_address("0x09e9222E96E7B4AE2a407B98d48e330053351EEe")
 L2_GATEWAY_ROUTER = string_to_evm_address("0x5288c571Fd7aD117beA99bF60FE0846C4E84F933")
 TRANSFER_ROUTED = (
-    b"\x85)\x1d\xff!a\xa9</\x12\xc8\x19\xd3\x18\x89\xc9lc\x04!\x16\xf5\xbcZ Z\xa7\x01\xc2\xc4)\xf5"
-)  # noqa: E501
+    b"\x85)\x1d\xff!a\xa9</\x12\xc8\x19\xd3\x18\x89\xc9lc\x04!\x16\xf5\xbcZ Z\xa7\x01\xc2\xc4)\xf5"  # noqa: E501
+)
 TOKEN_WITHDRAWAL_INITIATED = (
-    b"0s\xa7N\xcbr\x8d\x10\xbew\x9f\xe1\x9at\xa1B\x8e F\x8f[M\x16{\xf9\xc7=\x90g\x84}s"
-)  # noqa: E501
+    b"0s\xa7N\xcbr\x8d\x10\xbew\x9f\xe1\x9at\xa1B\x8e F\x8f[M\x16{\xf9\xc7=\x90g\x84}s"  # noqa: E501
+)
 ETH_WITHDRAWAL_INITIATED = (
-    b">z\xaf\xa7}\xbf\x18k\x7f\xd4\x88\x00k\xef\xf8\x93tL\xaa<Oo)\x9e\x8ap\x9f\xa2\x08st\xfc"
-)  # noqa: E501
+    b">z\xaf\xa7}\xbf\x18k\x7f\xd4\x88\x00k\xef\xf8\x93tL\xaa<Oo)\x9e\x8ap\x9f\xa2\x08st\xfc"  # noqa: E501
+)
 DEPOSIT_TX_TYPE = 100  # A deposit of ETH from L1 to L2 via the Arbitrum bridge.
 ERC20_DEPOSIT_FINALIZED = b'\xc7\xf2\xe9\xc5\\@\xa5\x0f\xbc!}\xfcp\xcd9\xa2"\x94\r\xfab\x14Z\xa0\xcaI\xeb\x955\xd4\xfc\xb2'  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/decoding/constants.py#L2'>rotkehlchen/chain/ethereum/decoding/constants.py~L2</a>
```diff
 from rotkehlchen.chain.evm.types import string_to_evm_address
 
 GTC_CLAIM = (
-    b"\x04g R\xdc\xb6\xb5\xb1\x9a\x9c\xc2\xec\x1b\x8fD\x7f\x1f^G\xb5\xe2L\xfa^O\xfbd\rc\xca+\xe7"
-)  # noqa: E501
+    b"\x04g R\xdc\xb6\xb5\xb1\x9a\x9c\xc2\xec\x1b\x8fD\x7f\x1f^G\xb5\xe2L\xfa^O\xfbd\rc\xca+\xe7"  # noqa: E501
+)
 ONEINCH_CLAIM = (
-    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"
-)  # noqa: E501
+    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"  # noqa: E501
+)
 GNOSIS_CHAIN_BRIDGE_RECEIVE = b"\x9a\xfdG\x90~%\x02\x8c\xda\xca\x89\xd1\x93Q\x8c0+\xbb\x12\x86\x17\xd5\xa9\x92\xc5\xab\xd4X\x15Re\x93"  # noqa: E501
 GOVERNORALPHA_PROPOSE = (
-    b"}\x84\xa6&:\xe0\xd9\x8d3)\xbd{F\xbbN\x8do\x98\xcd5\xa7\xad\xb4\\'L\x8b\x7f\xd5\xeb\xd5\xe0"
-)  # noqa: E501
+    b"}\x84\xa6&:\xe0\xd9\x8d3)\xbd{F\xbbN\x8do\x98\xcd5\xa7\xad\xb4\\'L\x8b\x7f\xd5\xeb\xd5\xe0"  # noqa: E501
+)
 GOVERNORALPHA_PROPOSE_ABI = '{"anonymous":false,"inputs":[{"indexed":false,"internalType":"uint256","name":"id","type":"uint256"},{"indexed":false,"internalType":"address","name":"proposer","type":"address"},{"indexed":false,"internalType":"address[]","name":"targets","type":"address[]"},{"indexed":false,"internalType":"uint256[]","name":"values","type":"uint256[]"},{"indexed":false,"internalType":"string[]","name":"signatures","type":"string[]"},{"indexed":false,"internalType":"bytes[]","name":"calldatas","type":"bytes[]"},{"indexed":false,"internalType":"uint256","name":"startBlock","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"endBlock","type":"uint256"},{"indexed":false,"internalType":"string","name":"description","type":"string"}],"name":"ProposalCreated","type":"event"}'  # noqa: E501
 
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L21'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L21</a>
```diff
 DEPOSIT = b"\xc1,W\xb1\xc7:,:.\xa4a>\x94v\xab\xb3\xd8\xd1F\x85z\xabs)\xe2BC\xfbYq\x0c\x82"
 REDEEM_UNDERLYING = b"\x9cN\xd5\x99\xcd\x85U\xb9\xc1\xe8\xcdvC$\r}q\xebv\xb7\x92\x94\x8cI\xfc\xb4\xd4\x11\xf7\xb6\xb3\xc6"  # noqa: E501
 LIQUIDATION_CALL = (
-    b"V\x86GW\xfd[\x1f\xc9\xf3\x8f_:\x98\x1c\xd8\xaeQ,\xe4\x1b\x90,\xf7?\xc5\x06\xee6\x9ck\xc27"
-)  # noqa: E501
+    b"V\x86GW\xfd[\x1f\xc9\xf3\x8f_:\x98\x1c\xd8\xaeQ,\xe4\x1b\x90,\xf7?\xc5\x06\xee6\x9ck\xc27"  # noqa: E501
+)
 
 
 class Aavev1Decoder(DecoderInterface):
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L70'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L70</a>
```diff
                 event.event_subtype = HistoryEventSubType.RECEIVE_WRAPPED
                 event.counterparty = CPT_AAVE_V1
                 event.notes = (
-                    f"Receive {amount} {atoken.symbol} from aave-v1 for {event.location_label}"
-                )  # noqa: E501
+                    f"Receive {amount} {atoken.symbol} from aave-v1 for {event.location_label}"  # noqa: E501
+                )
                 receive_event = event
             elif (
                 event.event_type == HistoryEventType.RECEIVE
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L82'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L82</a>
```diff
                 event.event_subtype = HistoryEventSubType.REWARD
                 event.counterparty = CPT_AAVE_V1
                 event.notes = (
-                    f"Gain {event.balance.amount} {atoken.symbol} from aave-v1 as interest"
-                )  # noqa: E501
+                    f"Gain {event.balance.amount} {atoken.symbol} from aave-v1 as interest"  # noqa: E501
+                )
 
         maybe_reshuffle_events(
             ordered_events=[deposit_event, receive_event],
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L135'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L135</a>
```diff
                 event.event_subtype = HistoryEventSubType.REWARD
                 event.counterparty = CPT_AAVE_V1
                 event.notes = (
-                    f"Gain {event.balance.amount} {atoken.symbol} from aave-v1 as interest"
-                )  # noqa: E501
+                    f"Gain {event.balance.amount} {atoken.symbol} from aave-v1 as interest"  # noqa: E501
+                )
                 interest_event = event
 
         maybe_reshuffle_events(
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L165'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L165</a>
```diff
                 # we are transfering the debt token
                 event.event_subtype = HistoryEventSubType.PAYBACK_DEBT
                 event.notes = (
-                    f"Payback {event.balance.amount} {asset.symbol} for an aave-v1 position"
-                )  # noqa: E501
+                    f"Payback {event.balance.amount} {asset.symbol} for an aave-v1 position"  # noqa: E501
+                )
                 event.counterparty = CPT_AAVE_V1
                 event.address = context.tx_log.address
                 event.extra_data = {
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py#L32'>rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py~L32</a>
```diff
 
 ENABLE_COLLATERAL = b'\x00\x05\x8aV\xea\x94e<\xdfO\x15-"z\xce"\xd4\xc0\n\xd9\x9e*C\xf5\x8c\xb7\xd9\xe3\xfe\xb2\x95\xf2'  # noqa: E501
 DISABLE_COLLATERAL = (
-    b'D\xc5\x8d\x816[f\xddK\x1a\x7f6\xc2Z\xa9{\x8cq\xc3a\xeeI7\xad\xc1\xa0\x00\x00"}\xb5\xdd'
-)  # noqa: E501
+    b'D\xc5\x8d\x816[f\xddK\x1a\x7f6\xc2Z\xa9{\x8cq\xc3a\xeeI7\xad\xc1\xa0\x00\x00"}\xb5\xdd'  # noqa: E501
+)
 DEPOSIT = b"\xdehW!\x95D\xbb[wF\xf4\x8e\xd3\x0b\xe68o\xef\xc6\x1b/\x86L\xac\xf5Y\x89;\xf5\x0f\xd9Q"
 WITHDRAW = (
-    b"1\x15\xd1D\x9a{s,\x98l\xba\x18$N\x89zE\x0fa\xe1\xbb\x8dX\x9c\xd2\xe6\x9el\x89$\xf9\xf7"
-)  # noqa: E501
+    b"1\x15\xd1D\x9a{s,\x98l\xba\x18$N\x89zE\x0fa\xe1\xbb\x8dX\x9c\xd2\xe6\x9el\x89$\xf9\xf7"  # noqa: E501
+)
 BORROW = b"\xc6\xa8\x980\x9e\x82>\xe5\x0b\xacd\xe4\\\xa8\xad\xbaf\x90\xe9\x9exA\xc4]uN*8\xe9\x01\x9d\x9b"  # noqa: E501
 REPAY = b"L\xdd\xe6\xe0\x9b\xb7U\xc9\xa5X\x9e\xba\xecd\x0b\xbf\xed\xff\x13b\xd4\xb2U\xeb\xf83\x97\x82\xb9\x94/\xaa"  # noqa: E501
 LIQUIDATION_CALL = (
-    b"\xe4\x13\xa3!\xe8h\x1d\x83\x1fM\xbc\xcb\xcay\r)R\xb5o\x97y\x08\xe4[\xe3s5S>\x00R\x86"
-)  # noqa: E501
+    b"\xe4\x13\xa3!\xe8h\x1d\x83\x1fM\xbc\xcb\xcay\r)R\xb5o\x97y\x08\xe4[\xe3s5S>\x00R\x86"  # noqa: E501
+)
 ETH_GATEWAYS = (
     string_to_evm_address("0xEFFC18fC3b7eb8E676dac549E0c693ad50D1Ce31"),
     string_to_evm_address("0xcc9a0B7c43DC2a5F023Bb9b738E45B0Ef6B06E04"),
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py#L117'>rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py~L117</a>
```diff
                     event.event_subtype = HistoryEventSubType.RECEIVE_WRAPPED
                     resolved_asset = event.asset.resolve_to_asset_with_symbol()
                     event.notes = (
-                        f"Receive {event.balance.amount} {resolved_asset.symbol} from AAVE v2"
-                    )  # noqa: E501
+                        f"Receive {event.balance.amount} {resolved_asset.symbol} from AAVE v2"  # noqa: E501
+                    )
                     event.counterparty = CPT_AAVE_V2
                 # Set protocol for both events
                 event.counterparty = CPT_AAVE_V2
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py#L232'>rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py~L232</a>
```diff
                 # we are transfering the aTOKEN
                 event.event_subtype = HistoryEventSubType.LIQUIDATE
                 event.notes = (
-                    f"An aave-v2 position got liquidated for {event.balance.amount} {asset.symbol}"
-                )  # noqa: E501
+                    f"An aave-v2 position got liquidated for {event.balance.amount} {asset.symbol}"  # noqa: E501
+                )
                 event.counterparty = CPT_AAVE_V2
                 event.address = tx_log.address
             elif (
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py#L246'>rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py~L246</a>
```diff
                 # we are transfering the debt token
                 event.event_subtype = HistoryEventSubType.PAYBACK_DEBT
                 event.notes = (
-                    f"Payback {event.balance.amount} {asset.symbol} for an aave-v2 position"
-                )  # noqa: E501
+                    f"Payback {event.balance.amount} {asset.symbol} for an aave-v2 position"  # noqa: E501
+                )
                 event.counterparty = CPT_AAVE_V2
                 event.address = tx_log.address
                 event.extra_data = {
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/airdrops/decoder.py#L49'>rotkehlchen/chain/ethereum/modules/airdrops/decoder.py~L49</a>
```diff
 
 UNISWAP_DISTRIBUTOR = string_to_evm_address("0x090D4613473dEE047c3f2706764f49E0821D256e")
 UNISWAP_TOKEN_CLAIMED = (
-    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"
-)  # noqa: E501
+    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"  # noqa: E501
+)
 
 BADGERHUNT = string_to_evm_address("0x394DCfbCf25C5400fcC147EbD9970eD34A474543")
 BADGER_HUNT_EVENT = (
-    b"\x8e\xaf\x15aI\x08\xa4\xe9\x02!A\xfeJYk\x1a\xb0\xcbr\xab2\xb2P#\xe3\xda*E\x9c\x9a3\\"
-)  # noqa: E501
+    b"\x8e\xaf\x15aI\x08\xa4\xe9\x02!A\xfeJYk\x1a\xb0\xcbr\xab2\xb2P#\xe3\xda*E\x9c\x9a3\\"  # noqa: E501
+)
 
 ONEINCH = string_to_evm_address("0xE295aD71242373C37C5FdA7B57F26f9eA1088AFe")
 ONEINCH_CLAIMED = (
-    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"
-)  # noqa: E501
+    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"  # noqa: E501
+)
 
 FPIS = string_to_evm_address("0x61A1f84F12Ba9a56C22c31dDB10EC2e2CA0ceBCf")
 CONVEX = string_to_evm_address("0x2E088A0A19dda628B4304301d1EA70b114e4AcCd")
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/airdrops/decoder.py#L68'>rotkehlchen/chain/ethereum/modules/airdrops/decoder.py~L68</a>
```diff
 
 FOX_DISTRIBUTOR = string_to_evm_address("0xe099e688D12DBc19ab46D128d1Db297575474a0d")
 FOX_CLAIMED = (
-    b"R\x897\xb30\x08-\x89*\x98\xd4\xe4(\xab-\xcc\xa7\x84KQ\xd2'\xa1\xc0\xaeg\xf0\xb5&\x1a\xcb\xd9"
-)  # noqa: E501
+    b"R\x897\xb30\x08-\x89*\x98\xd4\xe4(\xab-\xcc\xa7\x84KQ\xd2'\xa1\xc0\xaeg\xf0\xb5&\x1a\xcb\xd9"  # noqa: E501
+)
 
 ELFI_LOCKING = string_to_evm_address("0x02Bd4A3b1b95b01F2Aa61655415A5d3EAAcaafdD")
 ELFI_VOTE_CHANGE = (
-    b"3\x16\x1c\xf2\xda(\xd7G\xbe\x9d\xf16\xb6\xf3r\x93\x90)\x84\x94\x94rht1\x93\xc5=s\xd3\xc2\xe0"
-)  # noqa: E501
+    b"3\x16\x1c\xf2\xda(\xd7G\xbe\x9d\xf16\xb6\xf3r\x93\x90)\x84\x94\x94rht1\x93\xc5=s\xd3\xc2\xe0"  # noqa: E501
+)
 
 
 logger = logging.getLogger(__name__)
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/balancer/v2/decoder.py#L144'>rotkehlchen/chain/ethereum/modules/balancer/v2/decoder.py~L144</a>
```diff
         if asset == context.event.asset:
             context.event.event_subtype = HistoryEventSubType.RECEIVE
             context.event.notes = (
-                f"Receive {context.event.balance.amount} {asset.symbol} from Balancer v2"
-            )  # noqa: E501
+                f"Receive {context.event.balance.amount} {asset.symbol} from Balancer v2"  # noqa: E501
+            )
         else:
             context.event.event_subtype = HistoryEventSubType.SPEND
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/base_bridge/decoder.py#L26'>rotkehlchen/chain/ethereum/modules/base_bridge/decoder.py~L26</a>
```diff
 BRIDGE_ADDRESS = string_to_evm_address("0x49048044D57e1C92A77f79988d21Fa8fAF74E97e")
 TOKEN_PORTAL = string_to_evm_address("0x3154Cf16ccdb4C6d922629664174b904d80F2C35")
 TRANSACTION_DEPOSITED = (
-    b"\xb3\x815h\xd9\x99\x1f\xc9Q\x96\x1f\xcbLxH\x93WB@\xa2\x89%`M\t\xfcW|U\xbb|2"
-)  # noqa: E501
+    b"\xb3\x815h\xd9\x99\x1f\xc9Q\x96\x1f\xcbLxH\x93WB@\xa2\x89%`M\t\xfcW|U\xbb|2"  # noqa: E501
+)
 WITHDRAWAL_FINALIZED = b"\xdb\\vR\x85z\xa1c\xda\xad\xd6p\xe1\x16b\x8f\xb4.\x86\x9d\x8a\xc4%\x1e\xf8\x97\x1d\x9eW'\xdf\x1b"  # noqa: E501
 BRIDGE_TOKEN = b"\x7f\xf1&\xdb\x80$BK\xbf\xd9\x82n\x8a\xb8/\xf5\x916(\x9e\xa4@\xb0K9\xa0\xdf\x1b\x03\xb9\xca\xbf"  # noqa: E501
 WITHDRAWAL_TOKEN = b'\xd5\x9ce\xb3TE"X5\xc8?P\xb6\xed\xe0j{\xe0G\xd2.5ps\xe2P\xd9\xafSu\x18\xcd'
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/compound/decoder.py#L38'>rotkehlchen/chain/ethereum/modules/compound/decoder.py~L38</a>
```diff
 
 MAXIMILLION_ADDR = string_to_evm_address("0xf859A1AD94BcF445A406B892eF0d3082f4174088")
 MINT_COMPOUND_TOKEN = (
-    b"L \x9b_\xc8\xadPu\x8f\x13\xe2\xe1\x08\x8b\xa5jV\r\xffi\n\x1co\xef&9OL\x03\x82\x1cO"
-)  # noqa: E501
+    b"L \x9b_\xc8\xadPu\x8f\x13\xe2\xe1\x08\x8b\xa5jV\r\xffi\n\x1co\xef&9OL\x03\x82\x1cO"  # noqa: E501
+)
 REDEEM_COMPOUND_TOKEN = b"\xe5\xb7T\xfb\x1a\xbb\x7f\x01\xb4\x99y\x1d\x0b\x82\n\xe3\xb6\xaf4$\xac\x1cYv\x8e\xdbS\xf4\xec1\xa9)"  # noqa: E501
 BORROW_COMPOUND = b"\x13\xedhf\xd4\xe1\xeem\xa4o\x84\\F\xd7\xe5A \x88=u\xc5\xea\x9a-\xac\xc1\xc4\xca\x89\x84\xab\x80"  # noqa: E501
 REPAY_COMPOUND = (
-    b'\x1a*"\xcb\x03M&\xd1\x85K\xdcff\xa5\xb9\x1f\xe2^\xfb\xbb]\xca\xd3\xb05Tx\xd6\xf5\xc3b\xa1'
-)  # noqa: E501
+    b'\x1a*"\xcb\x03M&\xd1\x85K\xdcff\xa5\xb9\x1f\xe2^\xfb\xbb]\xca\xd3\xb05Tx\xd6\xf5\xc3b\xa1'  # noqa: E501
+)
 LIQUIDATE_BORROW = (
-    b")\x867\xf6\x84\xdapgO&P\x9b\x10\xf0~\xc2\xfb\xc7z3Z\xb1\xe7\xd6!ZK$\x84\xd8\xbbR"
-)  # noqa: E501
+    b")\x867\xf6\x84\xdapgO&P\x9b\x10\xf0~\xc2\xfb\xc7z3Z\xb1\xe7\xd6!ZK$\x84\xd8\xbbR"  # noqa: E501
+)
 DISTRIBUTED_SUPPLIER_COMP = (
-    b",\xae\xcd\x17\xd0/V\xfa\x89w\x05\xdc\xc7@\xda-#|7?phoN\r\x9b\xd3\xbf\x04\x00\xeaz"
-)  # noqa: E501
+    b",\xae\xcd\x17\xd0/V\xfa\x89w\x05\xdc\xc7@\xda-#|7?phoN\r\x9b\xd3\xbf\x04\x00\xeaz"  # noqa: E501
+)
 DISTRIBUTED_BORROWER_COMP = b"\x1f\xc3\xec\xc0\x87\xd8\xd2\xd1^#\xd0\x03*\xf5\xa4pY\xc3\x89-\x00=\x8e\x13\x9f\xdc\xb6\xbb2|\x99\xa6"  # noqa: E501
 
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/compound/decoder.py#L309'>rotkehlchen/chain/ethereum/modules/compound/decoder.py~L309</a>
```diff
                 and event.counterparty == CPT_COMPOUND
             ):
                 event.notes = (
-                    f"Repay {repaid_amount} {repaying_asset.symbol} in a compound liquidation"
-                )  # noqa: E501
+                    f"Repay {repaid_amount} {repaying_asset.symbol} in a compound liquidation"  # noqa: E501
+                )
                 event.extra_data = {"in_liquidation": True}
             elif (
                 event.event_type == HistoryEventType.RECEIVE
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/convex/constants.py#L17'>rotkehlchen/chain/ethereum/modules/convex/constants.py~L17</a>
```diff
 CVXCRV_REWARDS = string_to_evm_address("0x3Fe65692bfCD0e6CF84cB1E7d24108E434A7587e")
 
 CVX_WITHDRAW_TOPIC = (
-    b"p\x84\xf5Gf\x18\xd8\xe6\x0b\x11\xef\r}?\x06\x91FU\xad\xb8y>(\xff\x7f\x01\x8dLv\xd5\x05\xd5"
-)  # noqa: E501
+    b"p\x84\xf5Gf\x18\xd8\xe6\x0b\x11\xef\r}?\x06\x91FU\xad\xb8y>(\xff\x7f\x01\x8dLv\xd5\x05\xd5"  # noqa: E501
+)
 CVXCRV_REWARD_PAID_TOPIC = b"\xe2@6@\xbah\xfe\xd3\xa2\xf8\x8buWU\x1d\x19\x93\xf8K\x99\xbb\x10\xff\x83?\x0c\xf8\xdb\x0c^\x04\x86"  # noqa: E501
 CVXCRV_WITHDRAWAL_TOPIC = (
-    b"p\x84\xf5Gf\x18\xd8\xe6\x0b\x11\xef\r}?\x06\x91FU\xad\xb8y>(\xff\x7f\x01\x8dLv\xd5\x05\xd5"
-)  # noqa: E501
+    b"p\x84\xf5Gf\x18\xd8\xe6\x0b\x11\xef\r}?\x06\x91FU\xad\xb8y>(\xff\x7f\x01\x8dLv\xd5\x05\xd5"  # noqa: E501
+)
 CVX_LOCKER_REWARD_PAID_TOPIC = (
-    b"T\x07\x98\xdfF\x8d{#\xd1\x1f\x15o\xdb\x95L\xb1\x9a\xd4\x14\xd1Pr*{mU\xba6\x9d\xeay."
-)  # noqa: E501
+    b"T\x07\x98\xdfF\x8d{#\xd1\x1f\x15o\xdb\x95L\xb1\x9a\xd4\x14\xd1Pr*{mU\xba6\x9d\xeay."  # noqa: E501
+)
 BOOSTER_WITHDRAW_TOPIC = b"\x92\xcc\xf4P\xa2\x86\xa9W\xafRP\x9b\xc1\xc9\x93\x9d\x1ajH\x17\x83\xe1B\xe4\x1e$\x99\xf0\xbbf\xeb\xc6"  # noqa: E501
 
 WITHDRAWAL_TOPICS = {
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L50'>rotkehlchen/chain/ethereum/modules/convex/decoder.py~L50</a>
```diff
 log = RotkehlchenLogsAdapter(logger)
 
 DEPOSIT_EVENT = (
-    b"\x9eq\xbc\x8e\xea\x02\xa69i\xf5\t\x81\x8f-\xaf\xb9%E2\x90C\x19\xf9\xdb\xday\xb6{\xd3J_="
-)  # noqa: E501
+    b"\x9eq\xbc\x8e\xea\x02\xa69i\xf5\t\x81\x8f-\xaf\xb9%E2\x90C\x19\xf9\xdb\xday\xb6{\xd3J_="  # noqa: E501
+)
 REWARD_ADDED = (
-    b'\xde\x88\xa9"\xe0\xd3\xb8\x8b$\xe9b>\xfe\xb4d\x91\x9ck\xf9\xf6hW\xa6^+\xfc\xf2\xce\x87\xa9C='
-)  # noqa: E501
+    b'\xde\x88\xa9"\xe0\xd3\xb8\x8b$\xe9b>\xfe\xb4d\x91\x9ck\xf9\xf6hW\xa6^+\xfc\xf2\xce\x87\xa9C='  # noqa: E501
+)
 
 
 class ConvexDecoder(DecoderInterface, ReloadableCacheDecoderMixin):
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L144'>rotkehlchen/chain/ethereum/modules/convex/decoder.py~L144</a>
```diff
                         event.notes = f"Return {event.balance.amount} {crypto_asset.symbol} to convex {self.pools[context.tx_log.address]} pool"  # noqa: E501
                     else:
                         event.notes = (
-                            f"Return {event.balance.amount} {crypto_asset.symbol} to convex"
-                        )  # noqa: E501
+                            f"Return {event.balance.amount} {crypto_asset.symbol} to convex"  # noqa: E501
+                        )
                 else:
                     event.event_type = HistoryEventType.DEPOSIT
                     event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L155'>rotkehlchen/chain/ethereum/modules/convex/decoder.py~L155</a>
```diff
                         event.product = EvmProduct.GAUGE
                     else:
                         event.notes = (
-                            f"Deposit {event.balance.amount} {crypto_asset.symbol} into convex"
-                        )  # noqa: E501
+                            f"Deposit {event.balance.amount} {crypto_asset.symbol} into convex"  # noqa: E501
+                        )
                         if (
                             isinstance(crypto_asset, EvmToken)
                             and crypto_asset.protocol == CURVE_POOL_PROTOCOL
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L192'>rotkehlchen/chain/ethereum/modules/convex/decoder.py~L192</a>
```diff
                         event.product = EvmProduct.GAUGE
                     else:
                         event.notes = (
-                            f"Withdraw {event.balance.amount} {crypto_asset.symbol} from convex"
-                        )  # noqa: E501
+                            f"Withdraw {event.balance.amount} {crypto_asset.symbol} from convex"  # noqa: E501
+                        )
                         if (
                             isinstance(crypto_asset, EvmToken)
                             and crypto_asset.protocol == CURVE_POOL_PROTOCOL
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L102'>rotkehlchen/chain/ethereum/modules/curve/decoder.py~L102</a>
```diff
 
 GAUGE_DEPOSIT = b"\xe1\xff\xfc\xc4\x92=\x04\xb5Y\xf4\xd2\x9a\x8b\xfcl\xda\x04\xeb[\r<F\x07Q\xc2@,\\\\\xc9\x10\x9c"  # noqa: E501
 GAUGE_WITHDRAW = (
-    b"\x88N\xda\xd9\xceo\xa2D\r\x8aT\xcc\x124\x90\xeb\x96\xd2v\x84y\xd4\x9f\xf9\xc76a%\xa9BCd"
-)  # noqa: E501
+    b"\x88N\xda\xd9\xceo\xa2D\r\x8aT\xcc\x124\x90\xeb\x96\xd2v\x84y\xd4\x9f\xf9\xc76a%\xa9BCd"  # noqa: E501
+)
 GAUGE_VOTE = (
-    b"E\xca\x9aL\x8d\x01\x19\xeb2\x9eX\r(\xfeh\x9eHN\x1b\xe20\xda\x807\xad\xe9T}-%\xcc\x91"
-)  # noqa: E501
+    b"E\xca\x9aL\x8d\x01\x19\xeb2\x9eX\r(\xfeh\x9eHN\x1b\xe20\xda\x807\xad\xe9T}-%\xcc\x91"  # noqa: E501
+)
 GAUGE_CONTROLLER = string_to_evm_address("0x2F50D538606Fa9EDD2B11E2446BEb18C9D5846bB")
 
 TOKEN_EXCHANGE = (
-    b"\x8b>\x96\xf2\xb8\x89\xfaw\x1cS\xc9\x81\xb4\r\xaf\x00_c\xf67\xf1\x86\x9fppR\xd1Z=\xd9q@"
-)  # noqa: E501
+    b"\x8b>\x96\xf2\xb8\x89\xfaw\x1cS\xc9\x81\xb4\r\xaf\x00_c\xf67\xf1\x86\x9fppR\xd1Z=\xd9q@"  # noqa: E501
+)
 TOKEN_EXCHANGE_UNDERLYING = (
-    b"\xd0\x13\xca#\xe7ze\x00<,e\x9cTB\xc0\x0c\x80Sq\xb7\xfc\x1e\xbdL lA\xd1Sk\xd9\x0b"
-)  # noqa: E501
+    b"\xd0\x13\xca#\xe7ze\x00<,e\x9cTB\xc0\x0c\x80Sq\xb7\xfc\x1e\xbdL lA\xd1Sk\xd9\x0b"  # noqa: E501
+)
 EXCHANGE_MULTIPLE = b"\x14\xb5a\x17\x8a\xe0\xf3h\xf4\x0f\xaf\xd0H\\Oq)\xeaq\xcd\xc0\x0bL\xe1\xe5\x94\x0f\x9b\xc6Y\xc8\xb2"  # noqa: E501
 CURVE_SWAP_ROUTER = string_to_evm_address("0x99a58482BD75cbab83b27EC03CA68fF489b5788f")
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L206'>rotkehlchen/chain/ethereum/modules/curve/decoder.py~L206</a>
```diff
                 event.event_subtype = HistoryEventSubType.REMOVE_ASSET
                 event.counterparty = CPT_CURVE
                 event.notes = (
-                    f"Remove {event.balance.amount} {crypto_asset.symbol} from the curve pool"
-                )  # noqa: E501
+                    f"Remove {event.balance.amount} {crypto_asset.symbol} from the curve pool"  # noqa: E501
+                )
                 withdrawal_events.append(event)
             elif (  # Withdraw send wrapped
                 event.event_type == HistoryEventType.SPEND
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/diva/decoder.py#L27'>rotkehlchen/chain/ethereum/modules/diva/decoder.py~L27</a>
```diff
 logger = logging.getLogger(__name__)
 log = RotkehlchenLogsAdapter(logger)
 DELEGATE_CHANGED = (
-    b"14\xe8\xa2\xe6\xd9~\x92\x9a~T\x01\x1e\xa5H]}\x19m\xd5\xf0\xbaMN\xf9X\x03\xe8\xe3\xfc%\x7f"
-)  # noqa: E501
+    b"14\xe8\xa2\xe6\xd9~\x92\x9a~T\x01\x1e\xa5H]}\x19m\xd5\xf0\xbaMN\xf9X\x03\xe8\xe3\xfc%\x7f"  # noqa: E501
+)
 CLAIM_AIRDROP = (
-    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"
-)  # noqa: E501
+    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"  # noqa: E501
+)
 DIVA_AIDROP_CONTRACT = string_to_evm_address("0x777E2B2Cc7980A6bAC92910B95269895EEf0d2E8")
 DIVA_GOVERNOR = string_to_evm_address("0xFb6B7C11a55C57767643F1FF65c34C8693a11A70")
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/dxdaomesa/decoder.py#L25'>rotkehlchen/chain/ethereum/modules/dxdaomesa/decoder.py~L25</a>
```diff
 DEPOSIT = b"\xc1\x1c\xc3N\x93\xc6z\x938+\x99\xf2I\x8e\x997\x19\x87\x98\xf3\xc1\xc2\x88\x80\x08\xff\xc0\xee\xb8/h\xc4"  # noqa: E501
 ORDER_PLACEMENT = b"\xde\xcfo\xde\x82C\x98\x12\x99\xf7\xb7\xa7v\xf2\x9a\x9f\xc6z,\x98H\xe2]w\xc5\x0e\xb1\x1f\xa5\x8a~!"  # noqa: E501
 WITHDRAW_REQUEST = (
-    b",bE\xafPo\x0f\xc1\x08\x99\x18\xc0,\x1d\x01\xbd\xe9\xcc\x80v\t\xb34\xb3\xe7dMm\xfbZl^"
-)  # noqa: E501
+    b",bE\xafPo\x0f\xc1\x08\x99\x18\xc0,\x1d\x01\xbd\xe9\xcc\x80v\t\xb34\xb3\xe7dMm\xfbZl^"  # noqa: E501
+)
 WITHDRAW = b"\x9b\x1b\xfa\x7f\xa9\xeeB\n\x16\xe1$\xf7\x94\xc3Z\xc9\xf9\x04r\xac\xc9\x91@\xeb/dG\xc7\x14\xca\xd8\xeb"  # noqa: E501
 
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/ens/decoder.py#L55'>rotkehlchen/chain/ethereum/modules/ens/decoder.py~L55</a>
```diff
 ENS_REVERSE_RESOLVER = string_to_evm_address("0xA2C122BE93b0074270ebeE7f6b7292C7deB45047")
 
 NAME_RENEWED = (
-    b"=\xa2L\x02E\x82\x93\x1c\xfa\xf8&}\x8e\xd2M\x13\xa8*\x80h\xd5\xbd3}0\xecE\xce\xa4\xe5\x06\xae"
-)  # noqa: E501
+    b"=\xa2L\x02E\x82\x93\x1c\xfa\xf8&}\x8e\xd2M\x13\xa8*\x80h\xd5\xbd3}0\xecE\xce\xa4\xe5\x06\xae"  # noqa: E501
+)
 NAME_RENEWED_ABI = '{"anonymous":false,"inputs":[{"indexed":false,"internalType":"string","name":"name","type":"string"},{"indexed":true,"internalType":"bytes32","name":"label","type":"bytes32"},{"indexed":false,"internalType":"uint256","name":"cost","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"expires","type":"uint256"}],"name":"NameRenewed","type":"event"}'  # noqa: E501
 NEW_RESOLVER = (
-    b"3W!\xb0\x18f\xdc#\xfb\xee\x8bk,{\x1e\x14\xd6\xf0\\(\xcd5\xa2\xc94#\x9f\x94\tV\x02\xa0"
-)  # noqa: E501
+    b"3W!\xb0\x18f\xdc#\xfb\xee\x8bk,{\x1e\x14\xd6\xf0\\(\xcd5\xa2\xc94#\x9f\x94\tV\x02\xa0"  # noqa: E501
+)
 NAME_REGISTERED_SINGLE_COST = b'\xcaj\xbb\xe9\xd7\xf1\x14"\xcbl\xa7b\x9f\xbfo\xe9\xef\xb1\xc6!\xf7\x1c\xe8\xf0+\x9f*#\x00\x97@O'  # noqa: E501
 NAME_REGISTERED_SINGLE_COST_ABI = '{"anonymous":false,"inputs":[{"indexed":false,"internalType":"string","name":"name","type":"string"},{"indexed":true,"internalType":"bytes32","name":"label","type":"bytes32"},{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":false,"internalType":"uint256","name":"cost","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"expires","type":"uint256"}],"name":"NameRegistered","type":"event"}'  # noqa: E501
 NAME_REGISTERED_BASE_COST_AND_PREMIUM = b"i\xe3\x7f\x15\x1e\xb9\x8a\ta\x8d\xda\xa8\x0c\x8c\xfa\xf1\xceY\x96\x86|H\x9fE\xb5U\xb4\x12'\x1e\xbf'"  # noqa: E501
 NAME_REGISTERED_BASE_COST_AND_PREMIUM_ABI = '{"anonymous":false,"inputs":[{"indexed":false,"internalType":"string","name":"name","type":"string"},{"indexed":true,"internalType":"bytes32","name":"label","type":"bytes32"},{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":false,"internalType":"uint256","name":"baseCost","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"premium","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"expires","type":"uint256"}],"name":"NameRegistered","type":"event"}'  # noqa: E501
 TEXT_CHANGED_KEY_ONLY = (
-    b"\xd8\xc93K\x1a\x9c/\x9d\xa3B\xa0\xa2\xb3&)\xc1\xa2)\xb6D]\xadx\x94\x7fgKDDJuP"
-)  # noqa: E501
+    b"\xd8\xc93K\x1a\x9c/\x9d\xa3B\xa0\xa2\xb3&)\xc1\xa2)\xb6D]\xadx\x94\x7fgKDDJuP"  # noqa: E501
+)
 TEXT_CHANGED_KEY_ONLY_ABI = '{"anonymous":false,"inputs":[{"indexed":true,"internalType":"bytes32","name":"node","type":"bytes32"},{"indexed":true,"internalType":"string","name":"indexedKey","type":"string"},{"indexed":false,"internalType":"string","name":"key","type":"string"}],"name":"TextChanged","type":"event"}'  # noqa: E501
 TEXT_CHANGED_KEY_AND_VALUE = (
-    b"D\x8b\xc0\x14\xf1Sg&\xcf\x8dT\xff=d\x81\xed<\xbch<%\x91\xca Bt\x00\x9a\xfa\t\xb1\xa1"
-)  # noqa: E501
+    b"D\x8b\xc0\x14\xf1Sg&\xcf\x8dT\xff=d\x81\xed<\xbch<%\x91\xca Bt\x00\x9a\xfa\t\xb1\xa1"  # noqa: E501
+)
 TEXT_CHANGED_KEY_AND_VALUE_ABI = '{"anonymous":false,"inputs":[{"indexed":true,"internalType":"bytes32","name":"node","type":"bytes32"},{"indexed":true,"internalType":"string","name":"indexedKey","type":"string"},{"indexed":false,"internalType":"string","name":"key","type":"string"},{"indexed":false,"internalType":"string","name":"value","type":"string"}],"name":"TextChanged","type":"event"}'  # noqa: E501
 CONTENT_HASH_CHANGED = (
-    b"\xe3y\xc1bN\xd7\xe7\x14\xcc\t7R\x8a25\x9di\xd5(\x137vS\x13\xdb\xa4\xe0\x81\xb7-ux"
-)  # noqa: E501
+    b"\xe3y\xc1bN\xd7\xe7\x14\xcc\t7R\x8a25\x9di\xd5(\x137vS\x13\xdb\xa4\xe0\x81\xb7-ux"  # noqa: E501
+)
 ENS_GOVERNOR = string_to_evm_address("0x323A76393544d5ecca80cd6ef2A560C6a395b7E3")
 
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/golem/decoder.py#L24'>rotkehlchen/chain/ethereum/modules/golem/decoder.py~L24</a>
```diff
 log = RotkehlchenLogsAdapter(logger)
 
 MIGRATED = (
-    b"\x92\x8f\xd5S\x13$\xee\x87\xd7l\xc50}\xc3u\x80\x17M\xa7k\x85\xcdTm\xa61\xb2g\x0b\xc2f\xb5"
-)  # noqa: E501
+    b"\x92\x8f\xd5S\x13$\xee\x87\xd7l\xc50}\xc3u\x80\x17M\xa7k\x85\xcdTm\xa61\xb2g\x0b\xc2f\xb5"  # noqa: E501
+)
 
 
 class GolemDecoder(DecoderInterface):
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/kyber/decoder.py#L27'>rotkehlchen/chain/ethereum/modules/kyber/decoder.py~L27</a>
```diff
 
 KYBER_TRADE_LEGACY = b"\xf7$\xb4\xdff\x17G6\x12\xb5=\x7f\x88\xec\xc6\xea\x980t\xb3\t`\xa0I\xfc\xd0e\x7f\xfe\x80\x80\x83"  # noqa: E501
 KYBER_TRADE = (
-    b"\xd3\x0c\xa3\x99\xcbCP~\xce\xc6\xa6)\xa3\\\xf4^\xb9\x8c\xdaU\x0c'im\xcb\r\x8cJ8s\xcel"
-)  # noqa: E501
+    b"\xd3\x0c\xa3\x99\xcbCP~\xce\xc6\xa6)\xa3\\\xf4^\xb9\x8c\xdaU\x0c'im\xcb\r\x8cJ8s\xcel"  # noqa: E501
+)
 KYBER_LEGACY_CONTRACT = string_to_evm_address("0x9ae49C0d7F8F9EF4B864e004FE86Ac8294E20950")
 KYBER_LEGACY_CONTRACT_MIGRATED = string_to_evm_address(
     "0x65bF64Ff5f51272f729BDcD7AcFB00677ced86Cd"
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/liquity/decoder.py#L34'>rotkehlchen/chain/ethereum/modules/liquity/decoder.py~L34</a>
```diff
 LIQUITY_STAKING = string_to_evm_address("0x4f9Fbb3f1E99B56e0Fe2892e623Ed36A76Fc605d")
 
 STABILITY_POOL_GAIN_WITHDRAW = (
-    b'QEr"\xeb\xca\x92\xc35\xc9\xc8n+\xaa\x1c\xc0\xe4\x0f\xfa\xa9\x08JQE)\x80\xd5\xba\x8d\xec/c'
-)  # noqa: E501
+    b'QEr"\xeb\xca\x92\xc35\xc9\xc8n+\xaa\x1c\xc0\xe4\x0f\xfa\xa9\x08JQE)\x80\xd5\xba\x8d\xec/c'  # noqa: E501
+)
 STABILITY_POOL_LQTY_PAID = (
-    b"&\x08\xb9\x86\xa6\xac\x0flb\x9c\xa3p\x18\xe8\n\xf5V\x1e6bR\xae\x93`*\x96\xd3\xab.s\xe4-"
-)  # noqa: E501
+    b"&\x08\xb9\x86\xa6\xac\x0flb\x9c\xa3p\x18\xe8\n\xf5V\x1e6bR\xae\x93`*\x96\xd3\xab.s\xe4-"  # noqa: E501
+)
 STABILITY_POOL_EVENTS = {STABILITY_POOL_GAIN_WITHDRAW, STABILITY_POOL_LQTY_PAID}
 STAKING_LQTY_CHANGE = (
-    b"9\xdf\x0eR\x86\xa3\xef/B\xa0\xbfR\xf3,\xfe,X\xe5\xb0@_G\xfeQ/,$9\xe4\xcf\xe2\x04"
-)  # noqa: E501
+    b"9\xdf\x0eR\x86\xa3\xef/B\xa0\xbfR\xf3,\xfe,X\xe5\xb0@_G\xfeQ/,$9\xe4\xcf\xe2\x04"  # noqa: E501
+)
 STAKING_ETH_SENT = (
-    b"a\t\xe2U\x9d\xfavj\xae\xc7\x11\x83Q\xd4\x8aR?\nAW\xf4\x9c\x8dht\x9c\x8a\xc4\x13\x18\xad\x12"
-)  # noqa: E501
+    b"a\t\xe2U\x9d\xfavj\xae\xc7\x11\x83Q\xd4\x8aR?\nAW\xf4\x9c\x8dht\x9c\x8a\xc4\x13\x18\xad\x12"  # noqa: E501
+)
 STAKING_LQTY_EVENTS = {STAKING_LQTY_CHANGE, STAKING_ETH_SENT}
 STAKING_REWARDS_ASSETS = {A_ETH, A_LUSD}
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/liquity/decoder.py#L221'>rotkehlchen/chain/ethereum/modules/liquity/decoder.py~L221</a>
```diff
                     event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                     event.counterparty = CPT_LIQUITY
                     event.notes = (
-                        f"Stake {event.balance.amount} {self.lqty.symbol} in the Liquity protocol"
-                    )  # noqa: E501
+                        f"Stake {event.balance.amount} {self.lqty.symbol} in the Liquity protocol"  # noqa: E501
+                    )
                     event.extra_data = extra_data
                 elif event.location_label == user and event.event_type == HistoryEventType.RECEIVE:
                     event.event_type = HistoryEventType.STAKING
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/decoder.py#L78'>rotkehlchen/chain/ethereum/modules/makerdao/decoder.py~L78</a>
```diff
 CDPMANAGER_FROB = b"E\xe6\xbd\xcd\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00"  # noqa: E501
 
 SDAI_DEPOSIT = (
-    b"\xdc\xbc\x1c\x05$\x0f1\xff:\xd0g\xef\x1e\xe3\\\xe4\x99wbu.:\tR\x84uED\xf4\xc7\t\xd7"
-)  # noqa: E501
+    b"\xdc\xbc\x1c\x05$\x0f1\xff:\xd0g\xef\x1e\xe3\\\xe4\x99wbu.:\tR\x84uED\xf4\xc7\t\xd7"  # noqa: E501
+)
 SDAI_REDEEM = b"\xfb\xdey} \x1ch\x1b\x91\x05e)\x11\x9e\x0b\x02@|{\xb9jJ,u\xc0\x1f\xc9fr2\xc8\xdb"
 
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/decoder.py#L192'>rotkehlchen/chain/ethereum/modules/makerdao/decoder.py~L192</a>
```diff
                     event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                     event.counterparty = CPT_VAULT
                     event.notes = (
-                        f"Deposit {amount} {vault_asset.symbol} to {vault_type} MakerDAO vault"
-                    )  # noqa: E501
+                        f"Deposit {amount} {vault_asset.symbol} to {vault_type} MakerDAO vault"  # noqa: E501
+                    )
                     event.extra_data = {"vault_type": vault_type}
                     return DEFAULT_DECODING_OUTPUT
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/decoder.py#L215'>rotkehlchen/chain/ethereum/modules/makerdao/decoder.py~L215</a>
```diff
                     event.event_subtype = HistoryEventSubType.REMOVE_ASSET
                     event.counterparty = CPT_VAULT
                     event.notes = (
-                        f"Withdraw {amount} {vault_asset.symbol} from {vault_type} MakerDAO vault"
-                    )  # noqa: E501
+                        f"Withdraw {amount} {vault_asset.symbol} from {vault_type} MakerDAO vault"  # noqa: E501
+                    )
                     event.extra_data = {"vault_type": vault_type}
                     return DEFAULT_DECODING_OUTPUT
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/decoder.py#L463'>rotkehlchen/chain/ethereum/modules/makerdao/decoder.py~L463</a>
```diff
                         event.extra_data["cdp_id"] = cdp_id
 
                     event.notes = (
-                        f"Payback {event.balance.amount} DAI of debt to makerdao vault {cdp_id}"
-                    )  # noqa: E501
+                        f"Payback {event.balance.amount} DAI of debt to makerdao vault {cdp_id}"  # noqa: E501
+                    )
                     break
 
         else:  # collateral deposit
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L35'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L35</a>
```diff
 POOLED_ETHER_ADDRESS = string_to_evm_address("0xf53AD2c6851052A81B42133467480961B2321C09")
 PETH_BURN_EVENT_TOPIC = b"\xcc\x16\xf5\xdb\xb4\x872\x80\x81\\\x1e\xe0\x9d\xbd\x06sl\xff\xcc\x18D\x12\xcfzq\xa0\xfd\xb7]9|\xa5"  # noqa: E501
 PETH_MINT_EVENT_TOPIC = (
-    b"\x0fg\x98\xa5`y:T\xc3\xbc\xfe\x86\xa9<\xde\x1es\x08}\x94L\x0e\xa2\x05D\x13}A!9h\x85"
-)  # noqa: E501
+    b"\x0fg\x98\xa5`y:T\xc3\xbc\xfe\x86\xa9<\xde\x1es\x08}\x94L\x0e\xa2\x05D\x13}A!9h\x85"  # noqa: E501
+)
 SAI_CDP_MIGRATION_TOPIC = b'\n\t\x94\xe6\x12<i3\xeeu\x98\x86\x90WW\xde\xfe"\xc1\xf5\xd0<\xd1\xee1\\\xb6\xbd\xe8\xd1\x1a\xe8'  # noqa: E501
 MAKERDAO_SAITUB_CONTRACT = string_to_evm_address("0x448a5065aeBB8E423F0896E6c5D525C040f59af3")
 MAKERDAO_SAITAP_CONTRACT = string_to_evm_address("0xBda109309f9FafA6Dd6A9CB9f1Df4085B27Ee8eF")
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L109'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L109</a>
```diff
                         event.event_subtype = HistoryEventSubType.REMOVE_ASSET
                         event.counterparty = CPT_SAI
                         event.notes = (
-                            f"Withdraw {event.balance.amount} {self.eth.symbol} from CDP {cdp_id}"
-                        )  # noqa: E501
+                            f"Withdraw {event.balance.amount} {self.eth.symbol} from CDP {cdp_id}"  # noqa: E501
+                        )
                         return DEFAULT_DECODING_OUTPUT
 
         return DEFAULT_DECODING_OUTPUT
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L384'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L384</a>
```diff
                         event.event_type = HistoryEventType.DEPOSIT
                         event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                         event.notes = (
-                            f"Supply {event.balance.amount} {self.eth.symbol} to Sai vault"
-                        )  # noqa: E501
+                            f"Supply {event.balance.amount} {self.eth.symbol} to Sai vault"  # noqa: E501
+                        )
                         event.counterparty = CPT_SAI
                         return DEFAULT_DECODING_OUTPUT
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L405'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L405</a>
```diff
                 context.event.event_type = HistoryEventType.DEPOSIT
                 context.event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                 context.event.notes = (
-                    f"Supply {context.event.balance.amount} {self.weth.symbol} to Sai vault"
-                )  # noqa: E501
+                    f"Supply {context.event.balance.amount} {self.weth.symbol} to Sai vault"  # noqa: E501
+                )
                 context.event.counterparty = CPT_SAI
                 return TransferEnrichmentOutput(matched_counterparty=CPT_SAI)
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L414'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L414</a>
```diff
                 context.event.event_type = HistoryEventType.DEPOSIT
                 context.event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                 context.event.notes = (
-                    f"Increase CDP collateral by {context.event.balance.amount} {self.peth.symbol}"
-                )  # noqa: E501
+                    f"Increase CDP collateral by {context.event.balance.amount} {self.peth.symbol}"  # noqa: E501
+                )
                 context.event.counterparty = CPT_SAI
                 return TransferEnrichmentOutput(matched_counterparty=CPT_SAI)
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L428'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L428</a>
```diff
                 context.event.event_type = HistoryEventType.WITHDRAWAL
                 context.event.event_subtype = HistoryEventSubType.REMOVE_ASSET
                 context.event.notes = (
-                    f"Withdraw {context.event.balance.amount} {self.weth.symbol} from Sai vault"
-                )  # noqa: E501
+                    f"Withdraw {context.event.balance.amount} {self.weth.symbol} from Sai vault"  # noqa: E501
+                )
                 context.event.counterparty = CPT_SAI
                 return TransferEnrichmentOutput(matched_counterparty=CPT_SAI)
 
```
<a href='https://github.com/rotki/rotki/blob/562b18f1bf5194574ea2fc0e4aa3211b2dd8e6bf/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L437'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L437</a>
```diff
                 context.event.event_type = HistoryEventType.WITHDRAWAL
                 context.event.event_subtype = HistoryEventSubType.REMOVE_ASSET
                 context.event.notes = (
-                    f"Decrease CDP collateral by {context.event.balance.amount} {self.peth.symbol}"
-                )  # noqa: E501
+                    f"Decrease CDP collateral by {context.event.balance.amount} {self.peth.symbol}"  # noqa: E501
+                )
                 context.event.counterparty = CPT_SAI
                 return TransferEnrichmentOutput(matched_counterparty...*[Comment body truncated]*

---

_Label `formatter` added by @MichaReiser on 2023-11-02 06:12_

---

_Review requested from @charliermarsh by @MichaReiser on 2023-11-02 06:32_

---

_Marked ready for review by @MichaReiser on 2023-11-02 06:32_

---

_Label `style` added by @MichaReiser on 2023-11-02 06:39_

---

_Comment by @github-actions[bot] on 2023-11-02 07:13_

## `ruff-ecosystem` results
### Formatter (stable)
ℹ️ ecosystem check **detected format changes**. (+354 -382 lines in 70 files in 41 projects)

<details><summary><a href="https://github.com/PostHog/HouseWatch">PostHog/HouseWatch</a> (+4 -6 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/PostHog/HouseWatch/blob/bf61a8e227311b3bc2fecd53823e4c1b37eb4a6d/housewatch/settings/__init__.py#L260'>housewatch/settings/__init__.py~L260</a>
```diff
 CELERY_IMPORTS = []  # type: ignore
 CELERY_BROKER_URL = RABBITMQ_URL  # celery connects to rabbitmq
 CELERY_BEAT_MAX_LOOP_INTERVAL = (
-    30
-)  # sleep max 30sec before checking for new periodic events
+    30  # sleep max 30sec before checking for new periodic events
+)
 CELERY_RESULT_BACKEND = REDIS_URL  # stores results for lookup when processing
-CELERY_IGNORE_RESULT = (
-    True
-)  # only applies to delay(), must do @shared_task(ignore_result=True) for apply_async
+CELERY_IGNORE_RESULT = True  # only applies to delay(), must do @shared_task(ignore_result=True) for apply_async
 CELERY_RESULT_EXPIRES = timedelta(
     days=4
 )  # expire tasks after 4 days instead of the default 1
```

</p>
</details>
<details><summary><a href="https://github.com/RasaHQ/rasa">RasaHQ/rasa</a> (+5 -9 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/server.py#L880'>rasa/server.py~L880</a>
```diff
 
         try:
             async with app.ctx.agent.lock_store.lock(conversation_id):
-                tracker = await (
-                    app.ctx.agent.processor.fetch_tracker_and_update_session(
-                        conversation_id
-                    )
+                tracker = await app.ctx.agent.processor.fetch_tracker_and_update_session(
+                    conversation_id
                 )
 
                 output_channel = _get_output_channel(request, tracker)
```
<a href='https://github.com/RasaHQ/rasa/blob/0dd893a1b343b069cbd237c74ded10ffa984afb4/rasa/server.py#L933'>rasa/server.py~L933</a>
```diff
 
         try:
             async with app.ctx.agent.lock_store.lock(conversation_id):
-                tracker = await (
-                    app.ctx.agent.processor.fetch_tracker_and_update_session(
-                        conversation_id
-                    )
+                tracker = await app.ctx.agent.processor.fetch_tracker_and_update_session(
+                    conversation_id
                 )
                 output_channel = _get_output_channel(request, tracker)
                 if intent_to_trigger not in app.ctx.agent.domain.intents:
```

</p>
</details>
<details><summary><a href="https://github.com/alteryx/featuretools">alteryx/featuretools</a> (+3 -7 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/tests/synthesis/test_deep_feature_synthesis.py#L229'>featuretools/tests/synthesis/test_deep_feature_synthesis.py~L229</a>
```diff
 
 
 def test_ignore_columns_input_type(es):
-    error_msg = (
-        r"ignore_columns should be dict\[str -> list\]"
-    )  # need to use string literals to avoid regex params
+    error_msg = r"ignore_columns should be dict\[str -> list\]"  # need to use string literals to avoid regex params
     wrong_input_type = {"log": "value"}
     with pytest.raises(TypeError, match=error_msg):
         DeepFeatureSynthesis(
```
<a href='https://github.com/alteryx/featuretools/blob/1498c9a9723deff9b049e4bb70a4859c70331942/featuretools/tests/synthesis/test_deep_feature_synthesis.py#L253'>featuretools/tests/synthesis/test_deep_feature_synthesis.py~L253</a>
```diff
 
 
 def test_ignore_columns_with_nonstring_keys(es):
-    error_msg = (
-        r"ignore_columns should be dict\[str -> list\]"
-    )  # need to use string literals to avoid regex params
+    error_msg = r"ignore_columns should be dict\[str -> list\]"  # need to use string literals to avoid regex params
     wrong_input_keys = {1: ["a", "b", "c"]}
     with pytest.raises(TypeError, match=error_msg):
         DeepFeatureSynthesis(
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+32 -36 lines across 6 files)</summary>
<p>
<pre>ruff format --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/AWS_SystemManager/Integrations/AWSSystemManager/AWSSystemManager.py#L31'>Packs/AWS_SystemManager/Integrations/AWSSystemManager/AWSSystemManager.py~L31</a>
```diff
 }
 SERVICE_NAME = "ssm"  # Amazon Simple Systems Manager (SSM).
 DEFAULT_TIMEOUT = 600  # Default timeout for polling commands.
-MAXIMUM_COMMAND_TIMEOUT = (
-    2592000  # Maximum timeout for running commands in ssm (30 days).
-)
+MAXIMUM_COMMAND_TIMEOUT = 2592000  # Maximum timeout for running commands in ssm (30 days).
 MINIMUM_COMMAND_TIMEOUT = 30  # Minimum timeout for running commands in ssm.
 DEFAULT_INTERVAL_IN_SECONDS = 30  # Interval for polling commands.
 TERMINAL_AUTOMATION_STATUSES = {  # the status for run automation command
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/AWS_SystemManager/Integrations/AWSSystemManager/AWSSystemManager.py#L1480'>Packs/AWS_SystemManager/Integrations/AWSSystemManager/AWSSystemManager.py~L1480</a>
```diff
     aws_role_arn = params.get("roleArn")
     aws_role_session_name = params.get("roleSessionName")
     aws_role_session_duration = params.get("sessionDuration")
-    aws_role_policy = (
-        None  # added it for using AWSClient class without changing the code
-    )
+    aws_role_policy = None  # added it for using AWSClient class without changing the code
     timeout = params["timeout"]
     retries = params["retries"]
 
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L150'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L150</a>
```diff
         SMB_CONNECTIONS_QUERY_SUFFIX = "\n| summarize RemoteIPCount=dcount(RemoteIP) by DeviceName, InitiatingProcessFileName, InitiatingProcessId, InitiatingProcessCreationTime\n|{} limit {}"  # noqa: E501
         CREDENTIAL_DUMPING_QUERY_SUFFIX = "\n| project Timestamp, DeviceName, ActionType, FileName, ProcessCommandLine, AccountName, InitiatingProcessIntegrityLevel, InitiatingProcessTokenElevation\n| limit {}"  # noqa: E501
         MANAGEMENT_CONNECTION_QUERY_SUFFIX = (
-            "\n| summarize TotalCount=count() by DeviceName,LocalIP,RemoteIP,RemotePort\n| order by TotalCount\n| limit {}"
-        )  # noqa: E501
+            "\n| summarize TotalCount=count() by DeviceName,LocalIP,RemoteIP,RemotePort\n| order by TotalCount\n| limit {}"  # noqa: E501
+        )
 
         def __init__(
             self,
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L257'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L257</a>
```diff
         """QUERY PREFIX"""
 
         SCHEDULE_JOB_QUERY_PREFIX = (
-            'DeviceEvents | where ActionType == "ScheduledTaskCreated" and InitiatingProcessAccountSid != "S-1-5-18" and'
-        )  # noqa: E501
+            'DeviceEvents | where ActionType == "ScheduledTaskCreated" and InitiatingProcessAccountSid != "S-1-5-18" and'  # noqa: E501
+        )
         REGISTRY_ENTRY_QUERY_PREFIX = 'DeviceRegistryEvents | where ActionType == "RegistryValueSet" and'
         STARTUP_FOLDER_CHANGES_QUERY_PREFIX = 'DeviceFileEvents | where FolderPath contains @"\AppData\Roaming\Microsoft\Windows\Start Menu\Programs\Startup" and ActionType == "FileCreated" and'  # noqa: E501
         NEW_SERVICE_CREATED_QUERY_PREFIX = 'DeviceRegistryEvents | where RegistryKey contains @"HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services" and ActionType == "RegistryKeyCreated" and'  # noqa: E501
         SERVICE_UPDATED_QUERY_PREFIX = 'DeviceRegistryEvents | where RegistryKey contains @"HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services" and ActionType has_any ("RegistryValueSet","RegistryKeyCreated") and'  # noqa: E501
         FILE_REPLACED_QUERY_PREFIX = (
-            'DeviceFileEvents | where FolderPath contains @"C:\Program Files" and ActionType == "FileModified" and'
-        )  # noqa: E501
+            'DeviceFileEvents | where FolderPath contains @"C:\Program Files" and ActionType == "FileModified" and'  # noqa: E501
+        )
         NEW_USER_QUERY_PREFIX = 'DeviceEvents | where ActionType == "UserAccountCreated" and'
         NEW_GROUP_QUERY_PREFIX = 'DeviceEvents | where ActionType == "SecurityGroupCreated" and'
         GROUP_USER_CHANGE_QUERY_PREFIX = 'DeviceEvents | where ActionType == "UserAccountAddedToLocalGroup" and'
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L576'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L576</a>
```diff
         GENERIC_PROCESS_DETAILS_QUERY_PREFIX = "DeviceProcessEvents | where"
         BECAONING_QUERY_PREFIX = "DeviceNetworkEvents | where"
         POWERSHELL_EXECUTION_PROCESS_QUERY_PREFIX = (
-            'DeviceProcessEvents | where FileName in~ ("powershell.exe", "powershell_ise.exe",".ps") and'
-        )  # noqa: E501
+            'DeviceProcessEvents | where FileName in~ ("powershell.exe", "powershell_ise.exe",".ps") and'  # noqa: E501
+        )
         POWERSHELL_EXECUTION_PROCESS_UNSIGNED_QUERY_PREFIX = 'DeviceProcessEvents | where FileName in~ ("powershell.exe", "powershell_ise.exe",".ps") and ( InitiatingProcessFileName != "SenerIR.exe" and InitiatingProcessParentFileName != "MsSense.exe" and InitiatingProcessSignatureStatus != "Valid" ) and (InitiatingProcessFileName != "CompatTelRunner.exe" and InitiatingProcessParentFileName != "CompatTelRunner.exe")'  # noqa: E501
 
         """QUERY SUFFIX"""
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L741'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L741</a>
```diff
         """QUERY SUFFIX"""
         EXTERNAL_ADDRESSES_QUERY_SUFFIX = "\n| summarize TotalConnections = count() by DeviceName, RemoteIP, RemotePort, InitiatingProcessFileName,InitiatingProcessFolderPath | order by TotalConnections\n| limit {}"  # noqa: E501
         DNS_QUERY_SUFFIX = (
-            "| project Timestamp,DeviceName,ActionType,RemoteIP,Packetinfo = url_decode(AdditionalFields)\n| limit {}"
-        )  # noqa: E501
+            "| project Timestamp,DeviceName,ActionType,RemoteIP,Packetinfo = url_decode(AdditionalFields)\n| limit {}"  # noqa: E501
+        )
         ENCODED_COMMANDS_QUERY_SUFFIX = "\n| limit {}"
 
         def __init__(
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py#L894'>Packs/MicrosoftDefenderAdvancedThreatProtection/Integrations/MicrosoftDefenderAdvancedThreatProtection/MicrosoftDefenderAdvancedThreatProtection.py~L894</a>
```diff
         COMPROMISED_INFORMATION_QUERY_SUFFIX = "\n| project Timestamp, DeviceId, DeviceName, ActionType, FileName, FolderPath, SHA1, SHA256, MD5, InitiatingProcessFileName\n| limit {}"  # noqa: E501
         CONNECTED_DEVICES_QUERY_SUFFIX = "\n| summarize by DeviceName\n| limit {}"
         ACTION_TYPES_QUERY_SUFFIX = (
-            "\n| summarize Number_of_actions=count(ActionType) by ActionType,DeviceName | order by Number_of_actions\n| limit {}"
-        )  # noqa: E501
+            "\n| summarize Number_of_actions=count(ActionType) by ActionType,DeviceName | order by Number_of_actions\n| limit {}"  # noqa: E501
+        )
         COMMON_FILES_QUERY_SUFFIX = "\n| summarize Number_of_accoiated_events=count(FileName) by FileName, MD5, SHA1, SHA256 | order by Number_of_accoiated_events\n| limit {}"  # noqa: E501
 
         def __init__(
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/SentinelOne/Integrations/SentinelOneEventCollector/SentinelOneEventCollector_test.py#L7'>Packs/SentinelOne/Integrations/SentinelOneEventCollector/SentinelOneEventCollector_test.py~L7</a>
```diff
 """ CONSTANTS """
 
 ACTIVITIES_MOCK_URL = (
-    "https://test.com/web/api/v2.1/activities?createdAt__gt=2022-01-04+00%3A00%3A00&limit=1000&sortBy=createdAt&sortOrder=asc"
-)  # noqa: E501
+    "https://test.com/web/api/v2.1/activities?createdAt__gt=2022-01-04+00%3A00%3A00&limit=1000&sortBy=createdAt&sortOrder=asc"  # noqa: E501
+)
 ACTIVITIES_SECOND_MOCK_URL = "https://test.com/web/api/v2.1/activities?createdAt__gt=2022-09-06T20%3A37%3A55.912951Z&limit=1000&sortBy=createdAt&sortOrder=asc"  # noqa: E501
 THREATS_MOCK_URL = (
-    "https://test.com/web/api/v2.1/threats?createdAt__gt=2022-01-04+00%3A00%3A00&limit=1000&sortBy=createdAt&sortOrder=asc"
-)  # noqa: E501
+    "https://test.com/web/api/v2.1/threats?createdAt__gt=2022-01-04+00%3A00%3A00&limit=1000&sortBy=createdAt&sortOrder=asc"  # noqa: E501
+)
 THREATS_SECOND_MOCK_URL = "https://test.com/web/api/v2.1/threats?createdAt__gt=2022-12-20T15%3A51%3A17.514437Z&limit=1000&sortBy=createdAt&sortOrder=asc"  # noqa: E501
 ALERTS_MOCK_URL = "https://test.com/web/api/v2.1/cloud-detection/alerts?limit=1000&createdAt__gt=2022-01-04+00%3A00%3A00&sortBy=alertInfoCreatedAt&sortOrder=asc"  # noqa: E501
 ALERTS_SECOND_MOCK_URL = "https://test.com/web/api/v2.1/cloud-detection/alerts?limit=1000&createdAt__gt=2022-12-20T13%3A54%3A43.027000Z&sortBy=alertInfoCreatedAt&sortOrder=asc"  # noqa: E501
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/XsoarWebserver/Integrations/XSOARWebServer/XSOARWebServer.py#L87'>Packs/XsoarWebserver/Integrations/XSOARWebServer/XSOARWebServer.py~L87</a>
```diff
         link_uuid = str(uuid.uuid4())
         partial_link = f"{xsoar_external_url}/instance/execute/{integration_instance_name}/process/{entry_uuid}/{link_uuid}/"
         partial_link_port = (
-            f"{xsoar_external_url}:{port}/instance/execute/{integration_instance_name}/process/{entry_uuid}/{link_uuid}/"
-        )  # noqa: E501
+            f"{xsoar_external_url}:{port}/instance/execute/{integration_instance_name}/process/{entry_uuid}/{link_uuid}/"  # noqa: E501
+        )
         temp_link_tracker = {"response": "", "response_received": False, "emailaddress": email}
 
         for ind, action in enumerate(input_list):
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/XsoarWebserver/Integrations/XSOARWebServer/XSOARWebServer.py#L131'>Packs/XsoarWebserver/Integrations/XSOARWebServer/XSOARWebServer.py~L131</a>
```diff
         link_uuid = str(uuid.uuid4())
         partial_link = f"{xsoar_external_url}/instance/execute/{integration_instance_name}/processform/{entry_uuid}/{link_uuid}/"
         partial_link_port = (
-            f"{xsoar_external_url}:{port}/instance/execute/{integration_instance_name}/processform/{entry_uuid}/{link_uuid}/"
-        )  # noqa: E501
+            f"{xsoar_external_url}:{port}/instance/execute/{integration_instance_name}/processform/{entry_uuid}/{link_uuid}/"  # noqa: E501
+        )
 
         if xsoar_proxy == "true":
             action_url = partial_link
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Packs/rasterize/Integrations/rasterize/rasterize.py#L42'>Packs/rasterize/Integrations/rasterize/rasterize.py~L42</a>
```diff
 DEFAULT_W, DEFAULT_H = "600", "800"
 DEFAULT_W_WIDE = "1024"
 CHROME_USER_AGENT = (
-    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.64 Safari/537.36"
-)  # noqa
+    "Mozilla/5.0 (Macintosh; Intel Mac OS X 10_15_7) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/101.0.4951.64 Safari/537.36"  # noqa
+)
 MAX_FULLSCREEN_W = 8000
 MAX_FULLSCREEN_H = 8000
 DRIVER_LOG = f"{tempfile.gettempdir()}/chromedriver.log"
```
<a href='https://github.com/demisto/content/blob/d18164ad51622173451959ff0e2cf61c3fd06c05/Tests/Marketplace/search_and_install_packs.py#L37'>Tests/Marketplace/search_and_install_packs.py~L37</a>
```diff
 PACKS_DIR = "Packs"
 PACK_METADATA_FILE = Pack.USER_METADATA
 GITLAB_PACK_METADATA_URL = (
-    f"{{gitlab_url}}/api/v4/projects/{CONTENT_PROJECT_ID}/repository/files/{PACKS_DIR}%2F{{pack_id}}%2F{PACK_METADATA_FILE}"
-)  # noqa: E501
+    f"{{gitlab_url}}/api/v4/projects/{CONTENT_PROJECT_ID}/repository/files/{PACKS_DIR}%2F{{pack_id}}%2F{PACK_METADATA_FILE}"  # noqa: E501
+)
 
 
 @lru_cache
```

</p>
</details>
<details><summary><a href="https://github.com/docker/docker-py">docker/docker-py</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/docker/docker-py/blob/c38656dc7894363f32317affecc3e4279e1163f8/tests/unit/fake_api.py#L6'>tests/unit/fake_api.py~L6</a>
```diff
 
 FAKE_CONTAINER_ID = "81cf499cc928ce3fedc250a080d2b9b978df20e4517304c45211e8a68b33e254"
 FAKE_IMAGE_ID = (
-    "sha256:fe7a8fc91d3f17835cbb3b86a1c60287500ab01a53bc79c4497d09f07a3f0688"
-)  # noqa: E501
+    "sha256:fe7a8fc91d3f17835cbb3b86a1c60287500ab01a53bc79c4497d09f07a3f0688"  # noqa: E501
+)
 FAKE_EXEC_ID = "b098ec855f10434b5c7c973c78484208223a83f663ddaefb0f02a242840cb1c7"
 FAKE_NETWORK_ID = "1999cfb42e414483841a125ade3c276c3cb80cb3269b14e339354ac63a31b02c"
 FAKE_IMAGE_NAME = "test_image"
```

</p>
</details>
<details><summary><a href="https://github.com/milvus-io/pymilvus">milvus-io/pymilvus</a> (+6 -6 lines across 2 files)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/milvus-io/pymilvus/blob/9ad0ceada72396be890b746fe3a78ed4d74b3589/examples/example_bulkinsert_json.py#L50'>examples/example_bulkinsert_json.py~L50</a>
```diff
 _JSON_FIELD_NAME = "json_field"
 _VARCHAR_FIELD_NAME = "varchar_field"
 _DYNAMIC_FIELD_NAME = (
-    "$meta"
-)  # dynamic field, the internal name is "$meta", enable_dynamic_field=True
+    "$meta"  # dynamic field, the internal name is "$meta", enable_dynamic_field=True
+)
 
 # minio
 DEFAULT_BUCKET_NAME = "a-bucket"
```
<a href='https://github.com/milvus-io/pymilvus/blob/9ad0ceada72396be890b746fe3a78ed4d74b3589/examples/example_bulkinsert_numpy.py#L51'>examples/example_bulkinsert_numpy.py~L51</a>
```diff
 _JSON_FIELD_NAME = "json_field"
 _VARCHAR_FIELD_NAME = "varchar_field"
 _DYNAMIC_FIELD_NAME = (
-    "$meta"
-)  # dynamic field, the internal name is "$meta", enable_dynamic_field=True
+    "$meta"  # dynamic field, the internal name is "$meta", enable_dynamic_field=True
+)
 
 # minio
 DEFAULT_BUCKET_NAME = "a-bucket"
```

</p>
</details>
<details><summary><a href="https://github.com/python/mypy">python/mypy</a> (+2 -4 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/python/mypy/blob/371219347a6d17e16924bbabf3e693c6874e7138/mypy/semanal.py#L3100'>mypy/semanal.py~L3100</a>
```diff
             if s.rvalue.analyzed.info.tuple_type and not has_placeholder(
                 s.rvalue.analyzed.info.tuple_type
             ):
-                return (
-                    True
-                )  # This is a valid and analyzed named tuple definition, nothing to do here.
+                return True  # This is a valid and analyzed named tuple definition, nothing to do here.
         if len(s.lvalues) != 1 or not isinstance(s.lvalues[0], (NameExpr, MemberExpr)):
             return False
         lvalue = s.lvalues[0]
```

</p>
</details>
<details><summary><a href="https://github.com/reflex-dev/reflex">reflex-dev/reflex</a> (+3 -3 lines across 1 file)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/reflex-dev/reflex/blob/6e71393ed5f29234071da2b43192fc12d9bdbbd7/reflex/vars.py#L506'>reflex/vars.py~L506</a>
```diff
             else:
                 # apply operator to operands (left operand <operator> right_operand)
                 operation_name = (
-                    f"{left_operand._var_full_name} {op} {right_operand._var_full_name}"
-                )  # type: ignore
+                    f"{left_operand._var_full_name} {op} {right_operand._var_full_name}"  # type: ignore
+                )
                 operation_name = format.wrap(operation_name, "(")
         else:
             # apply operator to left operand (<operator> left_operand)
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+294 -304 lines across 55 files)</summary>
<p>
<pre>ruff format</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/accounting/export/csv.py#L158'>rotkehlchen/accounting/export/csv.py~L158</a>
```diff
 
         index = event.index + CSV_INDEX_OFFSET
         value_formula = f"{amount_column}{index}*H{index}"
-        total_value_formula = (
-            f"(F{index}*H{index}+G{index}*H{index})"
-        )  # formula of both free and taxable  # noqa: E501
+        total_value_formula = f"(F{index}*H{index}+G{index}*H{index})"  # formula of both free and taxable  # noqa: E501
         cost_basis_column = "K" if name == "taxable" else "L"
         cost_basis = f"{cost_basis_column}{index}"
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/accounting/structures/eth2.py#L53'>rotkehlchen/accounting/structures/eth2.py~L53</a>
```diff
 ]
 
 STAKING_DB_INSERT_QUERY_STR = (
-    "eth_staking_events_info(identifier, validator_index, is_exit_or_blocknumber) VALUES (?, ?, ?)"
-)  # noqa: E501
+    "eth_staking_events_info(identifier, validator_index, is_exit_or_blocknumber) VALUES (?, ?, ?)"  # noqa: E501
+)
 STAKING_DB_UPDATE_QUERY_STR = (
-    "UPDATE eth_staking_events_info SET validator_index=?, is_exit_or_blocknumber=?"
-)  # noqa: E501
+    "UPDATE eth_staking_events_info SET validator_index=?, is_exit_or_blocknumber=?"  # noqa: E501
+)
 
 
 class EthStakingEvent(HistoryBaseEntry, metaclass=ABCMeta):  # noqa: PLW1641  # hash in superclass
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py#L38'>rotkehlchen/chain/arbitrum_one/modules/arbitrum_one_bridge/decoder.py~L38</a>
```diff
 L2_ERC20_GATEWAY = string_to_evm_address("0x09e9222E96E7B4AE2a407B98d48e330053351EEe")
 L2_GATEWAY_ROUTER = string_to_evm_address("0x5288c571Fd7aD117beA99bF60FE0846C4E84F933")
 TRANSFER_ROUTED = (
-    b"\x85)\x1d\xff!a\xa9</\x12\xc8\x19\xd3\x18\x89\xc9lc\x04!\x16\xf5\xbcZ Z\xa7\x01\xc2\xc4)\xf5"
-)  # noqa: E501
+    b"\x85)\x1d\xff!a\xa9</\x12\xc8\x19\xd3\x18\x89\xc9lc\x04!\x16\xf5\xbcZ Z\xa7\x01\xc2\xc4)\xf5"  # noqa: E501
+)
 TOKEN_WITHDRAWAL_INITIATED = (
-    b"0s\xa7N\xcbr\x8d\x10\xbew\x9f\xe1\x9at\xa1B\x8e F\x8f[M\x16{\xf9\xc7=\x90g\x84}s"
-)  # noqa: E501
+    b"0s\xa7N\xcbr\x8d\x10\xbew\x9f\xe1\x9at\xa1B\x8e F\x8f[M\x16{\xf9\xc7=\x90g\x84}s"  # noqa: E501
+)
 ETH_WITHDRAWAL_INITIATED = (
-    b">z\xaf\xa7}\xbf\x18k\x7f\xd4\x88\x00k\xef\xf8\x93tL\xaa<Oo)\x9e\x8ap\x9f\xa2\x08st\xfc"
-)  # noqa: E501
+    b">z\xaf\xa7}\xbf\x18k\x7f\xd4\x88\x00k\xef\xf8\x93tL\xaa<Oo)\x9e\x8ap\x9f\xa2\x08st\xfc"  # noqa: E501
+)
 DEPOSIT_TX_TYPE = 100  # A deposit of ETH from L1 to L2 via the Arbitrum bridge.
 ERC20_DEPOSIT_FINALIZED = b'\xc7\xf2\xe9\xc5\\@\xa5\x0f\xbc!}\xfcp\xcd9\xa2"\x94\r\xfab\x14Z\xa0\xcaI\xeb\x955\xd4\xfc\xb2'  # noqa: E501
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/decoding/constants.py#L2'>rotkehlchen/chain/ethereum/decoding/constants.py~L2</a>
```diff
 from rotkehlchen.chain.evm.types import string_to_evm_address
 
 GTC_CLAIM = (
-    b"\x04g R\xdc\xb6\xb5\xb1\x9a\x9c\xc2\xec\x1b\x8fD\x7f\x1f^G\xb5\xe2L\xfa^O\xfbd\rc\xca+\xe7"
-)  # noqa: E501
+    b"\x04g R\xdc\xb6\xb5\xb1\x9a\x9c\xc2\xec\x1b\x8fD\x7f\x1f^G\xb5\xe2L\xfa^O\xfbd\rc\xca+\xe7"  # noqa: E501
+)
 ONEINCH_CLAIM = (
-    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"
-)  # noqa: E501
+    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"  # noqa: E501
+)
 GNOSIS_CHAIN_BRIDGE_RECEIVE = b"\x9a\xfdG\x90~%\x02\x8c\xda\xca\x89\xd1\x93Q\x8c0+\xbb\x12\x86\x17\xd5\xa9\x92\xc5\xab\xd4X\x15Re\x93"  # noqa: E501
 GOVERNORALPHA_PROPOSE = (
-    b"}\x84\xa6&:\xe0\xd9\x8d3)\xbd{F\xbbN\x8do\x98\xcd5\xa7\xad\xb4\\'L\x8b\x7f\xd5\xeb\xd5\xe0"
-)  # noqa: E501
+    b"}\x84\xa6&:\xe0\xd9\x8d3)\xbd{F\xbbN\x8do\x98\xcd5\xa7\xad\xb4\\'L\x8b\x7f\xd5\xeb\xd5\xe0"  # noqa: E501
+)
 GOVERNORALPHA_PROPOSE_ABI = '{"anonymous":false,"inputs":[{"indexed":false,"internalType":"uint256","name":"id","type":"uint256"},{"indexed":false,"internalType":"address","name":"proposer","type":"address"},{"indexed":false,"internalType":"address[]","name":"targets","type":"address[]"},{"indexed":false,"internalType":"uint256[]","name":"values","type":"uint256[]"},{"indexed":false,"internalType":"string[]","name":"signatures","type":"string[]"},{"indexed":false,"internalType":"bytes[]","name":"calldatas","type":"bytes[]"},{"indexed":false,"internalType":"uint256","name":"startBlock","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"endBlock","type":"uint256"},{"indexed":false,"internalType":"string","name":"description","type":"string"}],"name":"ProposalCreated","type":"event"}'  # noqa: E501
 
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L21'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L21</a>
```diff
 DEPOSIT = b"\xc1,W\xb1\xc7:,:.\xa4a>\x94v\xab\xb3\xd8\xd1F\x85z\xabs)\xe2BC\xfbYq\x0c\x82"
 REDEEM_UNDERLYING = b"\x9cN\xd5\x99\xcd\x85U\xb9\xc1\xe8\xcdvC$\r}q\xebv\xb7\x92\x94\x8cI\xfc\xb4\xd4\x11\xf7\xb6\xb3\xc6"  # noqa: E501
 LIQUIDATION_CALL = (
-    b"V\x86GW\xfd[\x1f\xc9\xf3\x8f_:\x98\x1c\xd8\xaeQ,\xe4\x1b\x90,\xf7?\xc5\x06\xee6\x9ck\xc27"
-)  # noqa: E501
+    b"V\x86GW\xfd[\x1f\xc9\xf3\x8f_:\x98\x1c\xd8\xaeQ,\xe4\x1b\x90,\xf7?\xc5\x06\xee6\x9ck\xc27"  # noqa: E501
+)
 
 
 class Aavev1Decoder(DecoderInterface):
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L70'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L70</a>
```diff
                 event.event_subtype = HistoryEventSubType.RECEIVE_WRAPPED
                 event.counterparty = CPT_AAVE_V1
                 event.notes = (
-                    f"Receive {amount} {atoken.symbol} from aave-v1 for {event.location_label}"
-                )  # noqa: E501
+                    f"Receive {amount} {atoken.symbol} from aave-v1 for {event.location_label}"  # noqa: E501
+                )
                 receive_event = event
             elif (
                 event.event_type == HistoryEventType.RECEIVE
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L82'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L82</a>
```diff
                 event.event_subtype = HistoryEventSubType.REWARD
                 event.counterparty = CPT_AAVE_V1
                 event.notes = (
-                    f"Gain {event.balance.amount} {atoken.symbol} from aave-v1 as interest"
-                )  # noqa: E501
+                    f"Gain {event.balance.amount} {atoken.symbol} from aave-v1 as interest"  # noqa: E501
+                )
 
         maybe_reshuffle_events(
             ordered_events=[deposit_event, receive_event],
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L135'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L135</a>
```diff
                 event.event_subtype = HistoryEventSubType.REWARD
                 event.counterparty = CPT_AAVE_V1
                 event.notes = (
-                    f"Gain {event.balance.amount} {atoken.symbol} from aave-v1 as interest"
-                )  # noqa: E501
+                    f"Gain {event.balance.amount} {atoken.symbol} from aave-v1 as interest"  # noqa: E501
+                )
                 interest_event = event
 
         maybe_reshuffle_events(
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py#L165'>rotkehlchen/chain/ethereum/modules/aave/v1/decoder.py~L165</a>
```diff
                 # we are transfering the debt token
                 event.event_subtype = HistoryEventSubType.PAYBACK_DEBT
                 event.notes = (
-                    f"Payback {event.balance.amount} {asset.symbol} for an aave-v1 position"
-                )  # noqa: E501
+                    f"Payback {event.balance.amount} {asset.symbol} for an aave-v1 position"  # noqa: E501
+                )
                 event.counterparty = CPT_AAVE_V1
                 event.address = context.tx_log.address
                 event.extra_data = {
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py#L32'>rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py~L32</a>
```diff
 
 ENABLE_COLLATERAL = b'\x00\x05\x8aV\xea\x94e<\xdfO\x15-"z\xce"\xd4\xc0\n\xd9\x9e*C\xf5\x8c\xb7\xd9\xe3\xfe\xb2\x95\xf2'  # noqa: E501
 DISABLE_COLLATERAL = (
-    b'D\xc5\x8d\x816[f\xddK\x1a\x7f6\xc2Z\xa9{\x8cq\xc3a\xeeI7\xad\xc1\xa0\x00\x00"}\xb5\xdd'
-)  # noqa: E501
+    b'D\xc5\x8d\x816[f\xddK\x1a\x7f6\xc2Z\xa9{\x8cq\xc3a\xeeI7\xad\xc1\xa0\x00\x00"}\xb5\xdd'  # noqa: E501
+)
 DEPOSIT = b"\xdehW!\x95D\xbb[wF\xf4\x8e\xd3\x0b\xe68o\xef\xc6\x1b/\x86L\xac\xf5Y\x89;\xf5\x0f\xd9Q"
 WITHDRAW = (
-    b"1\x15\xd1D\x9a{s,\x98l\xba\x18$N\x89zE\x0fa\xe1\xbb\x8dX\x9c\xd2\xe6\x9el\x89$\xf9\xf7"
-)  # noqa: E501
+    b"1\x15\xd1D\x9a{s,\x98l\xba\x18$N\x89zE\x0fa\xe1\xbb\x8dX\x9c\xd2\xe6\x9el\x89$\xf9\xf7"  # noqa: E501
+)
 BORROW = b"\xc6\xa8\x980\x9e\x82>\xe5\x0b\xacd\xe4\\\xa8\xad\xbaf\x90\xe9\x9exA\xc4]uN*8\xe9\x01\x9d\x9b"  # noqa: E501
 REPAY = b"L\xdd\xe6\xe0\x9b\xb7U\xc9\xa5X\x9e\xba\xecd\x0b\xbf\xed\xff\x13b\xd4\xb2U\xeb\xf83\x97\x82\xb9\x94/\xaa"  # noqa: E501
 LIQUIDATION_CALL = (
-    b"\xe4\x13\xa3!\xe8h\x1d\x83\x1fM\xbc\xcb\xcay\r)R\xb5o\x97y\x08\xe4[\xe3s5S>\x00R\x86"
-)  # noqa: E501
+    b"\xe4\x13\xa3!\xe8h\x1d\x83\x1fM\xbc\xcb\xcay\r)R\xb5o\x97y\x08\xe4[\xe3s5S>\x00R\x86"  # noqa: E501
+)
 ETH_GATEWAYS = (
     string_to_evm_address("0xEFFC18fC3b7eb8E676dac549E0c693ad50D1Ce31"),
     string_to_evm_address("0xcc9a0B7c43DC2a5F023Bb9b738E45B0Ef6B06E04"),
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py#L117'>rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py~L117</a>
```diff
                     event.event_subtype = HistoryEventSubType.RECEIVE_WRAPPED
                     resolved_asset = event.asset.resolve_to_asset_with_symbol()
                     event.notes = (
-                        f"Receive {event.balance.amount} {resolved_asset.symbol} from AAVE v2"
-                    )  # noqa: E501
+                        f"Receive {event.balance.amount} {resolved_asset.symbol} from AAVE v2"  # noqa: E501
+                    )
                     event.counterparty = CPT_AAVE_V2
                 # Set protocol for both events
                 event.counterparty = CPT_AAVE_V2
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py#L232'>rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py~L232</a>
```diff
                 # we are transfering the aTOKEN
                 event.event_subtype = HistoryEventSubType.LIQUIDATE
                 event.notes = (
-                    f"An aave-v2 position got liquidated for {event.balance.amount} {asset.symbol}"
-                )  # noqa: E501
+                    f"An aave-v2 position got liquidated for {event.balance.amount} {asset.symbol}"  # noqa: E501
+                )
                 event.counterparty = CPT_AAVE_V2
                 event.address = tx_log.address
             elif (
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py#L246'>rotkehlchen/chain/ethereum/modules/aave/v2/decoder.py~L246</a>
```diff
                 # we are transfering the debt token
                 event.event_subtype = HistoryEventSubType.PAYBACK_DEBT
                 event.notes = (
-                    f"Payback {event.balance.amount} {asset.symbol} for an aave-v2 position"
-                )  # noqa: E501
+                    f"Payback {event.balance.amount} {asset.symbol} for an aave-v2 position"  # noqa: E501
+                )
                 event.counterparty = CPT_AAVE_V2
                 event.address = tx_log.address
                 event.extra_data = {
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/airdrops/decoder.py#L49'>rotkehlchen/chain/ethereum/modules/airdrops/decoder.py~L49</a>
```diff
 
 UNISWAP_DISTRIBUTOR = string_to_evm_address("0x090D4613473dEE047c3f2706764f49E0821D256e")
 UNISWAP_TOKEN_CLAIMED = (
-    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"
-)  # noqa: E501
+    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"  # noqa: E501
+)
 
 BADGERHUNT = string_to_evm_address("0x394DCfbCf25C5400fcC147EbD9970eD34A474543")
 BADGER_HUNT_EVENT = (
-    b"\x8e\xaf\x15aI\x08\xa4\xe9\x02!A\xfeJYk\x1a\xb0\xcbr\xab2\xb2P#\xe3\xda*E\x9c\x9a3\\"
-)  # noqa: E501
+    b"\x8e\xaf\x15aI\x08\xa4\xe9\x02!A\xfeJYk\x1a\xb0\xcbr\xab2\xb2P#\xe3\xda*E\x9c\x9a3\\"  # noqa: E501
+)
 
 ONEINCH = string_to_evm_address("0xE295aD71242373C37C5FdA7B57F26f9eA1088AFe")
 ONEINCH_CLAIMED = (
-    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"
-)  # noqa: E501
+    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"  # noqa: E501
+)
 
 FPIS = string_to_evm_address("0x61A1f84F12Ba9a56C22c31dDB10EC2e2CA0ceBCf")
 CONVEX = string_to_evm_address("0x2E088A0A19dda628B4304301d1EA70b114e4AcCd")
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/airdrops/decoder.py#L68'>rotkehlchen/chain/ethereum/modules/airdrops/decoder.py~L68</a>
```diff
 
 FOX_DISTRIBUTOR = string_to_evm_address("0xe099e688D12DBc19ab46D128d1Db297575474a0d")
 FOX_CLAIMED = (
-    b"R\x897\xb30\x08-\x89*\x98\xd4\xe4(\xab-\xcc\xa7\x84KQ\xd2'\xa1\xc0\xaeg\xf0\xb5&\x1a\xcb\xd9"
-)  # noqa: E501
+    b"R\x897\xb30\x08-\x89*\x98\xd4\xe4(\xab-\xcc\xa7\x84KQ\xd2'\xa1\xc0\xaeg\xf0\xb5&\x1a\xcb\xd9"  # noqa: E501
+)
 
 ELFI_LOCKING = string_to_evm_address("0x02Bd4A3b1b95b01F2Aa61655415A5d3EAAcaafdD")
 ELFI_VOTE_CHANGE = (
-    b"3\x16\x1c\xf2\xda(\xd7G\xbe\x9d\xf16\xb6\xf3r\x93\x90)\x84\x94\x94rht1\x93\xc5=s\xd3\xc2\xe0"
-)  # noqa: E501
+    b"3\x16\x1c\xf2\xda(\xd7G\xbe\x9d\xf16\xb6\xf3r\x93\x90)\x84\x94\x94rht1\x93\xc5=s\xd3\xc2\xe0"  # noqa: E501
+)
 
 
 logger = logging.getLogger(__name__)
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/balancer/v2/decoder.py#L144'>rotkehlchen/chain/ethereum/modules/balancer/v2/decoder.py~L144</a>
```diff
         if asset == context.event.asset:
             context.event.event_subtype = HistoryEventSubType.RECEIVE
             context.event.notes = (
-                f"Receive {context.event.balance.amount} {asset.symbol} from Balancer v2"
-            )  # noqa: E501
+                f"Receive {context.event.balance.amount} {asset.symbol} from Balancer v2"  # noqa: E501
+            )
         else:
             context.event.event_subtype = HistoryEventSubType.SPEND
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/base_bridge/decoder.py#L26'>rotkehlchen/chain/ethereum/modules/base_bridge/decoder.py~L26</a>
```diff
 BRIDGE_ADDRESS = string_to_evm_address("0x49048044D57e1C92A77f79988d21Fa8fAF74E97e")
 TOKEN_PORTAL = string_to_evm_address("0x3154Cf16ccdb4C6d922629664174b904d80F2C35")
 TRANSACTION_DEPOSITED = (
-    b"\xb3\x815h\xd9\x99\x1f\xc9Q\x96\x1f\xcbLxH\x93WB@\xa2\x89%`M\t\xfcW|U\xbb|2"
-)  # noqa: E501
+    b"\xb3\x815h\xd9\x99\x1f\xc9Q\x96\x1f\xcbLxH\x93WB@\xa2\x89%`M\t\xfcW|U\xbb|2"  # noqa: E501
+)
 WITHDRAWAL_FINALIZED = b"\xdb\\vR\x85z\xa1c\xda\xad\xd6p\xe1\x16b\x8f\xb4.\x86\x9d\x8a\xc4%\x1e\xf8\x97\x1d\x9eW'\xdf\x1b"  # noqa: E501
 BRIDGE_TOKEN = b"\x7f\xf1&\xdb\x80$BK\xbf\xd9\x82n\x8a\xb8/\xf5\x916(\x9e\xa4@\xb0K9\xa0\xdf\x1b\x03\xb9\xca\xbf"  # noqa: E501
 WITHDRAWAL_TOKEN = b'\xd5\x9ce\xb3TE"X5\xc8?P\xb6\xed\xe0j{\xe0G\xd2.5ps\xe2P\xd9\xafSu\x18\xcd'
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/compound/decoder.py#L38'>rotkehlchen/chain/ethereum/modules/compound/decoder.py~L38</a>
```diff
 
 MAXIMILLION_ADDR = string_to_evm_address("0xf859A1AD94BcF445A406B892eF0d3082f4174088")
 MINT_COMPOUND_TOKEN = (
-    b"L \x9b_\xc8\xadPu\x8f\x13\xe2\xe1\x08\x8b\xa5jV\r\xffi\n\x1co\xef&9OL\x03\x82\x1cO"
-)  # noqa: E501
+    b"L \x9b_\xc8\xadPu\x8f\x13\xe2\xe1\x08\x8b\xa5jV\r\xffi\n\x1co\xef&9OL\x03\x82\x1cO"  # noqa: E501
+)
 REDEEM_COMPOUND_TOKEN = b"\xe5\xb7T\xfb\x1a\xbb\x7f\x01\xb4\x99y\x1d\x0b\x82\n\xe3\xb6\xaf4$\xac\x1cYv\x8e\xdbS\xf4\xec1\xa9)"  # noqa: E501
 BORROW_COMPOUND = b"\x13\xedhf\xd4\xe1\xeem\xa4o\x84\\F\xd7\xe5A \x88=u\xc5\xea\x9a-\xac\xc1\xc4\xca\x89\x84\xab\x80"  # noqa: E501
 REPAY_COMPOUND = (
-    b'\x1a*"\xcb\x03M&\xd1\x85K\xdcff\xa5\xb9\x1f\xe2^\xfb\xbb]\xca\xd3\xb05Tx\xd6\xf5\xc3b\xa1'
-)  # noqa: E501
+    b'\x1a*"\xcb\x03M&\xd1\x85K\xdcff\xa5\xb9\x1f\xe2^\xfb\xbb]\xca\xd3\xb05Tx\xd6\xf5\xc3b\xa1'  # noqa: E501
+)
 LIQUIDATE_BORROW = (
-    b")\x867\xf6\x84\xdapgO&P\x9b\x10\xf0~\xc2\xfb\xc7z3Z\xb1\xe7\xd6!ZK$\x84\xd8\xbbR"
-)  # noqa: E501
+    b")\x867\xf6\x84\xdapgO&P\x9b\x10\xf0~\xc2\xfb\xc7z3Z\xb1\xe7\xd6!ZK$\x84\xd8\xbbR"  # noqa: E501
+)
 DISTRIBUTED_SUPPLIER_COMP = (
-    b",\xae\xcd\x17\xd0/V\xfa\x89w\x05\xdc\xc7@\xda-#|7?phoN\r\x9b\xd3\xbf\x04\x00\xeaz"
-)  # noqa: E501
+    b",\xae\xcd\x17\xd0/V\xfa\x89w\x05\xdc\xc7@\xda-#|7?phoN\r\x9b\xd3\xbf\x04\x00\xeaz"  # noqa: E501
+)
 DISTRIBUTED_BORROWER_COMP = b"\x1f\xc3\xec\xc0\x87\xd8\xd2\xd1^#\xd0\x03*\xf5\xa4pY\xc3\x89-\x00=\x8e\x13\x9f\xdc\xb6\xbb2|\x99\xa6"  # noqa: E501
 
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/compound/decoder.py#L309'>rotkehlchen/chain/ethereum/modules/compound/decoder.py~L309</a>
```diff
                 and event.counterparty == CPT_COMPOUND
             ):
                 event.notes = (
-                    f"Repay {repaid_amount} {repaying_asset.symbol} in a compound liquidation"
-                )  # noqa: E501
+                    f"Repay {repaid_amount} {repaying_asset.symbol} in a compound liquidation"  # noqa: E501
+                )
                 event.extra_data = {"in_liquidation": True}
             elif (
                 event.event_type == HistoryEventType.RECEIVE
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/convex/constants.py#L17'>rotkehlchen/chain/ethereum/modules/convex/constants.py~L17</a>
```diff
 CVXCRV_REWARDS = string_to_evm_address("0x3Fe65692bfCD0e6CF84cB1E7d24108E434A7587e")
 
 CVX_WITHDRAW_TOPIC = (
-    b"p\x84\xf5Gf\x18\xd8\xe6\x0b\x11\xef\r}?\x06\x91FU\xad\xb8y>(\xff\x7f\x01\x8dLv\xd5\x05\xd5"
-)  # noqa: E501
+    b"p\x84\xf5Gf\x18\xd8\xe6\x0b\x11\xef\r}?\x06\x91FU\xad\xb8y>(\xff\x7f\x01\x8dLv\xd5\x05\xd5"  # noqa: E501
+)
 CVXCRV_REWARD_PAID_TOPIC = b"\xe2@6@\xbah\xfe\xd3\xa2\xf8\x8buWU\x1d\x19\x93\xf8K\x99\xbb\x10\xff\x83?\x0c\xf8\xdb\x0c^\x04\x86"  # noqa: E501
 CVXCRV_WITHDRAWAL_TOPIC = (
-    b"p\x84\xf5Gf\x18\xd8\xe6\x0b\x11\xef\r}?\x06\x91FU\xad\xb8y>(\xff\x7f\x01\x8dLv\xd5\x05\xd5"
-)  # noqa: E501
+    b"p\x84\xf5Gf\x18\xd8\xe6\x0b\x11\xef\r}?\x06\x91FU\xad\xb8y>(\xff\x7f\x01\x8dLv\xd5\x05\xd5"  # noqa: E501
+)
 CVX_LOCKER_REWARD_PAID_TOPIC = (
-    b"T\x07\x98\xdfF\x8d{#\xd1\x1f\x15o\xdb\x95L\xb1\x9a\xd4\x14\xd1Pr*{mU\xba6\x9d\xeay."
-)  # noqa: E501
+    b"T\x07\x98\xdfF\x8d{#\xd1\x1f\x15o\xdb\x95L\xb1\x9a\xd4\x14\xd1Pr*{mU\xba6\x9d\xeay."  # noqa: E501
+)
 BOOSTER_WITHDRAW_TOPIC = b"\x92\xcc\xf4P\xa2\x86\xa9W\xafRP\x9b\xc1\xc9\x93\x9d\x1ajH\x17\x83\xe1B\xe4\x1e$\x99\xf0\xbbf\xeb\xc6"  # noqa: E501
 
 WITHDRAWAL_TOPICS = {
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L50'>rotkehlchen/chain/ethereum/modules/convex/decoder.py~L50</a>
```diff
 log = RotkehlchenLogsAdapter(logger)
 
 DEPOSIT_EVENT = (
-    b"\x9eq\xbc\x8e\xea\x02\xa69i\xf5\t\x81\x8f-\xaf\xb9%E2\x90C\x19\xf9\xdb\xday\xb6{\xd3J_="
-)  # noqa: E501
+    b"\x9eq\xbc\x8e\xea\x02\xa69i\xf5\t\x81\x8f-\xaf\xb9%E2\x90C\x19\xf9\xdb\xday\xb6{\xd3J_="  # noqa: E501
+)
 REWARD_ADDED = (
-    b'\xde\x88\xa9"\xe0\xd3\xb8\x8b$\xe9b>\xfe\xb4d\x91\x9ck\xf9\xf6hW\xa6^+\xfc\xf2\xce\x87\xa9C='
-)  # noqa: E501
+    b'\xde\x88\xa9"\xe0\xd3\xb8\x8b$\xe9b>\xfe\xb4d\x91\x9ck\xf9\xf6hW\xa6^+\xfc\xf2\xce\x87\xa9C='  # noqa: E501
+)
 
 
 class ConvexDecoder(DecoderInterface, ReloadableCacheDecoderMixin):
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L144'>rotkehlchen/chain/ethereum/modules/convex/decoder.py~L144</a>
```diff
                         event.notes = f"Return {event.balance.amount} {crypto_asset.symbol} to convex {self.pools[context.tx_log.address]} pool"  # noqa: E501
                     else:
                         event.notes = (
-                            f"Return {event.balance.amount} {crypto_asset.symbol} to convex"
-                        )  # noqa: E501
+                            f"Return {event.balance.amount} {crypto_asset.symbol} to convex"  # noqa: E501
+                        )
                 else:
                     event.event_type = HistoryEventType.DEPOSIT
                     event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L155'>rotkehlchen/chain/ethereum/modules/convex/decoder.py~L155</a>
```diff
                         event.product = EvmProduct.GAUGE
                     else:
                         event.notes = (
-                            f"Deposit {event.balance.amount} {crypto_asset.symbol} into convex"
-                        )  # noqa: E501
+                            f"Deposit {event.balance.amount} {crypto_asset.symbol} into convex"  # noqa: E501
+                        )
                         if (
                             isinstance(crypto_asset, EvmToken)
                             and crypto_asset.protocol == CURVE_POOL_PROTOCOL
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/convex/decoder.py#L192'>rotkehlchen/chain/ethereum/modules/convex/decoder.py~L192</a>
```diff
                         event.product = EvmProduct.GAUGE
                     else:
                         event.notes = (
-                            f"Withdraw {event.balance.amount} {crypto_asset.symbol} from convex"
-                        )  # noqa: E501
+                            f"Withdraw {event.balance.amount} {crypto_asset.symbol} from convex"  # noqa: E501
+                        )
                         if (
                             isinstance(crypto_asset, EvmToken)
                             and crypto_asset.protocol == CURVE_POOL_PROTOCOL
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L102'>rotkehlchen/chain/ethereum/modules/curve/decoder.py~L102</a>
```diff
 
 GAUGE_DEPOSIT = b"\xe1\xff\xfc\xc4\x92=\x04\xb5Y\xf4\xd2\x9a\x8b\xfcl\xda\x04\xeb[\r<F\x07Q\xc2@,\\\\\xc9\x10\x9c"  # noqa: E501
 GAUGE_WITHDRAW = (
-    b"\x88N\xda\xd9\xceo\xa2D\r\x8aT\xcc\x124\x90\xeb\x96\xd2v\x84y\xd4\x9f\xf9\xc76a%\xa9BCd"
-)  # noqa: E501
+    b"\x88N\xda\xd9\xceo\xa2D\r\x8aT\xcc\x124\x90\xeb\x96\xd2v\x84y\xd4\x9f\xf9\xc76a%\xa9BCd"  # noqa: E501
+)
 GAUGE_VOTE = (
-    b"E\xca\x9aL\x8d\x01\x19\xeb2\x9eX\r(\xfeh\x9eHN\x1b\xe20\xda\x807\xad\xe9T}-%\xcc\x91"
-)  # noqa: E501
+    b"E\xca\x9aL\x8d\x01\x19\xeb2\x9eX\r(\xfeh\x9eHN\x1b\xe20\xda\x807\xad\xe9T}-%\xcc\x91"  # noqa: E501
+)
 GAUGE_CONTROLLER = string_to_evm_address("0x2F50D538606Fa9EDD2B11E2446BEb18C9D5846bB")
 
 TOKEN_EXCHANGE = (
-    b"\x8b>\x96\xf2\xb8\x89\xfaw\x1cS\xc9\x81\xb4\r\xaf\x00_c\xf67\xf1\x86\x9fppR\xd1Z=\xd9q@"
-)  # noqa: E501
+    b"\x8b>\x96\xf2\xb8\x89\xfaw\x1cS\xc9\x81\xb4\r\xaf\x00_c\xf67\xf1\x86\x9fppR\xd1Z=\xd9q@"  # noqa: E501
+)
 TOKEN_EXCHANGE_UNDERLYING = (
-    b"\xd0\x13\xca#\xe7ze\x00<,e\x9cTB\xc0\x0c\x80Sq\xb7\xfc\x1e\xbdL lA\xd1Sk\xd9\x0b"
-)  # noqa: E501
+    b"\xd0\x13\xca#\xe7ze\x00<,e\x9cTB\xc0\x0c\x80Sq\xb7\xfc\x1e\xbdL lA\xd1Sk\xd9\x0b"  # noqa: E501
+)
 EXCHANGE_MULTIPLE = b"\x14\xb5a\x17\x8a\xe0\xf3h\xf4\x0f\xaf\xd0H\\Oq)\xeaq\xcd\xc0\x0bL\xe1\xe5\x94\x0f\x9b\xc6Y\xc8\xb2"  # noqa: E501
 CURVE_SWAP_ROUTER = string_to_evm_address("0x99a58482BD75cbab83b27EC03CA68fF489b5788f")
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/curve/decoder.py#L206'>rotkehlchen/chain/ethereum/modules/curve/decoder.py~L206</a>
```diff
                 event.event_subtype = HistoryEventSubType.REMOVE_ASSET
                 event.counterparty = CPT_CURVE
                 event.notes = (
-                    f"Remove {event.balance.amount} {crypto_asset.symbol} from the curve pool"
-                )  # noqa: E501
+                    f"Remove {event.balance.amount} {crypto_asset.symbol} from the curve pool"  # noqa: E501
+                )
                 withdrawal_events.append(event)
             elif (  # Withdraw send wrapped
                 event.event_type == HistoryEventType.SPEND
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/diva/decoder.py#L27'>rotkehlchen/chain/ethereum/modules/diva/decoder.py~L27</a>
```diff
 logger = logging.getLogger(__name__)
 log = RotkehlchenLogsAdapter(logger)
 DELEGATE_CHANGED = (
-    b"14\xe8\xa2\xe6\xd9~\x92\x9a~T\x01\x1e\xa5H]}\x19m\xd5\xf0\xbaMN\xf9X\x03\xe8\xe3\xfc%\x7f"
-)  # noqa: E501
+    b"14\xe8\xa2\xe6\xd9~\x92\x9a~T\x01\x1e\xa5H]}\x19m\xd5\xf0\xbaMN\xf9X\x03\xe8\xe3\xfc%\x7f"  # noqa: E501
+)
 CLAIM_AIRDROP = (
-    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"
-)  # noqa: E501
+    b"N\xc9\x0e\x96U\x19\xd9&\x81&tg\xf7u\xad\xa5\xbd!J\xa9,\r\xc9=\x90\xa5\xe8\x80\xce\x9e\xd0&"  # noqa: E501
+)
 DIVA_AIDROP_CONTRACT = string_to_evm_address("0x777E2B2Cc7980A6bAC92910B95269895EEf0d2E8")
 DIVA_GOVERNOR = string_to_evm_address("0xFb6B7C11a55C57767643F1FF65c34C8693a11A70")
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/dxdaomesa/decoder.py#L25'>rotkehlchen/chain/ethereum/modules/dxdaomesa/decoder.py~L25</a>
```diff
 DEPOSIT = b"\xc1\x1c\xc3N\x93\xc6z\x938+\x99\xf2I\x8e\x997\x19\x87\x98\xf3\xc1\xc2\x88\x80\x08\xff\xc0\xee\xb8/h\xc4"  # noqa: E501
 ORDER_PLACEMENT = b"\xde\xcfo\xde\x82C\x98\x12\x99\xf7\xb7\xa7v\xf2\x9a\x9f\xc6z,\x98H\xe2]w\xc5\x0e\xb1\x1f\xa5\x8a~!"  # noqa: E501
 WITHDRAW_REQUEST = (
-    b",bE\xafPo\x0f\xc1\x08\x99\x18\xc0,\x1d\x01\xbd\xe9\xcc\x80v\t\xb34\xb3\xe7dMm\xfbZl^"
-)  # noqa: E501
+    b",bE\xafPo\x0f\xc1\x08\x99\x18\xc0,\x1d\x01\xbd\xe9\xcc\x80v\t\xb34\xb3\xe7dMm\xfbZl^"  # noqa: E501
+)
 WITHDRAW = b"\x9b\x1b\xfa\x7f\xa9\xeeB\n\x16\xe1$\xf7\x94\xc3Z\xc9\xf9\x04r\xac\xc9\x91@\xeb/dG\xc7\x14\xca\xd8\xeb"  # noqa: E501
 
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/ens/decoder.py#L55'>rotkehlchen/chain/ethereum/modules/ens/decoder.py~L55</a>
```diff
 ENS_REVERSE_RESOLVER = string_to_evm_address("0xA2C122BE93b0074270ebeE7f6b7292C7deB45047")
 
 NAME_RENEWED = (
-    b"=\xa2L\x02E\x82\x93\x1c\xfa\xf8&}\x8e\xd2M\x13\xa8*\x80h\xd5\xbd3}0\xecE\xce\xa4\xe5\x06\xae"
-)  # noqa: E501
+    b"=\xa2L\x02E\x82\x93\x1c\xfa\xf8&}\x8e\xd2M\x13\xa8*\x80h\xd5\xbd3}0\xecE\xce\xa4\xe5\x06\xae"  # noqa: E501
+)
 NAME_RENEWED_ABI = '{"anonymous":false,"inputs":[{"indexed":false,"internalType":"string","name":"name","type":"string"},{"indexed":true,"internalType":"bytes32","name":"label","type":"bytes32"},{"indexed":false,"internalType":"uint256","name":"cost","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"expires","type":"uint256"}],"name":"NameRenewed","type":"event"}'  # noqa: E501
 NEW_RESOLVER = (
-    b"3W!\xb0\x18f\xdc#\xfb\xee\x8bk,{\x1e\x14\xd6\xf0\\(\xcd5\xa2\xc94#\x9f\x94\tV\x02\xa0"
-)  # noqa: E501
+    b"3W!\xb0\x18f\xdc#\xfb\xee\x8bk,{\x1e\x14\xd6\xf0\\(\xcd5\xa2\xc94#\x9f\x94\tV\x02\xa0"  # noqa: E501
+)
 NAME_REGISTERED_SINGLE_COST = b'\xcaj\xbb\xe9\xd7\xf1\x14"\xcbl\xa7b\x9f\xbfo\xe9\xef\xb1\xc6!\xf7\x1c\xe8\xf0+\x9f*#\x00\x97@O'  # noqa: E501
 NAME_REGISTERED_SINGLE_COST_ABI = '{"anonymous":false,"inputs":[{"indexed":false,"internalType":"string","name":"name","type":"string"},{"indexed":true,"internalType":"bytes32","name":"label","type":"bytes32"},{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":false,"internalType":"uint256","name":"cost","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"expires","type":"uint256"}],"name":"NameRegistered","type":"event"}'  # noqa: E501
 NAME_REGISTERED_BASE_COST_AND_PREMIUM = b"i\xe3\x7f\x15\x1e\xb9\x8a\ta\x8d\xda\xa8\x0c\x8c\xfa\xf1\xceY\x96\x86|H\x9fE\xb5U\xb4\x12'\x1e\xbf'"  # noqa: E501
 NAME_REGISTERED_BASE_COST_AND_PREMIUM_ABI = '{"anonymous":false,"inputs":[{"indexed":false,"internalType":"string","name":"name","type":"string"},{"indexed":true,"internalType":"bytes32","name":"label","type":"bytes32"},{"indexed":true,"internalType":"address","name":"owner","type":"address"},{"indexed":false,"internalType":"uint256","name":"baseCost","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"premium","type":"uint256"},{"indexed":false,"internalType":"uint256","name":"expires","type":"uint256"}],"name":"NameRegistered","type":"event"}'  # noqa: E501
 TEXT_CHANGED_KEY_ONLY = (
-    b"\xd8\xc93K\x1a\x9c/\x9d\xa3B\xa0\xa2\xb3&)\xc1\xa2)\xb6D]\xadx\x94\x7fgKDDJuP"
-)  # noqa: E501
+    b"\xd8\xc93K\x1a\x9c/\x9d\xa3B\xa0\xa2\xb3&)\xc1\xa2)\xb6D]\xadx\x94\x7fgKDDJuP"  # noqa: E501
+)
 TEXT_CHANGED_KEY_ONLY_ABI = '{"anonymous":false,"inputs":[{"indexed":true,"internalType":"bytes32","name":"node","type":"bytes32"},{"indexed":true,"internalType":"string","name":"indexedKey","type":"string"},{"indexed":false,"internalType":"string","name":"key","type":"string"}],"name":"TextChanged","type":"event"}'  # noqa: E501
 TEXT_CHANGED_KEY_AND_VALUE = (
-    b"D\x8b\xc0\x14\xf1Sg&\xcf\x8dT\xff=d\x81\xed<\xbch<%\x91\xca Bt\x00\x9a\xfa\t\xb1\xa1"
-)  # noqa: E501
+    b"D\x8b\xc0\x14\xf1Sg&\xcf\x8dT\xff=d\x81\xed<\xbch<%\x91\xca Bt\x00\x9a\xfa\t\xb1\xa1"  # noqa: E501
+)
 TEXT_CHANGED_KEY_AND_VALUE_ABI = '{"anonymous":false,"inputs":[{"indexed":true,"internalType":"bytes32","name":"node","type":"bytes32"},{"indexed":true,"internalType":"string","name":"indexedKey","type":"string"},{"indexed":false,"internalType":"string","name":"key","type":"string"},{"indexed":false,"internalType":"string","name":"value","type":"string"}],"name":"TextChanged","type":"event"}'  # noqa: E501
 CONTENT_HASH_CHANGED = (
-    b"\xe3y\xc1bN\xd7\xe7\x14\xcc\t7R\x8a25\x9di\xd5(\x137vS\x13\xdb\xa4\xe0\x81\xb7-ux"
-)  # noqa: E501
+    b"\xe3y\xc1bN\xd7\xe7\x14\xcc\t7R\x8a25\x9di\xd5(\x137vS\x13\xdb\xa4\xe0\x81\xb7-ux"  # noqa: E501
+)
 ENS_GOVERNOR = string_to_evm_address("0x323A76393544d5ecca80cd6ef2A560C6a395b7E3")
 
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/golem/decoder.py#L24'>rotkehlchen/chain/ethereum/modules/golem/decoder.py~L24</a>
```diff
 log = RotkehlchenLogsAdapter(logger)
 
 MIGRATED = (
-    b"\x92\x8f\xd5S\x13$\xee\x87\xd7l\xc50}\xc3u\x80\x17M\xa7k\x85\xcdTm\xa61\xb2g\x0b\xc2f\xb5"
-)  # noqa: E501
+    b"\x92\x8f\xd5S\x13$\xee\x87\xd7l\xc50}\xc3u\x80\x17M\xa7k\x85\xcdTm\xa61\xb2g\x0b\xc2f\xb5"  # noqa: E501
+)
 
 
 class GolemDecoder(DecoderInterface):
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/kyber/decoder.py#L27'>rotkehlchen/chain/ethereum/modules/kyber/decoder.py~L27</a>
```diff
 
 KYBER_TRADE_LEGACY = b"\xf7$\xb4\xdff\x17G6\x12\xb5=\x7f\x88\xec\xc6\xea\x980t\xb3\t`\xa0I\xfc\xd0e\x7f\xfe\x80\x80\x83"  # noqa: E501
 KYBER_TRADE = (
-    b"\xd3\x0c\xa3\x99\xcbCP~\xce\xc6\xa6)\xa3\\\xf4^\xb9\x8c\xdaU\x0c'im\xcb\r\x8cJ8s\xcel"
-)  # noqa: E501
+    b"\xd3\x0c\xa3\x99\xcbCP~\xce\xc6\xa6)\xa3\\\xf4^\xb9\x8c\xdaU\x0c'im\xcb\r\x8cJ8s\xcel"  # noqa: E501
+)
 KYBER_LEGACY_CONTRACT = string_to_evm_address("0x9ae49C0d7F8F9EF4B864e004FE86Ac8294E20950")
 KYBER_LEGACY_CONTRACT_MIGRATED = string_to_evm_address(
     "0x65bF64Ff5f51272f729BDcD7AcFB00677ced86Cd"
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/liquity/decoder.py#L34'>rotkehlchen/chain/ethereum/modules/liquity/decoder.py~L34</a>
```diff
 LIQUITY_STAKING = string_to_evm_address("0x4f9Fbb3f1E99B56e0Fe2892e623Ed36A76Fc605d")
 
 STABILITY_POOL_GAIN_WITHDRAW = (
-    b'QEr"\xeb\xca\x92\xc35\xc9\xc8n+\xaa\x1c\xc0\xe4\x0f\xfa\xa9\x08JQE)\x80\xd5\xba\x8d\xec/c'
-)  # noqa: E501
+    b'QEr"\xeb\xca\x92\xc35\xc9\xc8n+\xaa\x1c\xc0\xe4\x0f\xfa\xa9\x08JQE)\x80\xd5\xba\x8d\xec/c'  # noqa: E501
+)
 STABILITY_POOL_LQTY_PAID = (
-    b"&\x08\xb9\x86\xa6\xac\x0flb\x9c\xa3p\x18\xe8\n\xf5V\x1e6bR\xae\x93`*\x96\xd3\xab.s\xe4-"
-)  # noqa: E501
+    b"&\x08\xb9\x86\xa6\xac\x0flb\x9c\xa3p\x18\xe8\n\xf5V\x1e6bR\xae\x93`*\x96\xd3\xab.s\xe4-"  # noqa: E501
+)
 STABILITY_POOL_EVENTS = {STABILITY_POOL_GAIN_WITHDRAW, STABILITY_POOL_LQTY_PAID}
 STAKING_LQTY_CHANGE = (
-    b"9\xdf\x0eR\x86\xa3\xef/B\xa0\xbfR\xf3,\xfe,X\xe5\xb0@_G\xfeQ/,$9\xe4\xcf\xe2\x04"
-)  # noqa: E501
+    b"9\xdf\x0eR\x86\xa3\xef/B\xa0\xbfR\xf3,\xfe,X\xe5\xb0@_G\xfeQ/,$9\xe4\xcf\xe2\x04"  # noqa: E501
+)
 STAKING_ETH_SENT = (
-    b"a\t\xe2U\x9d\xfavj\xae\xc7\x11\x83Q\xd4\x8aR?\nAW\xf4\x9c\x8dht\x9c\x8a\xc4\x13\x18\xad\x12"
-)  # noqa: E501
+    b"a\t\xe2U\x9d\xfavj\xae\xc7\x11\x83Q\xd4\x8aR?\nAW\xf4\x9c\x8dht\x9c\x8a\xc4\x13\x18\xad\x12"  # noqa: E501
+)
 STAKING_LQTY_EVENTS = {STAKING_LQTY_CHANGE, STAKING_ETH_SENT}
 STAKING_REWARDS_ASSETS = {A_ETH, A_LUSD}
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/liquity/decoder.py#L221'>rotkehlchen/chain/ethereum/modules/liquity/decoder.py~L221</a>
```diff
                     event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                     event.counterparty = CPT_LIQUITY
                     event.notes = (
-                        f"Stake {event.balance.amount} {self.lqty.symbol} in the Liquity protocol"
-                    )  # noqa: E501
+                        f"Stake {event.balance.amount} {self.lqty.symbol} in the Liquity protocol"  # noqa: E501
+                    )
                     event.extra_data = extra_data
                 elif event.location_label == user and event.event_type == HistoryEventType.RECEIVE:
                     event.event_type = HistoryEventType.STAKING
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/decoder.py#L78'>rotkehlchen/chain/ethereum/modules/makerdao/decoder.py~L78</a>
```diff
 CDPMANAGER_FROB = b"E\xe6\xbd\xcd\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00\x00"  # noqa: E501
 
 SDAI_DEPOSIT = (
-    b"\xdc\xbc\x1c\x05$\x0f1\xff:\xd0g\xef\x1e\xe3\\\xe4\x99wbu.:\tR\x84uED\xf4\xc7\t\xd7"
-)  # noqa: E501
+    b"\xdc\xbc\x1c\x05$\x0f1\xff:\xd0g\xef\x1e\xe3\\\xe4\x99wbu.:\tR\x84uED\xf4\xc7\t\xd7"  # noqa: E501
+)
 SDAI_REDEEM = b"\xfb\xdey} \x1ch\x1b\x91\x05e)\x11\x9e\x0b\x02@|{\xb9jJ,u\xc0\x1f\xc9fr2\xc8\xdb"
 
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/decoder.py#L192'>rotkehlchen/chain/ethereum/modules/makerdao/decoder.py~L192</a>
```diff
                     event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                     event.counterparty = CPT_VAULT
                     event.notes = (
-                        f"Deposit {amount} {vault_asset.symbol} to {vault_type} MakerDAO vault"
-                    )  # noqa: E501
+                        f"Deposit {amount} {vault_asset.symbol} to {vault_type} MakerDAO vault"  # noqa: E501
+                    )
                     event.extra_data = {"vault_type": vault_type}
                     return DEFAULT_DECODING_OUTPUT
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/decoder.py#L215'>rotkehlchen/chain/ethereum/modules/makerdao/decoder.py~L215</a>
```diff
                     event.event_subtype = HistoryEventSubType.REMOVE_ASSET
                     event.counterparty = CPT_VAULT
                     event.notes = (
-                        f"Withdraw {amount} {vault_asset.symbol} from {vault_type} MakerDAO vault"
-                    )  # noqa: E501
+                        f"Withdraw {amount} {vault_asset.symbol} from {vault_type} MakerDAO vault"  # noqa: E501
+                    )
                     event.extra_data = {"vault_type": vault_type}
                     return DEFAULT_DECODING_OUTPUT
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/decoder.py#L463'>rotkehlchen/chain/ethereum/modules/makerdao/decoder.py~L463</a>
```diff
                         event.extra_data["cdp_id"] = cdp_id
 
                     event.notes = (
-                        f"Payback {event.balance.amount} DAI of debt to makerdao vault {cdp_id}"
-                    )  # noqa: E501
+                        f"Payback {event.balance.amount} DAI of debt to makerdao vault {cdp_id}"  # noqa: E501
+                    )
                     break
 
         else:  # collateral deposit
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L35'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L35</a>
```diff
 POOLED_ETHER_ADDRESS = string_to_evm_address("0xf53AD2c6851052A81B42133467480961B2321C09")
 PETH_BURN_EVENT_TOPIC = b"\xcc\x16\xf5\xdb\xb4\x872\x80\x81\\\x1e\xe0\x9d\xbd\x06sl\xff\xcc\x18D\x12\xcfzq\xa0\xfd\xb7]9|\xa5"  # noqa: E501
 PETH_MINT_EVENT_TOPIC = (
-    b"\x0fg\x98\xa5`y:T\xc3\xbc\xfe\x86\xa9<\xde\x1es\x08}\x94L\x0e\xa2\x05D\x13}A!9h\x85"
-)  # noqa: E501
+    b"\x0fg\x98\xa5`y:T\xc3\xbc\xfe\x86\xa9<\xde\x1es\x08}\x94L\x0e\xa2\x05D\x13}A!9h\x85"  # noqa: E501
+)
 SAI_CDP_MIGRATION_TOPIC = b'\n\t\x94\xe6\x12<i3\xeeu\x98\x86\x90WW\xde\xfe"\xc1\xf5\xd0<\xd1\xee1\\\xb6\xbd\xe8\xd1\x1a\xe8'  # noqa: E501
 MAKERDAO_SAITUB_CONTRACT = string_to_evm_address("0x448a5065aeBB8E423F0896E6c5D525C040f59af3")
 MAKERDAO_SAITAP_CONTRACT = string_to_evm_address("0xBda109309f9FafA6Dd6A9CB9f1Df4085B27Ee8eF")
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L109'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L109</a>
```diff
                         event.event_subtype = HistoryEventSubType.REMOVE_ASSET
                         event.counterparty = CPT_SAI
                         event.notes = (
-                            f"Withdraw {event.balance.amount} {self.eth.symbol} from CDP {cdp_id}"
-                        )  # noqa: E501
+                            f"Withdraw {event.balance.amount} {self.eth.symbol} from CDP {cdp_id}"  # noqa: E501
+                        )
                         return DEFAULT_DECODING_OUTPUT
 
         return DEFAULT_DECODING_OUTPUT
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L384'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L384</a>
```diff
                         event.event_type = HistoryEventType.DEPOSIT
                         event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                         event.notes = (
-                            f"Supply {event.balance.amount} {self.eth.symbol} to Sai vault"
-                        )  # noqa: E501
+                            f"Supply {event.balance.amount} {self.eth.symbol} to Sai vault"  # noqa: E501
+                        )
                         event.counterparty = CPT_SAI
                         return DEFAULT_DECODING_OUTPUT
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L405'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L405</a>
```diff
                 context.event.event_type = HistoryEventType.DEPOSIT
                 context.event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                 context.event.notes = (
-                    f"Supply {context.event.balance.amount} {self.weth.symbol} to Sai vault"
-                )  # noqa: E501
+                    f"Supply {context.event.balance.amount} {self.weth.symbol} to Sai vault"  # noqa: E501
+                )
                 context.event.counterparty = CPT_SAI
                 return TransferEnrichmentOutput(matched_counterparty=CPT_SAI)
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L414'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L414</a>
```diff
                 context.event.event_type = HistoryEventType.DEPOSIT
                 context.event.event_subtype = HistoryEventSubType.DEPOSIT_ASSET
                 context.event.notes = (
-                    f"Increase CDP collateral by {context.event.balance.amount} {self.peth.symbol}"
-                )  # noqa: E501
+                    f"Increase CDP collateral by {context.event.balance.amount} {self.peth.symbol}"  # noqa: E501
+                )
                 context.event.counterparty = CPT_SAI
                 return TransferEnrichmentOutput(matched_counterparty=CPT_SAI)
 
```
<a href='https://github.com/rotki/rotki/blob/0b9c37b30c7e8609621db22c850da5ba8774fbe5/rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py#L428'>rotkehlchen/chain/ethereum/modules/makerdao/sai/decoder.py~L428</a>
```diff
                 context.event.event_type = HistoryEventType.WITHDRAWAL
                 context.event.event_subtype = HistoryEventSubType.REMOVE_ASSET
                 context.event.notes = (
-                    f"Withdraw {context.event.balance.amount} {self.weth.symbol} from Sai vault"
-                )  # noqa: E501
+                    f"Withdraw {context.event.balance.amount} {self.weth.symbol} from Sai vault"  # noqa: E501
+                )
                 context.event.counterparty = CPT_SAI
                 return TransferEnrichmentOutput(matched_co...*[Comment body truncated]*

---

_Comment by @konstin on 2023-11-02 12:30_

I like the new style

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:512 on 2023-11-02 17:32_

I'm trying to understand the relationship and expectations between this and the `if !inline_comments.is_empty() {` below. Could this not be moved outside of the closure?

---

_@charliermarsh reviewed on 2023-11-02 17:32_

---

_@charliermarsh reviewed on 2023-11-02 17:32_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/expression/mod.rs`:456 on 2023-11-02 17:32_

Would this work out-of-the-box if we _did_ associate the trailing statement comments on the expression, ignoring the suite formatting?

---

_@MichaReiser reviewed on 2023-11-03 01:19_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:456 on 2023-11-03 01:19_

No, it would still require the `if_group_breaks` and `if_group_fits` but it would avoid the unprecedented "the child steals comments from its parent".

---

_@MichaReiser reviewed on 2023-11-03 01:20_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:512 on 2023-11-03 01:20_

The `inine_comments.is_empty` is just an optimization so that we don't write the `if_group_breaks` and call into `trailing_comments` when the list is empty. It doesn't change the formatted result.

> Could this not be moved outside of the closure?

If you mean the `!inline_comments.is_empty` then no, because we always want to apply the `best_fit_parenthesize` style. What we could do is to duplicate the `best_fit_parenthesize` instead. No strong opinion on either of them.

---

_@charliermarsh approved on 2023-11-03 02:30_

I haven't reviewed the ecosystem changes in detail, but the PR and summary look right to me.

---

_Merged by @MichaReiser on 2023-11-03 05:13_

---

_Closed by @MichaReiser on 2023-11-03 05:13_

---

_Branch deleted on 2023-11-03 05:13_

---
