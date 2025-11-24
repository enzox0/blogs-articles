# Free Deployment Best Practices (Vercel & Render)

## Plan Your Project Footprint
- Audit runtime, bandwidth, and storage needs before deploying; free plans have tight quotas.
- Keep build artifacts small (tree-shake, compress assets) to stay within upload limits.
- Schedule stress tests during off-peak hours to avoid rate throttling.

## Code & Dependency Hygiene
- Lock dependency versions and prune unused packages to shorten cold starts.
- Pre-generate static pages whenever possible; SSR costs more resources.
- Add health checks and lightweight logging to detect failures without spamming logs.

## Vercel-Specific Tips
- Use edge functions only when latency matters; default serverless functions have higher free quotas.
- Configure `vercel.json` to cache static assets aggressively and cap serverless regions to one or two to reduce cold starts.
- Monitor Vercel Analytics for function invocations; refactor routes with high error rates to reduce retries.
- Employ environment variable encryption via Vercel dashboard instead of `.env` files pushed to Git.

## Render-Specific Tips
- Prefer static site or cron jobs over web services when workloads are bursty; they consume fewer free resources.
- Use Render's background worker for scheduled maintenance instead of keeping a web dyno idle.
- Set autoscaling to the minimum replica count and enable zero-downtime deploys only for production-critical services.
- Cache build dependencies with `render-build.sh` scripts to avoid repeated downloads.

## Shared Operational Practices
- Automate CI checks locally (lint, test) so failed builds don't burn free build minutes.
- Set up alerting for runtime quota usage and deploy errors (Vercel's Slack hooks, Render's email alerts).
- Document manual recovery steps; free tiers often lack support SLAs.

## Security & Compliance
- Rotate tokens quarterly; both platforms allow regenerating deploy hooks without downtime.
- Limit team access roles; free plans typically cap seats, so use least-privilege policies.
- Review third-party integrations and revoke unused ones to reduce attack surface.

## When to Upgrade
- Track cumulative downtime or throttling events; if they impact KPIs, justify a paid tier.
- Compare hidden costs (developer time, failed deploys) against paid plan pricing to decide on upgrades.

