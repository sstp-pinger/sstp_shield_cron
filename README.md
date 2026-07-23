# sstp_shield_cron

Scheduler-only repo for the SSTP Shield backend's VPNGate server sync. No
source code lives here — it exists purely so its GitHub Actions cron
(`.github/workflows/sync-servers.yml`) can run every 10 minutes on GitHub's
**public-repo unlimited Actions minutes**, instead of burning through the
2,000 minutes/month a private repo gets on GitHub Free (a 10-minute cron alone
is close to that limit).

Every run just does:

```sh
curl -X POST "$VERCEL_URL/api/internal/sync-servers" -H "X-Sync-Secret: $SYNC_SECRET"
```

against the deployed backend at
[sstp_shield_server](https://github.com/sstp-pinger/sstp_shield_server)
(private — that's where the actual sync logic lives).

## Setup

Repo secrets (Settings → Secrets and variables → Actions):

- `VERCEL_URL` — the backend's production URL, e.g.
  `https://sstp-shield-server-sstp-pingers-projects.vercel.app`
- `SYNC_SECRET` — must match the backend's `SYNC_SECRET` env var exactly.

Nothing else to configure. `workflow_dispatch` is enabled too, for a manual
trigger from the Actions tab or `gh workflow run sync-servers.yml`.
