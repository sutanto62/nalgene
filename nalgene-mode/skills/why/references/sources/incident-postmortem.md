# Incident & Postmortem Context

This plugin has no ticket-tracker or chat investigator, so look for incident signal inside the sources you do have:

- **Git**: commit messages like "fix for incident", "add defensive check", "hotfix", or a "revert" followed later by "re-apply with..." are strong signals. Same-day merges on a defensive-looking change are worth flagging even without a message that says so.
- **Openspec**: an archived change whose proposal mentions an incident, outage, or production issue by description (even without a ticket ID) is Direct evidence — quote it.

If a commit or PR references an external ticket or incident channel this plugin can't search (a Linear ID, a `#sev-NNN` mention), record it as an Additional Lead rather than chasing it — note that the project may have an issue tracker or chat history worth checking by hand.
