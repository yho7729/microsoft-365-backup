# office 365 backup solutions: native Microsoft 365 Backup, third-party tools, and the bring-your-own-storage math most reviews skip

If you're searching for office 365 backup solutions, you're really asking one of three questions: should I pay Microsoft for its native backup, buy a third-party SaaS tool, or build something myself? The short version: Microsoft 365 Backup costs $0.15 per GB per month and restores for free, but your recovery window tops out at 2 years. Third-party tools like Veeam, Acronis, and AvePoint add flexibility and longer retention at prices they don't publish openly. And there's a third route most comparison posts gloss over — pointing a backup tool at your own S3-compatible storage, where capacity costs a fraction of per-GB SaaS pricing.

This piece walks through all three, with the actual numbers, the fine print, and a frank look at who each option fits.

> **Quick picks**
> - Want zero setup and a bill from Microsoft? Use native **Microsoft 365 Backup**: $0.15/GB/month, restores free, backups are immutable.
> - Want malware scanning and long retention handled for you? Look at third-party platforms — Veeam, Acronis Cyber Protect Cloud, AvePoint, Datto SaaS Protection, and Keepit are the usual names.
> - Comfortable running backup software yourself? Point it at your own object storage. Sharktech's S3 Object Storage runs **$4.90/TB per month** with versioning and object locking built in — 👉 [check Sharktech S3 storage pricing and plans](https://bit.ly/SharKtech)

## Why "it's in Microsoft's cloud" doesn't mean it's backed up

Microsoft runs Exchange Online, OneDrive, and SharePoint with multiple physically redundant copies, and that redundancy handles hardware failures without you lifting a finger. That's availability, though — it's a different thing from backup.

Redundancy won't save you when someone deletes a mailbox, ransomware encrypts files through a legitimate user session, or a synced folder quietly propagates corruption to every copy. Microsoft's own documentation for Microsoft 365 Backup lists exactly these scenarios — malicious overwrites, ransomware, accidental deletion — as the problems the product exists to solve.

There's also a retention gap. Per Barracuda's comparison of backup platforms, Microsoft's native tooling retains key Entra ID components for only 30 days. If nobody notices a problem for six weeks, native recovery options get thin. That 30-day window is the single most-quoted reason third-party backup exists.

## Route 1: native Microsoft 365 Backup — what you actually get and pay

Microsoft's first-party backup covers SharePoint sites, OneDrive accounts, and Exchange Online mailboxes, and it's priced at **$0.15/GB/month** on all protected data. Restores are free. That pricing line matters more than any feature list, so let's put real numbers on it later.

The engineering is solid. Backups are immutable unless a backup admin deliberately deletes them through offboarding. Storage is append-only: SharePoint can add new content blobs but can never modify old ones, and Exchange items are backed up the same way, inaccessible to client processes like Outlook or OWA. Data never leaves the Microsoft 365 trust boundary and honors your existing data residency. Everything is auditable.

Recovery points are frequent — every 10 minutes for the last two weeks, weekly snapshots going back to 52 weeks. Restore speed is respectable: Microsoft cites up to 1,000 average-size mailboxes, sites, or accounts per restore at a median rate of 1–3 TB per hour.

The constraint is the recovery window: you configure it at 3, 6, 12, or 24 months, and existing policies default to 1 year. If you need retention beyond two years — common in regulated industries — native backup alone won't get you there. Also worth noting: file version restore for OneDrive and SharePoint is listed as coming soon, and GCC organizations are now supported.

## Route 2: third-party backup platforms — who does what

Every serious comparison of office 365 backup solutions ends up listing the same eight or so platforms. Here's the honest rundown based on what's verifiable, without pretending any of them published usable pricing (most don't — you'll request a quote):

| Platform | Best known for | Standout detail |
| --- | --- | --- |
| Barracuda Cloud-to-Cloud Backup | Security-first bundle | Covers Teams, Groups, Exchange, SharePoint, OneDrive, Planner, OneNote; malware scanning before restore |
| Veeam Backup for Microsoft 365 | Control and flexibility | 25+ restore types; can target AWS, Azure, or your own hardware |
| Acronis Cyber Protect Cloud | Integrated security | Built-in malware and antivirus scanning; up to 6 backups per day |
| AvePoint Cloud Backup | Broadest coverage | Includes niche workloads like Project Online and Viva Engage |
| Rubrik Security Cloud | Enterprise cyber resilience | Air-gapped backups and mass recovery |
| Datto SaaS Protection | MSP and SMB simplicity | Infinite retention by default; formerly Backupify |
| N-able Cove Data Protection | Unified dashboards | One console for Microsoft 365 plus servers and endpoints; 7-year retention |
| Keepit | Independent infrastructure | Own storage, blockchain-verified immutability, flat unlimited pricing |

A caveat you should know: the most widely circulated "top backup solutions" lists are written by the vendors themselves, and the vendor's own product usually lands in the number one spot. Treat pros-and-cons columns accordingly. The underlying feature facts above hold up across sources; the editorial rankings don't deserve the same trust.

One name in that table deserves a second look for this article's purposes: Veeam. Because it can back up to storage you control, it's the bridge to route 3.

## Route 3: bring your own storage — the math most reviews skip

Here's the angle that vendor comparison posts have no incentive to explain. Native Microsoft backup bills you per protected gigabyte, every month, forever. Third-party SaaS tools bill per user or per workload, also forever. The DIY route splits the bill in two: a backup software license plus raw storage, where raw storage is the cheap part.

Take a 2 TB tenant. Native Microsoft 365 Backup: 2,000 GB × $0.15 = **$300/month, $3,600/year**. Every year, indefinitely, as long as you keep the protection active.

The same 2 TB on Sharktech's S3 Object Storage: **$9.80/month** at the flat $4.90/TB rate — the per-TB price stays the same whether you run 1 TB or 100 TB. Even adding backup software on top, the storage line item alone is roughly 3% of what the native service charges for equivalent capacity. Add Sharktech's included 1 TB of monthly transfer (extra egress is $0.90/TB), and routine restore traffic mostly stays inside the bundle.

The trade is real, though, and worth naming plainly. The DIY route means you operate the backup tool, monitor the jobs, and test the restores. There's no vendor console where you click "recover my tenant." If your IT team is one person who's already stretched thin, the management overhead may cost more than the storage savings. This is a judgment call about your team, and both answers can be correct.

It also helps that Sharktech's storage has the features backup targets actually need: object versioning, object locking for retention and immutability-style protection, lifecycle rules for aging out old snapshots, and an S3-compatible API so tools like Rclone, Cyberduck, Nextcloud, and S3 Browser connect without custom glue code.

> If you want to price out your own tenant size at that flat rate, 👉 [see Sharktech's S3 Object Storage tiers from 1 TB to 100 TB](https://bit.ly/SharKtech)

## Sharktech's current plans, all in one table

Sharktech sells two products relevant here: S3 Object Storage (the DIY backup target) and hosted Acronis Backup Service (managed device and file backup). Every plan currently shown on the official site, in USD:

| Plan | What's included | Price | Billing cycle | Order |
| --- | --- | --- | --- | --- |
| S3 Object Storage 1 TB | 1 TB storage + 1 TB/mo transfer; extra egress $0.90/TB | $4.90/mo | Monthly | [ Order 1 TB](https://bit.ly/SharKtech) |
| S3 Object Storage 10 TB | Same terms, 10× capacity | $49/mo | Monthly | [ Order 10 TB](https://bit.ly/SharKtech) |
| S3 Object Storage 100 TB | Same terms; beyond 100 TB / 300 TB transfer, contact sales | $490/mo | Monthly | [ Order 100 TB](https://bit.ly/SharKtech) |
| Acronis Backup Hosting — monthly | 200 GB cloud storage; encryption, compression, file or full-system restore | $4/mo | Monthly | [ Start at $4/mo](https://bit.ly/SharKtech) |
| Acronis Backup Hosting — quarterly | Same 200 GB base | $8 / 3 months | Quarterly | [ Order quarterly](https://bit.ly/SharKtech) |
| Acronis Backup Hosting — semi-annual | Same 200 GB base | $12 / 6 months | Semi-annual | [ Order semi-annual](https://bit.ly/SharKtech) |
| Acronis Backup Hosting — annual | Same 200 GB base | $24/yr | Annual | [ Order annual](https://bit.ly/SharKtech) |

One honest note on the Acronis plan: Sharktech's blog post lists overage rates per billing cycle, but those numbers don't scale sensibly across cycles, and the current product page simply says the final recurring price depends on the capacity and options you select at checkout. So treat $4/month with 200 GB included as the anchor figure and confirm the per-GB overage for your usage on the checkout page before committing. The same applies to the optional File Sync & Share add-on.

## What Sharktech's Acronis hosting does and doesn't cover

This is where some readers will over-assume, so let's be precise. Sharktech's Acronis Backup Service, as described on their own pages, protects data from devices — documents, photos, videos, full systems — into their cloud, with a web console and mobile app for access. That's endpoint and file backup hosting.

What the available materials don't establish is whether this hosted plan includes Microsoft 365 workload backup (mailboxes, OneDrive, SharePoint) as an Acronis workload type. Don't buy it on that assumption. If your goal is specifically Office 365 data, the S3 Object Storage route with a backup tool you control is the verifiable path; the Acronis hosting plan is for backing up laptops, servers, and files. Sharktech does advertise 24/7 support with their own IT staff and redundant, high-availability backup infrastructure, and their blog pegs the service at roughly half the cost of competitors — that last figure is the company's own claim, not something independent sources have verified.

## FAQ

**Does Microsoft 365 really need a third-party backup?**
Not always. Native Microsoft 365 Backup now exists and is technically strong — immutable, append-only, fast restores. You need something beyond it if you need retention past 2 years, protection for Entra ID components (30-day native retention), per-user self-service restores, or coverage for workloads like Teams conversations and Planner. If none of those apply and $0.15/GB fits your budget, the native service is a legitimate answer.

**How much would native backup cost for my tenant?**
Multiply your protected data in GB by $0.15. A 500 GB tenant runs $75/month ($900/year); 5 TB runs $750/month ($9,000/year). Restores are free, so the bill is purely the protected footprint.

**Can I point Veeam or similar tools at my own storage?**
Veeam Backup for Microsoft 365 supports targeting AWS, Azure, or your own hardware, which is what makes the bring-your-own-storage model possible. Any S3-compatible object store with versioning and object locking — the features Sharktech's storage exposes — is a plausible target for tools that support the S3 API. Confirm your specific backup tool's compatibility list before buying storage.

**Is cheap object storage safe for backups?**
The price doesn't tell you much either way; the features do. What matters: versioning (survives overwrites), object locking (survives deletion attempts), and redundancy. Sharktech runs high-capacity HDD as the primary tier with an NVMe cache layer for active data, backed by their own data centers with DDoS protection. What you give up versus big-cloud storage is guaranteed low latency and full-block storage semantics — their own product page says the service is capacity-first and explicitly not a replacement for block storage or boot disks. For backup targets, where throughput matters more than latency, that trade usually works in your favor.

## So which route should you pick?

Match the option to your situation:

- **Small tenant, no IT staff, want it handled:** native Microsoft 365 Backup. Costly per GB, but genuinely zero-maintenance, and restores are free.
- **Regulated industry or long retention requirements:** a third-party platform with default infinite or multi-year retention (Datto and N-able are the known examples). Expect quote-based pricing.
- **Technical team, cost-sensitive, larger data footprint:** backup tool of your choice plus Sharktech S3 at $4.90/TB. On a 2 TB tenant, you're comparing roughly $120/year in storage against $3,600/year native — before counting the backup software license on either side.
- **Just need endpoint and file backup with someone else running the infrastructure:** Sharktech's hosted Acronis plan at $4/month with 200 GB is the entry point, and the annual plan works out to $2/month if the 200 GB base covers you.

Before you order either Sharktech product, check two things on the checkout page: your actual current Microsoft 365 data volume (so you size the storage tier right), and the overage rate for whatever you expect to exceed the included amounts. Then 👉 [open Sharktech's plans and pick your storage tier](https://bit.ly/SharKtech)
