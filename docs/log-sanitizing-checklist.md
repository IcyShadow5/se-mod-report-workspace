# Log Sanitizing Checklist

Remove or verify before sharing:

- Windows username paths.
- Steam IDs and player identifiers.
- Server IPs, ports, remote/RCON/admin endpoints.
- Tokens, passwords, API keys, webhooks.
- Full save/config files unless reviewed.
- Private Discord/server details.

Usually safe to keep:

- Mod names and Workshop IDs.
- Error messages.
- Stack traces.
- Timestamps.
- Non-private config values directly relevant to the issue.
