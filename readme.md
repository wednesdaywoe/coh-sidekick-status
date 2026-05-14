# coh-sidekick-status

Public status endpoint for [coh-sidekick.com](https://coh-sidekick.com).

Sidekick fetches `status.json` once on app load. If `status !== "ok"`, an in-app banner displays the `message` to all users.

## File: `status.json`

```json
{
  "status": "ok",
  "message": null
}




status	"ok" | "degraded" | "down"	ok hides the banner. degraded shows amber. down shows red.
message	string | null	Shown verbatim in the banner. Required when status is not ok.
Examples
Normal operation:


{ "status": "ok", "message": null }
Degraded (something is broken but the app still works):


{ "status": "degraded", "message": "Save/load is slow right now — investigating." }
Down (the app is unusable):


{ "status": "down", "message": "Sidekick is offline for emergency maintenance. Back by ~3:00 PM ET." }
How to deploy a change
Edit status.json on the main branch.
Commit + push.
GitHub Pages serves the new file within ~30s.
CDN caches it for up to 10 minutes (cache-control: max-age=600), so existing users may take that long to see updates on their next page load.
During an incident
Set status to degraded or down, write a clear, short message.
Push.
After the incident, set back to { "status": "ok", "message": null } and push again — don't leave a stale incident banner up.
Why is this a separate repo?
So the status page stays reachable even when the main Sidekick deploy is broken. If everything was in one repo, a bad Sidekick deploy could take down the way users find out Sidekick is broken.


