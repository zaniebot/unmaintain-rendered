```yaml
number: 9222
title: Parenthesize multi-context managers
type: pull_request
state: merged
author: MichaReiser
labels:
  - formatter
  - preview
assignees: []
merged: true
base: main
head: parenthesize-context-managers
created_at: 2023-12-21T03:52:44Z
updated_at: 2023-12-22T03:45:15Z
url: https://github.com/astral-sh/ruff/pull/9222
synced_at: 2026-01-12T15:55:28Z
```

# Parenthesize multi-context managers

---

_@MichaReiser_

## Summary

This PR implements Black's `wrap_multiple_context_managers_in_parens` and `improved_async_statements_handling` preview styles. 


Closes #8889
Closes #8890

## Test Plan

* Verified that the similarity index remains unchanged
* Reviewed the preview ecosystem results locally



---

_Label `formatter` added by @MichaReiser on 2023-12-21 03:53_

---

_Label `preview` added by @MichaReiser on 2023-12-21 03:53_

---

_Comment by @github-actions[bot] on 2023-12-21 04:09_

<!-- generated-comment ecosystem -->
## `ruff-ecosystem` results
### Formatter (stable)
✅ ecosystem check detected no format changes.

### Formatter (preview)
ℹ️ ecosystem check **detected format changes**. (+205 -124 lines in 31 files in 6 projects; 35 projects unchanged)

<details><summary><a href="https://github.com/commaai/openpilot">commaai/openpilot</a> (+3 -2 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/commaai/openpilot/blob/5e6290c4a24e31428b1c1d3d4580128b77e3efeb/selfdrive/thermald/tests/test_power_monitoring.py#L126'>selfdrive/thermald/tests/test_power_monitoring.py~L126</a>
```diff
         POWER_DRAW = 0  # To stop shutting down for other reasons
         TEST_TIME = 350
         VOLTAGE_SHUTDOWN_MIN_OFFROAD_TIME_S = 50
-        with pm_patch("VOLTAGE_SHUTDOWN_MIN_OFFROAD_TIME_S", VOLTAGE_SHUTDOWN_MIN_OFFROAD_TIME_S, constant=True), pm_patch(
-            "HARDWARE.get_current_power_draw", POWER_DRAW
+        with (
+            pm_patch("VOLTAGE_SHUTDOWN_MIN_OFFROAD_TIME_S", VOLTAGE_SHUTDOWN_MIN_OFFROAD_TIME_S, constant=True),
+            pm_patch("HARDWARE.get_current_power_draw", POWER_DRAW),
         ):
             pm = PowerMonitoring()
             pm.car_battery_capacity_uWh = CAR_BATTERY_CAPACITY_uWh
```

</p>
</details>
<details><summary><a href="https://github.com/demisto/content">demisto/content</a> (+79 -51 lines across 9 files)</summary>
<p>
<pre>ruff format --preview --exclude Packs/ThreatQ/Integrations/ThreatQ/ThreatQ.py</pre>
</p>
<p>

<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query_test.py#L909'>Packs/Active_Directory_Query/Integrations/Active_Directory_Query/Active_Directory_Query_test.py~L909</a>
```diff
     def mock_create_connection(server, server_ip, username, password, ntlm_connection, auto_bind):
         return MockConnection()
 
-    with patch("Active_Directory_Query.create_connection", side_effect=mock_create_connection), patch(
-        "Active_Directory_Query.Connection.unbind", side_effect=MockConnection.unbind
+    with (
+        patch("Active_Directory_Query.create_connection", side_effect=mock_create_connection),
+        patch("Active_Directory_Query.Connection.unbind", side_effect=MockConnection.unbind),
     ):
         command_results = Active_Directory_Query.test_credentials_command(BASE_TEST_PARAMS["server_ip"], ntlm_connection="true")
         assert command_results.readable_output == "Credential test with username username_test_credentials succeeded."
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/CybleEventsV2/Integrations/CybleEventsV2/CybleEventsV2_test.py#L188'>Packs/CybleEventsV2/Integrations/CybleEventsV2/CybleEventsV2_test.py~L188</a>
```diff
     collections = ["random_collections", "Darkweb Marketplaces", "Data Breaches", "Compromised Endpoints", "Compromised Cards"]
     incident_severity = ["Low", "Medium", "High"]
 
-    with capfd.disabled(), pytest.raises(
-        ValueError, match="The limit argument should contain a positive number," f" up to 1000, limit: {limit}"
+    with (
+        capfd.disabled(),
+        pytest.raises(ValueError, match="The limit argument should contain a positive number," f" up to 1000, limit: {limit}"),
     ):
         cyble_events(client, "POST", "some_random_token", url, args, {}, False, collections, incident_severity, True)
 
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/CybleEventsV2/Integrations/CybleEventsV2/CybleEventsV2_test.py#L203'>Packs/CybleEventsV2/Integrations/CybleEventsV2/CybleEventsV2_test.py~L203</a>
```diff
         "from": "0",
         "limit": "-1",
     }
-    with capfd.disabled(), pytest.raises(
-        ValueError, match="The limit argument should contain a positive number," f" up to 1000, limit: {args.get('limit', '50')}"
+    with (
+        capfd.disabled(),
+        pytest.raises(
+            ValueError,
+            match="The limit argument should contain a positive number," f" up to 1000, limit: {args.get('limit', '50')}",
+        ),
     ):
         validate_input(args=args)
 
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/CybleEventsV2/Integrations/CybleEventsV2/CybleEventsV2_test.py#L218'>Packs/CybleEventsV2/Integrations/CybleEventsV2/CybleEventsV2_test.py~L218</a>
```diff
         "from": "0",
         "limit": "-1",
     }
-    with capfd.disabled(), pytest.raises(
-        ValueError, match="The limit argument should contain a positive number," f" up to 1000, limit: {args.get('limit', '50')}"
+    with (
+        capfd.disabled(),
+        pytest.raises(
+            ValueError,
+            match="The limit argument should contain a positive number," f" up to 1000, limit: {args.get('limit', '50')}",
+        ),
     ):
         validate_input(args=args, is_iocs=True)
 
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/CybleEventsV2/Integrations/CybleEventsV2/CybleEventsV2_test.py#L234'>Packs/CybleEventsV2/Integrations/CybleEventsV2/CybleEventsV2_test.py~L234</a>
```diff
         "limit": "1",
     }
 
-    with capfd.disabled(), pytest.raises(
-        ValueError, match=f"Start date {args.get('start_date')} cannot " f"be after end date {args.get('end_date')}"
+    with (
+        capfd.disabled(),
+        pytest.raises(
+            ValueError, match=f"Start date {args.get('start_date')} cannot " f"be after end date {args.get('end_date')}"
+        ),
     ):
         validate_input(args=args, is_iocs=True)
 
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/McAfee-TIE/Integrations/McAfeeTIEV2/McAfeeTIEV2.py#L525'>Packs/McAfee-TIE/Integrations/McAfeeTIEV2/McAfeeTIEV2.py~L525</a>
```diff
 
 @contextlib.contextmanager
 def create_dxl_config(instance_cert: InstanceCertificates) -> DxlClientConfig:
-    with tempfile.NamedTemporaryFile(mode="w+", dir="./", suffix=".crt") as broker_certs_file, tempfile.NamedTemporaryFile(
-        mode="w+", dir="./", suffix=".crt"
-    ) as client_cert_file, tempfile.NamedTemporaryFile(mode="w+", dir="./", suffix=".key") as private_key_file:
+    with (
+        tempfile.NamedTemporaryFile(mode="w+", dir="./", suffix=".crt") as broker_certs_file,
+        tempfile.NamedTemporaryFile(mode="w+", dir="./", suffix=".crt") as client_cert_file,
+        tempfile.NamedTemporaryFile(mode="w+", dir="./", suffix=".key") as private_key_file,
+    ):
         broker_certs_file.delete
         create_temp_credentials(broker_certs_file, instance_cert.broker_ca_bundle)
         create_temp_credentials(client_cert_file, instance_cert.client_cert)
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/ProofpointEmailSecurity/Integrations/ProofpointEmailSecurityEventCollector/ProofpointEmailSecurityEventCollector.py#L47'>Packs/ProofpointEmailSecurity/Integrations/ProofpointEmailSecurityEventCollector/ProofpointEmailSecurityEventCollector.py~L47</a>
```diff
     if to_time:
         url += f"&toTime={to_time}"
     extra_headers = {"Authorization": f"Bearer {api_key}"}
-    with connect(
-        url.format(host=host, cluster_id=cluster_id, type=EventType.MESSAGE.value, time=since_time),
-        additional_headers=extra_headers,
-    ) as message_connection, connect(
-        url.format(host=host, cluster_id=cluster_id, type=EventType.MAILLOG.value, time=since_time),
-        additional_headers=extra_headers,
-    ) as maillog_connection:
+    with (
+        connect(
+            url.format(host=host, cluster_id=cluster_id, type=EventType.MESSAGE.value, time=since_time),
+            additional_headers=extra_headers,
+        ) as message_connection,
+        connect(
+            url.format(host=host, cluster_id=cluster_id, type=EventType.MAILLOG.value, time=since_time),
+            additional_headers=extra_headers,
+        ) as maillog_connection,
+    ):
         yield message_connection, maillog_connection
 
 
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/RDPCacheHunting/Scripts/SetIndicatorstype/SetIndicatorstype_test.py#L100'>Packs/RDPCacheHunting/Scripts/SetIndicatorstype/SetIndicatorstype_test.py~L100</a>
```diff
     ]
 
     # Patch demisto.context and argToList functions
-    with patch.object(demisto, "context", return_value={"ExtractedIndicators": test_data}), patch.object(
-        demisto, "results"
-    ), patch.object(random, "randint", return_value=123456):
+    with (
+        patch.object(demisto, "context", return_value={"ExtractedIndicators": test_data}),
+        patch.object(demisto, "results"),
+        patch.object(random, "randint", return_value=123456),
+    ):
         main()
         result = demisto.results.call_args[0][0]
 
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/RDPCacheHunting/Scripts/SetIndicatorstype/SetIndicatorstype_test.py#L113'>Packs/RDPCacheHunting/Scripts/SetIndicatorstype/SetIndicatorstype_test.py~L113</a>
```diff
 
 def test_main_without_data():
     # Mock the demisto context and argToList functions
-    with patch.object(demisto, "context", return_value={"ExtractedIndicators": None}), patch.object(
-        demisto, "results"
-    ) as mocked_results:
+    with (
+        patch.object(demisto, "context", return_value={"ExtractedIndicators": None}),
+        patch.object(demisto, "results") as mocked_results,
+    ):
         main()
 
         # Check if the results function was called with the correct parameters for the empty data case
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/Slack/Integrations/SlackV3/SlackV3_test.py#L5060'>Packs/Slack/Integrations/SlackV3/SlackV3_test.py~L5060</a>
```diff
     )
     req = SocketModeRequest(type="event", payload=SAMPLE_PAYLOAD, envelope_id=default_envelope_id)
     client = mock.MagicMock(spec=SocketModeClient)
-    with mock.patch.object(SlackV3, "get_user_details") as mock_get_user_details, mock.patch.object(
-        demisto, "debug"
-    ) as mock_debug, mock.patch.object(requests, "post") as mock_send_slack_request, mock.patch.object(
-        SlackV3, "reset_listener_health"
-    ), mock.patch.object(demisto, "handleEntitlementForUser") as mock_result:
+    with (
+        mock.patch.object(SlackV3, "get_user_details") as mock_get_user_details,
+        mock.patch.object(demisto, "debug") as mock_debug,
+        mock.patch.object(requests, "post") as mock_send_slack_request,
+        mock.patch.object(SlackV3, "reset_listener_health"),
+        mock.patch.object(demisto, "handleEntitlementForUser") as mock_result,
+    ):
         # Set the return value of the mocked get_user_details function
         mock_user = {"id": "mock_user_id", "name": "mock_user"}
         mock_get_user_details.return_value = mock_user
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/Workday/Integrations/WorkdaySignOnEventCollector/WorkdaySignOnEventCollector_test.py#L524'>Packs/Workday/Integrations/WorkdaySignOnEventCollector/WorkdaySignOnEventCollector_test.py~L524</a>
```diff
     }
 
     # When: Calling the fetch_sign_on_events_command
-    with patch.object(Client, "retrieve_events", return_value=mock_retrieve_response), patch(
-        "demistomock.getLastRun", return_value=mock_last_run
+    with (
+        patch.object(Client, "retrieve_events", return_value=mock_retrieve_response),
+        patch("demistomock.getLastRun", return_value=mock_last_run),
     ):
         client = Client(
             "mock_url",
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/Workday/Integrations/WorkdaySignOnEventCollector/WorkdaySignOnEventCollector_test.py#L570'>Packs/Workday/Integrations/WorkdaySignOnEventCollector/WorkdaySignOnEventCollector_test.py~L570</a>
```diff
     }
 
     # Mocking demisto.command to return 'fetch-events'
-    with patch("demistomock.command", return_value="fetch-events"), patch(
-        "demistomock.getLastRun", return_value={"some": "data"}
-    ), patch("demistomock.setLastRun") as mock_set_last_run, patch("demistomock.params", return_value=mock_params), patch(
-        "WorkdaySignOnEventCollector.Client"
-    ) as mock_client, patch(
-        "WorkdaySignOnEventCollector.fetch_sign_on_events_command"
-    ) as mock_fetch_sign_on_events_command, patch(
-        "WorkdaySignOnEventCollector.send_events_to_xsiam"
-    ) as mock_send_events_to_xsiam:
+    with (
+        patch("demistomock.command", return_value="fetch-events"),
+        patch("demistomock.getLastRun", return_value={"some": "data"}),
+        patch("demistomock.setLastRun") as mock_set_last_run,
+        patch("demistomock.params", return_value=mock_params),
+        patch("WorkdaySignOnEventCollector.Client") as mock_client,
+        patch("WorkdaySignOnEventCollector.fetch_sign_on_events_command") as mock_fetch_sign_on_events_command,
+        patch("WorkdaySignOnEventCollector.send_events_to_xsiam") as mock_send_events_to_xsiam,
+    ):
         # Mocking the output of fetch_sign_on_events_command
         mock_events = [{"event": "data"}]
         mock_new_last_run = {"new": "data"}
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Packs/Zoom/Integrations/Zoom/Zoom_test.py#L2487'>Packs/Zoom/Integrations/Zoom/Zoom_test.py~L2487</a>
```diff
         "messages": "entitlement",
     }
     # Mock the required functions (get_integration_context, set_to_integration_context_with_retries) and any other dependencies
-    with patch("Zoom.get_integration_context") as mock_get_integration_context, patch(
-        "Zoom.set_to_integration_context_with_retries"
-    ) as mock_set_integration_context:
+    with (
+        patch("Zoom.get_integration_context") as mock_get_integration_context,
+        patch("Zoom.set_to_integration_context_with_retries") as mock_set_integration_context,
+    ):
         # Mock the return values of the mocked functions
         mock_get_integration_context.return_value = {"messages": []}
         fixed_timestamp = "2023-09-09 20:08:50"
```
<a href='https://github.com/demisto/content/blob/dbb76a65f09a791b246739dba95b5a60563b3075/Tests/tests_e2e/content/xsoar_saas/test_e2e_xsoar_saas.py#L174'>Tests/tests_e2e/content/xsoar_saas/test_e2e_xsoar_saas.py~L174</a>
```diff
         instance_name=get_integration_instance_name(integration_params, default="SlackV3"),
     ):
         playbook_id_name = "TestSlackAskE2E"
-        with save_playbook(
-            xsoar_saas_client,
-            playbook_path="Tests/tests_e2e/content/xsoar_saas/TestSlackAskE2E.yml",
-            playbook_id=playbook_id_name,
-            playbook_name=playbook_id_name,
-        ), save_incident(xsoar_saas_client, playbook_id=playbook_id_name) as incident_response:
+        with (
+            save_playbook(
+                xsoar_saas_client,
+                playbook_path="Tests/tests_e2e/content/xsoar_saas/TestSlackAskE2E.yml",
+                playbook_id=playbook_id_name,
+                playbook_name=playbook_id_name,
+            ),
+            save_incident(xsoar_saas_client, playbook_id=playbook_id_name) as incident_response,
+        ):
             # make sure the playbook finished successfully
             assert xsoar_saas_client.poll_playbook_state(
                 incident_response.id, expected_states=(InvestigationPlaybookState.COMPLETED,)
```

</p>
</details>
<details><summary><a href="https://github.com/fronzbot/blinkpy">fronzbot/blinkpy</a> (+13 -8 lines across 1 file)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/tests/test_blinkpy.py#L69'>tests/test_blinkpy.py~L69</a>
```diff
         self.assertEqual(self.blink.last_refresh, None)
         self.assertEqual(self.blink.check_if_ok_to_update(), True)
         self.assertEqual(self.blink.last_refresh, None)
-        with mock.patch(
-            "blinkpy.sync_module.BlinkSyncModule.refresh", return_value=True
-        ), mock.patch("blinkpy.blinkpy.Blink.get_homescreen", return_value=True):
+        with (
+            mock.patch(
+                "blinkpy.sync_module.BlinkSyncModule.refresh", return_value=True
+            ),
+            mock.patch("blinkpy.blinkpy.Blink.get_homescreen", return_value=True),
+        ):
             await self.blink.refresh(force=True)
 
         self.assertEqual(self.blink.last_refresh, now)
```
<a href='https://github.com/fronzbot/blinkpy/blob/5e58c1a14f7a8b2919c173358496d25242ad86de/tests/test_blinkpy.py#L81'>tests/test_blinkpy.py~L81</a>
```diff
     async def test_not_available_refresh(self):
         """Check that setup_post_verify executes on refresh when not avialable."""
         self.blink.available = False
-        with mock.patch(
-            "blinkpy.sync_module.BlinkSyncModule.refresh", return_value=True
-        ), mock.patch(
-            "blinkpy.blinkpy.Blink.get_homescreen", return_value=True
-        ), mock.patch("blinkpy.blinkpy.Blink.setup_post_verify", return_value=True):
+        with (
+            mock.patch(
+                "blinkpy.sync_module.BlinkSyncModule.refresh", return_value=True
+            ),
+            mock.patch("blinkpy.blinkpy.Blink.get_homescreen", return_value=True),
+            mock.patch("blinkpy.blinkpy.Blink.setup_post_verify", return_value=True),
+        ):
             self.assertTrue(await self.blink.refresh(force=True))
             with mock.patch("time.time", return_value=time.time() + 4):
                 self.assertFalse(await self.blink.refresh())
```

</p>
</details>
<details><summary><a href="https://github.com/pandas-dev/pandas">pandas-dev/pandas</a> (+29 -20 lines across 5 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/pandas-dev/pandas/blob/e0e47e8d37d4fc7c2e7634c5f9ca0ab7e44c1291/pandas/core/apply.py#L1320'>pandas/core/apply.py~L1320</a>
```diff
 
         # Convert from numba dict to regular dict
         # Our isinstance checks in the df constructor don't pass for numbas typed dict
-        with set_numba_data(self.obj.index) as index, set_numba_data(
-            self.columns
-        ) as columns:
+        with (
+            set_numba_data(self.obj.index) as index,
+            set_numba_data(self.columns) as columns,
+        ):
             res = dict(nb_func(self.values, columns, index))
 
         return res
```
<a href='https://github.com/pandas-dev/pandas/blob/e0e47e8d37d4fc7c2e7634c5f9ca0ab7e44c1291/pandas/tests/frame/methods/test_first_and_last.py#L60'>pandas/tests/frame/methods/test_first_and_last.py~L60</a>
```diff
         obj = tm.get_obj(obj, frame_or_series)
 
         msg = "'first' only supports a DatetimeIndex index"
-        with tm.assert_produces_warning(
-            FutureWarning, match=deprecated_msg
-        ), pytest.raises(TypeError, match=msg):  # index is not a DatetimeIndex
+        with (
+            tm.assert_produces_warning(FutureWarning, match=deprecated_msg),
+            pytest.raises(TypeError, match=msg),
+        ):  # index is not a DatetimeIndex
             obj.first("1D")
 
         msg = "'last' only supports a DatetimeIndex index"
-        with tm.assert_produces_warning(
-            FutureWarning, match=last_deprecated_msg
-        ), pytest.raises(TypeError, match=msg):  # index is not a DatetimeIndex
+        with (
+            tm.assert_produces_warning(FutureWarning, match=last_deprecated_msg),
+            pytest.raises(TypeError, match=msg),
+        ):  # index is not a DatetimeIndex
             obj.last("1D")
 
     def test_last_subset(self, frame_or_series):
```
<a href='https://github.com/pandas-dev/pandas/blob/e0e47e8d37d4fc7c2e7634c5f9ca0ab7e44c1291/pandas/tests/io/parser/common/test_read_errors.py#L58'>pandas/tests/io/parser/common/test_read_errors.py~L58</a>
```diff
     msg = "'utf-8' codec can't decode byte"
 
     # Stream must be binary UTF8.
-    with open(path, "rb") as handle, codecs.StreamRecoder(
-        handle, utf8.encode, utf8.decode, codec.streamreader, codec.streamwriter
-    ) as stream:
+    with (
+        open(path, "rb") as handle,
+        codecs.StreamRecoder(
+            handle, utf8.encode, utf8.decode, codec.streamreader, codec.streamwriter
+        ) as stream,
+    ):
         with pytest.raises(UnicodeDecodeError, match=msg):
             parser.read_csv(stream)
 
```
<a href='https://github.com/pandas-dev/pandas/blob/e0e47e8d37d4fc7c2e7634c5f9ca0ab7e44c1291/pandas/tests/io/pytables/test_append.py#L939'>pandas/tests/io/pytables/test_append.py~L939</a>
```diff
     df1.iloc[1, df1.columns.get_indexer(["A", "B"])] = np.nan
     df = concat([df1, df2], axis=1)
 
-    with ensure_clean_store(setup_path) as store, pd.option_context(
-        "io.hdf.dropna_table", True
+    with (
+        ensure_clean_store(setup_path) as store,
+        pd.option_context("io.hdf.dropna_table", True),
     ):
         # dropna=False shouldn't synchronize row indexes
         store.append_to_multiple(
```
<a href='https://github.com/pandas-dev/pandas/blob/e0e47e8d37d4fc7c2e7634c5f9ca0ab7e44c1291/pandas/tests/io/test_gcs.py#L113'>pandas/tests/io/test_gcs.py~L113</a>
```diff
     """
     if compression == "zip":
         # Only compare the CRC checksum of the file contents
-        with zipfile.ZipFile(BytesIO(result)) as exp, zipfile.ZipFile(
-            BytesIO(expected)
-        ) as res:
+        with (
+            zipfile.ZipFile(BytesIO(result)) as exp,
+            zipfile.ZipFile(BytesIO(expected)) as res,
+        ):
             for res_info, exp_info in zip(res.infolist(), exp.infolist()):
                 assert res_info.CRC == exp_info.CRC
     elif compression == "tar":
-        with tarfile.open(fileobj=BytesIO(result)) as tar_exp, tarfile.open(
-            fileobj=BytesIO(expected)
-        ) as tar_res:
+        with (
+            tarfile.open(fileobj=BytesIO(result)) as tar_exp,
+            tarfile.open(fileobj=BytesIO(expected)) as tar_res,
+        ):
             for tar_res_info, tar_exp_info in zip(
                 tar_res.getmembers(), tar_exp.getmembers()
             ):
```

</p>
</details>
<details><summary><a href="https://github.com/rotki/rotki">rotki/rotki</a> (+59 -29 lines across 11 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/rotkehlchen.py#L1178'>rotkehlchen/rotkehlchen.py~L1178</a>
```diff
         if oracle != HistoricalPriceOracle.CRYPTOCOMPARE:
             return  # only for cryptocompare for now
 
-        with contextlib.suppress(
-            UnknownAsset
+        with (
+            contextlib.suppress(UnknownAsset)
         ):  # if suppress -> assets are not crypto or fiat, so we can't query cryptocompare  # noqa: E501
             self.cryptocompare.create_cache(
                 from_asset=from_asset,
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/api/test_balances.py#L1049'>rotkehlchen/tests/api/test_balances.py~L1049</a>
```diff
         side_effect={ChainID.ETHEREUM: ()},
     )  # noqa: E501
 
-    with account_balance_patch, query_tokens_patch, price_inquirer_patch, defi_query_patch, vaults_patch, multieth_balance_patch, protocols_patch, proxies_inquirer_patch:  # noqa: E501
+    with (
+        account_balance_patch,
+        query_tokens_patch,
+        price_inquirer_patch,
+        defi_query_patch,
+        vaults_patch,
+        multieth_balance_patch,
+        protocols_patch,
+        proxies_inquirer_patch,
+    ):  # noqa: E501
 
         def query_blockchain_balance(num: int) -> Any:
             """Refreshes blockchain balances `num` number of times"""
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/api/test_misc.py#L96'>rotkehlchen/tests/api/test_misc.py~L96</a>
```diff
         expected_version, rotki.data_dir, latest_version=expected_version
     )  # noqa: E501
 
-    with version_patch, release_patch, patch.dict(
-        os.environ, {"ROTKI_ACCEPT_DOCKER_RISK": "whatever"}
+    with (
+        version_patch,
+        release_patch,
+        patch.dict(os.environ, {"ROTKI_ACCEPT_DOCKER_RISK": "whatever"}),
     ):  # noqa: E501
         response = requests.get(
             url=api_url_for(
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/api/test_snapshots.py#L579'>rotkehlchen/tests/api/test_snapshots.py~L579</a>
```diff
     # check that POST with the file works.
     csv_dir2 = str(tmpdir_factory.mktemp("test_csv_dir_2"))
     _create_snapshot_with_valid_data_for_post(csv_dir2, Timestamp(1651075))
-    with open(
-        f"{csv_dir2}/{BALANCES_FOR_IMPORT_FILENAME}", encoding="utf8"
-    ) as balances_file, open(
-        f"{csv_dir2}/{LOCATION_DATA_IMPORT_FILENAME}", encoding="utf8"
-    ) as locations_file:  # noqa: E501
+    with (
+        open(f"{csv_dir2}/{BALANCES_FOR_IMPORT_FILENAME}", encoding="utf8") as balances_file,
+        open(f"{csv_dir2}/{LOCATION_DATA_IMPORT_FILENAME}", encoding="utf8") as locations_file,
+    ):  # noqa: E501
         response = requests.post(
             api_url_for(
                 rotkehlchen_api_server,
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/data_migrations/test_migrations.py#L481'>rotkehlchen/tests/data_migrations/test_migrations.py~L481</a>
```diff
             websocket_connection=websocket_connection,
         )
 
-    with GlobalDBHandler().conn.write_ctx() as write_cursor:  # check the global db for the polygon etherscan node  # noqa: E501
+    with (
+        GlobalDBHandler().conn.write_ctx() as write_cursor
+    ):  # check the global db for the polygon etherscan node  # noqa: E501
         assert (
             write_cursor.execute(
                 'SELECT COUNT(*) FROM default_rpc_nodes WHERE name="polygon etherscan"'
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/data_migrations/test_migrations.py#L494'>rotkehlchen/tests/data_migrations/test_migrations.py~L494</a>
```diff
             ).fetchone()[0]
             == 1
         )  # noqa: E501
-    with rotki.data.db.user_write() as write_cursor:  # check the user db for the polygon etherscan node  # noqa: E501
+    with (
+        rotki.data.db.user_write() as write_cursor
+    ):  # check the user db for the polygon etherscan node  # noqa: E501
         assert (
             write_cursor.execute(
                 'SELECT COUNT(*) FROM rpc_nodes WHERE name="polygon etherscan"'
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/exchanges/test_bitstamp.py#L164'>rotkehlchen/tests/exchanges/test_bitstamp.py~L164</a>
```diff
     def mock_api_query_response(endpoint):  # pylint: disable=unused-argument
         return MockResponse(HTTPStatus.OK, '{"key"}')
 
-    with patch.object(
-        mock_bitstamp, "_api_query", side_effect=mock_api_query_response
-    ), pytest.raises(RemoteError):  # noqa: E501
+    with (
+        patch.object(mock_bitstamp, "_api_query", side_effect=mock_api_query_response),
+        pytest.raises(RemoteError),
+    ):  # noqa: E501
         mock_bitstamp.query_balances()
 
 
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/exchanges/test_bitstamp.py#L228'>rotkehlchen/tests/exchanges/test_bitstamp.py~L228</a>
```diff
     def mock_api_query_response(endpoint):  # pylint: disable=unused-argument
         return MockResponse(HTTPStatus.OK, '{"link_balance": "1.00000000"}')
 
-    with patch("rotkehlchen.exchanges.bitstamp.Inquirer", return_value=inquirer), patch.object(
-        mock_bitstamp, "_api_query", side_effect=mock_api_query_response
+    with (
+        patch("rotkehlchen.exchanges.bitstamp.Inquirer", return_value=inquirer),
+        patch.object(mock_bitstamp, "_api_query", side_effect=mock_api_query_response),
     ):  # noqa: E501
         assert mock_bitstamp.query_balances() == ({}, "")
 
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/external_apis/test_bisq_market.py#L18'>rotkehlchen/tests/external_apis/test_bisq_market.py~L18</a>
```diff
 def test_missing_market_request():
     """Test that error is correctly raised when there is no market. Not VCRed since
     the only failure is timeout here and VCR does not record them"""
-    with patch(
-        "rotkehlchen.db.settings.CachedSettings.get_timeout_tuple", return_value=(1, 1)
-    ), pytest.raises(RemoteError):  # noqa: E501
+    with (
+        patch("rotkehlchen.db.settings.CachedSettings.get_timeout_tuple", return_value=(1, 1)),
+        pytest.raises(RemoteError),
+    ):  # noqa: E501
         get_bisq_market_price(A_3CRV.resolve_to_crypto_asset())
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/integration/test_blockchain.py#L125'>rotkehlchen/tests/integration/test_blockchain.py~L125</a>
```diff
 
     assert addr1 in blockchain.accounts.eth
 
-    with etherscan_patch, evmtokens_max_chunks_patch, defi_balances_mock, add_defi_mock, beaconchain_patch:  # noqa: E501
+    with (
+        etherscan_patch,
+        evmtokens_max_chunks_patch,
+        defi_balances_mock,
+        add_defi_mock,
+        beaconchain_patch,
+    ):  # noqa: E501
         greenlets = [
             # can't call query_eth_balances directly since we have to update totals
             gevent.spawn_later(
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/unit/test_bitcoin.py#L584'>rotkehlchen/tests/unit/test_bitcoin.py~L584</a>
```diff
             check_balances(balances)
 
             # Third source fails - FATALITY!!!
-            with patch(
-                "rotkehlchen.chain.bitcoin._query_mempool_space",
-                MagicMock(side_effect=RemoteError("Fatality")),
-            ), pytest.raises(RemoteError):  # noqa: E501
+            with (
+                patch(
+                    "rotkehlchen.chain.bitcoin._query_mempool_space",
+                    MagicMock(side_effect=RemoteError("Fatality")),
+                ),
+                pytest.raises(RemoteError),
+            ):  # noqa: E501
                 get_bitcoin_addresses_balances(addresses)
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/unit/test_inquirer.py#L429'>rotkehlchen/tests/unit/test_inquirer.py~L429</a>
```diff
 @pytest.mark.parametrize("use_clean_caching_directory", [True])
 @pytest.mark.parametrize("should_mock_current_price_queries", [False])
 def test_find_curve_lp_token_price(inquirer_defi, ethereum_manager):
-    with GlobalDBHandler().conn.write_ctx() as write_cursor:  # querying curve lp token price normally triggers curve cache query. Set all query ts to now, so it does not happen.  # noqa: E501
+    with (
+        GlobalDBHandler().conn.write_ctx() as write_cursor
+    ):  # querying curve lp token price normally triggers curve cache query. Set all query ts to now, so it does not happen.  # noqa: E501
         write_cursor.execute(
             "UPDATE general_cache SET last_queried_ts=? WHERE key=?", (ts_now(), "CURVE_LP_TOKENS")
         )  # noqa: E501
```
<a href='https://github.com/rotki/rotki/blob/edf2ec2647a4ebccb7b78fe4f38758fb656cf80e/rotkehlchen/tests/unit/test_tasks_manager.py#L248'>rotkehlchen/tests/unit/test_tasks_manager.py~L248</a>
```diff
     )  # pylint: disable=protected-member  # noqa: E501
     queried_receipts = set()
     try:
-        with gevent.Timeout(
-            timeout
-        ), receipt_get_patch as receipt_task_mock, mock_evm_chains_with_transactions():  # noqa: E501
+        with (
+            gevent.Timeout(timeout),
+            receipt_get_patch as receipt_task_mock,
+            mock_evm_chains_with_transactions(),
+        ):  # noqa: E501
             task_manager.schedule()
             with database.conn.read_ctx() as cursor:
                 while len(queried_receipts) != 2:
```

</p>
</details>
<details><summary><a href="https://github.com/sphinx-doc/sphinx">sphinx-doc/sphinx</a> (+22 -14 lines across 4 files)</summary>
<p>
<pre>ruff format --preview</pre>
</p>
<p>

<a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/sphinx/builders/__init__.py#L362'>sphinx/builders/__init__.py~L362</a>
```diff
             # save the environment
             from sphinx.application import ENV_PICKLE_FILENAME
 
-            with progress_message(__("pickling environment")), open(
-                path.join(self.doctreedir, ENV_PICKLE_FILENAME), "wb"
-            ) as f:
+            with (
+                progress_message(__("pickling environment")),
+                open(path.join(self.doctreedir, ENV_PICKLE_FILENAME), "wb") as f,
+            ):
                 pickle.dump(self.env, f, pickle.HIGHEST_PROTOCOL)
 
             # global actions
```
<a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/sphinx/builders/linkcheck.py#L72'>sphinx/builders/linkcheck.py~L72</a>
```diff
 
         output_text = path.join(self.outdir, "output.txt")
         output_json = path.join(self.outdir, "output.json")
-        with open(output_text, "w", encoding="utf-8") as self.txt_outfile, open(
-            output_json, "w", encoding="utf-8"
-        ) as self.json_outfile:
+        with (
+            open(output_text, "w", encoding="utf-8") as self.txt_outfile,
+            open(output_json, "w", encoding="utf-8") as self.json_outfile,
+        ):
             for result in checker.check(self.hyperlinks):
                 self.process_result(result)
 
```
<a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/sphinx/util/docutils.py#L232'>sphinx/util/docutils.py~L232</a>
```diff
 @contextmanager
 def patch_docutils(confdir: str | None = None) -> Generator[None, None, None]:
     """Patch to docutils temporarily."""
-    with patched_get_language(), patched_rst_get_language(), using_user_docutils_conf(
-        confdir
-    ), du19_footnotes():
+    with (
+        patched_get_language(),
+        patched_rst_get_language(),
+        using_user_docutils_conf(confdir),
+        du19_footnotes(),
+    ):
         yield
 
 
```
<a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_build_linkcheck.py#L800'>tests/test_build_linkcheck.py~L800</a>
```diff
 
 @pytest.mark.sphinx("linkcheck", testroot="linkcheck-localserver", freshenv=True)
 def test_too_many_requests_retry_after_int_delay(app, capsys, status):
-    with http_server(make_retry_after_handler([(429, "0"), (200, None)])), mock.patch(
-        "sphinx.builders.linkcheck.DEFAULT_DELAY", 0
-    ), mock.patch("sphinx.builders.linkcheck.QUEUE_POLL_SECS", 0.01):
+    with (
+        http_server(make_retry_after_handler([(429, "0"), (200, None)])),
+        mock.patch("sphinx.builders.linkcheck.DEFAULT_DELAY", 0),
+        mock.patch("sphinx.builders.linkcheck.QUEUE_POLL_SECS", 0.01),
+    ):
         app.build()
     content = (app.outdir / "output.json").read_text(encoding="utf8")
     assert json.loads(content) == {
```
<a href='https://github.com/sphinx-doc/sphinx/blob/35965903177c6ed9a6afb62ccd33243a746a3fc0/tests/test_build_linkcheck.py#L859'>tests/test_build_linkcheck.py~L859</a>
```diff
 
 @pytest.mark.sphinx("linkcheck", testroot="linkcheck-localserver", freshenv=True)
 def test_too_many_requests_retry_after_without_header(app, capsys):
-    with http_server(make_retry_after_handler([(429, None), (200, None)])), mock.patch(
-        "sphinx.builders.linkcheck.DEFAULT_DELAY", 0
+    with (
+        http_server(make_retry_after_handler([(429, None), (200, None)])),
+        mock.patch("sphinx.builders.linkcheck.DEFAULT_DELAY", 0),
     ):
         app.build()
     content = (app.outdir / "output.json").read_text(encoding="utf8")
```

</p>
</details>




---

_@MichaReiser reviewed on 2023-12-21 09:13_

---

_Review comment by @MichaReiser on `crates/ruff_python_formatter/src/expression/mod.rs`:541 on 2023-12-21 09:13_

It seems I don't understand these rules after all :(

---

_Marked ready for review by @MichaReiser on 2023-12-21 09:59_

---

_@charliermarsh reviewed on 2023-12-22 02:23_

---

_Review comment by @charliermarsh on `crates/ruff_python_formatter/src/statement/stmt_with.rs`:145 on 2023-12-22 02:23_

That's a lot of cases, nice job figuring it out...

---

_@charliermarsh approved on 2023-12-22 02:24_

---

_Merged by @MichaReiser on 2023-12-22 03:41_

---

_Closed by @MichaReiser on 2023-12-22 03:41_

---

_Branch deleted on 2023-12-22 03:41_

---
