# ONS + MES + RSS Dedicated Server Compatibility Report

Public sanitized evidence package for testing **Orbital NPC Spawns** with **Modular Encounters Systems** on a **Real Solar Systems-scale Torch dedicated server**.

This package is meant for a Steam Workshop discussion, GitHub issue, or Discord support thread. It contains the report, concise analysis, relevant diagnostic trace, extracted evidence, and sanitized full logs. It does not contain source code, save files, credentials, IP addresses, Steam IDs, or raw private server data.

## Short summary

- ONS loaded on a Torch dedicated server.
- MES was detected and the MES API was ready.
- ONS intercepted a MES NPC grid and applied a stable-orbit/pass-by velocity correction.
- The corrected grid initially reached the target velocity, then slowed down after ONS released control.
- MES also warned about NPC grid spawning beyond 6500 km from world center.
- The tested RSS-scale intercept occurred roughly 38,731 km from world center.

## Start here

- Main report: `reports/Bug_Report_ONS_MES_RSS_Dedicated_Server.md`
- Follow-up analysis: `reports/ONS_MES_RSS_Test_Analysis.md`
- Evidence index: `evidence/README.md`

## Folder layout

```text
reports/
  Bug_Report_ONS_MES_RSS_Dedicated_Server.md
  ONS_MES_RSS_Test_Analysis.md
evidence/
  diagnostic-trace/
  key-extracts/
  mod-list/
logs/
  full-sanitized/
STEAM_DISCUSSION_POST.md
```

## Sanitization

The files were reviewed for public sharing. Sensitive values are redacted with placeholders such as:

- `<IP_ADDRESS>`
- `<PORT>`
- `<SERVER_NAME>`
- `<SAVE_PATH>`
- `<TORCH_INSTANCE>`
- `<TORCH_ROOT>`
- `<TEST_PLAYER>`

The package intentionally excludes private staging folders, internal templates, raw save data, and unrelated supplemental Steam client logs.
